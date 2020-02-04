<template>
  <b-container class="mt-4">
    <h1>Поиск респондентов</h1>
    <b-form @submit.prevent="onSubmit" class="mt-4">
      <b-container v-for="(c, index) in conditions" :key="c.key" fluid class="condition">
        <b-form-row>
          <b-col cols="3">
            Условие {{ index + 1 }}
          </b-col>
          <b-col>
            <b-input-group>
              <b-form-select v-model="conditions[index].type" @change="onChangeConditionType" :options="conditionTypes" />
              <b-input-group-append>
                <b-button @click="onRemoveCondition(index)" variant="success">
                  Удалить условие
                </b-button>
              </b-input-group-append>
            </b-input-group>
          </b-col>
          </b-col>
        </b-form-row>
        <component ref="conditions" :is="conditions[index].type" class="mt-3" />
      </b-container>
      <b-container v-if="!allConditionsUsed" @click="onAddCondition" fluid class="add-condition-btn mb-4">
        <b-icon icon="plus" font-scale="3" />
        <div>
          Нажмите, чтобы добавить новое условие выборки.<br>
          Все условия связываются между собой логическим "И"
        </div>
      </b-container>
      <b-alert v-model="showQueryParams" variant="success" dismissible>
        Query params: {{ queryParams }}
      </b-alert>
      <b-button type="submit" variant="primary">
        Отправить</b-botton>
      </b-button>
    </b-form>
  </b-container>
</template>

<script>
import AgeCondition from '~/components/age_condition'
import CardTypeCondition from '~/components/card_type_condition'
import CardStatusCondition from '~/components/card_status_condition'

export default {
  components: {
    AgeCondition, CardTypeCondition, CardStatusCondition // eslint-disable-line vue/no-unused-components
  },
  data () {
    return {
      conditions: [],
      allConditionsUsed: false,
      conditionKeys: 0,
      queryParams: null,
      showQueryParams: false,
      conditionTypes: [
        { value: null, text: 'Выберите условие', disabled: true },
        { value: 'AgeCondition', text: 'Возраст респондента', disabled: false },
        { value: 'CardTypeCondition', text: 'Тип карты лояльности', disabled: false },
        { value: 'CardStatusCondition', text: 'Статус карты лояльности', disabled: false }
      ]
    }
  },
  methods: {
    onSubmit (evt) {
      const params = {}
      this.$refs.conditions && this.$refs.conditions.forEach((x, i) => {
        x.prepareQueryParams(params)
      })
      this.queryParams = JSON.stringify(params)
      this.showQueryParams = true
    },

    onAddCondition () {
      this.conditions.push({ type: null, key: ++this.conditionKeys })
      this.allConditionsUsed = this.conditions.length === this.conditionTypes.length - 1
    },

    onRemoveCondition (index) {
      this.$delete(this.conditions, index)
      this.freshConditionTypes()
      this.allConditionsUsed = false
    },

    onChangeConditionType (selectedType) {
      this.freshConditionTypes()
    },

    freshConditionTypes () {
      this.conditionTypes.forEach((x, i) => {
        if (x.value !== null) {
          x.disabled = this.conditions.some(c => c.type === x.value)
        }
      })
    }
  }
}
</script>

<style>
.add-condition-btn {
  cursor: pointer;
  border-radius: 6px;
  border: 1px solid gray;
  padding: 0.8em 2em 1.2em;
  text-align: center;
  color: green;
  user-select: none;
}

.condition {
  border: 1px solid gray;
  padding: 1.4em;
  border-radius: 6px;
  margin-bottom: 1.5em;
}
</style>
