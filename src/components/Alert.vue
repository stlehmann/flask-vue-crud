<template>
  <div>
    <b-alert :variant="variant" :show="dismissCountDown" fade
             dismissible @dismissed="dismissCountDown=0">
      {{ message }}
    </b-alert>
    <br>
  </div>
</template>

<script>
export default {
  name: 'AlertComponent',
  data() {
    return {
      dismissCountDown: 0,
      message: '',
      variant: 'success',
    };
  },
  methods: {
    show(message, variant = 'success') {
      this.dismissCountDown = 3;
      this.message = message;
      this.variant = variant;
    },
  },
};
</script>

<style scoped>

</style>
