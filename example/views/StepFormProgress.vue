<template lang="html">
  <form id="tlh-form" class="example basic" @submit.prevent="handleFormSubmission">
    <div class="stepsWrapper">
      <div class="stepFormProgress">
        <template v-for="(step, index) in totalSteps">
          <div class="stepProgress" :key="index + 1" :class="{ currentStep: currentStep == index + 1, completedStep: currentStep > index + 1 }">Step {{index + 1}}
          </div>
        </template>
      </div>

      <transition name="slide-fade" mode="out-in" appear>
        <form-step v-if="currentStep == 1" :key="1">
          <form-select
            name="degreeLevel"
            v-model="selectedDegreeLevel"
            label="Select Degree Level"
            @option-selected="$bus.$emit('set-focus', 'program')"
            :options="levels">
          </form-select>

         <form-select
           name="program"
           v-model="submit.program"
           v-show="selectedDegreeLevel"
           transition="vertical-slide"
           :label="selectedDegreeLevel + ' Programs'"
           defaultText="Select a Program"
           :options="programsForSelectedDegreeLevel">
         </form-select>
        </form-step>

        <form-step v-if="currentStep == 2" :key="2">
          <form-first-name v-model="submit.firstName"></form-first-name>
          <form-last-name v-model="submit.lastName"></form-last-name>
        </form-step>

        <form-step v-if="currentStep == 3" :key="3">
          <form-phone v-model="submit.phone" validation="required"></form-phone>
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
  </form>
</template>

<script>
export default {}
</script>

<style lang="scss">
  // .step {
  //   min-height: 100px;
  // }
  // .stepFormProgress {
  //   display: flex;
  //   justify-content: space-around;
  //   flex-flow: row wrap;
  //   margin: 1em;
  //   .stepProgress {
  //     padding: .5em;
  //     transition: .25s ease;
  //     position: relative;
  //     border-bottom: 3px solid transparent;
  //
  //     &.currentStep {
  //       background-color: #565656;
  //       color: white;
  //       border-bottom: 3px solid #8d8d8d;
  //     }
  //
  //     &.completedStep {
  //
  //       &:after {
  //         content: "\2713";
  //         font-size: 1.5em;
  //         font-weight: 500;
  //         position: absolute;
  //         left: 100%;
  //         bottom: 2px;
  //         transition: .4s ease;
  //       }
  //     }
  //
  //   }
  // }
</style>
