<template>
  <div class="avatar">
    <img :src="value" />
  </div>
</template>

<script>
export default {
  props: ['value']
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 100%;
    max-width: 200px;
    border-radius: 50%;
  }
</style>
