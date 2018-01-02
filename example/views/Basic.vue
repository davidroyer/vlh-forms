<template lang="html">
  <form id="tlh-form" class="example basic" @submit.prevent="handleFormSubmission">
    <form-select
      @option-selected="$bus.$emit('set-focus', 'program')"
      key="1" name="degreeLevel" label="Select Degree Level"
      v-model="selectedDegreeLevel" :options="levels">
    </form-select>

    <form-select
      transition="vertical-slide" name="program" label="Select a Program" v-if="selectedDegreeLevel" v-model="submit.program" :options="programsForSelectedDegreeLevel"></form-select>

    <form-first-name v-model="submit.firstName"></form-first-name>
    <form-last-name v-model="submit.lastName"></form-last-name>
    <form-zip v-model="submit.zip"></form-zip>
    <form-phone v-model="submit.phone" validation="required"></form-phone>
    <form-email v-model="submit.email" validation="required|email"></form-email>
    <form-submit-button text="Get Info"></form-submit-button>
    <form-legal-text school="Alvernia University" fontSize="1.1em" textColor="blue"></form-legal-text>
  </form>
</template>

<script>
  export default {
    created () {

    },
    mounted() {
      this.$nextTick(function () {
        this.addClass()
      })
    },
    methods: {
      addClass () {
        let Form = document.getElementById('tlh-form')
        if (Form && this.helpTextColor ) {
          Form.classList.add(this.helpTextColor);
        }
      }
    }
  }
</script>
