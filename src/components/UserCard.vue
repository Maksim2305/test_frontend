<template>
  <v-card class="pa-5">
      <div>
          <div class="d-flex justify-space-center">
              <div class="mr-5 ">
                <div>
                  Имя: {{ user.firstname }}
                </div>
                <div>
                  Фамилия: {{ user.lastname }}
                </div>
                <div>
                  Email: {{ user.email }}
                </div>
                <div>
                  Телефон: {{ user.phone }}
                </div>
                <div>
                  Пол: {{ user.gender === 'female' ? 'женский' : 'мужской' }}
                </div>
                <div>
                  Сайт: {{ user.website }}
                </div>
                <div>
                  Дата рождения: {{ user.birthday }}
                </div>
              </div>
              <div>
                <div>
                  Страна: {{ user.address.country }}
                </div>
                <div>
                  Город: {{ user.address.city }}
                </div>
                <div>
                  Улица: {{ user.address.street }}
                </div>
                <div>
                  zip: {{ user.address.zipcode }}
                </div>
              </div>
            
             
          </div>
          <v-card-actions>
            <v-spacer></v-spacer>
            <UserEdit :user="user"/>
          </v-card-actions>
      </div>
  </v-card>
</template>

<script>
import UserEdit from './UserEdit.vue'
export default {
  components: {
    UserEdit
  },
  props: {
    user: {
      type: Object,
      required: true
    }
  },
  methods: {
    edit () {
      this.$emit('edit-user', this.user)
    }
  }
  
}

</script>