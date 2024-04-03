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
      <UsersList />
    </v-main>
  </v-app>
</template>

<script>
import UsersList from './components/UsersList.vue';


export default {
  name: "App",
  components : {
    UsersList
  },

  data: () => ({}),

};
</script>
