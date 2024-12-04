<template>
    <form class="stack-small" @submit.prevent="onSubmit">
      <div>
        <label class="edit-label">
          Edit Name for &quot;{{props.label}}&quot;
        </label>
        <input
          type="text"
          autocomplete="off"
          v-model.lazy.trim="newLabel" />
      </div>
      <div class="btn-group">
        <button type="button" class="btn" @click="onCancel">
          Cancel
        </button>
        <button type="submit" class="btn btn__primary">
          Save
        </button>
      </div>
    </form>
</template>
<script setup>
//编辑属于input的子类,label是父传子的数据,newLabel是子传父的数据
import {ref} from 'vue'
const props=defineProps(['label'])
const newLabel=ref(props.label)
const emit=defineEmits(['item-edited','edit-cancelled'])
const onSubmit=()=>{
    if(newLabel&&newLabel.value!=props.label){
      emit('item-edited',newLabel.value)
    }else{
      emit('edit-cancelled')
    }
}
const onCancel=()=>{
    emit('edit-cancelled')
}
</script>
<style scoped>
  .edit-label {
    font-family: Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #0b0c0c;
    display: block;
    margin-bottom: 5px;
  }
  input {
    display: inline-block;
    margin-top: 0.4rem;
    width: 100%;
    min-height: 4.4rem;
    padding: 0.4rem 0.8rem;
    border: 2px solid #565656;
  }
  form {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  }
  form > * {
    flex: 0 0 100%;
  }
</style>