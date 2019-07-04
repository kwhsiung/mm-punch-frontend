<template>
  <section class="wrapper">
    <AppSignUp
      v-show="currentForm === 'signUp'"
      @toggleChangePassword="currentForm = 'changePassword'"
    />
    <AppChangePassword
      v-show="currentForm === 'changePassword'"
      @toggleSignUp="currentForm = 'signUp'"
    />
  </section>
</template>

<script>
import AppSignUp from '@/components/AppSignUp.vue'
import AppChangePassword from '@/components/AppChangePassword.vue'

export default {
  components: {
    AppSignUp,
    AppChangePassword
  },
  data() {
    return {
      currentForm: 'signUp'
    }
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
