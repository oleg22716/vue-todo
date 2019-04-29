
<template>
  <div class="hello">
    <div class="holder">
      <form @submit.prevent="addElem">
        
      <input type="text" placeholder="Enter what you have to do.." v-model="elem" v-validate="'min:3'" name="input">
      
      
      <transition name="alert-in" enter-active-class="animated jello" leave-active-class="animated lightSpeedOut">
      <p class="alert" v-if="errors.has('input')">{{ errors.first('input') }} </p>
      </transition>
      </form>
      {{ elem }}

      <ul>
        <transition-group name="list" enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
          <li v-for="(data, index) in todos" :key="index" v-bind:class="{ stroked: isCheckedArray[index], 'base': !isCheckedArray[index]}">{{ index }}. {{data.elem}}
            <i class="fa" v-bind:class="{ 'fa-circle': !isCheckedArray[index], 'fa-check-circle': isCheckedArray[index]}" style="font-size:24px;color:black" v-on:click="remove(index)"></i></li>
            

          
        </transition-group>
      </ul>
<!-- 
      <p v-if="todos.length >= 1">you have something left to do</p>
      <p v-else>You have finished. Well done!</p> -->
    </div>
  </div>
</template>

<script>
import { constants } from 'crypto'; 
export default {
  name: "TodoList",
  data() {
    return {
      elem: "",
      isCheckedArray:[true,true,false,false],
      todos: [
        { elem: "При удалении - зачеркивать." },
        { elem: "Убрать чекбокс" },
        {
          elem:
            "yarn run lint - пофиксить ошибки"
        },
        { elem: "описать настройку среды в документации с нуля, если нет ни хостов ни вебсервера на локальной машине" }
      ]
    }
  },
  methods: {
    addElem(){
      this.$validator.validateAll().then((result) => {
        if(result){
          this.todos.push({elem: this.elem});
          this.elem = '';
          this.isCheckedArray.push(false);
          console.log('Elem is: ' + this.isCheckedArray);
        }
        else {
          console.log('Not valid');

        }
      })
      
    },
    remove(id){
      // this.todos[id].elem.style.setProperty("text-decoration", "line-through");
      console.log("in remove. b4"+this.isCheckedArray[id])
      this.isCheckedArray[id]=true;
      console.log("in remove. after"+this.isCheckedArray[id])
      this.isCheckedArray.push('');
      this.isCheckedArray.splice(this.isCheckedArray.length-1)
      
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./TodoList.css" scoped>
/* h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
} */
</style>
