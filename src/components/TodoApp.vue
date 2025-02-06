<script setup>
import { ref } from "vue";

const newTask = ref("");
const tasks = ref([]);
const editingIndex = ref(null);
const editingText = ref("");

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({ text: newTask.value, completed: false });
    newTask.value = "";
  }
};

const deleteTask = (index) => tasks.value.splice(index, 1);

const editTask = (index) => {
  editingIndex.value = index;
  editingText.value = tasks.value[index].text;
};

const saveTask = () => {
  if (editingText.value.trim() !== "") {
    tasks.value[editingIndex.value].text = editingText.value;
    cancelEdit();
  }
};

const cancelEdit = () => {
  editingIndex.value = null;
  editingText.value = "";
};
</script>


<template>
  <div class="todo">
    <h1 class="todo__title">My Todo List</h1>

    <div class="todo__input">
      <input v-model="newTask" class="todo__input-field" placeholder="Enter a new task..." />
      <button class="todo__button todo__button--add" @click="addTask">Add</button>
    </div>

    <ul class="todo__list">
      <li v-for="(task, index) in tasks" :key="index" class="todo__item">
        <input type="checkbox" v-model="task.completed" class="todo__checkbox" />
        <span :class="{ 'todo__text--completed': task.completed }" class="todo__text">
          {{ task.text }}
        </span>
        <button class="todo__button todo__button--edit" @click="editTask(index)">Edit</button>
        <button class="todo__button todo__button--delete" @click="deleteTask(index)">Delete</button>
      </li>
    </ul>

    <div v-if="editingIndex !== null" class="todo__modal">
      <div class="todo__modal-content">
        <input v-model="editingText" class="todo__input-field" />
        <button class="todo__button todo__button--save" @click="saveTask">Save</button>
        <button class="todo__button todo__button--cancel" @click="cancelEdit">Cancel</button>
      </div>
    </div>
  </div>
</template>



<style lang="scss">
/* === BEM Style === */

.todo {
  max-width: 400px;
  margin: 20px auto;
  padding: 20px;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.todo__title {
  font-size: 24px;
  margin-bottom: 15px;
}

.todo__input {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.todo__input-field {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.todo__button {
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}


.todo__button--add {
  background: #42b983;
  color: white;
}

.todo__button--edit {
  background: #ffcc00;
  margin-left: 20px;
  margin-right: 5px;
}

.todo__button--delete {
  background: #ff4d4d;
  color: white;
}

.todo__button--save {
  background: #2ecc71;
}

.todo__button--cancel {
  background: #e74c3c;
}

/* Task List */
.todo__list {
  list-style: none;
  padding: 0;
}

.todo__item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px;
  border-bottom: 1px solid #eee;
}

/* Task completion */
.todo__text--completed {
  text-decoration: line-through;
  color: #888;
}

/* Modal */
.todo__modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.todo__modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
}
</style>
