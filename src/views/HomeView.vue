<template>
  <div class="container" id="Kanban">
    <div v-for="(column, index) in columns" :key="index" class="column">
      <div class="column-header">
        {{ column.name }}
      </div>
      <div class="column-body overflow-auto">
        <!-- hier kun je nieuwe taken bijvoegen aan de hand van de knop toevoegen -->
        <div class="input-group mb-3">
          <input
            v-model="newTaskNames[index]"
            type="text"
            placeholder="Name a task"
            class="form-control"
          />
          <button
            @click="addTask(index, newTaskNames[index], id)"
            class="btn btn-outline-secondary"
          >
            +
          </button>
        </div>
        <TaskItem
          v-for="(task, taskIndex) in column.tasks"
          :key="taskIndex"
          :task="task"
          :index="index"
          :taskIndex="taskIndex"
          :columns="columns"
          @dragstart="onDragStart(task, $event, index, task.id)"
          draggable="true"
          ref="dragArea"
          @dragover.prevent
          @drop="onDrop(index, taskIndex, $event)"
          @deletetask="deleteTask"
          @updatename="updateName"
        />
        <div
          class="versleepZone mb-3"
          draggable="false"
          ref="dragArea"
          @dragover.prevent
          @drop="pushEnd(index, $event)"
        >
          Add task to bottom
        </div>
      </div>
    </div>
  </div>
  <div id="footer">&copy; Emmeline Vanhaecke &amp; Lucas De Laere</div>
</template>

<script>
import TaskItem from "@/components/TaskItem";
export default {
  components: { TaskItem },
  data() {
    return {
      id: 0,
      columns: [
        {
          name: "Backlog",
          tasks: [],
        },
        {
          name: "To Do",
          tasks: [],
        },
        {
          name: "Done",
          tasks: [],
        },
      ],
      newTaskNames: ["", "", ""],
    };
  },
  methods: {
    //called from TaskItem
    updateName(index, taskIndex, taskName) {
      console.log(index, taskIndex, taskName);
      this.columns[index].tasks[taskIndex].name = taskName;
    },
    pushEnd(index, event) {
      event.preventDefault();
      const task = JSON.parse(event.dataTransfer.getData("text")).name;
      const columnId = parseInt(event.dataTransfer.getData("column"));
      const taskId = parseInt(event.dataTransfer.getData("id"));
      for (let [i, item] of this.columns[columnId].tasks.entries()) {
        if (item.id === taskId) {
          this.columns[columnId].tasks.splice(i, 1);
        }
      }
      this.columns[index].tasks.push({
        name: task,
        id: taskId,
      });
      console.log(this.columns);
    },
    addTask(columnIndex, taskName, taskId) {
      this.columns[columnIndex].tasks.push({
        name: taskName,
        id: taskId,
      });
      this.newTaskNames[columnIndex] = "";
      this.id += 1;
    },
    onDragStart(task, event, columnIndexFrom, taskId) {
      event.dataTransfer.setData("text", JSON.stringify(task));
      event.dataTransfer.setData("column", columnIndexFrom);
      event.dataTransfer.setData("id", taskId);
    },
    onDrop(columnIndex, taskIndex, event) {
      //Index = column index to, taskIndex index to
      event.preventDefault();
      const task = JSON.parse(event.dataTransfer.getData("text")).name;
      const columnId = parseInt(event.dataTransfer.getData("column")); //columnId from
      const taskId = parseInt(event.dataTransfer.getData("id")); //taskId
      //delete task in old column
      for (let [i, item] of this.columns[columnId].tasks.entries()) {
        if (item.id === taskId) {
          this.columns[columnId].tasks.splice(i, 1);
        }
      }
      //insert task in new column
      this.columns[columnIndex].tasks.splice(taskIndex + 1, 0, {
        name: task,
        id: taskId,
      });
      console.log(this.columns);
    },
    deleteTask(task, columnIndex) {
      const index = this.columns[columnIndex].tasks.indexOf(task);
      if (index > -1) {
        this.columns[columnIndex].tasks.splice(index, 1);
      }
      console.log(this.columns);
    },
  },
};
</script>

<style>
.container {
  display: flex;
}

.column {
  flex: 1;
}

.column-header {
  background-color: #f2f2f2;
  padding: 10px;
  font-weight: bold;
}

.column-body {
  min-height: 100px;
  max-height: 85vh;
  border: 2px solid #ccc;
  text-align: center;
  line-height: 100px;
  font-size: 20px;
  color: #ccc;
}
#Kanban {
  margin-top: 20px;
  height: 95vh;
}

.versleepZone {
  color: black;
  background-color: skyblue;
}
#footer {
  width: 100%;
  bottom: 0;
}
</style>
