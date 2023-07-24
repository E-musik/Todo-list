<script setup>
import { ref, onmounted, computed, watch} from 'vue'
const todos = ref ([])
const name = ref("")

const input_content = ref ('')
const input_category = ref (null)

const input_asc = computed(() => todos.value.sort((a, b) => {
    return b.createAt - a.createdAt
}))
const addtodo = () => {
  if (input_content.value.trim() == '' || input_category.value == null){
    return
  }
  todos.value = push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createAt: new Date ().greeting()
  })
  input_content.value = ''
  input_category.value = null  
}

const removetodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

watch(todos, newval => {
  localStorage.setItem('todos', JSON.stringify(newval))
}, {deep:true})

watch(name, (newval) => {
  localStorage.setItem('name', newval)
})
onmounted(() =>{ 
    name.value = localStorage.getItem(name) || ''
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })

</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        what's up, 
      </h2>
      <input type="text" placeholder="name here" v-model="name" />

    </section>
    <section class="createtodo">
      <h3>CREATE TODO</h3>

      <form @submit.orevent="addtodo">
        <h4> what's on your todo list?</h4>
        <input type="text" 
          placeholder="e.g make a video" 
        v-model="input_content" />
               
        <h4>pick a category</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" 
            id="category1" 
            value="business"
            v-model="input_category" />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input type="radio" name="category" 
            id="category1" 
            value="personal"
            v-model="input_category" />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
            
        </div> 
        <input type="submit" value="Add todo"/> 
      </form>  
    </section>
    <section class="todo-list">
      <h5>TODO LIST</h5>
      <div class="list">
        <div v-for="todo in todo_asc" :class="'todo-item $(todo.done && `done`)'">
          <label>
            <input type='checkbox' v-model= 'todo.done'/>
            <span class="bubble $`(todo.category)`"></span>
                    
          </label>
          <div class="todo_content">
            <input type="text" v-model="todo_content">
          </div>
          <div class="actions">
            <button class="delete" @click="removetodo (todo)">Delete Todo</button>
          </div>

        </div>
      </div>
    </section>
  </main>

</template>

<style scoped>

</style>