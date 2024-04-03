<template>
  <v-app>
    <v-main>
      <!-- 1. Отображение информации о пользователе -->
      <!-- <br /> -->
      <!-- TODO сделать в отдельном блоке отображение информации о выбранном пользователе с использование v-card -->
      <!-- 2. Выбор пользователя -->
      <!-- <br /> -->
      <!-- TODO используйте v-autocomplete для выбора пользователя -->
      <!-- 3. Редактирование пользователя -->
      <!-- TODO добавить возможность редактировать данные пользователя -->
      <div>
        <v-autocomplete
          v-if="usersList.length"
          class="pa-4 user-autocomplete"
          v-model="selectedUser"
          label="Выберите пользователя"
          :items="usersList"
          item-text="firstname"
          item-id="id"
          return-object
          clearable
          :loading="loading"
        ></v-autocomplete>
        <div v-else class="text-center">Пользователи не найдены</div>
        <UserCard :user="selectedUser" v-if="selectedUser" />
        <div v-else class="text-center">Пользователь не выбран</div>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import UserCard from "./components/UserCard.vue";

export default {
  name: "App",
  components: {
    UserCard,
  },

  data() {
    return {
      usersList: [],
      selectedUser: null,
      loading: false,
    };
  },
  async mounted() {
    await this.getUsers();
  },
  methods: {
    async getUsers() {
      this.loading = true;
      try {
        const data = await fetch(
          "https://fakerapi.it/api/v1/persons?_locale=ru_RU"
        );
        const res = await data.json();
        this.usersList = res.data;
      } catch (error) {
        console.error(error);
      }
      this.loading = false;
    },
  },
};
</script>

<style scoped>
.user-autocomplete {
  max-width: 500px;
  margin: auto;
}
</style>
