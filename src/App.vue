<template>
  <!-- <transition enter-active-class="animate__animated animate__bounceIn" leave-active-class="animate__animated animate__bounceOut"> -->

  <transition 
    enter-active-class="animate__animated animate__bounceInLeft animate__faster"
    leave-active-class="animate__animated animate__bounceOutRight animate__faster"
    >
    <div v-if="display" class="bad-message">
      {{ message }}
    </div>
  </transition>

  <div id="content">
    <p class="animate__animated animate__bounceIn">
      <strong>Active:</strong> {{ activeTasks }} | <strong>Done:</strong>
      {{ doneTasks }} | <strong>Total:</strong> {{ activeTasks + doneTasks }}
    </p>
    <h1 class="animate__animated animate__bounceInDown">Todo List App</h1>

    <div class="animate__animated animate__bounceInLeft">
      <input
        type="text"
        v-model="currentTask"
        maxlength="30"
        @keyup.enter="currentIndex < 0 ? addTask() : updateTask()"
      />
      <button id="addbtn" @click="currentIndex < 0 ? addTask() : updateTask()">
        Add
      </button>
    </div>
    <transition-group tag="ul" name="list">
      <li
        v-for="task in tasks"
        :key="task.id"
        v-bind:class="{ completeTask: task.done }"
        @dblclick="completeTask(task)"
      >
        <div class="data">
          {{ task.name }}
        </div>
        <div class="button">
          <button @click="editTask(task)">Edit</button>
          <button @click="removeTask(task)">Delete</button>
        </div>
      </li>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "Todo List",
  data: () => ({
    id: 0,
    display: false,
    message: "Please, type a task!",
    currentIndex: -1,
    currentTask: "",
    doneTasks: 0,
    activeTasks: 0,
    tasks: [],
  }),
  methods: {
    countTasks() {
      this.doneTasks = 0;
      this.activeTasks = 0;
      for (let i = 0; i < this.tasks.length; i++) {
        if (!this.tasks[i].done) {
          this.activeTasks++;
        }
        if (this.tasks[i].done) {
          this.doneTasks++;
        }
      }
    },
    generateId() {
      this.id += 1;
      return this.id;
    },
    getCurrentIndex(task) {
      let index = this.tasks.findIndex(function (item) {
        return item.name === task.name;
      });
      return index;
    },
    addTask() {
      if (this.currentTask != "") {
        this.tasks.push({
          id: this.generateId(),
          name: this.currentTask,
          done: false,
        });
        this.currentTask = "";
        this.countTasks();
      } else {
        this.showMessage();
      }
    },
    editTask(task) {
      this.currentIndex = this.getCurrentIndex(task);
      this.currentTask = this.tasks[this.currentIndex].name;
    },
    removeTask(task) {
      this.currentIndex = this.getCurrentIndex(task);
      this.tasks.splice(this.currentIndex, 1);
      this.countTasks();
      this.currentIndex = -1;
    },
    updateTask() {
      this.tasks[this.currentIndex].name = this.currentTask;
      this.currentTask = "";
      this.currentIndex = -1;
    },
    completeTask(task) {
      task.done = !task.done;
      this.countTasks();
      console.log("Task:", task.name + " / " + task.done);
    },
    showMessage() {
      this.display = true;
      setTimeout(() => (this.display = false), 2000);
    },
  },
};
</script>

<style>
#app {
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  text-align: center;
}

body {
  background-color: aquamarine;
}

p {
  border-bottom: dotted thin gray;
  padding-bottom: 5px;
  text-align: center;
}

#content {
  width: 480px;
  margin: 1rem auto;
  padding: 1rem;
  border-radius: 5px;
}

ul {
  position: relative;
  padding: 2rem;
}

ul > li {
  list-style-type: none;
  cursor: pointer;
  user-select: none;
  margin-right: 0.5rem;
  text-align: left;
  /* border: solid gray thin;  */
  padding: 5px;
  border-radius: 5px;
  background-color: bisque;
  margin-bottom: 5px;
  display: flex;
  width: 400px;
}

.data {
  flex: 1 300px;
}
.button {
  flex: 2 150px;
}

button {
  margin-left: 5px;
  border-radius: 5px;
  user-select: none;
  border: thin gray solid;
  cursor: pointer;
}

input[type="text"] {
  width: 200px;
}

input[type="checkbox"] {
  margin-right: 0.2rem;
  cursor: pointer;
}

input[type="checkbox"]:checked {
  box-shadow: 0 0 6px gray;
}

.completeTask {
  text-decoration: line-through;
  color: green;
}

.list-enter-from {
  opacity: 0;
  transform: scale(0.5);
}

.list-enter-to {
  opacity: 1;
  transform: scale(1);
}

.list-enter-active {
  transition: all 0.4s ease;
}

.list-leave-from {
  opacity: 1;
  transform: scale(1);
}

.list-leave-to {
  opacity: 0;
  transform: scale(0.8);
}

.list-leave-active {
  transition: all 0.4s ease;
  position: absolute;
}

.list-move {
  transition: all 0.3s ease;
}

.bad-message {
  background-color: red;
  width: 300px;
  color: white;
  font-weight: bold;
  text-align: center;
  border-radius: 5px;

  margin: 1rem auto;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;

  /* transform: translate(-50%, 0); */
}

/* .fade-enter-from{
  opacity: 0;
}
.fade-enter-to{
  opacity: 1;
}
.fade-enter-active{
  transition: opacity .5s ease;
}

.fade-leave-from{

}
.fade-leave-to{

}
.fade-leave-active{

} */
</style>
