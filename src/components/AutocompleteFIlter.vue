<script>
import {defineComponent} from 'vue'

export default defineComponent({
  name: "AutocompleteFIlter",
  data: () => ({
    value: null,

  }),
  methods: {
    saveFilter() {
      if (!this.value) return
      this.$emit('saveFilter', this.value)
    },
    clearFilter() {
      this.$emit('saveFilter', '')
    }

  }
})
</script>

<template>
  <div>
    <v-autocomplete
        v-model="value"
        label="Autocomplete"
        multiple="true"
        clearable="true"
        :items="['CAPSTONE', 'Wise One Looks Ahead (NROL-162)', 'There and Back Again', 'Starlink Group 4-12', 'STP-27AD2', 'Wyoming']"
        variant="outlined"
        @click:clear="clearFilter"
    />
    <v-btn @click="saveFilter">
      Применить
    </v-btn>
  </div>
</template>

<style scoped lang="scss">

</style>