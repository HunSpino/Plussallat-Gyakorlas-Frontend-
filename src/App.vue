<template>
  <div>
    <h1>Plüssök</h1>
    <table>
      <thead>
        <tr>
          <th>Azonsító</th>
          <th>Plüss Név</th>
          <th>Műveletek</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="pluss in plusses" v-bind:key="pluss.id">
          <td>{{ pluss.id }}</td>
          <td>{{ pluss.name }}</td>
          <td>
            <button @click="deletePluss(pluss.id)">Delete</button>
            <button @click="editPluss(pluss.id)">Edit</button>
          </td>
        </tr>
        <tr>
          <td>
            <input type="hidden" v-model="pluss.id">
          </td>
          <td>
            <input type="text" v-model="pluss.name">
          </td>
          <td>
            <button v-if="mod_new" @click="newPluss" :disabled="saving">Létrehoz</button>
            <button v-if="!mod_new" @click="savePluss" :disabled="saving">Mentés</button>
            <button v-if="!mod_new" @click="cancelEdit" :disabled="saving">Mégse</button>
          </td>
        </tr>
      </tbody>
    </table>
    <button @click="loadData">Adatok betöltése</button>
  </div>
</template>

<script>


export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      mod_new: true, 
      saving: false,
      pluss: {
        id: null,
        name: ''
      },
      plusses: []
    }
  },
  methods: {
    async loadData () {
     let Response = await fetch('http://127.0.0.1:8000/api/plusses')
     let data = await Response.json()
     this.plusses = data
    },
    async deletePluss(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/plusses/${id}`, {
        method: 'DELETE'
      })
      console.log(Response)
      await this.loadData()
    },
    async newPluss() {
      this.saving='disabled'
     await fetch('http://127.0.0.1:8000/api/plusses', {
       method: 'POST',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.pluss) 
     })
     await this.loadData()
     this.saving=false
     this.resetForm()
    },
    async savePluss() {
      this.saving='disabled'
     await fetch(`http://127.0.0.1:8000/api/plusses/${this.pluss.id}`, {
       method: 'PATCH',
       headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
       },
       body: JSON.stringify(this.pluss) 
     })
     await this.loadData()
     this.saving=false
     this.resetForm()
    },
    async editPluss(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/plusses/${id}`)
      let data = await Response.json()
      this.pluss = {...data};
      this.mod_new = false
    },
    cancelEdit () {
      this.resetForm()
    },
    resetForm() {
      this.pluss = {
        id: null,
        name: ''
      }
      this.mod_new = true
    }
  },
  mounted() {
    this.loadData()
  }
}
</script>

<style>
/*#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}*/
</style>
