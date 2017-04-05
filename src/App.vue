<template>
  <div id="app">
    <h1>TODO LIST</h1>
    <input type="text" placeholder="What do you need to do?" v-model="newTask" v-on:keyup.enter="addTask"/>
    <div id="todo-app" class="todo-app-container" v-cloak v-show="tasks.length">
      <h2>What you have: </h2>
      <ul class="todo-list">
        <li v-for="task in tasks" class="task" :class="{checked: task.checked}">
          <input type="checkbox" v-model="task.checked" v-on:click="toggleTask()"/>
          <label>{{task.text}}</label>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data () {
      return {
        tasks: [
          {
            text: 'This is sample task1.',
            checked: false
          },
          {
            text: 'This is sample task2.',
            checked: false
          }
        ],
        newTask: ''
      }
    },
    methods: {
      addTask () {
        var newTask = this.newTask.trim()
        this.tasks.push({
          text: newTask,
          checked: false
        })
        this.saveTasks()

        // Reset newTask
        this.newTask = ''
      },
      toggleTask () {
        this.saveTasks()
      },
      loadTasks () {
        return JSON.parse(localStorage.getItem('todo-list') || '[]')
      },
      saveTasks () {
        localStorage.setItem('todo-list', JSON.stringify(this.tasks))
      }
    },
    created: function () {
      var tasks = this.loadTasks()
      if (tasks.length > 0) {
        this.tasks = tasks
      }
    }
  }
</script>

<style lang="scss">
  [v-cloak] {
    display: none;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;

    input[type="text"] {
      padding: 10px;
      width: 50%;
    }

    .todo-app-container {
      width: 50%;
      margin: 0 auto;
    }

    h1, h2 {
      font-weight: normal;
    }

    ul {
      list-style-type: none;
      padding: 0;
    }

    li {
      margin: 0 10px;

      &.task.checked {
        label {
          text-decoration: line-through;
        }
      }
    }

    a {
      color: #42b983;
    }
  }
</style>
