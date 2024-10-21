<template>
  <div class="col" >
    <div class="card">
      <slot name="image"></slot>
      <div class="card-body">
          <slot name="name"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
props: ["id"],
};
</script>

<style>
.card {
border: 2px solid var(--text-color);
border-radius: 10px;
overflow: hidden;
}
.card-img-top {
height: 200px;
object-fit: cover;
}

.card-body{
  background: var(--bg-black-100) !important;
}

.card-title{
  color: var(--text-color);
}
</style>
