<template>
  <component
    v-for="item in contentBlock"
    :key="item"
    :is="prefix + item.type"
    :value="item.content"
  ><slot/></component>

</template>

<script>
import AppContentTitle from '../FormElements/AppContentTitle'
import AppContentSubtitle from '../FormElements/AppContentSubtitle'
import AppContentAvatar from '../FormElements/AppContentAvatar'
import AppContentText from '../FormElements/AppContentText'
export default {
  props: ['contentBlock'],
  data () {
    return {
      prefix: 'app-content-'
    }
  },
  components: {
    AppContentTitle,
    AppContentSubtitle,
    AppContentAvatar,
    AppContentText
  }
}
</script>
