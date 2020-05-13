<template>
  <modalAuth title="Sign up" @close="$emit('close')" @toggle="$emit('toggle')">
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
          <p class="errorText" v-if="!$v.password.minLength">
            Password must include at least {{ $v.password.$params.minLength.min }}!
          </p>
          <input
            type="password"
            v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()"/>
        </div>
                <!-- repeat password -->
        <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error }">
          <label>Repeat password:</label>
          <p class="errorText" v-if="!$v.repeatPassword.sameAs">Passwords don't match!</p>
          <input type="password" v-model="repeatPassword" :class="{ error: $v.repeatPassword.$error }" @change="$v.repeatPassword.$touch()" />
        </div>
        <!-- button -->
        <button class="btn btnPrimary">Sign up</button>
      </form>
    </div>
  </modalAuth>
</template>

<script>
import { required, email, sameAs, minLength } from 'vuelidate/lib/validators';
import modalAuth from '@/components/UI/ModalAuth.vue';
export default {
  components: {
    modalAuth,
  },
  data() {
    return {
      email: '',
      password: '',
      repeatPassword: ''
    };
  },
  validations: {
    email: {
      required,
      email,
    },
    password: {
      required,
      minLength: minLength(6)
    },
    repeatPassword: {
      sameAsPassword: sameAs('password')
    }
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
