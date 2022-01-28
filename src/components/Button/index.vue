<template>
  <v-btn
    x-large
    class="boton btn-lg btn-block border-0 text-center  text-white inline-block w-100 p-3 mb-3"
    style=" background-color: black;"
    :loading="loading"
    @click="loader = 'loading'"
  >
    Ingresar
  </v-btn>
</template>
<script>
export default {
  data() {
    return {
      loader: null,
      loading: false,
    };
  },
  watch: {
    loader() {
      const l = this.loader;
      this[l] = !this[l];

      setTimeout(() => (this[l] = false), 500);

      this.loader = null;
    },
  },
};
</script>

