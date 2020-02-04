<template>
  <b-container fluid class="px-0">
    <b-form-row>
      <b-col cols="3">
        Статус
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
        { value: 'is_active', text: 'Активна' },
        { value: 'not_active', text: 'Не активна' }
      ]
    }
  },
  methods: {
    prepareQueryParams (params) {
      params.card_status = this.value
    }
  }
}
</script>
