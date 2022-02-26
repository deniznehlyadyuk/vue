<template>
  <v-card style="flex: 1; margin: 5pt;">
    <v-card-title>
      {{ user.name }}
    </v-card-title>
    <v-card-subtitle>
      {{ user.email }}
    </v-card-subtitle>
    <v-card-text>
      Aşağıdan albümlere, postlara veya todo arşivine erişebilirsiniz.
    </v-card-text>
    <v-card-actions>
      <v-btn
        small
        outlined
        color="teal accent-4"
        @click="routeToUserAlbums"
      >
        Albümler
      </v-btn>
      <v-btn
        small
        outlined
        color="indigo accent-4"
        @click="routeToUserTodos"
      >
        Todo
      </v-btn>
      <v-btn
        small
        outlined
        color="light-blue accent-4"
        @click="routeToUserPosts"
      >
        Paylaşımlar
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true
    }
  },
  methods: {
    routeToUserAlbums () {
      this.$router.push({
        path: 'users/albums',
        query: {
          userId: this.user.id
        }
      })
    },
    routeToUserTodos () {
      this.$router.push({
        path: 'users/todos',
        query: {
          userId: this.user.id
        }
      })
    },
    routeToUserPosts () {
      this.$router.push({
        path: 'users/posts',
        query: {
          userId: this.user.id
        }
      })
    }
  }
}
</script>
