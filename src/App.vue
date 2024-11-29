<script setup>
import {ref,onMounted,computed,watch } from 'vue'

const todos=ref([])
const name = ref('')

const input_content=ref('')
const input_category = ref(null)

const todos_asc=computed(() => todos.value.sort((a,b)=>{
  return b.createAt - a.createAt
}))

const addTodo =()=>{
  if(input_content.value.trim()==='' || input_content.value.trim()===null)
{
  return
}

todos.value.push({
  content:input_content.value,
  category:input_category.value,
  done:false,
  createAt:new Date().getTime()
},{deep:true})

}

watch(todos,newValue=>{
localStorage.setItem('todo',JSON.stringify(newValue));
})

watch(name, (newValue)=>{
  localStorage.setItem('name', newValue)

})

onMounted(()=>{
  name.value=localStorage.getItem('name') || ''
  todos.value =JSON.parse(localStorage.getItem('todo')) || []
})
</script>

<template>
<main class="app">
  <section class="greeting">
    <h2 class="title">
      hello,<input type="text" placeholder="your name" v-model="name" />
    </h2>

    <section class="create-todo">
      <h2>CREATE A TODO</h2>

      <form @submit.prevent="addTodo">
        <h3>Whats on your todo?</h3>

        <input type="text" placeholder="eg. make a video" v-model="input_content" />
         <h4>Select a Category</h4>
         <div class="options">
          <label>
            <input type="radio" name="category" value="business" v-model="input_category" />
            <span class="bubble business">
            </span>
            <div>Business</div>
          </label>

          <label>
            <input type="radio" name="category" value="personal" v-model="input_category" />
            <span class="bubble personal">
            </span>
            <div>personal</div>
          </label>
          {{ input_category }}
         </div>
         <input type="submit" value="Add Todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3>MY TODO LIST</h3>
      <div class="list">
        <div 
          v-for="td in todos_asc" 
          :key="td.id" 
          :class="['todo-item', { done: td.done }]"
        >
       <label>
        <input type="checkbox" v-model="td.done"/>
        <span :class='`bubble ${td.category}`'></span> 
       </label>

       <div class="todo-content">
        <input type="text" v-model="td.content"/>
       </div>
     </div>

      </div>
    </section>
  </section>
</main>
</template>

