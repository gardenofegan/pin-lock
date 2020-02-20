<template>
  <div>
    <Lock :key="componentKey"/>
  </div>
</template>

<script>
import Lock from '../components/Lock'

export default {
  data: () => ({
    componentKey: 0,
  }),
  components: {
    Lock
  }
}
</script>
<style scoped lang="scss">

</style>
