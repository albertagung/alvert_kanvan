<template lang="html">
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-2">
        <!-- FORM INPUT -->
        <div class="formInput">
          <div class="container">
            <form @submit.prevent="postTodo">
              <div class="form-group">
                <label for="inputTitle">Title</label>
                <input type="text" class="form-control" placeholder="Input title" v-model="inputTask.title">
              </div>
              <div class="form-group">
                <label for="exampleInputPassword1">Description</label>
                <textarea type="text" class="form-control" placeholder="Input description" v-model="inputTask.description"></textarea>
              </div>
              <div class="form-group">
                <label for="exampleInputPassword1">Point</label>
                <input type="text" class="form-control" placeholder="Input point" v-model="inputTask.point">
              </div>
              <div class="form-group">
                <label for="exampleInputPassword1">Assigned to</label>
                <input type="text" class="form-control" placeholder="Input assigned" v-model="inputTask.assigned">
              </div>
              <button type="submit" class="btn btn-primary">Submit</button>
            </form>
          </div>
        </div>
      </div>
      <div class="col-md-10">
        <!-- LIST TODOS -->
        <div class="listTodos">
          <div class="container">
            <!-- BACK LOG -->
            <div class="backLog">
              <h4>Back Log</h4>
              <hr>
              <div class="row">
                <div class="col-md-3" v-for="todo in listTodos" v-if="todo.status === 0">
                  <div class="card">
                    <div class="card-header">
                      {{todo.title}}
                    </div>
                    <div class="card-body">
                      <h4 class="card-title">{{todo.description}}</h4>
                      <p class="card-text">{{todo.point}}</p>
                      <p class="card-text">{{todo.assigned}}</p>
                      <button class="btn btn-success" type="button" name="button" @click="todoBack(todo)">Todo</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- TODO -->
            <div class="todoList">
              <h4>Todo</h4>
              <hr>
              <div class="row">
                <div class="col-md-3" v-for="todo in listTodos" v-if="todo.status === 1">
                  <div class="card">
                    <div class="card-header">
                      {{todo.title}}
                    </div>
                    <div class="card-body">
                      <h4 class="card-title">{{todo.description}}</h4>
                      <p class="card-text">{{todo.point}}</p>
                      <p class="card-text">{{todo.assigned}}</p>
                      <button class="btn btn-success" type="button" name="button" @click="backLog(todo)">Back Log</button>
                      <button class="btn btn-success" type="button" name="button" @click="doing(todo)">Doing</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- DOING -->
            <div class="doingList">
              <h4>Doing</h4>
              <hr>
              <div class="row">
                <div class="col-md-3" v-for="todo in listTodos" v-if="todo.status === 2">
                  <div class="card">
                    <div class="card-header">
                      {{todo.title}}
                    </div>
                    <div class="card-body">
                      <h4 class="card-title">{{todo.description}}</h4>
                      <p class="card-text">{{todo.point}}</p>
                      <p class="card-text">{{todo.assigned}}</p>
                      <button class="btn btn-bg-dark" type="button" name="button" @click="todoBack(todo)">Todo</button>
                      <button class="btn btn-bg-dark" type="button" name="button" @click="done(todo)">Done</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- DONE -->
            <div class="doneList">
              <h4>Done</h4>
              <hr>
              <div class="row">
                <div class="col-md-3" v-for="todo in listTodos" v-if="todo.status === 3">
                  <div class="card">
                    <div class="card-header">
                      {{todo.title}}
                    </div>
                    <div class="card-body">
                      <h4 class="card-title">{{todo.description}}</h4>
                      <p class="card-text">{{todo.point}}</p>
                      <p class="card-text">{{todo.assigned}}</p>
                      <button class="btn btn-bg-dark" type="button" name="button" @click="doing(todo)">Doing</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'
// Initialize Firebase
var config = {
  databaseURL: 'https://alvert-kanvan.firebaseio.com',
  projectId: 'alvert-kanvan'
}
let app = Firebase.initializeApp(config)
let db = app.database()
export default {
  name: 'MainPage',
  data () {
    return {
      inputTask: {
        title: null,
        description: null,
        point: null,
        assigned: null,
        status: 0
      }
    }
  },
  firebase: {
    listTodos: db.ref('todos')
  },
  methods: {
    postTodo () {
      db.ref('todos').push(this.inputTask)
      this.inputTask.title = null
      this.inputTask.description = null
      this.inputTask.point = null
      this.inputTask.assigned = null
      this.inputTask.status = 0
    },
    backLog (todo) {
      let data = db.ref(`/todos/${todo['.key']}`)
      data.update(
        {
          'status': 0
        }
      )
    },
    doing (todo) {
      let data = db.ref(`/todos/${todo['.key']}`)
      data.update(
        {
          'status': 2
        }
      )
    },
    todoBack (todo) {
      let data = db.ref(`/todos/${todo['.key']}`)
      data.update(
        {
          'status': 1
        }
      )
    },
    done (todo) {
      let data = db.ref(`/todos/${todo['.key']}`)
      data.update(
        {
          'status': 3
        }
      )
    }
  }
}
</script>

<style lang="css">
body {
  background-color: #eee;
  margin-top: 0
}
.listTodos .row {
  padding-top: 2em;
  padding-bottom: 2em
}

.todoList .card {
  background-color: grey
}
.doneList .card {
  background-color: yellow
}
.doingList .card {
  background-color: blue;
  color: white
}
.backLog .card {
  background-color: red;
  color: white
}
</style>
