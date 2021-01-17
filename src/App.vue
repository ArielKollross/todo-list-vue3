<template>
  <div class="section">
    <h1 class="title">My Tasks - Vue3</h1>

    <div class="task-action">
     <button 
        class="show-btn"
        @click="handleShowHideList">
        Mostrar lista
      </button>
    </div>
     
    <section>
      <input 
        class="task-input"
        type="text"
        placeholder="Digite uma nova tarefa"
        @keyup.enter="handleAddTask" 
        v-model="state.currentTask"
      >

      <p class="task-description">Um click duplo marca a tarefa como completa.</p>

      <ul v-show="state.showList">
        <li
          class="list-item" 
          v-for="(task, index) in state.tasks" :key="index"
          @dblclick="handleCompletedTask(task)"
        >
          {{ task.name }}
          <button
            class="remove-btn"
            @click="handleRemoveTask(task)"
            >&times;</button>

            <span 
              v-show="task.isDone"
              class="task-completed"
            >Completed!</span>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import { reactive } from 'vue'

export default {
  setup () {
    // new 'databind'
    const state = reactive({
      currentTask: '',
      showList: true,
      tasks: [
        { name: 'Todo 1', isDone: false }
      ],
    })

    // Vue3 dont use more "this", so we dont need more the methods, we can return just functions
    function handleShowHideList () {
      state.showList = !state.showList;
    }

    function handleAddTask() {
      state.tasks.push({
        name: state.currentTask,
        isDone: false,
      });

      state.currentTask = '';
    }

    function handleRemoveTask(task) {
      const newTasks = state.tasks.filter(el => el.name !== task.name )

      state.tasks = newTasks;
    }

    function handleCompletedTask(task) {
      const newTasks = state.tasks.map( el => {
        if( el.name === task.name ) {
          return {
            ...el,
            isDone: !el.isDone,
          };
        }
          return { ...el };
      });

      state.tasks = newTasks;
    }

    // wills be acessible on my tample, but not necessaraly is reactive 
    return {
      state,
      handleShowHideList,
      handleAddTask,
      handleRemoveTask,
      handleCompletedTask,
    };
  },
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  outline: 0;
  box-sizing: border-box;
  list-style: none;
}
body, input, button { 
  font-size: 16px;
  font-family: 'Roboto', sans-serif; 
}
.section {
  margin: 50px auto;
  padding: 40px 20px;
  max-width: 780px;
  text-align: center;
  
  background-color: #F0F0F5 ;
  border-radius: 10px;
}
.title {
  margin: 80px 0 64px;
  font-size: 40px;
  color: #3D3D4D;
}
.task-action{
  width: 500px;
}
.show-btn{
  width: 150px;
  height: 48px;
  border: 0;
  border-radius: 8px;
  background-color: #04d361;
  color: #FFF;
  font-weight: bold;
  margin-bottom: 24px;
}
.task-description{
  margin: 36px 0;
}
.list-item{
  margin-top: 16px;
  text-align: center;
}
.task-input{
  height: 48px;
  min-width: 400px;
  border: 0;
  border-radius: 10px;
  text-align: center;
  color: #737380;
}
.remove-btn{
  width: 24px;
  height: 24px;
  border-radius: 50%;
  border: 0;
  background-color: #c53030;
  font-size: 18px;
  font-weight: bold;
  color: #FFF;

  margin-left: 16px;
}
.task-completed{
  margin-left: 16px;
  border: 1px solid #04d361;
  border-radius: 8px;
  padding: 6px;
  color: #04d361;
}
</style>