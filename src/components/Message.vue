<template>
<div class="message">
  <p>{{message}}</p>
</div>

</template>

<style lang="scss" scoped>

  p {
    color: tomato;
  }
</style>
<script>
export default {
  props: {
      message: {
          type: String,
          required: true
      }
  }
}
</script>