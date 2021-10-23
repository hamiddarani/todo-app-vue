<template>
      <div :class="[todo.completed ? 'bg-gray-200' : '','w-full h-16 my-2 rounded overflow-hidden px-4 shadow-md bg-white flex justify-between items-center']">
          <div class="font-semibold">{{todo.text}}
            <span class="text-white rounded bg-green-600 px-2 py-1 ml-5" v-if="todo.completed">completed</span>
          </div>
          <div class="flex items-center gap-2">
            <input class="bg-gray-200 mr-3 w-full h-12 rounded px-4 placeholder-gray-500 focus:outline-none focus:ring-4 focus:ring-blue-600 focus:ring-opacity-40"
            v-if="isEdited"
            placeholder="edit this todo"
            v-model.trim="editedText"
            @keypress.enter="handleEditTodo(todo.id)"
            />
            <i class="fas fa-trash-alt cursor-pointer text-red-600" @click="handleDeleteTodo(index)"></i>
            <i class="fas fa-edit cursor-pointer text-yellow-600" @click="isEditedHandler" v-if=" !isEdited && !todo.completed"></i>
            <i class="fas fa-thumbs-up cursor-pointer text-yellow-600" v-else-if="isEdited && !todo.completed" @click="handleEditTodo(todo.id)"></i>
            <i :class="[todo.completed ? 'fas fa-undo' : 'fas fa-check' ,'cursor-pointer text-green-600']" @click="handleCompletedTodo(todo.id)"></i>
          </div>
      </div>
</template>

<script>
import { ref  } from 'vue';
export default {
    name : "Todo",
    props : {
        todo : {
            type : Object,
            required : true
        },
        index : {
            type : Number,
            required : true
        }
    },
    setup(props, {emit}){

        const editedText = ref('');
        const isEdited = ref(false);

        const handleCompletedTodo = (id) => {
            emit('completed' , id);
        }
        const handleDeleteTodo = index => {
            emit('deleted' , index);
        }
        const isEditedHandler = (id) => {
            isEdited.value = true;
            editedText.value = props.todo.text
        }
        const handleEditTodo = id => {
            emit('edited' , id , editedText);
            isEdited.value = false
        }

        return{
            handleCompletedTodo,
            handleDeleteTodo,
            handleEditTodo,
            isEditedHandler,
            editedText,
            isEdited,
        }
    }

}
</script>

<style>


</style>