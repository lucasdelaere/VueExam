<template>
  <div class="task bg-light">
    <!-- De taak-div heeft ook het kenmerk versleepbaar, waardoor het geschikt is om te worden gesleept. -->

    <!-- invoerelement om tekst bij te voegen. De button plaatst een verwijder button bij. -->
    <div class="input-group">
      <!-- v-model kan niet gebruikt worden op props (deze inheriten namelijk van de parent (HomeView), gebruik in plaats daarvan een v-bind (:value) in combinatie met een v-on listener (@input) die de update emit naar de parent -->
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

    <!-- Button trigger modal -->
    <button
      type="button"
      class="btn btn-success"
      data-bs-toggle="modal"
      :data-bs-target="`#exampleModal${task.id}`"
    >
      Checklist ({{ task.takenChecked.filter((x) => x === true).length }}/2)
    </button>
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
    task: Object, //contains name, id, takenChecked & taken
    index: Number,
    taskIndex: Number,
    columns: Array,
  },
  methods: {
    updateName(inputTaskName) {
      this.$emit("updatename", this.index, this.taskIndex, inputTaskName);
    },
    deleteTask(task, index) {
      this.$emit("deletetask", task, index);
    },
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
