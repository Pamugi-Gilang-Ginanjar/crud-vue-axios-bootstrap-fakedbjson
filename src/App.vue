<template>
  <div class="container">
      <div class="col-md-8 offset-md-2 py-5">
        <form @submit.prevent="add">
          <div class="input-group mb-3">
          <input type="hidden" class="form-control" placeholder="Input Nama" v-model="form.id">
          <input type="text" class="form-control" placeholder="Input Nama" v-model="form.name">
          <div class="input-group-append">
            <button class="btn btn-primary" type="submit" v-if="!updateSubmit">add</button>
            <button class="btn btn-primary" type="submit" v-else @click.prevent="update(form)">update</button>
          </div>
          </div>
        </form>
        <table class="table table-bordered table-dark">
          <thead>
            <tr class="text-center">
              <th scope="col">Nama</th>
              <th scope="col">Edit</th>
              <th scope="col">Hapus</th>
            </tr>
          </thead>
          <tbody v-for="user in users" :key="user.id">
            <tr>
              <td>{{ user.name }}</td>
              <td><button class="btn btn-primary btn-sm" @click="edit(user)">Edit</button></td>
              <td><button class="btn btn-primary btn-sm" @click="del(user)">Delete</button></td>
            </tr>
          </tbody>
        </table>
      </div>
  </div>
</template>

<script>
/* eslint-disable */
import axios from 'axios'
export default {
  data() {
    return {
      form: {
        id: '',
        name: ''
      },
      users: '',
      updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load() {
      axios.get('http://localhost:3000/users').then(res => {
        this.users = res.data //respon dari rest api dimasukan ke varibel users
      }).catch ((err) => {
        console.log(err);
      })
    },
    add() {
      axios.post('http://localhost:3000/users/', this.form).then(res => {
        this.load()
        this.form.id = ''
        this.form.name = ''
      })
    },
    edit(user){
      this.updateSubmit = true
      this.form.id = user.id
      this.form.name = user.name
    },
    update(form){
      return axios.put('http://localhost:3000/users/' + form.id , {name: this.form.name}).then(res => {
        this.load()
        this.form.id = ''
        this.form.name = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
      })
    },
    del(user){
      axios.delete('http://localhost:3000/users/' + user.id).then(res => {
        this.load()
        let index = this.users.indexOf(this.users.id)
        this.users.splice(index,1)
      })
    }
  }
}
</script>
 <style scoped>
 .btn-sm, .btn-group-sm > .btn {
    width: -webkit-fill-available !important;
}
 </style>
