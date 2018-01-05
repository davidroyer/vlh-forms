<template>
  <transition :name="transition">
    <div class="form-item">
      <label :for="name">{{label}}</label>
      <select
        v-model="selectedOption"
        @change="onChange($event.target.value)"
        class="select"
        :class="{hasError: errors.has(name), 'validField': fieldValidity }"
        v-validate="fieldValidation"
        :name="name"
        :ref="name"
        :id="fieldId"
        :aria-describedby="fieldId + '_help'"
        :data-vv-as="label"
        :placeholder="fieldPlaceholder"
      >
        <option key="initial" value="" v-text="defaultText"></option>
        <option :key="option.id" v-for="option in options" :value="option.id">{{ option.name }}</option>
      </select>

      <transition name="fade">
        <div
          :id="name + '_help'"
          v-if="showHelp"
          class="help hasError"
          v-text="errors.first(name)">
        </div>
      </transition>

    </div>
  </transition>
</template>

<script>

export default {
  name: 'form-select',
  props: {
    label: {
      type: String,
      required: true
    },
    name: {
      type: String,
      required: true,
    },
    id: String,
    value: '',
    options: {
      type: Array,
      required: true
    },
    defaultText: {
      type: String,
      default: 'Select'
    },
    placeholder: String,
    validation: {
      type: [String, Object],
    },
    optional: {
      type: Boolean,
      default: false
    },
    focusOnEnter: {
      type: Boolean,
      default: false
    },
    transition: {
      type: String,
      default: 'none'
    }
  },
  data () {
    return {
      hasFocus: false,
      selectedOption: '',
      fieldPlaceholder: this.placeholder ? this.placeholder : '',
      helpStyles: {
        color: '#ca0000'
      }
    }
  },
  inject: ['$validator'],

  mounted () {
    this.selectedOption = this.value
    // this.handleFocusOnEnter()
    // this.focusListener()
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

  methods: {
    checkFieldValidity (field) {
      return field.dirty && field.valid && field.validated  ? true : false
    },

    focusListener () {
      let vm = this
      this.$bus.$on('set-focus', name => {
        if (vm.$refs[name]) {
          setTimeout(() => {
            console.log('INSIDE TIMEOUT');
            this.$refs[name].focus()
          }, 500);
        }
      })
      // this.$bus.$on('set-focus', name => {
      //   this.$nextTick(function() {
      //     this.setFocus(name)
      //   })
      // })
    },

    setFocus(name) {
      this.$refs[name] ? this.$refs[name].focus() : ''
    },

    handleFocusOnEnter () {
      console.log('handleFocus ran!');
      if (this.focusOnEnter && this.selectedOption != '') {
        this.$refs[this.name].focus()
      }
    },

    onChange(selectedValue) {
    	this.$emit('input', selectedValue);
      selectedValue !== '' ? this.$emit('option-selected') : ''
    }
  },

  watch: {
    value: function (newValue) {
      this.selectedOption = newValue
    }
  }
}
</script>

<style lang="css">
label {
  display: block;
}
</style>
