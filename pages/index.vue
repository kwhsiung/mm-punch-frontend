<template>
  <section class="wrapper">
    <AppSignUp />
  </section>
</template>

<script>
import AppSignUp from '@/components/AppSignUp.vue'

export default {
  components: {
    AppSignUp
  }
}
</script>

<style lang="stylus" scoped>
.wrapper
  height 100vh
  display flex
  justify-content center
  align-items center
</style>
