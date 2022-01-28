<!-- @format -->

<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :sort-by="sortBy.toLowerCase()"
    class="elevation-12"
  >
   

    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize"> Refresca </v-btn>
    </template>
  </v-data-table>
</template>
<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    search: "",
    filter: {},
    sortBy: "name",
    headers: [
      {
        text: "Nombre",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Telefono", value: "phone", sortable: false },
      { text: "Fecha nacimiento", value: "birth_date", sortable: false },
      { text: "Direccion", value: "direction", sortable: false },
      { text: "Correo electronico", value: "email", sortable: false },

      { text: "Acciones", value: "actions", sortable: false },
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      name: "",
      phone: 0,
      birth_date: 0,
      direction: 0,
      email: 0,
    },
    defaultItem: {
      name: "",
      phone: 0,
      birth_date: 0,
      direction: 0,
      email: 0,
    },
    keys: ["Nombre", "Telefono", "Correo electronico"],
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "Nuevo contacto" : "Eliminar contacto";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.desserts = [
        {
          name: "Oscar Fiscal",
          phone: 3103195434,
          birth_date: "12/12/12",
          direction: "calle falsa 123",
          email: "oscarfiscalcp@gmail.com",
        },
        {
          name: "Andres Lopez",
          phone: 1234567899,
          birth_date: "12/12/12",
          direction: "calle 23 45 67",
          email: "andresl@gmail.com",
        },
        {
          name: "Juan Perez",
          phone: 43243556765,
          birth_date: "28/12/22",
          direction: "cra 1 45 56",
          email: "jperez@gmail.com",
        },
        {
          name: "Antonio Gonzalez",
          phone: 56743323211,
          birth_date: "02/17/98",
          direction: "calle brava 123",
          email: "antgonzales@gmail.com",
        },
      ];
    },

    
  },
};
</script>
