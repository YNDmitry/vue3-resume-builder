<template>
  <div class="container column">
    <app-form-control></app-form-control>
  </div>

  <div class="container">
    <app-comments></app-comments>
  </div>
</template>

<script>
import AppFormControl from './components/AppFormControl'
import AppComments from './components/AppComments'
export default {
  components: {
    AppFormControl,
    AppComments
  }
}
</script>
