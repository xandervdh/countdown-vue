<template>
  <div>
    <counter
    :year="2020"
    :month="11"
    :day="16"
    :hours="13"
    :minutes="30"
    :seconds="0"
    :miliseconds="0"
    :text="'Time until end of break!'"
    :done="'Get to work'"
    />
  </div>
</template>

<script>
import Counter from '@/components/Counter.vue'
export default {
  components: {
    Counter
  }
}
</script>

<style scoped>

</style>
