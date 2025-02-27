<template>
  <div class="todo">
    <TodoHeader
        :current-todo-status="currentTodoStatus"
        @update-tab="updateTab"/>
    <TodoList
        :computed-todo="computedTodo"
        @check-todo="toggleTodoComplete"
        @update-todo-msg="updateTodoMsg"
        @delete-todo="deleteTodo"/>
    <TodoInput
        @add-todo="addTodo"/>
  </div>
</template>

<script>
import TodoHeader from "@/components/TodoHeader.vue";
import TodoList from "@/components/TodoList.vue";
import TodoInput from "@/components/TodoInput.vue";
import {STORAGE_KEYS} from "@/constants/local-storage-constants.js";

export default {
  components: {
    TodoHeader,
    TodoList,
    TodoInput
  },
  data() {
    return {
      todoList: [], // 할 일 목록을 저장하고 있는 배열. todoItem 객체를 요소로 가짐.
      currentTodoStatus: 'all'
    }
  },
  computed: {
    computedTodo() {
      if (this.currentTodoStatus === 'all') {
        return this.todoList;
      }
      if (this.currentTodoStatus === 'completed') {
        return this.todoList.filter(todo => todo.completed);
      }
    },
  },
  watch: {
    todoList: {
      handler(newTodoList, oldTodoList) {
        console.log('이전 todoList:', JSON.stringify(oldTodoList));
        console.log('변경된 todoList:', JSON.stringify(newTodoList));

        // 로컬 스토리지에 데이터 저장
        localStorage.setItem(STORAGE_KEYS.TODO_LIST, JSON.stringify(newTodoList));
      },
      deep: true
    }
  },
  created() {
    // 로컬 스토리지에서 데이터 가져와서 초기화
    const savedTodoList = localStorage.getItem(STORAGE_KEYS.TODO_LIST);
    if (savedTodoList) {
      this.todoList = JSON.parse(savedTodoList);
    }
  },
  methods: {
    updateTab(tab) {
      this.currentTodoStatus = tab;
    },
    addTodo(todoInputMsg) {
      console.log(todoInputMsg);

      const todoItem = {
        id: Math.random(), // 고유값
        msg: todoInputMsg, // 할 일 텍스트
        completed: false // 할 일 완료 여부
      };

      this.todoList.push(todoItem);
    },
    toggleTodoComplete(todoItemIdToToggle) {
      this.todoList = this.todoList.map(todoItem =>
          // 스프레드 연산자(...)를 사용해 모든 속성을 복사하고
          // completed 속성만 반전시킨 새로운 객체를 만든다.
          todoItem.id === todoItemIdToToggle ? {...todoItem, completed: !todoItem.completed} : todoItem
      )
    },
    deleteTodo(todoItemId) {
      this.todoList = this.todoList.filter(todoItem => todoItem.id !== todoItemId);
    },
    updateTodoMsg({id: todoItemId, msg: msgToBe}) {
      this.todoList = this.todoList.map(todoItem =>
          todoItem.id === todoItemId ? {...todoItem, msg: msgToBe} : todoItem
      )
    },
  }
}
</script>

<style></style>
