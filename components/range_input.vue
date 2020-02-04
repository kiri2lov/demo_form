<template>
  <b-form-row>
    <b-col>
      <b-input-group prepend="от">
        <b-form-input @input.native="$emit('update:lower', $event.target.value)" type="number" />
      </b-input-group>
    </b-col>
    <b-col>
      <b-input-group prepend="до">
        <b-form-input @input.native="$emit('update:upper', $event.target.value)" type="number" />
      </b-input-group>
    </b-col>
    <b-col>
      <b-button @click="$emit('delete')" variant="outline-info">
        <b-icon icon="trash" />
      </b-button>
    </b-col>
  </b-form-row>
</template>
