<template>
  <div class="app">

    <h1>Tasks</h1>
    
    <form class="form">

      <textarea 
        class="form__input" 
        type="text"  
        placeholder="Enter the task here" 
        v-model="inputText"
        :autofocus="isAutoFocused">
        </textarea>

      <button 
        class="form__button" 
        type="submit" 
        @click.prevent="addTask">
        Add
        </button>

    </form>
    
    <ol class="task-list">

      <Input 
        v-for="task in tasksList" 
        :key="task.id" 
        :taskData="task" 
        @done="taskIsDone"
        @deleteTask="deleteTask"/>

    </ol>
    
  </div>
</template>

<script>
import Input from '@/components/Input.vue';

export default {
  name: 'App',
  components: {
    Input,
  },
  data() {
    return {
      inputText: '',
      tasksList: [],
      isAutoFocused: false,
    };
  },
  created() {
    document.title = 'Tasks in local storage'
    const storage = localStorage.getItem('tasksList');
    if (storage) {
      this.tasksList = JSON.parse(storage);
    }
    window.onbeforeunload = () => {
      localStorage.setItem("tasksList", JSON.stringify(this.tasksList));
    };
  },
  methods: {
    addTask() {
      if (!this.inputText) return;
      const timeNow = Date.now();

      const data = { 
        id: '_' + timeNow, 
        text: this.inputText, 
        done: false,
        createdTime: new Date(timeNow),
       }
      this.tasksList.unshift(data);
    },
    taskIsDone(taskId, isDone) {
      this.tasksList.find(task => task.id === taskId).done = isDone;
      this.$forceUpdate();
    },
    deleteTask(taskId) {
      this.tasksList = this.tasksList.filter(task => task.id !== taskId);
    },
  },
};
</script>

<style>
  .app {
    --color-gray: #999999;
    width: 576px;
    margin: 0 auto;
    color: var(--color-gray);
  }

  @media only screen and (max-width: 577px) {
    .app {
      width: 100%;
    }
  }

  .form {
    display: flex;
    column-gap: 10px;
  }

  .form__input {
    flex: 1;
    height: 74px;
    border: 1px solid transparent;
    border-radius: 3px;
  }

  .form__input:hover {
    border: 1px solid  var(--color-gray);
  }

  .form__button {
    opacity: .1;
    text-transform: uppercase;
  }

  .form__button:hover {
    cursor: pointer;
    opacity: 1;
  }

  .task-list {
    display: flex;
    flex-direction: column;
    row-gap: 10px;
    padding: 0;
  }
</style>