<template>
  <md-layout
    :md-flex        = "to.size && to.size.def ? to.size.def : 100"
    :md-flex-xsmall = "to.size && to.size.xs ? to.size.xs   : 100"
    :md-flex-small  = "to.size && to.size.sm ? to.size.sm   : 100">
    <md-input-container :class="[ to.inputType, to.wrapperClasses, {'formly-has-value': model[field.key], 'formly-has-focus': form[field.key].$active, 'has-error md-input-invalid has-danger': hasError}]">
      <label
        v-if="to.label" :for="to.id ? to.id : null"> {{to.label}} </label>
      <md-select
        :class="to.classes"
        :id="to.id ? to.id : null"
        :required="field.required"
        v-model="model[field.key]"
        :name="field.key"
        :disabled="to.disabled"
        v-formly-atts="to.atts"
        @selected="(obj, str) => sendObject(obj)"
        @blur="onBlur"
        @focus="onFocus"
        @click="onClick"
        @change="onChange"
        @keyup="onKeyup"
        @keydown="onKeydown">
        <md-option
          v-for="option in field.options"
          :key="option.value"
          :value="option.value || option">
            <span class="optionId">{{option.label}}</span>
        </md-option>
      </md-select>
    </md-input-container>
  </md-layout>
</template>

<script>
 import baseField from './baseField';
 export default {
    mixins: [baseField],
    data: function data() {
      return {}
    },
    created() {
      // console.log('THIS.MODEL[THIS.FIELD]', this.field, null, 2)

    },
    methods: {
      
    }
 }

</script>
