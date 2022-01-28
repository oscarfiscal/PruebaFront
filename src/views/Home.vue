<!-- @format -->

<template>
  <div>
    <h1>Registro</h1>

    <FormContact />
  </div>
</template>

<script>
import FormContact from "@/components/FormContact/index.vue";

export default {
  name: "Home",

  components: {
    FormContact,
  },
};
</script>
