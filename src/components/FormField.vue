<template lang="html">
  <div class="form-item">

     <label v-text="label" :for="name"></label>
     <input
       v-validate="fieldValidation"
       class="input"
       :name="name"
       :ref="name"
       :autocomplete="autocomplete"
       :pattern="pattern"
       :id="fieldId"
       :type="type"
       :placeholder="fieldPlaceholder"
       :value="value"
       :data-vv-as="label"
       :data-vv-name="name"
       :data-vv-delay="300"
       :class="{
         hasError: errors.has(name),
         validField: fieldValidity }"
       :role="fieldRole"
       :aria-describedby="fieldId + '_help'"
       @input="onInput($event.target.value)"
       @blur="onBlur($event)"
       @focus="setFocus"
      >

     <transition name="fade">
       <div
         :id="fieldId + '_help'"
         v-if="showHelp"
         class="help hasError"
         v-text="errors.first(name)">
       </div>
     </transition>

   </div>
</template>

<script>

export default {
  name: 'form-field',
  props: {

    label: {
      type: String,
      default: 'Form Field'
    },

    name: {
      type: String,
      default: 'formField',
    },

    id: String,

    value: {
      type: String,
      required: true
    },

    role: String,

    placeholder: String,

    type: {
      type: String,
      default: 'text'
    },

    validation: {
      type: [String, Object]
    },

    optional: {
      type: Boolean,
      default: false
    },

    autocomplete: {
      type: String,
    },

    pattern: {
      type: String,
    }
  },

  data () {
    return {
      hasFocus: false,
      fieldPlaceholder: this.placeholder ? this.placeholder : false,
      fieldRole: this.role ? this.role : false,
      helpStyles: {
        color: '#ca0000'
      }
    }
  },
  computed: {
    fieldId () {
      return this.id ? this.id : this.name
    },

    fieldValidation () {
      return this.optional ? { rules: { required: false} } : this.validation || 'required'
    },

    fieldValidity () {
      return this.fields[this.name] == undefined ? false : this.checkFieldValidity(this.fields[this.name])
    },

    showHelp () {
      return this.errors.has(this.name)
    }
  },

  mounted () {
    let vm = this
    // let Refs = vm.$refs

    this.$bus.$on('set-focus', name => {
      if (name == this.name) {
        this.$refs[name].focus()
      }
      // if (vm.$refs[name]) {
      //   setTimeout(() => {
      //     console.log('INSIDE TIMEOUT');
      //     this.$refs[name].focus()
      //   }, 500);
      //
      // }
    })
  },

  methods: {
    checkFieldValidity (field) {
      return field.dirty && field.valid && field.validated  ? true : false
    },

    setFocus () {
      this.hasFocus = true
    },

    onInput (val) {
      this.$emit('input', val)
    },

    onBlur (blurEvent) {
      this.hasFocus = false
    }
  },
  inject: ['$validator']
}
</script>
