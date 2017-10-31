<template>
  <md-layout
    :class="[ to.inputType]"
    :md-flex        = "to.size && to.size.def ? to.size.def : 100"
    :md-flex-xsmall = "to.size && to.size.xs ? to.size.xs   : 100"
    :md-flex-small  = "to.size && to.size.sm ? to.size.sm   : 100">

    <md-input-container :class="[ to.inputType, to.wrapperClasses, {'formly-has-value': model[field.key], 'formly-has-focus': form[field.key].$active, 'has-error md-input-invalid has-danger': hasError}]">
      <!-- <md-icon class="warn">warn<md-tooltip>autocomplete!</md-tooltip></md-icon> -->
      <label v-if="to.label" :for="to.id ? to.id : null">{{to.label}} {{allCatalogsLoaded}}</label>
      <md-autocomplete
        v-model="model[field.key]"
        :list="autocompleteList"
        :print-attribute="to.print"
        :debounce="to.debounce ? to.debounce : 500"
        :minChars="0"
        :id="to.id ? to.id : null"
        :required="field.required"
        :disabled="to.disabled"
        :readonly="to.readonly"
        @selected="(obj, str) => sendObject(obj)"
        @blur="onBlur"
        @focus="onFocus"
        @click="onClick"
        @change="onChange"
        @keyup="onKeyup"
        @keydown="onKeydown"
        v-formly-atts="to.atts">
      </md-autocomplete>
      <error-display :form="form" :field="field.key"></error-display>
    </md-input-container>
  </md-layout>
</template>
<!-- @selected is the    -->
<script>
import baseField      from './baseField';
import { mapGetters } from 'vuex'
import { mapState }   from 'vuex'
import store          from '@/store/index.js';

// var filterList = (list, query) => {
//   var arr = [];
//   for(var i = 0; i < list.length; i++) {
//     if(list[i].name.indexOf(query) !== -1)
//       arr.push(list[i]);
//
//     if(arr.length > 5)
//       break;
//   }
//   console.log('ARR', arr)
//   return arr;
// }

export default {
  mixins: [baseField],
  data: function data() {
    return {
      autocompleteList : [{ value:'0', label:'Cargando listado'}]
    }
  },
  mounted() {
    if (this.allCatalogsLoaded) {
      this.updateCatalogs(this.field.templateOptions.catalog)
    }
  },
  methods: {
    updateCatalogs(catalog) {
      this.autocompleteList = store.getters.getCatalog(catalog)
    }
  },
  computed : {
     localComputed () { /* ... */ },
    ...mapState({
      allCatalogsLoaded: state => state.catalogsLoaded,
    }),
  },
  watch: {
    'allCatalogsLoaded': function () {
      if (this.allCatalogsLoaded) {
        this.updateCatalogs(this.field.templateOptions.catalog)
      }
    }
  }
}
</script>
