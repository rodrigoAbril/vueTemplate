<template>
  <md-layout
    :md-flex        = "to.size && to.size.def ? to.size.def : 100"
    :md-flex-xsmall = "to.size && to.size.xs ? to.size.xs   : 100"
    :md-flex-small  = "to.size && to.size.sm ? to.size.sm   : 100">
    <md-input-container class="formly-textarea" :class="[ to.inputType, to.wrapperClasses, {'formly-has-value': model[field.key], 'formly-has-focus': form[field.key].$active, 'has-error md-input-invalid has-danger': hasError}]">
      <label v-if="to.label" :for="to.id ? to.id : null">{{to.label}}</label>
      <md-textarea
        :class="to.classes"
        :id="to.id ? to.id : null"
        :required="field.required"
        :disabled="to.disabled"
        v-model="model[field.key]"
        @blur="onBlur"
        @focus="onFocus"
        @click="onClick"
        @change="onChange"
        @keyup="onKeyup"
        @keydown="onKeydown"
        v-formly-atts="to.atts"></md-textarea>
      <error-display :form="form" :field="field.key"></error-display>
    </md-input-container>
  </md-layout>
</template>

<script>
 import baseField from './baseField';
 export default {
     mixins: [baseField]
 }
</script>
