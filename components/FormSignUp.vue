<template>
  <div>
    <div class="field">
      <label class="label">ID</label>
      <div class="control has-icons-left has-icons-right">
        <input
          v-model="formId"
          :class="['input', { 'is-danger': isFormIdInvalid }]"
          type="text"
          placeholder="MXXXX"
        />
        <span class="icon is-small is-left">
          <i class="fas fa-user" />
        </span>
      </div>
    </div>
    <div class="field">
      <label class="label">Password</label>
      <div class="control has-icons-left has-icons-right">
        <input
          v-model="formPassword"
          :class="['input', { 'is-danger': isFormPasswordInvalid }]"
          type="password"
          placeholder="AXXXXXXXXX"
        />
        <span class="icon is-small is-left">
          <i class="fas fa-lock" />
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isFormIdInvalid: {
      type: Boolean,
      default: false
    },
    isFormPasswordInvalid: {
      type: Boolean,
      default: false
    },
    propsFormId: {
      type: String,
      required: true
    },
    propsFormPassword: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      internalFormId: this.propsFormId,
      internalFormPassword: this.propsFormPassword
    }
  },
  computed: {
    formId: {
      set(value) {
        this.$emit('update:propsFormId', value)
      },
      get() {
        return this.internalFormId
      }
    },
    formPassword: {
      set(value) {
        this.$emit('update:propsFormPassword', value)
      },
      get() {
        return this.internalFormPassword
      }
    }
  },
  watch: {
    propsFormId() {
      this.internalFormId = this.propsFormId
    },
    propsFormPassword() {
      this.internalFormPassword = this.propsFormPassword
    }
  }
}
</script>

<style lang="stylus" scoped></style>
