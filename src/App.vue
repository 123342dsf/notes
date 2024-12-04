<template>
  <div id="app">
    <h1>To-Do-List</h1>
    <h2>{{ listSummary }}</h2>
    <ToDoForm @add-todo="addToDo"/>
    <ul aria-labelledby="list-summary" class="stack-large">
      <ToDoItem 
      v-for="(item,index) in ToDoItems"
      :key="item.id"
      :id="item.id"
      :checked="item.done" :label="item.label"
      @checkbox-changed="updateDoneStatus(index)"
      @item-edited="editToDo(index,$event)"
      @item-deleted="deleteToDo(index)"/>
      <!-- <ToDoItem :checked="isDone" :label="label"/> -->
    </ul>
  </div>
</template>
<script setup>
import { ref ,reactive,computed } from 'vue'
import ToDoItem from './components/ToDoItem.vue'
import ToDoForm from "./components/ToDoForm.vue";
const ToDoItems = reactive([
  { label: "Learn Vue", done: false },
  { label: "Create a Vue project with the CLI", done: true },
  { label: "Have fun", done: true }
]);
const addToDo=(labelValue)=>{
  ToDoItems.push({label:labelValue,done:false})
}
const updateDoneStatus=(toDoId)=>{
  ToDoItems[toDoId].done=!ToDoItems[toDoId].done
}
//计算属性：文字信息
const listSummary = computed(() => {
  const numberFinishedItems = ToDoItems.filter((item) => item.done).length;
  return `${numberFinishedItems} out of ${ToDoItems.length} items completed`;
});
//编辑功能,带参数是为$event!!
const editToDo=(toDoId,newLabel)=>{
  ToDoItems[toDoId].label=newLabel
}
const deleteToDo=(toDoId)=>{
  console.log('删除中')
  ToDoItems.splice(toDoId,1)
}
//收到form子组件的传递的值
</script>
<style>
  /* 全局样式 */
  .btn {
    padding: 0.8rem 1rem 0.7rem;
    border: 0.2rem solid #4d4d4d;
    cursor: pointer;
    text-transform: capitalize;
  }
  .btn__danger {
    color: #fff;
    background-color: #ca3c3c;
    border-color: #bd2130;
  }
  .btn__filter {
    border-color: lightgrey;
  }
  .btn__danger:focus {
    outline-color: #c82333;
  }
  .btn__primary {
    color: #fff;
    background-color: #000;
  }
  .btn-group {
    display: flex;
    justify-content: space-between;
  }
  .btn-group > * {
    flex: 1 1 auto;
  }
  .btn-group > * + * {
    margin-left: 0.8rem;
  }
  .label-wrapper {
    margin: 0;
    flex: 0 0 100%;
    text-align: center;
  }
  [class*="__lg"] {
    display: inline-block;
    width: 100%;
    font-size: 1.9rem;
  }
  [class*="__lg"]:not(:last-child) {
    margin-bottom: 1rem;
  }
  @media screen and (min-width: 620px) {
    [class*="__lg"] {
      font-size: 2.4rem;
    }
  }
  .visually-hidden {
    position: absolute;
    height: 1px;
    width: 1px;
    overflow: hidden;
    clip: rect(1px 1px 1px 1px);
    clip: rect(1px, 1px, 1px, 1px);
    clip-path: rect(1px, 1px, 1px, 1px);
    white-space: nowrap;
  }
  [class*="stack"] > * {
    margin-top: 0;
    margin-bottom: 0;
  }
  .stack-small > * + * {
    margin-top: 1.25rem;
  }
  .stack-large > * + * {
    margin-top: 2.5rem;
  }
  @media screen and (min-width: 550px) {
    .stack-small > * + * {
      margin-top: 1.4rem;
    }
    .stack-large > * + * {
      margin-top: 2.8rem;
    }
  }
  /* 全局样式结束 */
  #app {
    background: #fff;
    margin: 2rem 0 4rem 0;
    padding: 1rem;
    padding-top: 0;
    position: relative;
    box-shadow:
      0 2px 4px 0 rgba(0, 0, 0, 0.2),
      0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
  }
  @media screen and (min-width: 550px) {
    #app {
      padding: 4rem;
    }
  }
  #app > * {
    max-width: 50rem;
    margin-left: auto;
    margin-right: auto;
  }
  #app > form {
    max-width: 100%;
  }
  #app h1 {
    display: block;
    min-width: 100%;
    width: 100%;
    text-align: center;
    margin: 0;
    margin-bottom: 1rem;
  }
</style>
