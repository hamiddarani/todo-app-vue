<script>
import { reactive , ref , onMounted } from "vue";
import Todo from './components/Todo.vue'
export default{
name : "App",
components : {
 Todo
},
setup(){

const text = ref('');
const todos = reactive([
  {id : 1 , text : "hello world" , completed : false}, 
  {id : 2 , text : "hello world" , completed : false}, 
  ]);
const input = ref(null)


onMounted(() => {
    input.value.focus()
})

const addTodo = () => {
  todos.unshift({id : Math.floor(Math.random() * 10000) , text : text.value , completed:false});
  text.value = ""
}

const completedTodo = id => {
  const todo =todos.find(todo => todo.id === id);
  todo.completed = ! todo.completed;
}

const deleteTodo = index => {
    todos.splice(index , 1);
}
const editTodo = (id , text) => {
   const todo =todos.find(todo => todo.id === id);
   todo.text = text
}


return{
  todos,
  text,
  addTodo,
  completedTodo,
  deleteTodo,
  editTodo,
  input,
};

}

}


</script>



<template>
 <div class="bg-gray-100 min-h-screen w-full ">
     <div class=" w-full md:w-1/2 xl:w1/3 flex flex-col items-center mx-auto p-4">
       <p class="text-gray-700 font-bold text-2xl">TODO APP</p>
     <div class="w-full  h-10 rounded mt-4 relative">
       <input class=" w-full h-full rounded px-4 placeholder-gray-500 focus:outline-none focus:ring-4 focus:ring-blue-600 focus:ring-opacity-40"
        placeholder="add a new todo"
        v-model.trim="text"
        @keypress.enter="addTodo"
        ref="input"
        />
       <i class="fas fa-arrow-right absolute h-full flex items-center top-0 right-2 cursor-pointer" @click="addTodo"></i>
     </div>
     </div>
        <transition name="switch" mode="out-in">
          <transition-group tag="div" name="fade" appear class="w-full md:w-1/2 xl:w1/3 flex flex-col items-center mt-4 mx-auto p-4 relative" v-if="todos.length">
            <Todo v-for="(todo , index) in todos" :key="todo.id" :todo="todo" :index="index" @completed="completedTodo" @deleted="deleteTodo" @edited = "editTodo"/>
          </transition-group>
          <div class="w-full md:w-1/2 xl:w1/3 flex flex-col items-center mt-4 mx-auto p-4" v-else>
                <p>wooooo there is no todo <span class="text-green-600 font-extrabold">add your first todo</span></p>
          </div> 
        </transition>
 </div>

 
</template>

<style>

.fade-enter-from,
.fade-leave-to{
  opacity: 0;
  transform: scale(0.6)
}
.fade-leave-from,
.fade-enter-to{
  opacity: 1;
  transform: scale(1)
}
.fade-enter-active{
  transition: all 0.4s ease;
}
.fade-leave-active{
  transition: all 0.4s ease;
  position: absolute;
}
.fade-move{
  transition: all 0.3s ease
}

.switch-enter-from,.switch-leave-to{
  opacity: 0;
  transform: translateY(20px);
}
.switch-enter-active,.switch-leave-active{
  transition: all 0.3s ease;
}


</style>
