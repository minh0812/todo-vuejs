<template>
  <div class="row todo-content">
    <div class="col-md-6">
      <div class="todo-list not-done">
        <h1>TODOS</h1>
        <div class="input-group mb-3">
          <input type="text" class="form-control" placeholder="Enter your work" v-model="textContent">
          <div class="input-group-append">
            <button class="btn btn-outline-secondary" type="button" id="button-addon2" @click="addTask()">Add</button>
          </div>
        </div>
        <hr>
        <ul class="list-unstyled" v-for="(todo, index) in todos" :key="todo.id">
          <li class="ui-state-default li-items mt-1">
            <div class="input-group">
              <div class="input-group-prepend">
                <div class="input-group-text">
                  <input type="checkbox" aria-label="Radio button for following text input" :checked="todo.checked" v-model="todo.checked">
                </div>
              </div>
              <input type="text" class="form-control" v-model="todo.title" v-bind:class="{'done-task' : todo.completed}">
              <div class="input-group-append remove-icon">
                <span class="input-group-text" @click="delTask(index)">&#10060;</span>
              </div>
            </div>
          </li>
        </ul>
        <hr>
        <div class="todo-footer row">
          <div class="col-md-6">
            <div class="form-check form-check-inline" @click="checkAll()">
              &#9989;
              <label class="form-check-label" for="inlineRadio1">Check all</label>
            </div>
            <div class="form-check form-check-inline" @click="unCheckAll()">
              &#10062;
              <label class="form-check-label" for="inlineRadio2">UnCheck all</label>
            </div>
          </div>
          <div class="col-md-6 save-all">
            <div class="form-check form-check-inline save-all">
              <button type="button" class="btn btn-success btn-sm" @click="doneAll()">DONE ALL &#10004;</button>
            </div>
            <div class="form-check form-check-inline save-all">
              <button type="button" class="btn btn-dark btn-sm" @click="delAll()">DEL ALL &#10006;</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {uuid} from 'vue-uuid'
export default {
  name: 'Todo',
  data: () => {
    return {
      todos: [],
      textContent: ''
    }
  },
  mounted: function () {
    if (localStorage.todos) {
      this.todos = JSON.parse(localStorage.todos)
    }
  },
  updated: function () {
    localStorage.todos = JSON.stringify(this.todos)
  },
  methods: {
    addTask: function () {
      if (this.textContent.trim().length === 0) {
        return
      }
      this.code++
      this.todos.push({
        'id': uuid.v1(),
        'title': this.textContent,
        'checked': false,
        'completed': false
      })
      this.textContent = ''
      // localStorage.todos = JSON.stringify(this.todos)
    },
    delTask: function (index) {
      if (confirm('Are you sure you want to delete task?')) {
        this.todos.splice(index, 1)
      }
    },
    checkAll: function () {
      this.todos.forEach(todo => {
        todo.checked = true
      })
    },
    unCheckAll: function () {
      this.todos.forEach(todo => {
        todo.checked = false
      })
    },
    doneAll: function () {
      if (confirm('Are you sure you want to done all tasks?')) {
        this.todos.forEach(todo => {
          if (todo.checked) {
            todo.completed = true
          }
        })
      }
    },
    delAll: function () {
      if (confirm('Are you sure you want to delete all tasks?')) {
        this.todos = this.todos.filter(item => {
          return (!item.checked && !item.completed)
        })
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .todo-content {
    display: flex;
    justify-content: center;
  }

  .input-group-text{
    height: 100%
  }
  .todo-list h1 {
    margin-top: 20px;
    padding-bottom: 20px;
    text-align: center;
    font-weight: bold;
  }

  .todo-footer {
    background-color: #d2e8ca;
    padding: 10px 20px 15px;
  }

  #done-items li {
    padding: 10px 0;
    border-bottom: 1px solid #ddd;
    text-decoration: line-through;
  }

  .done-task {
    text-decoration: line-through;
  }

  input.form-control.done-task {
    color: orange;
  }

  .form-check-inline {
    cursor: pointer;
  }

  .remove-icon span {
    cursor: pointer;
  }

  .save-all {
    float: right;
  }
</style>
