<template>
  <modalAuth title="Login" @close="$emit('close')" @toggle="$emit('toggle')">
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <!-- email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.email.email">Email is not correct!</p>
          <input v-model="email" :class="{ error: $v.email.$error }" @change="$v.email.$touch()" />
        </div>
        <!-- password -->
        <div class="form-item" :class="{ errorInput: $v.password.$error }">
          <label>Password:</label>
          <p class="errorText" v-if="!$v.password.required">Filed is required!</p>
          <input
            type="password"
            v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()"/>
        </div>
        <!-- button -->
        <button class="btn btnPrimary">Login</button>
      </form>
    </div>
  </modalAuth>
</template>

<script>
import { required, email } from 'vuelidate/lib/validators';
import modalAuth from '@/components/UI/ModalAuth.vue';
export default {
  components: {
    modalAuth,
  },
  data() {
    return {
      email: '',
      password: '',
    };
  },
  validations: {
    email: {
      required,
      email,
    },
    password: {
      required,
    },
  },
  methods: {
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          password: this.password
        };
        console.log(user);
        this.email = '';
        this.password = '';
        this.$v.$reset();
        this.$emit('close');
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.form-item .errorText {
  display: none;
  margin-bottom: 8px;
  font-size: 13.4px;
  color: #ca3939;
}
.form-item {
  &.errorInput .errorText {
    display: block;
  }
}
input.error {
  border-color: #ca3939;
}
</style>
