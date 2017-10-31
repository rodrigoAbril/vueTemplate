<template>
  <md-layout
    :md-flex        = "to.size && to.size.def ? to.size.def : 100"
    :md-flex-xsmall = "to.size && to.size.xs ? to.size.xs   : 100"
    :md-flex-small  = "to.size && to.size.sm ? to.size.sm   : 100">
    <h1 :class="to.class ? to.class : 'md-title'" v-model="model[field.key]">{{to.label}}</h1>
  </md-layout>

</template>
<script>
import baseField from './baseField';
export default {
  mixins: [baseField],
}
</script>
<style lang="sass" scoped>
</style>
