<template>
    <div class="item">
     <p ><span>Ваше имя:</span> {{ user.name }}</p>
     <p :class="{'complet': user.isCompl}"><span>Ваше цель:</span> {{ user.pass }}</p>
     <p :class="{'complet': user.isCompl}"><span>Ваш план:</span> {{ user.email }}</p>

     <button class="btn"  @click="deletUser(index)" type="button">Удалить</button>
     <button  class="btn"  @click="editUser(index)" type="button">Редактировать</button>
     <button  class="btn"   @click="done(index)" type="button">Задача выполнена</button>
  </div>
</template>
  
  <script>
   export default {

    props: {
        user:{
            type: Object,
            required: true
        },
        index:{
            type: Number,
            required: true
        },
        deletUser:{
            type: Function,
            required: true
        }
    },
     data() {
       return {
     
       }
     },
     methods: {
        editUser(index){
            this.$emit('edit', index)
        },
        done(index){
            this.$emit('done', index)
        }
      
     }
   }
  </script>
  
  <style scoped>
    .item{
        border: 1px solid red;
        border-radius: 20px;
        padding: 15px;
    }
    .btn{
        display: block;
        margin: 0px auto 10px auto;
        width: 80%;
        height: 35px;
        border-radius:10px ;
        
    }
    .btn:last-child{
        margin: 0px auto 0px auto;
    }
    .complet{
        text-decoration: line-through;
    }
  </style>