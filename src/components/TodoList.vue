<template>
  <div>
    <input 
      type="text" 
      class="todo-input" 
      placeholder="What needs to be done" 
      v-model="task" 
      @keyup.enter="addTask">

    <div v-for="(task, index) in tasks" :key="task.id" class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" v-model="task.completed">

        <div v-if="! task.editing" @dblclick="editTask(task)" class="todo-item-label" :class="{ completed : task.completed}">
          {{ task.title }}
        </div>

        <input 
          v-else class="todo-item-edit" 
          type="text" v-model="task.title" 
          @blur="doneEdit(task)" 
          @keyup.enter="doneEdit(task)" 
          @keyup.esc="cancelEdit(task)"
          v-focus>
      </div>
      <div class="remove-item" @click="removeTask(index)">
        &times;
      </div>
    </div>


    <div class="extra-container">
      <div><label><input type="checkbox">Clear All</label></div>

      <div>{{ remaining }} items left</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',

  data () {
    return {
      task: '',
      idForTask: 3,
      beforeEditCache: '',
      tasks: [
        {
          'id': 1,
          'title': 'One',
          'completed': false,
          'editing': false
        },
        {
          'id': 2,
          'title': 'Two',
          'completed': false,
          'editing': false
        }
      ]
    }
  },

  computed: {
    remaining() {
      return this.tasks.filter(task => ! task.completed).length;
    }
  },

  directives: {
    focus: {
      inserted: function (el) {
        el.focus();
      }
    }
  },

  methods: {
    addTask() {
      if (this.task.trim().length == 0)
        return;

      this.tasks.push({
        id: this.idForTask,
        title: this.task,
        completed: false
      });

      this.task = '';
      this.idForTask++;
    },

    editTask(task) {
      this.beforeEditCache = task.title;
      task.editing = true;
    },

    doneEdit(task) {
      if (task.title.trim().length == 0)
        task.title = this.beforeEditCache;

      task.editing = false;
    },

    cancelEdit(task) {
      task.title = this.beforeEditCache;
      task.editing = false;
    },

    removeTask(index) {
      this.tasks.splice(index, 1);
    }
  }
}
</script>

<style lang="scss">
    .todo-input {
      width: 100%;
      padding: 10px 18px;
      font-size: 18px;
      margin-bottom: 16px;

      &:focus {
        outline: 0;
      }
    }

    .todo-item {
      margin-bottom: 12px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .remove-item {
      cursor: pointer;
      margin-left: 14px;
      &:hover {
        color: black;
      }
    }

    .todo-item-left {
      display: flex;
      align-items: center;
    }

    .todo-item-label {
      padding: 10px;
      border: 1px solid white;
      margin-left: 12px;
    }

    .todo-item-edit {
      font-size: 24px;
      color: #2c3e50;
      margin-left: 12px;
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      font-family: 'Avenir', Helvetica, Arial, sans-serif;

      &:focus {
        outline: none;
      }
    }

    .completed {
      text-decoration: line-through;
      color: grey;
    }

    .extra-container {
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 16px;
      border-top: 1px solid lightgrey;
      padding-top: 14px;
      margin-bottom: 14px;
    }

    button {
      font-size: 14px;
      background-color: white;
      appearance: none;

      &:hover {
        background: lightgreen;
      }

      &:focus {
        outline: none;
      }
    }

    .active {
      background: lightgreen;
    }
</style>
