<template>
  <md-layout
    :md-flex        = "to.size && to.size.def ? to.size.def : 100"
    :md-flex-xsmall = "to.size && to.size.xs ? to.size.xs   : 100"
    :md-flex-small  = "to.size && to.size.sm ? to.size.sm   : 100">

    <md-boards
      :md-controls="true"
      :id="to.id"
      :class="[to.wrapperClasses, {'has-error has-danger': hasError}]" class="md-transparent">
      <md-board id="slide1"
        v-for="option in field.options"
        v-model="model[field.key]"   :key="model[field.key]"
        @blur="onBlur"
        @focus="onFocus"
        @click="onClick"
        @change="onChange"
        @keyup="onKeyup"
        @keydown="onKeydown"
        v-formly-atts="to.atts"
        :class="to.classes">
        <img src="field.options.src" alt="field.options.alt">
      </md-board>
    </md-boards>
    <error-display :form="form" :field="field.key"></error-display>
  </md-layout>
</template>

<script>
 import baseField from './baseField';
 export default {
     mixins: [baseField]
 }
</script>
