<template>
  <div :class="['todo-item',todo.completed ? 'completed' :'' ]">
    <input
      type="checkbox"
      class="toggle"
      v-model="todo.completed"
    />

    <label  @dblclick="editTodo">{{todo.content}} </label>
    <!--<input-->
      <!--type="text"-->
      <!--v-focus="true"-->
      <!--v-model="todo.content"-->
      <!--v-if="editedTodo !==null && editedTodo.id===todo.id"-->
      <!--@keyup.enter="editDone(todo)" @keyup.esc="cancelEdit(todo)"-->
    <!--&gt;-->
    <button v-if="!todo.deleted" class="destroy" @click="deleteTodo" ></button>
  </div>
</template>

<script>
export default {
  name: "item",
  props: {                       // 接收父组件传来的参数 todo
    todo: {
      type: Object,          // 组件参数校验
      required: true
    }
  },
  data(){
    return{
      editedTodo: null, // 用于暂存编辑前的 to do 状态
    }
  },
  methods: {
    deleteTodo() {
      this.$emit('del', this.todo.id)   //向父组件触发事件，带上参数
    },
    editTodo() {
      this.$emit('edit', this.todo.id)   //向父组件触发事件，带上参数
    },
    editDone: function (todo) {
      if (todo.content === '') {
        this.deleteTodo(todo)
      }
      this.editedTodo = null
    },
    cancelEdit: function (todo) {
      todo.content = this.editedTodo.content;
      this.editedTodo = null
    },
  }
}
</script>

<style lang="stylus" scoped>

  .todo-item {
    position relative
    background #ffffff
    font-size 25px
    border-bottom 1px solid rgba(0, 0, 0, .06)
    &:hover {
      .destroy:after {
        content: 'x'
      }
    }
    label {
      white-space pre-line
      word-break break-all
      padding 15px
      margin-left 45px
      display block
      line-height 1.2
      transition color 0.4s
    }
    &.completed {
      label {
        color #d9d9d9
        text-decoration line-through
      }
    }
  }

  .toggle {
    text-align center
    width 600px
    height 25px
    position absolute
    top 0
    bottom 0
    margin auto 0
    border none
    outline none
    appearance none
  }

  .toggle:before {
    content: url('../assets/img/round.png')
    position absolute
    left 12px
    cursor pointer
  }

  .toggle:checked:before {
    content: url('../assets/img/done.png')
    position absolute
    left 12px
    cursor pointer
  }

  .destroy {
    position absolute
    top 50%
    right 10px
    bottom 0
    width 40px
    height 40px
    margin auto 0
    font-size 30px
    color #cc9a9a
    margin-bottom 11px
    transition: color 0.2s east-out
    background-color transparent
    appearance none
    border-width 0
    cursor pointer
    outline none
  }
</style>
