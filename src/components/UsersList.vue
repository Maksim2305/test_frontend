<template>
  <div>
    <v-autocomplete
      class="pa-4"
      style="width: 500px"
      label="Выберите пользователя"
      :items="usersList"
      v-model="selectedUser"
      return-object
      clearable
      outlined
      dense
      :search-input.sync="searchedText"
      :hide-no-data="true"
    >
      <template v-slot:selection="{ item }">
        <span>{{ item.firstname }} {{ item.lastname }}</span>
      </template>

      <template v-slot:item="{ item }">
        <v-list-item-content>
          <v-list-item-title>{{ item.firstname }}</v-list-item-title>
          <v-list-item-subtitle style="color: grey">
            {{ item.lastname }}
          </v-list-item-subtitle>
          <v-list-item-subtitle style="color: grey">
            {{ item.email }}
          </v-list-item-subtitle>
        </v-list-item-content>
      </template>
    </v-autocomplete>
    <v-data-table
      :headers="headers"
      :items="users"
      :hideDefaultFooter="true"
      :loading="loading"
    >
      <template v-slot:item="{ item }">
        <tr>
          <td>{{ item.firstname }}</td>
          <td>{{ item.lastname }}</td>
          <td>{{ item.email }}</td>
          <td>{{ item.phone }}</td>
          <td>
            <UserCard :user="item" @edit-user="editUser" />
          </td>
        </tr>
      </template>
    </v-data-table>
    <v-snackbar
      v-model="snackbar"
      :timeout="2000"
      :color="snackbarInfo.type"
      variant="outlined"
    >
      {{ snackbarInfo.text }}
    </v-snackbar>
  </div>
</template>

<script>
import axios from "axios";
import UserCard from "./UserCard.vue";
export default {
  name: "UsersList",
  components: {
    UserCard,
  },

  data() {
    return {
      usersList: [],
      savedCopy: [],
      selectedUser: null,
      searchedText: "",
      snackbarInfo: {},
      loading: false,
      headers: [
        { text: "Имя", value: "firstname" },
        { text: "Фамилия", value: "lastname" },
        { text: "Email", value: "email" },
        { text: "Телефон", value: "phone" },
        { text: "Действия", value: null },
      ],
      snackbar: false,
    };
  },
  computed: {
    users() {
      if (this.selectedUser) {
        return this.usersList.filter(
          (user) => user.id === this.selectedUser.id
        );
      } else {
        return this.usersList;
      }
    },
  },
  async mounted() {
    await this.getUsers();
  },
  methods: {
    async getUsers() {
      this.loading = true;
      try {
        const { data } = await axios.get(
          `https://fakerapi.it/api/v1/persons?_locale=ru_RU`
        );
        this.usersList = data.data;
        this.savedCopy = this.usersList;
      } catch (error) {
        this.snackbar = true;
        this.snackbarInfo = {
          type: "error",
          text: error.message,
        };
      }
      this.loading = false;
    },
    editUser(user) {
      const index = this.usersList.findIndex((u) => u.id == user.id);
      this.usersList.splice(index, 1, user);
      if (this.selectedUser) {
        this.selectedUser = user;
      }
      this.snackbar = true;
      this.snackbarInfo = {
        type: "success",
        text: "Изменения сохранены",
      };
    },
   
  },
  watch: {
    searchedText() {
      if (this.searchedText) {
        this.usersList = this.usersList.filter(
          (user) =>
            user.firstname
              .toLowerCase()
              .includes(this.searchedText.toLowerCase()) ||
            user.lastname
              .toLowerCase()
              .includes(this.searchedText.toLowerCase()) ||
            user.email.toLowerCase().includes(this.searchedText.toLowerCase())
        );
      } else {
        this.usersList = this.savedCopy;
      }
    },
  },
};
</script>

<style scoped></style>
