<template lang="html">
  <div class="example stepForm">
    <div class="stepsWrapper">

      <transition name="slide-fade" mode="out-in" appear>
        <form-step v-if="currentStep == 1" :key="1">
         <form-select
           name="degreeLevel"
           optional v-model="selectedDegreeLevel"
           label="Select Degree Level"
           @option-selected="$bus.$emit('set-focus', 'program')"
           :options="levels">
         </form-select>

          <form-select
            name="program"
            ref="progselect"
            optional v-model="submit.program"
            v-show="selectedDegreeLevel"
            transition="vertical-slide"
            :label="selectedDegreeLevel + ' Programs'"
            defaultText="Select a Program"
            :options="programsForSelectedDegreeLevel">
          </form-select>
        </form-step>

        <form-step v-if="currentStep == 2" :key="2">
         <form-first-name ref="fn" optional v-model="submit.firstName"></form-first-name>
         <form-last-name optional v-model="submit.lastName"></form-last-name>
        </form-step>

        <form-step v-if="currentStep == 3" :key="3">
          <form-phone optional v-model="submit.phone" validation="required"></form-phone>
          <form-email v-model="submit.email" validation="required|email"></form-email>
          <form-zip placeholder="Your Zip" v-model="submit.zip"></form-zip>
        </form-step>
      </transition>
    </div>

    <step-form-controls
      :steps="totalSteps"
      :active-step="currentStep"
      @previous-step="handlePreviousStep"
      @next-step="handleNextStep"
      submitBtnText="Submit Now">
    </step-form-controls>

    <form-legal-text></form-legal-text>
  </div>
</template>

<script>
export default {
  methods: {

  }

}
</script>
