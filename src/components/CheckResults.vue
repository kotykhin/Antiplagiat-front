<template>
  <div class="my-4 ma-0 text-md-center">
    <div class="text-h4 primary--text darken-2">Uniqueness</div>
    <div class="body-1 my-4">
      <strong>Percent: </strong> {{ conclusion.percent }} %
    </div>
    <div class="body-1 my-4">
      <strong>Time spent: </strong> {{ conclusion.time }} sec
    </div>
    <div class="body-1 my-4">
      <strong>Conclusion: </strong> {{ conclusion.conclusion }}
    </div>

    <v-btn @click="$emit('checkMore')">Check more</v-btn>
  </div>
</template>

<script>
export default {
  name: "CheckResults",
  props: {
    conclusion: {},
  },
};
</script>

<style scoped>
</style>