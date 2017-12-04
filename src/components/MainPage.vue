<template lang="html">
  <div>
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
    <!-- LIST TODOS -->
    <div class="listTodos">
      <div class="container">
        <!-- TODO -->
        <div class="todoList">
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
                  <button class="btn btn-success" type="button" name="button" @click="doing(todo)">Doing</button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- DOING -->
        <div class="doingList">
          <div class="row">
            <div class="col-md-3" v-for="todo in listTodos" v-if="todo.status === 2" :key="todo.title">
              <div class="card">
                <div class="card-header">
                  {{todo.title}}
                </div>
                <div class="card-body">
                  <h4 class="card-title">{{todo.description}}</h4>
                  <p class="card-text">{{todo.point}}</p>
                  <p class="card-text">{{todo.assigned}}</p>
                  <button class="btn btn-bg-dark" type="button" name="button" @click="todoBack(todo)">Todo</button>
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
let db = app.database().ref('todos')
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
    listTodos: db
  },
  methods: {
    postTodo () {
      db.push(this.inputTask)
      this.title = null
      this.description = null
      this.point = null
      this.assigned = null
      this.status = 0
    },
    doing (todo) {
      let data = db.child(todo['.key'])
      data.update(
        {
          'status': 2
        }
      )
    },
    todoBack (todo) {
      let data = db.child(todo['.key'])
      data.update(
        {
          'status': 0
        }
      )
    }
  }
}
</script>

<style lang="css">
</style>
