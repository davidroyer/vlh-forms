<template>
    <p class="legal-text" :style="legalStyles">By requesting information, I am providing express consent for {{schoolName}} to contact me by email, phone and text, including my wireless phone number, regarding degree and enrollment information using automated technology. Standard message and data rates may apply to text messages. There is no obligation to enroll.</p>
  </div>
</template>

<script>
export default {
  name: 'form-legal-text',
  props: {
    school: {
      type: String,
      default: 'UPDATE ME IN WORDPRESS VIA FORM SETTINGS PAGE'
    },
    textColor: {
      type: String,
      default: '#222'
    },
    fontSize: {
      type: String,
      default: '12px'
    },
    padding: {
      type: String,
      default: '0px'
    },
    disableStyles: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    schoolName () {
      return this.$parent.schoolDisplayName || this.school
    }
  },

  data () {
    return {
      defaultStyles: {
        color: this.textColor,
        fontSize: this.fontSize,
        padding: this.padding
      },
      legalStyles: this.disableStyles ? {} : {
        color: this.textColor,
        fontSize: this.fontSize,
        padding: this.padding
      }
    }
  }
}
</script>

<style lang="scss">
  .legal-text {
    color: inherit;
    .light & {
      color: #f9f9f9 !important;
    }

    .dark & {
      color: #222 !important;
    }
  }
</style>
