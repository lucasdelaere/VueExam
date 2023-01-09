<template>
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
          <h1 class="modal-title fs-5" :id="`exampleModalLabel${id}`">
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
              checked
            />
            <input type="text" value="taak1" />
          </div>
          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              value=""
              :id="`taak2_${id}`"
            />
            <input type="text" value="taak2" />
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
    id: Number,
  },
  data() {},
};
</script>
