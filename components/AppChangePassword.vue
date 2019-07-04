<template>
  <div class="box">
    <FormChangePassword />
    <div class="buttons">
      <ButtonGreen>
        確認修改
      </ButtonGreen>
      <ButtonYellow @click.native="$emit('toggleSignUp')">
        加入會員
      </ButtonYellow>
    </div>
    <div class="hints">
      <p class="hints__hint--success">
        修改成功
      </p>
      <p class="hints__hint--fail">
        會員不存在
      </p>
      <p class="hints__hint--fail">
        舊密碼錯誤
      </p>
      <p class="hints__hint--fail">
        請再確認新密碼
      </p>
    </div>
  </div>
</template>

<script>
import FormChangePassword from '@/components/FormChangePassword.vue'
import ButtonGreen from '@/components/ButtonGreen.vue'
import ButtonYellow from '@/components/ButtonYellow.vue'

export default {
  components: {
    FormChangePassword,
    ButtonGreen,
    ButtonYellow
  }
}
</script>

<style lang="stylus" scoped>
.buttons
  display flex
  justify-content space-between
  margin 50px 0 0 0

.hints
  &__hint
    &--success
      color #00d1b2
    &--fail
      color #ff3860

.box
  width 500px
</style>
