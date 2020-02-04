<template>
  <b-container fluid class="px-0">
    <b-form-row>
      <b-col cols="3">
        Тип
      </b-col>
      <b-col col lg="5">
        <b-form-checkbox-group
          v-model="value"
          :options="options"
          switches
        />
      </b-col>
    </b-form-row>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      value: [],
      options: [
        { value: 'silver', text: 'Silver' },
        { value: 'gold', text: 'Gold' },
        { value: 'platinum', text: 'Platinum' }
      ]
    }
  },
  methods: {
    prepareQueryParams (params) {
      params.card_type = this.value
    }
  }
}
</script>
