<template>
  <md-layout
    :md-flex        = "to.size && to.size.def ? to.size.def : 100"
    :md-flex-xsmall = "to.size && to.size.xs ? to.size.xs   : 100"
    :md-flex-small  = "to.size && to.size.sm ? to.size.sm   : 100">
    <div class="formly-list" :class="[ to.inputType, to.wrapperClasses, {'formly-has-value': model[field.key], 'formly-has-focus': form[field.key].$active, 'has-error md-input-invalid has-danger': hasError}]" :id="to.id">
      <label :class="to.labelClasses" class="list-label"> {{field.templateOptions.label}}</label>
      <br v-if="field.templateOptions.label">
      <span v-for="option in field.options"  :key="option.value || option" >
        <md-checkbox
          v-if="to.inputType == 'checkbox'"
          v-model="model[field.key]"
          :name="field.key"
          :disabled="to.disabled"
          :value="option.value || option"
          :md-value="option.value || option"
          @blur="onBlur" @focus="onFocus" @click="onClick" @change="onChange" @keyup="onKeyup" @keydown="onKeydown" v-formly-atts="to.atts" :class="to.classes">
            {{option.label || option}}
        </md-checkbox>
        <md-radio
          v-if="to.inputType == 'radio'"
          v-model="model[field.key]"
          :name="field.key"
          :disabled="to.disabled"
          :md-value="option.value || option"
          :value="option.value || option"
          :required="field.required"
          @blur="onBlur" @focus="onFocus" @click="onClick" @change="onChange" @keyup="onKeyup" @keydown="onKeydown" v-formly-atts="to.atts" :class="to.classes">
            {{option.label || option}}
        </md-radio>
      </span>
      <error-display :form="form" :field="field.key"></error-display>
    </div>
  </md-layout>
</template>

<script>
 import baseField from './baseField';
 export default {
     mixins: [baseField],
    //  created: function(){
    //      //set the default value to be an array if it's a checkbox
    //      let type = this.to.inputType;
    //      if ( (!type || type == 'checkbox') && this.model[ this.field.key ] == '') this.$set(this.model, this.field.key, []);
    //  }
 }
</script>
<style lang="sass">
  .list-label
    pointer-events: auto
    top: 0
    opacity: 1
    font-size: 12px
    color: rgba(0, 0, 0, 0.54)

  .has-error label,
  .has-error .list-label,
  .md-input-invalid label,
  .md-input-invalid .list-label,
  .has-danger label,
  .has-danger .list-label
    color: #e65100;
</style>
