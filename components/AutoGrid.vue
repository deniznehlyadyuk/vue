<template>
  <DxResponsiveBox
    v-if="itemLength > 0"
    :screen-by-width="screen"
  >
    <DxCol
      v-for="index in lgColCount"
      :key="`lg-col-${index}`"
      :ratio="1"
      screen="lg"
    />
    <DxRow
      v-for="index in Math.ceil(itemLength / lgColCount)"
      :key="`lg-row-${index}`"
      :ratio="1"
      screen="lg"
    />
    <DxCol
      v-for="index in mdColCount"
      :key="`md-col-${index}`"
      :ratio="1"
      screen="md"
    />
    <DxRow
      v-for="index in Math.ceil(itemLength / mdColCount)"
      :key="`md-row-${index}`"
      :ratio="1"
      screen="md"
    />
    <DxCol
      v-for="index in smColCount"
      :key="`sm-col-${index}`"
      :ratio="1"
      screen="sm"
    />
    <DxRow
      v-for="index in Math.ceil(itemLength / smColCount)"
      :key="`sm-row-${index}`"
      :ratio="1"
      screen="sm"
    />
    <DxCol
      v-for="index in xsColCount"
      :key="`xs-col-${index}`"
      :ratio="1"
      screen="xs"
    />
    <DxRow
      v-for="index in Math.ceil(itemLength / xsColCount)"
      :key="`xs-row-${index}`"
      :ratio="1"
      screen="xs"
    />
    <DxItem
      v-for="(item, index) in items"
      :key="item.id"
    >
      <DxLocation
        :row="calculateRow(index, lgColCount)"
        :col="calculateCol(index, lgColCount)"
        screen="lg"
      />
      <DxLocation
        :row="calculateRow(index, mdColCount)"
        :col="calculateCol(index, mdColCount)"
        screen="md"
      />
      <DxLocation
        :row="calculateRow(index, smColCount)"
        :col="calculateCol(index, smColCount)"
        screen="sm"
      />
      <DxLocation
        :row="calculateRow(index, xsColCount)"
        :col="calculateCol(index, xsColCount)"
        screen="xs"
      />
      <template #default>
        <slot
          name="card"
          :item="item"
        />
      </template>
    </DxItem>
  </DxResponsiveBox>
</template>

<script>
import {
  DxResponsiveBox,
  DxItem,
  DxCol,
  DxRow,
  DxLocation
} from 'devextreme-vue/responsive-box'

export default {
  components: {
    DxResponsiveBox,
    DxCol,
    DxRow,
    DxLocation,
    DxItem
  },
  props: {
    items: {
      type: Array,
      required: true
    },
    lgColCount: {
      type: Number,
      default: 4
    },
    mdColCount: {
      type: Number,
      default: 3
    },
    smColCount: {
      type: Number,
      default: 2
    },
    xsColCount: {
      type: Number,
      default: 1
    }
  },
  computed: {
    itemLength () {
      return this.items.length
    }
  },
  methods: {
    screen (width) {
      if (width < 640) {
        return 'xs'
      }
      if (width < 1280) {
        return 'sm'
      }
      if (width < 1920) {
        return 'md'
      }
      return 'lg'
    },
    calculateRow (index, factor) {
      return Math.floor(index / factor)
    },
    calculateCol (index, factor) {
      return index % factor
    }
  }
}
</script>
