<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <div id="app">
            <!-- first modal -->
            <button class="btn btnPrimary" @click="modalFirst = true">Show first modal</button>
            <modal title="First modal" v-show="modalFirst" @close="modalFirst = false">
              <div slot="body">
                <p>text text text text text text text</p>
                <button class="btn btnPrimary" @click="modalFirst = false">Well done</button>
              </div>
            </modal>

            <!-- second modal -->
            <button class="btn btnPrimary" @click="modalSecond.show = true">
              Show modal with form
            </button>
            <modal
              title="Modal with form"
              v-show="modalSecond.show"
              @close="modalSecond.show = false"
            >
              <div slot="body">
                <form @submit.prevent="submitSecondForm">
                  <label>Name:</label>
                  <input type="text" required v-model="modalSecond.name" />
                  <label>Email:</label>
                  <input type="email" required v-model="modalSecond.email" />
                  <button class="btn btnPrimary" @click="submitSecondForm">Submit</button>
                </form>
              </div>
            </modal>

            <!-- modal with validate -->
            <button class="btn btnPrimary" @click="modalValidate = !modalValidate">
              Show modal with form + validate
            </button>
            <modalValidate v-if="modalValidate" @close="modalValidate = false" />

            <!-- modal with authorisation -->
            <button class="btn btnPrimary" @click="modalValidateAuth = !modalValidateAuth">
              Login
            </button>
            <modalValidateAuth v-if="modalValidateAuth" @close="modalValidateAuth = false" @toggle="toggleAuth"/>
          <!-- modal with registration -->
            <button class="btn btnPrimary" @click="modalValidateReg = !modalValidateReg">
              Sign up
            </button>
            <modalValidateReg v-if="modalValidateReg" @close="modalValidateReg = false" @toggle="toggleAuth"/>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import modal from '@/components/UI/Modal.vue';
import modalValidate from '@/components/ModalValidate.vue';
import modalAuth from '@/components/UI/ModalAuth.vue';
import modalValidateAuth from '@/components/ModalValidateAuth.vue';
import modalValidateReg from '@/components/ModalValidateReg.vue';
export default {
  components: {
    modal,
    modalValidate,
    modalAuth,
    modalValidateAuth,
    modalValidateReg
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: '',
      },
      modalValidate: false,
      modalValidateAuth: false,
      modalValidateReg: false
    };
  },
  methods: {
    submitSecondForm() {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email,
      });
      this.modalSecond.name = '';
      this.modalSecond.email = '';
      this.modalSecond.show = false;
    },
    toggleAuth() {
      this.modalValidateReg = !this.modalValidateReg;
      this.modalValidateAuth = !this.modalValidateAuth;
    }
  },
};
</script>

<style></style>
