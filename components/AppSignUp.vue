<template>
  <div class="box">
    <FormSignUp
      :is-form-id-invalid="isFormIdInvalid"
      :is-form-password-invalid="isFormPasswordInvalid"
      :props-form-id.sync="formId"
      :props-form-password.sync="formPassword"
    />
    <div class="buttons">
      <ButtonGreen
        :class="{ 'is-loading': isFirebaseReqPending }"
        @click.native="submit"
      >
        確認加入
      </ButtonGreen>
      <ButtonYellow
        v-show="false"
        @click.native="$emit('toggleChangePassword')"
      >
        修改密碼
      </ButtonYellow>
    </div>
    <div v-show="showResult" class="hints">
      <p v-show="isFirebaseReqSuccess" class="hints__hint--success">
        加入成功
      </p>
      <p v-show="isFirebaseReqFail" class="hints__hint--fail">
        加入失敗
      </p>
      <p v-show="isMemberExist" class="hints__hint--fail">
        會員已存在
      </p>
    </div>
  </div>
</template>

<script>
import FormSignUp from '@/components/FormSignUp.vue'
import ButtonGreen from '@/components/ButtonGreen.vue'
import ButtonYellow from '@/components/ButtonYellow.vue'

export default {
  components: {
    FormSignUp,
    ButtonGreen,
    ButtonYellow
  },
  data() {
    return {
      formId: '',
      formPassword: '',
      isFormIdInvalid: false,
      isFormPasswordInvalid: false,
      isFirebaseReqPending: false,
      isFirebaseReqSuccess: false,
      isFirebaseReqFail: false,
      isMemberExist: false
    }
  },
  computed: {
    showResult() {
      return !this.isFormIdInvalid && !this.isFormPasswordInvalid
    }
  },
  methods: {
    resetFirebaseStatus() {
      this.isFirebaseReqPending = false
      this.isFirebaseReqSuccess = false
      this.isFirebaseReqFail = false
      this.isMemberExist = false
    },
    validateFormId() {
      this.isFormIdInvalid = this.formId === ''
    },
    validateFormPassword() {
      this.isFormPasswordInvalid = this.formPassword === ''
    },
    signUp() {
      this.isFirebaseReqPending = true
      this.$checkMemberExist(this.formId)
        .then((res) => {
          if (res === 'not exist') {
            this.$setMember({
              id: this.formId,
              password: this.formPassword
            })
              .then(() => {
                this.isFirebaseReqPending = false
                this.isFirebaseReqSuccess = true
              })
              .catch(() => {
                this.isFirebaseReqPending = false
                this.isFirebaseReqFail = true
              })
          } else {
            this.isFirebaseReqPending = false
            this.isMemberExist = true
          }
        })
        .catch(() => {
          this.isFirebaseReqPending = false
          this.isFirebaseReqFail = true
        })
    },
    submit() {
      this.resetFirebaseStatus()
      this.validateFormId()
      this.validateFormPassword()

      if (this.formId !== '' && this.formPassword !== '') {
        this.signUp()
      }
    }
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
