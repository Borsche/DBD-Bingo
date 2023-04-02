<template>
  <div class="content">
    <div></div>
    <table>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 5)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <img draggable="false" src="../assets/Border.png">
          <div :id="task.id"></div>
        </td>
      </tr>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 10 && index >= 5)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <img draggable="false" src="../assets/Border.png">
          <div :id="task.id"></div>
        </td>
      </tr>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 15 && index >= 10)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <img draggable="false" src="../assets/Border.png">
          <div :id="task.id"></div>
        </td>
      </tr>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 20 && index >= 15)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <img draggable="false" src="../assets/Border.png">
          <div :id="task.id"></div>
        </td>
      </tr>
      <tr>
        <td v-for="task in tasks.filter((task, index) => index < 25 && index >= 20)" :key="task.index" :title="task.hint" @click="tick(task.id)">
          {{task.task}}
          <img draggable="false" src="../assets/Border.png">
          <div :id="task.id"></div>
        </td>
      </tr>
    </table>
    <div>
      <button v-on:click="copyToClipboard" class="btn">Copy To Clipboard</button>
      <button v-on:click="generateTaskAndCode" class="btn">Generate New Board</button>
    </div>
  </div>
</template>

<script>
import TASKS from '@/assets/tasks.json'

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
      console.log("Generating Code")
      let code = "";
      this.tasks.forEach(task => {
        let codeSnippet = task.id.toString(16);
        if(codeSnippet.length < 2)
          code += "0"; // add 0 as prefix so we will always have a 2 digit hex length  
        code += codeSnippet;
      })

      return code.slice(0, code.length - 1);
    },
    getTasksFromCode(stringcode) {
      const hexLength = 2;
      // var codes = stringcode.split("-");
      const taskCopy = TASKS.all.slice();
      taskCopy.concat(TASKS.survivor.slice());
      const bingoTasks = [];

      for(let i = 0; i < stringcode.length; i += hexLength) {
        const task = taskCopy.find(task => task.id === parseInt(stringcode.substring(i, i+2), 16))
        bingoTasks.push(task);
        // console.log(task, stringcode.substring(i, i+2), parseInt(stringcode.substring(i, i+2), 16));
      }

      return bingoTasks;
    },
    copyToClipboard: function() {
      console.log("Copiing")
      navigator.clipboard.writeText(window.location.origin + "?" + this.code).then(() => {
        console.log("Copied Code");
      })
    },
    generateTaskAndCode: function() {
      this.tasks = this.getRandomTasks();
      this.code = this.generateCode();

      history.pushState(null, '', window.location.pathname + '?' + this.code);
      return this.tasks;
    },
    tick(id) {
      if(document.getElementById(id).classList.contains('tick'))
        document.getElementById(id).className = ""
      else
        document.getElementById(id).className = "tick"
    },
  },
  beforeMount() {
    
    // get tasks from url
    this.code = window.location.search.substring(1, window.location.search.length);

    this.tasks = this.code == "" ? this.generateTaskAndCode() : this.getTasksFromCode(this.code);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

td {
  height: 140px;
  width: 140px;
  box-shadow: inset 5px 5px 50px 10px rgba(0, 0, 0, 0.452);
  background-color: #583c11;
  background-image: url('../assets/LittleIcon.png');
  position:relative;
  border-radius: 5px;
}

table {
  margin: auto;
  border-collapse: separate;
  border-spacing: 2px;
  user-select: none;
  -moz-user-select: none;
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
  box-shadow: inset 5px 5px 50px 10px rgba(0, 0, 0, 0.452);
  background-color: #75531f;
  background-image: url('../assets/LittleIcon.png');
}

td:hover img {
  display: block;
  user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  z-index: 100;
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

.btn {
  width: 250px;
  height: 100px;
  border: white 1px solid;
  color: white;
  background-color: rgba(0, 0, 0, 0.452)
}

.btn:hover {
  background-color: rgba(53, 53, 53, 0.452);
}

.border {
  background-image: url('../assets/Border.png');
  width: 150px;
  height: 150Px;
}

td img {
  position: absolute;
  top: -4px;
  left: -5px;
  display: none;

  user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}

.content {
  display: flex;
  align-items: center;
}

.content > table {
}

.content > div {
  flex-grow: 1;
  width: 30%;
}

</style>
