<script setup>
import { onMounted, ref } from "vue";

const name = ref("John Doe");
const status = ref("active");
const tasks = ref(["Task One", "Task Two", "Task Three"]);

const newTask = ref(null);

const addTask = () => {
  if (newTask.value) tasks.value.push(newTask.value);

  newTask.value = null;
};

const deleteTask = (index) => {
  if (tasks.value.length > 0 && tasks.value.length > index)
    tasks.value.splice(index, 1);
};

const toggleStatus = () => {
  status.value = status.value === "active" ? "inactive" : "active";
};

onMounted(async () => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await res.json();

    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log(error);
  }
});
</script>

<template>
  <h1>Hi, {{ name }}</h1>

  <p v-if="status == 'active'">User is active</p>
  <p v-else>User is inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add task</label>
    <br />
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <br />
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="index">
      <span>{{ task }}</span>
      &nbsp;
      <button @click="deleteTask(index)">delete</button>
    </li>
  </ul>

  <br />
  <button @click="toggleStatus">Toggle Status</button>
</template>
