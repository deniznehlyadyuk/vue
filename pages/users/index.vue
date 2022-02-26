<template>
  <div>
    <DxResponsiveBox
      v-if="userLength > 0"
      :screen-by-width="screen"
    >
      <DxCol
        v-for="index in 3"
        :key="`lg-col-${index}`"
        :ratio="1"
        screen="lg"
      />
      <DxRow
        v-for="index in Math.ceil(userLength / 3)"
        :key="`lg-row-${index}`"
        :ratio="1"
        screen="lg"
      />
      <DxCol
        v-for="index in 2"
        :key="`md-col-${index}`"
        :ratio="1"
        screen="md"
      />
      <DxRow
        v-for="index in Math.ceil(userLength / 2)"
        :key="`md-row-${index}`"
        :ratio="1"
        screen="md"
      />
      <DxCol
        v-for="index in 1"
        :key="`sm-col-${index}`"
        :ratio="1"
        screen="sm"
      />
      <DxRow
        v-for="index in Math.ceil(userLength / 1)"
        :key="`sm-row-${index}`"
        :ratio="1"
        screen="sm"
      />
      <DxItem
        v-for="(user, index) in users"
        :key="user.id"
      >
        <DxLocation
          :row="calculateRow(index, 3)"
          :col="calculateCol(index, 3)"
          screen="lg"
        />
        <DxLocation
          :row="calculateRow(index, 2)"
          :col="calculateCol(index, 2)"
          screen="md"
        />
        <DxLocation
          :row="calculateRow(index, 1)"
          :col="calculateCol(index, 1)"
          screen="sm"
        />
        <template #default>
          <Card
            :user="user"
          />
        </template>
      </DxItem>
    </DxResponsiveBox>
  </div>
</template>

<script>
import {
  DxCol,
  DxLocation,
  DxResponsiveBox,
  DxRow,
  DxItem
} from 'devextreme-vue/responsive-box'
import Card from '../../components/User/Card'

export default {
  components: {
    DxResponsiveBox,
    DxCol,
    DxRow,
    DxLocation,
    DxItem,
    Card
  },
  data () {
    return {
      users: []
    }
  },
  created () {
    this.$axios.get('users')
      .then((response) => {
        this.users = response.data
      })
  },
  computed: {
    userLength () {
      return this.users.length
    }
  },
  methods: {
    screen (width) {
      if (width < 992) {
        return 'sm'
      }
      if (width < 1200) {
        return 'md'
      }
      return 'lg'
    },
    calculateRow (index, factor) {
      const value = Math.floor(index / factor)
      console.log(value, factor)
      return value
    },
    calculateCol (index, factor) {
      const value = index % factor
      console.log(value, factor)
      console.log('---')
      return value
    }
  }
}
</script>
