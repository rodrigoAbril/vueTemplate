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
        :id="to.id ? to.id : null"
        :name="field.key"
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
        v-cleave-cc="to.format ? inputFormat : noFormat"
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
import cleave     from 'cleave.js'
import validators from '@/api/validators.js'

Vue.directive('cleaveCc',{
    componentUpdated: function (el, binding) {
        if (el.value.trim().length>0 ) {
            //el.value = moment(el.value, binding.value).format('MM/DD/YYYY');
            // var cleave = new Cleave(el, binding.oldValue)
            var cleave = new Cleave(el, binding.oldValue)
        }
    }
});
export default {
  mixins: [baseField],
  data : function(){
    return {
      maxLenght     : 100000,
      //TODO update maxLenght from field templateOptions ...
      inputFormat   : {},
      noFormat      : {
        //TODO when there is no format fort a field cleave shouldnt be used
        //HACK this mekes the cleave.js behave when the field does not have a format .... it is ugly as sin
        blocks      : [0, 10000],
        delimiter   : '',

      }
    }
  },
  created() {
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
    }
    switch(this.to.format)  {
      default : case undefined:
        break
      case 'onlyLetters'    :
        this.$options.propsData.field.validators = {
          onlyLetters       : {
            expression      : validators.onlyLetters,
            message         : 'Contiente caracteres invalidos'
          }
        }
        break
      case 'time'           :
        this.inputFormat  = {
          delimiter         : ':',
          blocks            : [2,2,2],
          numericOnly       : true,
        },
        this.$options.propsData.field.validators = {
          isTimeCorrect     : {
            expression      : validators.isTimeCorrect,
            message         : 'Debe de ser HH:MM:SS'
          }
        }
        break;
      case 'date'           :
        this.inputFormat  = {
          date              : true,
          datePattern       : ['d', 'm', 'Y'],
          delimiter         : '/'
        }
        break;
      case 'dateIsThisYear' :
        this.inputFormat      = {
          date              : true,
          datePattern       : ['d', 'm', 'Y'],
          delimiter         : '/'
        }
        this.$options.propsData.field.validators = {
          isCurrentYear     : {
            expression      : validators.isThisYear,
            message         : 'Debe ser del año en curso al dia de hoy'
          }
        }
        break;
      case 'numeric'          :
        this.inputFormat  = {
          numericOnly       : true,
          blocks            : [0,9999999999999999999999],
          delimiter         : ''
        }
        break
      case 'phone'          :
        this.inputFormat  = {
          blocks            : [8,8],
          numericOnly       : true,
          delimiter         : '-'
        },
        this.$options.propsData.field.validators = {
          length            : {
            expression      : 'model[field.key].length >= 8',
            message         : 'Debe tener al menos 8 digitos'
          }
        }
        break;
    }
  },
  methods: {
    // onChange: function(e){
    //   this.$set(this.form[this.field.key], '$dirty', true)
    //   this.runFunction('onChange', e)
    //   if ( this.to.inputType == 'file' ){
    //     this.$set(this.model, this.field.key, this.$el.querySelector('input').files)
    //   }
    // }
  }
}
</script>
