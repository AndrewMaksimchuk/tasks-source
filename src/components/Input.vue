<template>
  
  <li 
    class="task-list__task"
    :class="{ delete: taskData.done }">

    <input 
      type="checkbox" 
      :id="taskData.id" 
      @click="done" 
      :checked="taskData.done">
    <label 
      class="task-list__task-label" 
      :for="taskData.id" 
      :title="'Створено: ' + (taskData.createdTime || 'невідомо')">  
      {{ taskData.text }}
    </label>

    <div class="side-section">

      <img 
        class="task-list__task-icon" 
        src="@/assets/trash.svg" 
        alt="delete icon" 
        @click="deleteTask">
        
    </div>

  </li>

</template>

<script>
export default {
  name: 'Input',
  props: {
    taskData: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isEditabled: false,
    };
  },
  methods: {
    done() {
      let isDone = null;
      if (this.taskData.done === true) {
        isDone = false;
      } else {
        isDone = true;
      }
      this.$emit('done', this.taskData.id, isDone);
    },
    deleteTask() {
      this.$emit('deleteTask', this.taskData.id);
    },
  },
}
</script>

<style>
  .side-section {
    width: 14px;
    display: flex;
    flex-direction: column;
    align-items: center;
    row-gap: 5px;
    margin-left: auto;
  }

  .task-list__task {
    position: relative;
    display: flex;
    column-gap: 5px;
  }

  .task-list__task-icon {
    width: 14px;
    height: 14px;
    opacity: .1;
  }

  .task-list__task-icon:hover {
    cursor: pointer;
    opacity: 1;
  }

  .task-list__task-label {
    width: 100%;
    padding-right: 14px;
    white-space: pre-line;
  }

  .delete {
    text-decoration: line-through;
  }
</style>
