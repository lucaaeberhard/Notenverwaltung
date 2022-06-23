<template>
  <div class="home">
    <GradeComponent></GradeComponent>
  </div>
</template>

<script>
// @ is an alias to /src
import GradeComponent from '@/components/GradeComponent.vue'

export default {
  name: 'HomeView',
  components: {
    GradeComponent
  }
}
</script>
