<template>
  <div class="helper">
    <span class="left">剩余 {{unFinishedTodoLength}} 项未完成</span>
    <span class="tabs">
      <span
        v-for="state in states"
        :key="state"
        :class="[state,filter === state? 'actived':'']"
        @click="toggleFilter(state)"
      >
        {{state}}
      </span>
      <span class="clear" @click="clearCompleted">清除已完成</span>
      <span class="clear" @click="clearAll">清除全部</span>
    </span>
  </div>
</template>

<script>
export default {
  name: "tab",
  props: {
    filter: {
      type: String,
      required: true,
    },
    todos: {
      type: Array,
      required: true,
    }
  },
  computed: {
    unFinishedTodoLength() {
      return this.todos.filter(todo => !todo.completed).length
    }
  },
  data() {
    return {
      states: ['all', 'active', 'completed', 'deleted']
    }
  },
  methods: {
    clearAll() {
      this.$emit('clearAll')
    },
    clearCompleted() {
      this.$emit('clearCompleted')
    },
    toggleFilter(state) {
      this.$emit('toggle', state)
    }
  }
}
</script>

<style lang="stylus" scoped>
  .helper {
    font-weight 100
    height 36px
    justify-content space-between
    padding 5px 0
    line-height 30px
    background-color #fff
    font-size 14px
    font-smoothing: antialiased
  }

  .left, .clear, .tabs {
    padding 0 10px
    box-sizing border-box
  }

  .left{
    width 180px
    text-align left
  }

  .clear {
    width auto
    text-align right
    cursor pointer
  }

  .tabs {
    width 200px
    justify-content space-around
    * {
      display inline-block
      padding 0 10px
      cursor pointer
      border 1px solid rgba(175, 47, 47, 0)
      &.actived {
        border-color rgba(175, 47, 47, 0.4)
        border-radius 5px
      }
    }
  }

</style>
