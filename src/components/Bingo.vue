<template>
  <div>
    <h1>Dead by Daylight - Bingo</h1>
    <table>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 5)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <div :id="task.id"></div>
        </td>
      </tr>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 10 && index >= 5)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <div :id="task.id"></div>
        </td>
      </tr>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 15 && index >= 10)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <div :id="task.id"></div>
        </td>
      </tr>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 20 && index >= 15)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <div :id="task.id"></div>
        </td>
      </tr>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 25 && index >= 20)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <div :id="task.id"></div>
        </td>
      </tr>
    </table>
    <button v-on:click="copyToClipboard">Copy Code</button><br>
    <button v-on:click="generateTaskAndCode">Generate New Board</button>
  </div>
</template>

<script>
import TASKS from '../assets/tasks.json'

export default {
  name: 'Bingo',
  data: () => {
    return {
      tasks: [],
      code: ""
    }
  },
  methods: {
    getRandomTasks() {
      const bingoAmount = 25;
      const taskCopy = TASKS.all.slice();
      const bingoTasks = [];

      taskCopy.concat(TASKS.survivor.slice());

      console.log(taskCopy);

      for (let i = 0; i < bingoAmount; i++) {
        let random = Math.floor(Math.random() * taskCopy.length)

        bingoTasks.push(taskCopy[random])
        taskCopy.splice(random, 1);
      }

      console.log(bingoTasks);

      return bingoTasks
    },
    generateCode() {
      var code = "";
      this.tasks.forEach(task => {
        code += task.id + "-";
      })

      return code.slice(0, code.length - 1);
    },
    getTasksFromCode(stringcode) {
      var codes = stringcode.split("-");
      const taskCopy = TASKS.all.slice();
      taskCopy.concat(TASKS.survivor.slice());
      const bingoTasks = [];

      for(let i = 0; i < codes.length; i++) {
        for(let j = 0; j < taskCopy.length; j++) {
          if(taskCopy[j].id == codes[i]) {
            bingoTasks.push(taskCopy[j]);
          }
        }
      }

      return bingoTasks;
    },
    copyToClipboard: function() {
      console.log("Copiing")
      navigator.clipboard.writeText(window.location.origin + "/" + this.code).then(() => {
        console.log("Copied Code");
      })
    },
    generateTaskAndCode: function() {
      this.tasks = this.getRandomTasks();
      this.code = this.generateCode();

      window.location.pathname = "/"+this.code;
    },
    tick(id) {
      if(document.getElementById(id).classList.contains('tick'))
        document.getElementById(id).className = ""
      else
        document.getElementById(id).className = "tick"
    },
  },
  beforeMount:function() {

    // get tasks from url
    this.code = window.location.pathname.substr(1, window.location.pathname.length);
    console.log(this.code);

    this.tasks = this.code == "/" || this.code == "" ? this.generateTaskAndCode() : this.getTasksFromCode(this.code);
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

td {
  height: 150px;
  width: 150px;
  border: 1px solid #dce0e4;
  box-shadow: inset 5px 5px 50px 10px rgba(0, 0, 0, 0.452);
  background-color: #583c11;
  background-image: url('../assets/LittleIcon.png');
}

table {
  margin: auto;
  border-collapse: collapse;
}

.tick {
  background-image: url('../assets/slash.png');
  width: 150px;
  height: 150Px;
  transform: rotate(-45deg);
  position: absolute;
  margin-top: -85px;
  margin-left: -5px;
}

/*
td:hover div:not(.tick) {
  background-image: url('../assets/slash.png');
  width: 150px;
  height: 150Px;
  transform: rotate(-45deg);
  position: absolute;
  margin-top: -85px;
  margin-left: -5px;
} */

td:hover {
  height: 150px;
  width: 150px;
  border: 1px solid #dce0e4;
  box-shadow: inset 5px 5px 50px 10px rgba(0, 0, 0, 0.452);
  background-color: #75531f;
  background-image: url('../assets/LittleIcon.png');
}

td:hover div.tick {
  background-image: url('../assets/slash.png');
  filter: saturate(50%) brightness(150%);
  width: 150px;
  height: 150Px;
  transform: rotate(-45deg);
  position: absolute;
  margin-top: -85px;
  margin-left: -5px;
}

</style>
