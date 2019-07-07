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
        :class="{ 'is-loading': isFirebaseReqPendingSignUp }"
        @click.native="submitSignUp"
      >
        確認加入
      </ButtonGreen>
      <ButtonRed
        :class="{ 'is-loading': isFirebaseReqPendingQuit }"
        @click.native="submitQuit"
      >
        幫我退出
      </ButtonRed>
    </div>
    <div v-show="showResult" class="hints">
      <div class="hints__sign-up-hints-wrapper">
        <p v-show="isFirebaseReqSuccessSignUp" class="hints__hint--success">
          加入成功
        </p>
        <p v-show="isFirebaseReqFailSignUp" class="hints__hint--fail">
          加入失敗
        </p>
        <p v-show="isMemberExistSignUp" class="hints__hint--fail">
          會員已存在
        </p>
      </div>
      <div class="hints__quit-hints-wrapper">
        <p v-show="isFirebaseReqSuccessQuit" class="hints__hint--success">
          退出成功
        </p>
        <p v-show="isFirebaseReqFailQuit" class="hints__hint--fail">
          退出失敗
        </p>
        <p v-show="isMemberNotExistQuit" class="hints__hint--fail">
          會員不存在
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import FormSignUp from '@/components/FormSignUp.vue'
import ButtonGreen from '@/components/ButtonGreen.vue'
import ButtonRed from '@/components/ButtonRed.vue'

export default {
  components: {
    FormSignUp,
    ButtonGreen,
    ButtonRed
  },
  data() {
    return {
      formId: '',
      formPassword: '',
      isFormIdInvalid: false,
      isFormPasswordInvalid: false,

      // For sign up
      isFirebaseReqPendingSignUp: false,
      isFirebaseReqSuccessSignUp: false,
      isFirebaseReqFailSignUp: false,
      isMemberExistSignUp: false,

      // For quit
      isFirebaseReqPendingQuit: false,
      isFirebaseReqSuccessQuit: false,
      isFirebaseReqFailQuit: false,
      isMemberNotExistQuit: false
    }
  },
  computed: {
    showResult() {
      return !this.isFormIdInvalid && !this.isFormPasswordInvalid
    }
  },
  methods: {
    resetFirebaseStatus() {
      this.isFirebaseReqPendingSignUp = false
      this.isFirebaseReqSuccessSignUp = false
      this.isFirebaseReqFailSignUp = false
      this.isMemberExistSignUp = false

      this.isFirebaseReqPendingQuit = false
      this.isFirebaseReqSuccessQuit = false
      this.isFirebaseReqFailQuit = false
      this.isMemberNotExistQuit = false
    },
    validateFormId() {
      this.isFormIdInvalid = this.formId === ''
    },
    validateFormPassword() {
      this.isFormPasswordInvalid = this.formPassword === ''
    },
    signUp() {
      this.isFirebaseReqPendingSignUp = true
      this.$checkMemberExist(this.formId)
        .then((res) => {
          if (res === 'not exist') {
            this.$setMember({
              id: this.formId,
              password: this.formPassword
            })
              .then(() => {
                this.isFirebaseReqPendingSignUp = false
                this.isFirebaseReqSuccessSignUp = true
              })
              .catch(() => {
                this.isFirebaseReqPendingSignUp = false
                this.isFirebaseReqFailSignUp = true
              })
          } else {
            this.isFirebaseReqPendingSignUp = false
            this.isMemberExistSignUp = true
          }
        })
        .catch(() => {
          this.isFirebaseReqPendingSignUp = false
          this.isFirebaseReqFailSignUp = true
        })
    },
    quit() {
      this.isFirebaseReqPendingQuit = true
      this.$checkMemberExist(this.formId)
        .then((res) => {
          if (res === 'not exist') {
            this.isFirebaseReqPendingQuit = false
            this.isMemberNotExistQuit = true
          } else {
            this.$removeMember({
              id: this.formId,
              password: this.formPassword
            })
              .then(() => {
                this.isFirebaseReqPendingQuit = false
                this.isFirebaseReqSuccessQuit = true
              })
              .catch(() => {
                this.isFirebaseReqPendingQuit = false
                this.isFirebaseReqFailQuit = true
              })
          }
        })
        .catch(() => {
          this.isFirebaseReqPendingQuit = false
          this.isFirebaseReqFailQuit = true
        })
    },
    submitSignUp() {
      this.resetFirebaseStatus()
      this.validateFormId()
      this.validateFormPassword()

      if (this.formId !== '' && this.formPassword !== '') {
        this.signUp()
      }
    },
    submitQuit() {
      this.resetFirebaseStatus()
      this.validateFormId()
      this.validateFormPassword()
      if (this.formId !== '' && this.formPassword !== '') {
        this.quit()
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
  display flex
  justify-content space-between
  &__quit-hints-wrapper
    text-align right
  &__hint
    &--success
      color #00d1b2
    &--fail
      color #ff3860

.box
  width 500px
</style>
