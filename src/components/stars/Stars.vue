<template>
  <div>
    <i
      v-for="(star, i) in count"
      :key="i"
      class="fas fa-star fa-4x"
    />
  </div>
</template>
<style lang="scss" scoped>
  @import "../../assets/variables.scss";
  
  i {
    color: $yellowPrimary;
  }
</style>

<script>
export default {
  name: 'Stars',
  props: {
    count: {
      type: Number,
      default: 0
    }
  }
}
</script>
