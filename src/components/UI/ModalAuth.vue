<template>
  <transition name="modal">
    <div class="modal__wrapper" @click="$emit('close')">
      <div class="modal-content" @click.stop="">
        <!-- header -->
        <div class="modal-header">
          <span class="modal-title"> {{ title }} </span>
          <span class="button-close" @click="$emit('close')">×</span>
        </div>

        <!-- body -->
        <div class="modal-body">
          <slot name="body">Defalt body</slot>
        </div>
        <footer @click="$emit('toggle')">{{ footer }}</footer>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      footer: this.title === 'Login' ? 'I need an account' : 'A have an account',
    };
  },
};
</script>

<style lang="scss" scoped>
.modal-enter,
.modal-leave-active {
  opacity: 0;
}
.modal-enter .modal-content,
.modal-leave-active .modal-content {
  transform: scale(1.2);
}
.modal__wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  transition: opacity 0.2s ease;
  right: 0;
  z-index: 998;
  background-color: rgba(00, 00, 00, 0.48);
}

.modal-content {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: relative;
  max-width: 600px;
  padding: 18px 0 0 0;
  background-color: #fff;
  border: 1px solid #dcdfe6;
  transition: all 0.2s ease;
  border-radius: 8px;
  z-index: 999;
  overflow: hidden;
  @media screen and (min-width: 900px) {
    min-width: 500px;
  }
}
.modal-header {
  display: flex;
  align-self: center;
  justify-content: space-between;
  padding-bottom: 20px;
  span {
    font-size: 24px;
  }
  .button-close {
    cursor: pointer;
    margin-left: 40px;
  }
}
.modal-body {
  text-align: center;
  flex-grow: 1;
  & p {
    margin-bottom: 20px;
  }
}
footer {
  background-color: rgb(221, 221, 221);
  height: 50px;
  margin-top: 30px;
  width: 100%;
  text-align: center;
  padding-top: 10px;
  cursor: pointer;
}
</style>
