<template>
  <md-layout
    :class="[ to.inputType]"
    :md-flex        = "to.size && to.size.def ? to.size.def : 100"
    :md-flex-xsmall = "to.size && to.size.xs ? to.size.xs   : 100"
    :md-flex-small  = "to.size && to.size.sm ? to.size.sm   : 100">

    <md-input-container :class="[ to.inputType, to.wrapperClasses, {'formly-has-value': model[field.key], 'formly-has-focus': form[field.key].$active, 'has-error md-input-invalid has-danger': hasError}]" :md-has-password="to.inputType=='password'">
      <md-icon v-if="to.inputIcon">{{to.inputIcon}}</md-icon>
      <label v-if="to.label" :for="to.id ? to.id : null">{{to.label}}</label>

      <md-input
        v-if="to.inputType != 'file'"
        v-model="model[field.key]"
        :name="field.key"
        :id="to.id ? to.id : null"
        :required="field.required"
        :disabled="to.disabled"
        :readonly="to.readonly"
        :type="to.inputType"
        @blur="onBlur"
        @focus="onFocus"
        @click="onClick"
        @change="onChange"
        @keyup="onKeyup"
        @keydown="onKeydown"
        v-formly-atts="to.atts">
      </md-input>

      <md-file
        v-if="to.inputType == 'file'"
        v-model="model[field.key]"
        :id="to.id ? to.id : null"
        :required="field.required"
        :disabled="to.disabled"
        :readonly="to.readonly"
        :type="to.inputType"
        :name="field.key"
        @blur="onBlur"
        @focus="onFocus"
        @click="onClick"
        @change="onChange"
        @keyup="onKeyup"
        @keydown="onKeydown"
        v-formly-atts="to.atts" >
      </md-file>
      <error-display :form="form" :field="field.key"></error-display>
    </md-input-container>
  </md-layout>
</template>

<script>
import Vue        from 'vue'
import baseField  from './baseField';
import validators from '@/api/validators.js'

export default {
  mixins: [baseField],
  data : function(){
    return {}
  },
  mounted() {
    // console.log(this.$options.propsData.field.validators)
    switch(this.to.validity){
      default : case undefined:
        break
      case 'email'          :
        this.$options.propsData.field.validators = {
          email             : {
            expression      : validators.onlyEmail,
            message         : 'No es un email valido.'
          }
        }
        break;
      case 'onlyLetters'    :
        this.$options.propsData.field.validators = {
          onlyLetters       : {
            expression      : validators.onlyLetters,
            message         : 'contiente caracteres invalidos'
          }
        }
        break;
    }
  },
  methods: {}
}
</script>
