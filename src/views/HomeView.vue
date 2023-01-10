<template>
  <div class="container d-flex gap-3" id="Kanban">
    <!-- loop over the different columns (Backlog, To Do, Done) -->
    <div v-for="(column, index) in columns" :key="index" class="column">
      <div class="column-header">
        {{ column.name }}
      </div>
      <div class="column-body overflow-auto">
        <!-- here you can add tasks to a column -->
        <div class="input-group mb-3">
          <input
            v-model="newTaskNames[index]"
            type="text"
            placeholder="Name a task"
            class="form-control"
          />
          <button
            @click="addTask(index, newTaskNames[index], id)"
            class="btn btn-primary btn-outline-secondary text-white"
          >
            +
          </button>
        </div>
        <!-- TaskItem represents one task in a column, so we loop (v-for) over all tasks per column -->
        <!-- We used the HTML5 drag & drop events, but one could also use a drag API such as https://github.com/SortableJS/Vue.Draggable -->
        <TaskItem
          v-for="(task, taskIndex) in column.tasks"
          :key="taskIndex"
          :task="task"
          :index="index"
          :taskIndex="taskIndex"
          :columns="columns"
          @dragstart="onDragStart(task, $event, index)"
          draggable="true"
          ref="dragArea"
          @dragover.prevent
          @drop="onDrop(index, $event, taskIndex)"
          @deletetask="deleteTask"
          @updatename="updateName"
          @updatechecklisthome="updateChecklist"
        />
        <!-- dragging a task to this div will add it to the bottom of the column -->
        <div
          class="dragBottom mb-3"
          draggable="false"
          ref="dragArea"
          @dragover.prevent
          @drop="onDrop(index, $event)"
        >
          Drag here to add to bottom
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
      // task id
      id: 0,
      //tasks per column
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
      // names in the 'Name a task' input field
      newTaskNames: ["", "", ""],
    };
  },
  methods: {
    // temporarily save task data when it's being dragged
    onDragStart(task, event, columnIndexFrom) {
      event.dataTransfer.setData("task", JSON.stringify(task));
      event.dataTransfer.setData("column", columnIndexFrom);
    },
    // called when a task is dropped in a new place. Get task data and change column data accordingly
    onDrop(index, event, taskIndex = undefined) {
      //Index = column index to, taskIndex index to
      event.preventDefault();
      const taskObj = JSON.parse(event.dataTransfer.getData("task"));
      const task = taskObj.name;
      const taskId = taskObj.id;
      const taken = taskObj.taken;
      const takenChecked = taskObj.takenChecked;
      const columnId = parseInt(event.dataTransfer.getData("column"));
      // delete task where it came from
      for (let [i, item] of this.columns[columnId].tasks.entries()) {
        if (item.id === taskId) {
          this.columns[columnId].tasks.splice(i, 1);
        }
      }
      // add task 1 above specified index in column
      if (taskIndex !== undefined) {
        //insert task in new column
        this.columns[index].tasks.splice(taskIndex + 1, 0, {
          name: task,
          id: taskId,
          taken: taken,
          takenChecked: takenChecked,
        });
        // add task to end of column
      } else {
        //insert task at end of column
        this.columns[index].tasks.push({
          name: task,
          id: taskId,
          taken: taken,
          takenChecked: takenChecked,
        });
      }
    },
    // add a new task
    addTask(columnIndex, taskName, taskId) {
      this.columns[columnIndex].tasks.push({
        name: taskName,
        id: taskId,
        takenChecked: [false, false], //shows whether a task is done or not
        taken: ["", ""], //name of task
      });
      this.newTaskNames[columnIndex] = "";
      this.id += 1;
    },
    // delete a task (called from TaskItem)
    deleteTask(task, columnIndex) {
      const index = this.columns[columnIndex].tasks.indexOf(task);
      if (index > -1) {
        this.columns[columnIndex].tasks.splice(index, 1);
      }
    },
    //called from TaskItem
    updateName(index, taskIndex, taskName) {
      this.columns[index].tasks[taskIndex].name = taskName;
    },
    //called from TaskItem
    updateChecklist(index, taskIndex, arr, pos, value) {
      //array to update, position in that array to update and value to update it with
      this.columns[index].tasks[taskIndex][arr][pos] = value;
    },
  },
};
</script>

<style>
.column {
  flex: 1;
}

.column-header {
  background-color: #f2f2f2;
  padding: 10px;
  font-weight: bold;
}

.column-body {
  background: #f2f2f2;
  min-height: 100px;
  max-height: 85vh;
  border: 2px solid #ccc;
  text-align: center;
  line-height: 100px;
  font-size: 20px;
  color: #ccc;
}
#Kanban {
  padding-top: 20px;
  height: 95vh;
}

.dragBottom {
  font-size: 12px;
  color: black;
  background-color: skyblue;
}
#footer {
  width: 100%;
  bottom: 0;
  color: white;
}
</style>
