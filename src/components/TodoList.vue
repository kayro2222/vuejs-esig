<template>
  <div class="container">
    <div class="container-fluid">
      <div class="d-flex justify-content-center">
        <div class="col-xs-12 col-sm-7 col-md-7">
          <div class="panel">
            <input type="text" class="todo-input" placeholder="What's need to be done?" v-model="newTodo" @keyup.enter="addTodo">
            <div class="main-content">
              <transition-group enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
                <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining" @finishedEdit="finishedEdit">
                </todo-item>
              </transition-group>
              <div class="content-footer">
                <div class="filters-footer">
                  <todo-check-all></todo-check-all>
                  <todo-filtered></todo-filtered>
                  <todo-items-remaining></todo-items-remaining>
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
import TodoItem from './TodoItem'
import TodoItemsRemaining from './TodoItemsRemaining'
import TodoCheckAll from './TodoCheckAll'
import TodoFiltered from './TodoFiltered'
//import TodoClearCompleted from './TodoClearCompleted'

export default {
  name: 'todo-list',
  components: {
    TodoItem,
    TodoItemsRemaining,
    TodoCheckAll,
    TodoFiltered,
  },
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
    }
  },
  created() {
    this.$store.dispatch('retrieveTodos')
  },
  computed: {
    anyRemaining(){
      return this.$store.getters.anyRemaining
    },
    todosFiltered(){
      return this.$store.getters.todosFiltered
    },
  },
  methods: {
    addTodo(){
      if(this.newTodo.trim().lenght == 0){
        return 
      }
      
      this.$store.dispatch('addTodo', {
        id: this.idForTodo,
        title: this.newTodo,
      })

      this.newTodo = ''
      this.idForTodo++
    },
    checkAllTodos(){
      this.$store.state.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted(){
      this.$store.state.todos = this.$store.state.todos.filter(todo => !todo.completed)
    },
    finishedEdit(data){
      const index = this.$store.state.todos.findIndex(item => item.id == data.id)
      this.$store.state.todos.splice(data.index, 1, data.todo)
    }
  }
}
</script>

<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input{
  width: 100%;
  font-size: 24px;
  padding: 16px 16px 16px 40px;
  border: none;
  background: rgba(0, 0, 0, 0.003);
  box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
  &:focus{
    outline: 0;
  }
}

.todo-input::placeholder{
  opacity: 0.3;
  font-style: italic;
}

.main-content{
  border-top: 1px solid #e6e6e6;
  border-bottom: 1px solid #ededed;
}

.content{
  border-bottom: 1px solid #ededed;
  animation-duration: 0.3s;
}

.label-input, .input-edit{
  width: 85%;
  padding: 10px 18px;
  font-size: 18px;
  text-align: left;
}

span.delete-item{
  cursor: pointer;
}

.completed{
  text-decoration: line-through;
  color: gray;
}

.panel{
  background: #fff;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}

.filters-footer{
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  padding-top: 14px;
  margin-bottom: 14px;
}

button {
  font-size: 14px;
  background-color: #fff;
  appearance: none;
  border: 1px solid transparent;
  border-radius: 3px;
  &:hover {
    border-color: rgba(175, 47, 47, 0.2);
  }

  &:focus {
    outline: none;
  }
}

.clear{
  border: none;
  &:hover{
    text-decoration: underline;
  }
}

.active {
  border-color: rgba(175, 47, 47, 0.2);
}

// CSS Transitions
.fade-enter-active, .fade-leave-active {
  transition: opacity .2s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

.remaining{
  margin-right: 14px;
}

.all-checked{
  margin-left: 14px;
  margin-top: 3px;
}
</style>
