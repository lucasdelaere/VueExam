<template>
  <div class="task bg-light">
    <div class="input-group">
      <!-- v-model should not be used on props (these inherit from the parent (HomeView)). Instead, use a v-bind (:value) in combination with a v-on listener (@input) which emits the update to the parent -->
      <input
        :value="task.name"
        type="text"
        class="form-control"
        @input="updateName($event.target.value)"
      />
      <button
        @click="deleteTask(task, index)"
        class="btn btn-danger btn-outline-secondary text-white"
      >
        -
      </button>
    </div>

    <!-- Button trigger checklist modal (CheckListItem) -->
    <button
      type="button"
      class="btn btn-success"
      data-bs-toggle="modal"
      :data-bs-target="`#exampleModal${task.id}`"
    >
      <!-- show the amount of checked items -->
      Checklist ({{ task.takenChecked.filter((x) => x === true).length }}/2)
    </button>
    <!-- checklist modal -->
    <CheckListItem
      :id="task.id"
      :name="task.name"
      :taken="task.taken"
      :takenChecked="task.takenChecked"
      @updatechecklist="updateChecklist"
    >
    </CheckListItem>
  </div>
</template>

<script>
import CheckListItem from "@/components/CheckListItem";
export default {
  name: "TaskItem",
  components: { CheckListItem },
  props: {
    task: Object, // contains name, id, takenChecked & taken
    index: Number, // index of column
    taskIndex: Number, // index of current task
    columns: Array,
  },
  methods: {
    //emit 'updatename' event to parent with new name, column index and taskIndex
    updateName(inputTaskName) {
      this.$emit("updatename", this.index, this.taskIndex, inputTaskName);
    },
    //emit 'deletetask' event to parent with the task and column index
    deleteTask(task, index) {
      this.$emit("deletetask", task, index);
    },
    //emit 'updatechecklisthome' event to parent with array, position and value to update, column index and task index
    updateChecklist(arr, pos, value) {
      //array to update, position in that array to update and value to update it with
      this.$emit(
        "updatechecklisthome",
        this.index,
        this.taskIndex,
        arr,
        pos,
        value
      );
    },
  },
};
</script>

<style scoped>
.task {
  background-color: #f8f8f8;
  padding: 10px;
  margin-bottom: 20px;
  margin-left: 10px;
  margin-right: 10px;
  cursor: move;
  border: 1px solid black;
}
</style>
