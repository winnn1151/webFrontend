<template>
  <div>
    <h1>Selamat Datang</h1>
    <div>Berikut daftar tugas kita:</div>
    <ul>
      <li v-for = "item in todos" :key= item.Id>{{item.todo}} <button @click="hapus(item.Id)">X</button> </li> 
    </ul>
    <input v-model="myText">
    <button @click="tambahkan">Tambah Tugas</button>
    </div>
</template>

<script>
import axios from 'axios';
export default {
  data: ()=>{
    return{
      todos: [],
      myText:'',
    }
  },
  created: function(){
    const username = localStorage.getItem('usr')
    const password = localStorage.getItem('pwd')
    axios.get(`http://localhost:3000/todo`,{headers: {username,password}})
    .then(result=> {
        this.todos = result.data
      })
  },
  methods:{
    tambahkan: function(){
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.post(`http://localhost:3000/todo`,{todo: this.myText},{headers: {username,password}})
      axios.get(`http://localhost:3000/todo`,{headers: {username,password}})
      .then(result=> {
        this.todos = result.data
      })
    },
    hapus: function(id){
      const username = localStorage.getItem('usr')
      const password = localStorage.getItem('pwd')
      axios.delete(`http://localhost:3000/todo/${id}`,{headers: {username,password}})
      axios.get(`http://localhost:3000/todo`,{headers: {username,password}})
      .then(result=> {
        this.todos = result.data
      })
    }
  },
}
</script>
