<template>
    <div class="stack-small" v-if="!isEditing">
        <div class="custom-checkbox">
            <input type="checkbox"  
            id="todo-item" 
            :checked="props.checked"
            class="checkbox"
            @change="emit('checkbox-changed')"/>
            <label  class='checkbox-label' for="todo-item">{{ props.label }}</label>
        </div>
        <div class="btn-group">
            <button type="button" class="btn" @click="isEditing=true" ref="editButton">Edit</button>
            <button type="button" class="btn btn__danger" @click="emit('item-deleted')">Delete</button>
        </div>
    </div>
    <ToDoItemEditForm v-else :label="props.label" @item-edited="itemEdited" @edit-cancelled="editCancelled"/>
</template>
<script setup>
import ToDoItemEditForm from './ToDoItemEditForm.vue';
import {ref ,nextTick} from 'vue';
const props=defineProps(['checked','label'])
const emit = defineEmits(['checkbox-changed','item-edited','item-deleted']);
const isEditing=ref(false);
//props传递的数据是只读的，不能改变，只能将新数据向上传给父组件
const editButton = ref(null);

//焦点处理
const focusEditButton = async () => {
    await nextTick();  // 确保 DOM 更新
    if (editButton.value) {
    } else {
        console.error('Edit button is not available');
    }
};
const itemEdited=(newLabel)=>{
    emit('item-edited',newLabel);
    isEditing.value=false;
    focusEditButton();

}

const editCancelled=()=>{
    isEditing.value=false;
    focusEditButton();
}
</script>
<style scoped>
.custom-checkbox{
    padding-bottom: 20px;
    display: block;
}
.checkbox{
    width: 44px;
    height: 44px;
    font-family: Arial, sans-serif;
    padding: 5px;
    border: 2px solid #0b0c0c;
    border-radius: 0;
    line-height: 1.25;
    float: left;
}
.checkbox-label{
    height: 38px;
    font-size:20px;
    display: block;

}
</style>