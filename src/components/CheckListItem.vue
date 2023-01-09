<template>
  <!-- CheckListItem zien behandelen we als een child van TaskItem (die op zijn beurt een child is van HomeView -->
  <!-- Modal -->
  <div
    class="modal fade"
    :id="`exampleModal${id}`"
    tabindex="-1"
    :aria-labelledby="`exampleModalLabel${id}`"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5 text-dark" :id="`exampleModalLabel${id}`">
            checklist
          </h1>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              value=""
              :id="`taak1_${id}`"
              :checked="takenChecked[0]"
              @change="updateChecklist('takenChecked', 0, !takenChecked[0])"
            />
            <!--of !$event.target.checked -->
            <input
              type="text"
              :value="taken[0]"
              class="form-control"
              @change="updateChecklist('taken', 0, $event.target.value)"
            />
          </div>
          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              value=""
              :id="`taak2_${id}`"
              :checked="takenChecked[1]"
              @change="updateChecklist('takenChecked', 1, !takenChecked[1])"
            />
            <input
              type="text"
              :value="taken[1]"
              class="form-control"
              @change="updateChecklist('taken', 1, $event.target.value)"
            />
          </div>
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CheckListItem",
  props: {
    id: Number, //unique id for each task and corresponding modal
    taken: Array,
    takenChecked: Array,
  },
  methods: {
    updateChecklist(arr, pos, value) {
      //array to update, position in that array to update and value to update it with
      this.$emit("updatechecklist", arr, pos, value);
    },
  },
};
</script>
