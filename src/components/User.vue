<template>
  <div>
    <h1>Selamat datang</h1>
    <ul>
      <li v-for="item in users" :key="item.id">{{ item.deskripsi }}<button @click="hapus(item.id)">X</button></li>
    </ul>
    <input v-model="username"/>
    <input v-model="password"/>
    <button @click="tambah">Add</button>
  </div>
</template>

<script>
import axios from 'axios'

export default { 
  data: function () {
    return {
      users: [],
      username: '',
      password: ''
    }
  },
  created: function() {
    axios.get('http://localhost:3000/user')
    .then(result=>{
      this.users = result.data
    })
  },
  methods: {
    tambah: function() {
      const newItem = {deskripsi: this.myText}
      axios.post('http://localhost:3000/user', newItem)
      this.users.push(newItem)
    },
    hapus: function(id) {
      axios.delete(`http://localhost:3000/user/${id}`)
      this.users.splice(id-1, 1)
    }
  }
}
</script>

