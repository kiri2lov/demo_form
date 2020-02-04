<template>
  <b-container fluid class="px-0">
    <b-form-row>
      <b-col cols="3">
        Диапазон
      </b-col>
      <b-col col lg="5">
        <RangeInput
          v-for="(range, index) in ranges"
          v-bind.sync="range.value"
          :key="range.key"
          @delete="onDeleteRange(index)"
          class="mb-3"
        />
        <b-form-row>
          <b-col>
            <b-button @click="onAddRange" variant="outline-info">
              <b-icon icon="plus" /> Добавить диапазон
            </b-button>
          </b-col>
        </b-form-row>
      </b-col>
    </b-form-row>
  </b-container>
</template>

<script>
import RangeInput from '~/components/range_input'

export default {
  components: { RangeInput },
  data () {
    return {
      rangeKeys: 0,
      ranges: []
    }
  },
  created () {
    this.onAddRange()
  },
  methods: {
    onDeleteRange (index) {
      if (this.ranges.length > 1) {
        this.$delete(this.ranges, index)
      }
    },
    onAddRange () {
      this.ranges.push({ key: ++this.rangeKeys, value: { lower: null, upper: null } })
    },
    prepareQueryParams (params) {
      params.age_range = this.ranges.map(x => x.value).filter(x => x.lower || x.upper)
    }
  }
}
</script>

<style scoped>
</style>
