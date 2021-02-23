<template>
  <div>
    <li
      @mouseenter="mouseHandler(true)"
      @mouseleave="mouseHandler(false)"
      :style="{ backgroundColor: bgColor, color: myColor }"
    >
      <label>
        <input type="checkbox" v-model="isComplete" />
        <span>{{ todo.title }}</span>
      </label>
      <button class="btn btn-danger" v-show="isShow" @click="delTodo">
        删除
      </button>
    </li>
  </div>
</template>

<script lang='ts'>
import { defineComponent, ref, computed } from 'vue'

import { Todo } from '../types/todo'

export default defineComponent({
  name: 'Item',
  props: {
    todo: {
      type: Object as () => Todo, // 强制将Object类型转为自定义的Todo类型
      required: true
    },
    deleteTodo: {
      type: Function,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    updateTodo: {
      type: Function,
      required: true
    }
  },

  setup(props) {
    const myColor = ref('black')
    const bgColor = ref('white')
    const isShow = ref(false)

    /* 鼠标移入移出切换样式 */
    const mouseHandler = (flag: boolean) => {
      if (flag) {
        myColor.value = 'red'
        bgColor.value = 'pink'
        isShow.value = true
      } else {
        myColor.value = 'black'
        bgColor.value = 'white'
        isShow.value = false
      }
    }

    /* 删除todo */
    const delTodo = () => {
      if (window.confirm('确定要删除吗?')) {
        props.deleteTodo(props.index)
      }
    }
    
    /* 是否完成 */
    const isComplete = computed({
      get() {
        return props.todo.isCompleted 
      },
      set(val) {
        props.updateTodo(props.todo, val)
      }
    })

    return {
      mouseHandler,
      bgColor,
      myColor,
      isShow,
      delTodo,
      isComplete
    }
  }
})
</script>

<style>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  /* display: none; */
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
</style>