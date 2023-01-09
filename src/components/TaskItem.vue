<template>
  <div class="task">
    <!-- De taak-div heeft ook het kenmerk versleepbaar, waardoor het geschikt is om te worden gesleept. -->

    <!-- invoerelement om tekst bij te voegen. De button plaatst een verwijder button bij. -->
    <div class="mb-3 input-group">
      <input
        :value="task.name"
        type="text"
        @change="updateName()"
        class="form-control"
      />
      <button
        @click="deleteTask(task, index)"
        class="btn btn-outline-secondary"
      >
        -
      </button>
    </div>

    <!-- Button trigger modal -->
    <button
      type="button"
      class="btn btn-primary"
      data-bs-toggle="modal"
      :data-bs-target="`#exampleModal${task.id}`"
    >
      Add checklist
    </button>
    <CheckListItem :id="task.id"> </CheckListItem>
  </div>
</template>

<script>
import CheckListItem from "@/components/CheckListItem";
export default {
  name: "TaskItem",
  components: { CheckListItem },

  props: {
    task: Object,
    index: Number,
    taskIndex: Number,
    columns: Array,
  },
  methods: {
    updateName(taskName) {
      this.$emit("updatename", this.index, this.taskIndex, taskName);
    },
    deleteTask(task, index) {
      this.$emit("deletetask", task, index);
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
