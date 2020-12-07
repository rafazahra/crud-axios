<template>
  <form @submit.prevent="add">
        <input type="hidden" v-model="form.id">
        <input type="text" v-model="form.nama" placeholder="nama">
        <input type="text" v-model="form.tempat_tanggal_lahir" placeholder="tempat tanggal lahir">
        <input type="text" v-model="form.sekolah" placeholder="sekolah">
        <input type="text" v-model="form.jurusan" placeholder="jurusan">
        <input type="text" v-model="form.kelas" placeholder="kelas">
        <button type="submit" v-show="!updateSubmit">add</button>  
        <button type="button" v-show="updateSubmit" @click="update(form)">Update</button> 
    </form>
  <div>
    <ul v-for="user in users" :key="user.id">
      <li>
        <span>{{user.nama}}</span> &#160;
        <span>{{user.tempat_tanggal_lahir}}</span> &#160;
        <span>{{user.sekolah}}</span> &#160;
        <span>{{user.jurusan}}</span> &#160;
        <span>{{user.kelas}}</span> &#160;
        <button @click="edit(user)">edit</button> || <button @click="del(user)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
/* eslint-disable */
import axios from 'axios'
export default {
  data(){
    return{
        form: {
          id: '',
          nama: '',
          tempat_tanggal_lahir: '',
          sekolah: '',
          jurusan: '',
          kelas: ''
        },
        users: '',
        updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://localhost:3000/users/').then(res =>{
        this.users = res.data
      }).catch ((err) => {
        console.log(err);
      })
    },
   add(){
      axios.post('http://localhost:3000/users/', this.form).then(res => {
          this.load()
          this.form.nama = ''
          this.form.tempat_tanggal_lahir = ''
          this.form.sekolah = ''
          this.form.jurusan = ''
          this.form.kelas = ''
      })
    },
    edit(user){
      this.updateSubmit = true
      this.form.id = user.id
      this.form.nama = user.nama
      this.form.tempat_tanggal_lahir = user.tempat_tanggal_lahir
      this.form.sekolah = user.sekolah
      this.form.jurusan = user.jurusan
      this.form.kelas = user.kelas
    },
    update(form){
      return axios.put('http://localhost:3000/users/' + form.id , {nama: this.form.nama, tempat_tanggal_lahir: this.form.tempat_tanggal_lahir, 
      sekolah: this.form.sekolah, jurusan: this.form.jurusan, kelas: this.form.kelas}).then (res => {
        this.load()
        this.form.id = ''
        this.form.nama = ''
        this.form.tempat_tanggal_lahir = ''
        this.form.sekolah = ''
        this.form.jurusan = ''
        this.form.kelas = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
      })
    },
     del(user){
      axios.delete('http://localhost:3000/user/'+ user.id).then(res =>{
        this.load()
        let index = this.user.indexOf(form.nama)
        this.users.splice(index,1)
      }) 
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
