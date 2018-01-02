<template>
  <div id="app">

    <div class="header">
      <h1>Examples</h1>
      <nav class="navigation">
        <router-link to="/">Home</router-link>
        <router-link to="/basic">Basic</router-link>
        <router-link to="/degree-filtering">Degree Filtering</router-link>
        <router-link to="/optional-field">Optional Form Field</router-link>
        <router-link to="/step-form">Step Form</router-link>
        <router-link to="/step-form-progress">Step Form - Progress</router-link>
        <router-link to="/step-form-stepper">Step Form - Stepper</router-link>
        <router-link to="/custom-select-text">Custom Select Text</router-link>
      </nav>
    </div>
    <h2>{{step}}</h2>
    <button @click="$Fuex.handlePreviousStep($Fuex.state.currentStep)">Previous</button>
    <button @click="$Fuex.handleNextStep($Fuex.state.currentStep)">Next</button>
    <transition name="slide-fade" mode="out-in" appear>
      <h2 key="chooseExample" v-if="$route.path == '/'" class="currentExample">Choose an Example</h2>
      <h2 v-else :key="$route.fullPath" class="currentExample">
        {{$route.name}} Example
      </h2>
    </transition>

    <transition mode="out-in" name="slide-fade" appear>
      <router-view :key="$route.fullPath"></router-view>
    </transition>

  </div>
</template>

<script>
import axios from 'axios'
// data () {
//   return {
//     formState: Fuex.state
//   }
// },
export default {
  mounted () {
    this.registerZipValidator()
  },
  computed: {
    formState () {
      return this.$Fuex.state
    },
    step () {
      return this.$Fuex.state.currentStep
    }
  },
  methods: {
    registerZipValidator () {
      var vm = this
      var isZip = (value) => {
        return axios.get(`https://api.zippopotam.us/us/${value}`)
          .then(function(response) {
            let info = response.data.places[0]
            vm.submit.city = info['place name']
            vm.submit.state = info['state']
            return {
              valid: true
            }
          })
          .catch(function(error) {
            return {
              valid: false,
              data: {
                message: `${value} is not valid zip.`
              }
            }
          });
      }
      this.$validator.extend('validZip', {
        validate: isZip,
        getMessage: (field, params, data) => {
          return data.message;
        }
      });
    }
  }
}
</script>

<style lang="scss">
@import './example.scss';

.stepsWrapper {
  position: relative;
}
.list-complete-leave-active {
  position: absolute;
}

/* NOTE: StepForm */
.stepFormWrapper {
    display: flex;
    flex-flow: row wrap;
}
.stepFormControls {
    flex-basis: 100%;
    display: flex;
    flex-flow: row wrap;
}
// .form-transtiion-group {
//   // display: flex;
// }
// .form-item {
//   // transition: all .52s;
//   transition: all .4s;
//   // display: inline-block;
//   margin-right: 10px;
//   width: 100%;
//   top: 0;
// }
.test-transition-enter, .test-transition-leave-to
/* .test-transition-leave-active below version 2.1.8 */ {
  opacity: 0;

  transform: translateY(-10px);
  // transform: translateY(-10px);

}
.test-transition-leave-active {
  position: absolute;
  // transition: all .2s;

}
.testTranstitionWrapper {
  position: relative;
}
.test-transition-move {
  // transition: all 1s;
}

.header {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;
  width: 100%;
  align-items: center;
  padding: 1em;
}
.navigation {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;

  a {
  padding: .5em 1.25em;
  margin: .5em;
  border-radius: 3px;
  transition: all .35s ease;
  color: #202020;
  font-weight: 500;

  &.router-link-exact-active.router-link-active {
    background: #202020;
    color: white;
  }
}


}

</style>
