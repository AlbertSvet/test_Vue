<template>
  <input class="inp" v-model="UserName" type="text" placeholder="Имя">
  <input class="inp"  v-model="UserEmail" type="text" placeholder="Цель">
  <input class="inp"  v-model="UserPass" type="text" placeholder="План">
  <button class="btn" @click="sendData()" type="button">Добавить</button>

  <p class="error" v-if="error">{{ error }}</p>
  
  <div v-if="users.length == 0">
     <p>Список пуст</p>
  </div>
  <div v-else-if="users.length == 1">
     <p>Добавлена одна задача</p>
  </div>
  <div v-else="users.length > 1">
     <p>Добавлено больше одной задачи</p>
  </div>
  <div class="container">  
    <Users v-for="(el, index) in users" 
    :key="index" 
    :user='el' 
    :index ='index'
    :deletUser = 'deletUser'
    @edit="editUser"
    @done = 'done'
    />
  </div>
</template>

<script>
import Users from './components/Users.vue';
 export default {
  components: {Users},
   data() {
     return {
      users: [],
      UserName: '',
      UserPass: '',
      UserEmail: '',
      error: '',
      editIndex: null
     }
   },
   created(){
    this.load();
   },
   methods: {
    load(){
      const storUser = localStorage.getItem('users');
      if(storUser){
        this.users = JSON.parse(storUser)
      }
    },
    saveUser(){
      localStorage.setItem('users', JSON.stringify(this.users))
    },

    sendData(){
      if(this.UserName && this.UserPass && this.UserEmail){

            if(this.editIndex !== null) {
              this.users[this.editIndex] = {
                name: this.UserName,
                pass: this.UserPass,
                email: this.UserEmail,
              };
              this.editIndex = null;
              this.UserName = ''
              this.UserPass = ''
              this.UserEmail = '' 
              
          }else{
            this.users.push({
            name: this.UserName,
            pass: this.UserPass,
            email: this.UserEmail,
            }),
            this.UserName = ''
            this.UserPass = ''
            this.UserEmail = ''  
            this.error = ''    
          }
          // 
          this.saveUser(); 
     }else{
      this.error = 'Заполните все поля'
     }
    },
    deletUser(index){
      this.users.splice(index, 1)
         // 
         this.saveUser(); 
    },
    editUser(index) {
  
      this.editIndex = index;
      this.UserName = this.users[index].name;
      this.UserPass = this.users[index].pass;
      this.UserEmail = this.users[index].email;
    },
    done(index){
      this.users[index].isCompl = true
           // 
           this.saveUser(); 
    }
   }
 }
</script>

<style scoped>
  .container{
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 30px;
  }
  .error{
    color: red;
    font-weight: 600;
  }
  .inp{
    padding: 0px 15px;
    height: 40px;
    font-size: 18px;
    margin-right: 15px;
    border-radius:5px;
    border: 1px solid rgba(0, 0, 0, 0.349);
  }
  .btn{
    height: 40px;
    width: 150px;
    border-radius: 5px;
    
  }
</style>
