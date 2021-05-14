<template>
  <div class="users">
    <div class="contanier">
      <section>
        <h5 class="title">Novo usuário</h5>
        <form  @submit.prevent="createUser">
          <input type="text" placeholder="Nome" v-model="form.name">
          <input type="email" placeholder="Email" id="" v-model="form.email">
          <button type="submit" class="btn">Cadastrar</button>
        </form>
      </section>
      <section class="users--list">
        <h5 class="title">{{title}}</h5>

        <ul>
          <li v-for="user in users" :key="user.id" class="user--detail btn">
            <div class="info">
            <p>{{user.name}}</p>
            <small>{{user.email}}</small>

            </div>
            <a  class="destroy btn" @click="destroyUser(user.id)">X</a>

          </li>
        </ul>
      </section>
    </div>
  </div>
</template>

<script>
import axios from './utils/axios'
export default {
data(){
  return{
    title: 'Lista de Usuários',
    users:[],
    form:{
      name: '',
      email: ''
    }

  }
},
created(){
  this.fetchUsers()
},
methods:{
  async fetchUsers(){
    try {
      const {data} = await axios.get('/users');
      this.users = data
      
    } catch (error) {
      console.log(error)
    }
  },
  async createUser(){
    try {
      
      const {data} = await
       axios.post('/users', this.form)
      this.users.push(data)
  
      this.form.name = ''
      this.form.email = ''
    } catch (error) {
      console.warn(error)
    }
  },
  async destroyUser(id){
    try {
      await axios.delete(`/users/${id}`)
  
      const userIndex = this.users.findIndex((user) => user.id === id);
      this.users.splice(userIndex, 1)
      
    } catch (error) {
      console.warn(error)
    }
  }
}

}
</script>
<style scoped>
.users{
  /* border: 1px solid red; */
  height: 100vh;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.title{
  color: rgba(0,0,0,0.7);
}
form{
  display: flex;
  flex-direction: column;
}
input{
  margin: .7em 0;
  padding: 1em;
  background: transparent;
  outline: none;
  border: 3px solid rgba(255, 255, 255, 0.2);
  border-radius: 10px;

  box-shadow: 2px 2px 2px rgba(0,0,0, 0.08), 
  -3px -2px 3px rgba(255,255,255, 0.2)
  ,2px 2px 2px rgba(0,0,0,0.08) inset;

}
.contanier{
  /* border: 1px solid red; */
  width: 80%;
}
section{
  /* border: 1px solid; */
  /* widows: 100%; */
}
.btn{
  margin: 10px 0;
  border: none;
  font-size: 1em;
  background-color: transparent;
  padding: 1em;
  border-radius: 10px;
  color: #787878;
  cursor: pointer;
  outline: none;
  text-shadow: 1px 1px 0 rgba(255, 255, 255, .4);
  transition: box-shadow ease 250ms;
  border-bottom: 1px solid rgba(255, 255, 255, .3);
  border-right: 1px solid rgba(255, 255, 255, .3);

  box-shadow: -6px -6px 16px rgba(255,255,255, .6),
              6px 6px 16px rgba(0,0,0, .2);
}
.btn:hover{
box-shadow: -6px -6px 8px rgba(255,255,255, .6),
              6px 6px 8px rgba(0,0,0, .2);
}
.btn:active{
box-shadow:
            -6px -6px 8px rgba(255,255,255, 0 ),
            6px 6px 8px rgba(0,0,0,0),
            inset -6px -6px 8px rgba(255,255,255, .6),
            inset 6px 6px 8px rgba(0,0,0, .2);
}
.users--list{
  margin: 1em 0;
}
ul{
  margin: .5em 0;
}
.user--detail{
  
  margin: 10px 0;
  /* border: 1px solid; */
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: .2em .3em;
}
.destroy{
  /* border: 1px solid; */
  padding: .3em;
  width: 20px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.info p{
  font-weight: 600;
}
</style>

