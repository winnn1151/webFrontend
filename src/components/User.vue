<template>
  <div>
    <h1>Berikut adalah semua pengguna kita</h1>
    <ul>
      <li v-for = "item in users" :key= item.id>{{item.username}} <button @click="hapus(item.id)">X</button> </li> 
    </ul>
    <input v-model="username">
    <input v-model="password">
    <button @click="tambahkan">Add</button>
    </div>
</template>

<script>
import axios from 'axios';
export default {
  data: ()=>{
    return{
      users: [],
      username:'',
      password:'',
    }
  },

  mounted: function(){
    const username = localStorage.getItem('usr')
    const password = localStorage.getItem('pwd')
    axios.get(`http://localhost:3000/user`,{headers: {username,password}})
    .then(result=> {
        this.users = result.data
      })
  },
  methods:{
    tambahkan: function(){
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      const newItem = {username: this.username, password: this.password}
      axios.post(`http://localhost:3000/user`, newItem,{headers: {username,password}})
      .then(()=>{
        axios.get('http://localhost:3000/user', {headers: {username,password}})
            .then(result=>{
              this.users = result.data
              this.username = ''
              this.password = ''
              })
        })
    },
    hapus: function(id){
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.delete(`http://localhost:3000/user/${id}`,{headers: {username,password}})
      .then(()=>{
        axios.get('http://localhost:3000/user',{headers: {username,password}})
          .then(result=>{
            this.users = result.data
          })
      })
    }
  },
}
</script>
