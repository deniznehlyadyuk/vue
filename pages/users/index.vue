<template>
  <div>
    <AutoGrid
      v-if="userLength > 0"
      :items="users"
    >
      <template #card="{ item }">
        <Card
          :user="item"
        />
      </template>
    </AutoGrid>
  </div>
</template>

<script>
import Card from '../../components/User/Card'
import AutoGrid from '../../components/AutoGrid'

export default {
  components: {
    AutoGrid,
    Card
  },
  data () {
    return {
      users: []
    }
  },
  computed: {
    userLength () {
      return this.users.length
    }
  },
  created () {
    this.$axios.get('users')
      .then((response) => {
        this.users = response.data
      })
  }
}
</script>
