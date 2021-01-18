<template>
  <form class="card card-w30" @submit.prevent="submitHandler">
    <app-select
      v-model:select="select"
      @update="selectChangeHandler"
    ></app-select>

    <app-text-area
      :label="'Значение'"
      :error="errors.areaValue"
      v-model:value="areaValue"
    ></app-text-area>

    <app-btn :color="'primary'" :disabled="isDisable">Добавить</app-btn>
  </form>

  <div class="card card-w70">
    <app-loader v-if="loader"></app-loader>
    <template v-else>
      <app-content
        v-if="firstResume"
        :contentBlock="contentBlock"
      >
      </app-content>
      <p v-if="firstResume">
        <app-btn :color="'danger'" @action="removeContent">Очистить резюме</app-btn>
      </p>
      <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </template>
  </div>
</template>

<script>
import AppBtn from './uiElements/AppBtn'
import AppTextArea from './uiElements/AppTextArea'
import AppSelect from './uiElements/AppSelect'
import AppContent from './FormElements/AppContent'
import AppLoader from './uiElements/AppLoader.vue'
import axios from 'axios'
export default {
  data () {
    return {
      select: 'title',
      areaValue: '',
      contentBlock: [],
      loader: false,
      dataLink: 'https://next-coursework-default-rtdb.firebaseio.com/content.json',
      errors: {
        errorValueNumber: 3,
        areaValue: null
      }
    }
  },
  async mounted () {
    await this.loadContent()
  },
  methods: {
    formIsValid () {
      let isValid = true
      if (this.areaValue.length <= 2) {
        this.errors.areaValue = `Значение не может быть меньше '${this.errors.errorValueNumber}' символов`
        isValid = false
      } else {
        this.errors.areaValue = null
      }
      return isValid
    },
    selectChangeHandler (selectType) {
      this.select = selectType
    },
    async submitHandler () {
      try {
        if (this.formIsValid()) {
          const response = await axios.post(this.dataLink, {
            content: this.areaValue,
            type: this.select
          })
          this.contentBlock.push({
            id: response.name,
            type: this.select,
            content: this.areaValue
          })
          this.select = 'title'
          this.areaValue = ''
          this.alertInterval()
        }
      } catch (error) {
        console.log(error)
      }
    },
    async loadContent () {
      try {
        this.loader = true
        const { data } = await axios.get(this.dataLink)
        this.contentBlock = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.loader = false
      } catch (error) {
        this.loader = false
        console.log(error.message)
      }
    },
    async removeContent () {
      const { data } = await axios.delete(this.dataLink)
      this.contentBlock.length = data
    }
  },
  computed: {
    isDisable () {
      return !this.areaValue.length
    },
    firstResume () {
      return this.contentBlock.length
    }
  },
  watch: {
    areaValue (value) {
      if (value.length >= 3) {
        this.errors.areaValue = null
      } else if (value.length === 0) {
        this.errors.areaValue = null
      }
    }
  },
  components: {
    AppBtn,
    AppTextArea,
    AppSelect,
    AppContent,
    AppLoader
  }
}
</script>

<style scoped>
.loader {
    background: black;
    background: -moz-linear-gradient(left,black 10%, rgba(255, 255, 255, 0) 42%);
    background: -webkit-linear-gradient(left, black 10%, rgba(255, 255, 255, 0) 42%);
    background: -o-linear-gradient(left, black 10%, rgba(255, 255, 255, 0) 42%);
    background: -ms-linear-gradient(left, black 10%, rgba(255, 255, 255, 0) 42%);
    background: linear-gradient(to right, black 10%, rgba(255, 255, 255, 0) 42%);
}
.loader:before {
  background: black;
}
.loader:after {
    background: #ffffff;
}
</style>
