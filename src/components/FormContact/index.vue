<!-- @format -->

<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :search="search"
    :sort-by="sortBy.toLowerCase()"
    class="elevation-12"
  >
    <template v-slot:top>
      <v-toolbar class="mb-2" color="indigo darken-5" dark>
        <v-toolbar-title>Registro de contacto</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn class="mx-2" fab dark v-bind="attrs" v-on="on">
              <v-icon dark> mdi-plus </v-icon>
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.name"
                      label="Nombre"
                      prepend-icon="mdi-account"
                      :rules="[(v) => !!v || 'Campo Obligatorio *']"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.phone"
                      label="Telefono"
                      prepend-icon="mdi-cellphone"
                      type="number"
                      :rules="numberRule"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.birth_date"
                      type="date"
                      label="Fecha Nacimiento"
                      prepend-icon="mdi-calendar"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.direction"
                      label="Direccion"
                      prepend-icon="mdi-home"
                      :rules="[(v) => !!v || 'Campo Obligatorio *']"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.email"
                      label="Correo electronico"
                        prepend-icon="mdi-email"
                      :rules="Rules"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn color="blue darken-1" text @click="close">
                Cancelar
              </v-btn>
              <v-btn color="blue darken-1" text @click="save"> Guardar </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">
              Quieres eliminar este contacto?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete"
                >Cancel</v-btn
              >
              <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
      <v-col cols="12" sm="12">
        <v-text-field
          v-model="search"
          clearable
          flat
          solo-inverted
          hide-details
          prepend-inner-icon="mdi-magnify"
          label="Buscar contacto"
        ></v-text-field>
      </v-col>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-btn x-small class="mx-2" fab color="blue" @click="editItem(item)">
        <v-icon color="white"> mdi-pencil </v-icon>
      </v-btn>
      <v-btn x-small class="mx-2" fab color="red" @click="deleteItem(item)">
        <v-icon color="white"> mdi-delete </v-icon>
      </v-btn>
    </template>
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
    Rules: [
      (v) =>
        /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
        "Ingresa un correo valido",
    ],
    numberRule: [
      (v) => !!v || "Campo Obligatorio *",

      (v) => v.length <= 10 || "debe tener 10 digitos",
    ],
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
      phone: "",
      birth_date: "",
      direction: "",
      email: "",
    },
    defaultItem: {
      name: "",
      phone: "",
      birth_date: "",
      direction: "",
      email: "",
    },
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

    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem);
      } else {
        this.desserts.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>
