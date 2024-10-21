<template>
  <div
    class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <slot name="name"></slot>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body white">
            <slot name="image"></slot>
            <slot name="title"></slot>
            <slot name="quote"></slot>
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn"
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
export default {};
</script>

<style>
.modal-content{
    background-color: var(--bg-black-100) !important;
}

.modal-header{
    border-bottom: var(--text-color) solid 1px;
}

.modal-footer{
    border-top: var(--text-color) solid 1px;
}

</style>
