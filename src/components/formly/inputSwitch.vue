<template>
  <md-layout
    :md-flex        = "to.size && to.size.def ? to.size.def : 100"
    :md-flex-xsmall = "to.size && to.size.xs ? to.size.xs   : 100"
    :md-flex-small  = "to.size && to.size.sm ? to.size.sm   : 100">
    <md-switch
      v-model="model[field.key]"
      :name="field.key.toString()"
      :disabled="to.disabled"
      :readonly="to.readonly"
      :id="to.id ? to.id : field.key.toString()"
      :class="[ to.inputType, to.wrapperClasses, {'formly-has-value': model[field.key], 'formly-has-focus': form[field.key].$active, 'has-error md-input-invalid has-danger': hasError}]"
      @blur="onBlur"
      @focus="onFocus"
      @click="onClick"
      @change="onChange"
      @keyup="onKeyup"
      @keydown="onKeydown"
      v-formly-atts="to.atts">
      {{to.label}} <span v-if="field.required">*</span>
    </md-switch>
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
