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
  mounted: function(){
    axios.get(`http://localhost:3000/todo`)
    .then(result=> {
        this.todos = result.data
      })
  },
  methods:{
    tambahkan: function(){
      axios.post(`http://localhost:3000/todo`,{todo: this.myText})
      axios.get(`http://localhost:3000/todo`)
      .then(result=> {
        this.todos = result.data
      })
    },
    hapus: function(id){
      axios.delete(`http://localhost:3000/todo/${id}`)
      axios.get(`http://localhost:3000/todo`)
      .then(result=> {
        this.todos = result.data
      })
      // alert(id)
    }
  },
}
</script>
