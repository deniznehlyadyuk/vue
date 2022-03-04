<template>
  <div>
    <div v-if="itemsLength > 0">
      <v-row
        v-for="(x, i) in rowCount"
        :key="i"
      >
        <v-col
          v-for="(y, j) in calculateColCount(i)"
          :key="j"
          :xl="12 / xlColCount"
          :lg="12 / lgColCount"
          :md="12 / mdColCount"
          :sm="12 / smColCount"
          :xs="12 / xsColCount"
        >
          <slot
            name="card"
            :item="items[colCount * i + y - 1]"
          />
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    items: {
      type: Array,
      required: true
    },
    xlColCount: {
      type: Number,
      default: 4
    },
    lgColCount: {
      type: Number,
      default: 3
    },
    mdColCount: {
      type: Number,
      default: 2
    },
    smColCount: {
      type: Number,
      default: 1
    },
    xsColCount: {
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      colCount: 5
    }
  },
  computed: {
    itemsLength () {
      return this.items.length
    },
    rowCount () {
      return Math.ceil(this.itemsLength / this.colCount)
    }
  },
  mounted () {
    this.setColCountValue(window.innerWidth)
    window.addEventListener('resize', () => {
      this.setColCountValue(window.innerWidth)
    })
  },
  methods: {
    setColCountValue (width) {
      if (width > 1904) {
        // xl
        this.colCount = this.xlColCount
      } else if (width > 1264) {
        // lg
        this.colCount = this.lgColCount
      } else if (width > 960) {
        // md
        this.colCount = this.mdColCount
      } else if (width > 600) {
        // sm
        this.colCount = this.smColCount
      } else {
        // xs
        this.colCount = this.xsColCount
      }
    },
    calculateColCount (rowIndex) {
      if (rowIndex * this.colCount + this.colCount > this.itemsLength) {
        return this.itemsLength % this.colCount
      }
      return this.colCount
    }
  }
}
</script>
