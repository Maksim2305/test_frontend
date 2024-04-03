<template>
  <v-dialog v-model="dialog" :max-width="500">
    <template v-slot:activator="{ on, attrs }">
      <v-btn v-bind="attrs" v-on="on"> Редактировать </v-btn>
    </template>
    <v-card v-if="selectedUser">
      <v-toolbar elevation="1">
        <div
          class="d-flex align-center justify-space-between"
          style="width: 100%"
        >
          <v-toolbar-title>Информация о пользователе</v-toolbar-title>
          <v-btn icon @click="close">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </div>
      </v-toolbar>
      <v-container>
        <v-form ref="formRef" v-model="valid">
        <v-text-field
          label="Имя"
          outlined
          dense
          :autofocus="true"
          v-model="selectedUser.firstname"
          :rules="[rules.required]"
        ></v-text-field>

        <v-text-field
          label="Фамилия"
          outlined
          dense
          v-model="selectedUser.lastname"
          :rules="[rules.required]"
        ></v-text-field>

        <v-text-field
          label="Email"
          outlined
          dense
          v-model="selectedUser.email"
          type="email"
          :rules="[rules.required]"
        ></v-text-field>

        <v-text-field
          label="Телефон"
          outlined
          dense
          v-model="selectedUser.phone"
          :rules="[rules.required]"
        ></v-text-field>
        <v-radio-group v-model="selectedUser.gender">
          <v-radio label="Мужской" value="male"></v-radio>
          <v-radio label="Женский" value="female"></v-radio>
        </v-radio-group>

        <v-text-field
          label="Сайт"
          outlined
          dense
          v-model="selectedUser.website"
          :rules="[rules.required]"
        ></v-text-field>
        <v-text-field
          label="Дата рождения"
          outlined
          dense
          v-model="selectedUser.birthday"
          :rules="[rules.required]"
        ></v-text-field>
        <v-text-field
          label="Страна"
          outlined
          dense
          v-model="selectedUser.address.country"
          :rules="[rules.required]"
        ></v-text-field>
        <v-text-field
          label="Город"
          outlined
          dense
          v-model="selectedUser.address.city"
          :rules="[rules.required]"
        ></v-text-field>
        <v-text-field
          label="Улица"
          outlined
          dense
          v-model="selectedUser.address.street"
          :rules="[rules.required]"
        ></v-text-field>
        <v-text-field
          label="zip"
          outlined
          dense
          v-model="selectedUser.address.zipcode"
          :rules="[rules.required]"
        ></v-text-field>
      </v-form>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn depressed outlined @click="save"> Сохранить </v-btn>
          <v-btn depressed outlined @click="close"> Отменить </v-btn>
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
      valid: true,
      rules: {
        required: (value) => !!value || "Обязательное поле",
      },
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
      if (this.$refs['formRef'].validate()) {
        // put запрос
        this.close();
      }
     
    },
    close() {
      this.dialog = false;
      this.selectedUser = null;
    },
  },
};
</script>

<style></style>
