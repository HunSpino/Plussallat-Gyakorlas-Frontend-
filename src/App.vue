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
            <button>Delete</button>
            <button>Edit</button>
          </td>
        </tr>
        <tr>
          <td>
            <input type="hidden" v-model="pluss.id">
          </td>
          <td>
            <input type="text" v-model="pluss.name">
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
