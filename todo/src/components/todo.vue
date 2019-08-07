<template>
  <section class="my-app">
    <input
      type="text"
      autofocus="autofocus"
      placeholder="添加Todo"
      class="add-input"
      @keyup.enter="addTodo"
    />
    <item
      v-for="todo in filteredTodos"
      :todo="todo"
      :key="todo.id"
      @del="deleteTodo"
      @edit="editTodo"
    />
    <!--:to do  绑定，父组件使用item子组件，通过绑定的同名参数todo，向子组件传递数据-->
    <tabs
      :filter="filter"
      :todos="todos"
      @toggle="toggleFilter"
      @clearAll="clearAll"
      @clearCompleted="clearCompleted"
    />
  </section>
</template>

<script>
import item from "@/components/item"
import tabs from './tabs.vue'

const STORAGE_KEY = 'vue2.x-todo-tutorial'
let todoStorage = {
  fetch: function () {
    let todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    todos.forEach((todo, index) => {
      todo.id = index
    });
    todoStorage.uid = todos.length;
    return todos
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}

export default {
  name: 'todo',
  components: {
    item,
    tabs
  },
  data() {
    return {
      todos: todoStorage.fetch(),
      recycleBin: [], // 用于存放已经删除的 to do
      newTodoContent: '',
      checkEmpty: false, // 增加一个检查空值标志
      filter: 'all', // 默认为 all
    }
  },
  // // 这是新增的方法
  methods: {
    // 新增
    addTodo: function (e) {
      // e 事件对象
      if (e.target.value === '') {
        this.checkEmpty = true
        return
      }
      this.todos.push({id: todoStorage.uid++, content: e.target.value, completed: false, deleted: false});
      e.target.value = ''
      this.checkEmpty = false
    },

    // 删除
    deleteTodo: function (id) {
      let deletedTodo = this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1)[0]
      deletedTodo.deleted = true
      this.recycleBin.unshift(deletedTodo);
    },
    toggleFilter: function (state) {
      this.filter = state
    },
    editTodo: function (id) {
      this.editedTodo = this.todos.findIndex(todo => todo.id === id)
    },
    clearCompleted: function () {
      if (!confirm('确认清除全部已完成的待办事项？')) {
        return
      }
      let completedTodos = []
      let activedTodos = []
      this.todos.forEach(todo => {
        if (todo.completed) {
          todo.deleted = true
          completedTodos.push(todo)
        }
        else {
          activedTodos.push(todo)
        }
      });
      this.recycleBin.unshift(...completedTodos);
      this.todos = activedTodos
    },
    clearAll: function () {
      if (!confirm('确认清除全部待办事项？')) {
        return
      }
      this.todos.forEach(todo => todo.deleted = true);
      console.log(this.todos)
      this.recycleBin.unshift(...this.todos);
      console.log(this.recycleBin)
      this.todos = [];
    },
  },
  // 这是新增的计算属性
  computed: {

    emptyChecked: function () {
      return this.newTodoContent.length === 0 && this.checkEmpty
    },

    filteredTodos: function () {
      if (this.filter === 'all') {
        return this.todos
      } else if (this.filter === 'completed') {
        return this.todos.filter(todo => todo.completed)
      } else if (this.filter === 'active') {
        return this.todos.filter(todo => !todo.completed)
      }
      else {
        return this.recycleBin
      }
    },


  },
  directives: {
    /*指令*/
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  watch: {
    todos: {
      handler: function (todos) {
        todoStorage.save(todos)
      },
      deep: true
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
  .completed {
    text-decoration: line-through
  }

  .my-app {
    width 600px
    margin: 0 auto
    box-shadow: 0 0 5px #666
  }
  .add-input {
    positon: relative;
    margin 0
    width 100%
    font-size 24px
    font-family inherit
    font-weight: inherit
    line-height 1.4rem
    /*border 0;*/
    outline none
    color inherit
    /*padding 6px*/
    /*border 1px solid #999*/
    box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0)
    box-sizing border-box
    font-smoothing: antialiased;
    padding 16px 16px 16px 60px
    border none
  }

</style>
