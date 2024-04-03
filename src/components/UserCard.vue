<template>
  <v-dialog v-model="dialog" persistent :max-width="500">
    <template v-slot:activator="{ on, attrs }">
      <v-btn v-bind="attrs" v-on="on" icon small dense>
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
    </template>
    <v-card v-if="selectedUser">
      <v-toolbar elevation="1">
        <div
          class="d-flex align-center justify-space-between"
          style="width: 100%"
        >
          <v-toolbar-title>Информация о пользователе</v-toolbar-title>
          <v-btn icon @click="cancel">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </div>
      </v-toolbar>
      <v-container>
        <v-text-field
          placeholder="Имя"
          outlined
          dense
          :autofocus="true"
          v-model="selectedUser.firstname"
        ></v-text-field>

        <v-text-field
          placeholder="Фамилия"
          outlined
          dense
          v-model="selectedUser.lastname"
        ></v-text-field>

        <v-text-field
          placeholder="Email"
          outlined
          dense
          v-model="selectedUser.email"
          type="email"
        ></v-text-field>

        <v-text-field
          placeholder="Телефон"
          outlined
          dense
          v-model="selectedUser.phone"
        ></v-text-field>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn depressed outlined @click="save"> Сохранить </v-btn>
          <v-btn depressed outlined @click="cancel"> Отменить </v-btn>
        </v-card-actions>
      </v-container>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "UserCard",
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      dialog: false,
      selectedUser: null,
    };
  },
  watch: {
    dialog() {
      if (this.dialog) {
        this.selectedUser = JSON.parse(JSON.stringify(this.user));
      }
    },
  },

  methods: {
    save() {
      if(JSON.stringify(this.user) !== JSON.stringify(this.selectedUser)){
        this.$emit("edit-user", this.selectedUser);
      }
      this.cancel();
    },
    cancel() {
      this.dialog = false;
      this.selectedUser = null;
    },
  },
};
</script>

<style></style>
