<template>
  <modal title="Modal with form" @close="$emit('close')">
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <!-- name -->
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name:</label>
          <p class="errorText" v-if="!$v.name.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.name.minLength">
            Name must include at least {{ $v.name.$params.minLength.min }}!
          </p>
          <input v-model="name" :class="{ error: $v.name.$error }" @change="$v.name.$touch()" />
        </div>
        <!-- password -->
        <div class="form-item" :class="{ errorInput: $v.password.$error }">
          <label>Password:</label>
          <p class="errorText" v-if="!$v.password.required">Filed is required!</p>
           <p class="errorText" v-if="!$v.password.minLength">
            Password must include at least {{ $v.password.$params.minLength.min }}!
          </p>
          <input type="password" v-model="password" :class="{ error: $v.password.$error }" @change="$v.password.$touch()" />
        </div>
        <!-- repeat password -->
        <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error }">
          <label>Repeat password:</label>
          <p class="errorText" v-if="!$v.repeatPassword.sameAs">Passwords don't match!</p>
          <input type="password" v-model="repeatPassword" :class="{ error: $v.repeatPassword.$error }" @change="$v.repeatPassword.$touch()" />
        </div>
        <!-- email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.email.email">Email is not correct!</p>
          <input v-model="email" :class="{ error: $v.email.$error }" @change="$v.email.$touch()" />
        </div>
        <!-- button -->
        <button class="btn btnPrimary">Submit</button>
      </form>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators';

import modal from '@/components/UI/Modal.vue';
export default {
  components: {
    modal,
  },
  data() {
    return {
      name: '',
      email: '',
      password: '',
      repeatPassword: '',
    };
  },
  validations: {
    name: {
      required,
      minLength: minLength(4),
    },
    password: {
      required,
      minLength: minLength(6)
    },
    repeatPassword: {
      sameAsPassword: sameAs('password')
    },
    email: {
      required,
      email,
    },
  },
  methods: {
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          name: this.name,
          email: this.email,
        };
        console.log(user);
        this.name = '';
        this.email = '';
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
