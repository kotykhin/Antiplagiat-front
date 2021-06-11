<template>
  <div class="mt-3 d-flex flex-column justify-center">
    <v-overlay :absolute="true" :value="isLoading">
      <v-progress-circular
        indeterminate
        color="primary"
        class="ma-1"
      ></v-progress-circular>
    </v-overlay>

    <div class="text-h6 primary--text text-center text-capitalize mb-3">
      Paste your texts
    </div>
    <v-form lazy-validation v-model="isValid">
      <v-row>
        <v-col cols="6" class="pr-1">
          <v-textarea
            rows="13"
            outlined
            name="input-7-4"
            label="Paste your text"
            key="text1"
            v-model="text1"
            :rules="[
              (v) =>
                (v || ' ').split(' ').length - 1 <= maxLength ||
                'Text must be 10000 words or less',
            ]"
          ></v-textarea>
        </v-col>
        <v-col cols="6" class="pl-1">
          <v-textarea
            rows="13"
            outlined
            name="input-7-4"
            label="Paste your text"
            key="text2"
            v-model="text2"
            :rules="[
              (v) =>
                (v || ' ').split(' ').length - 1 <= maxLength ||
                'Text must be 10000 words or less',
            ]"
          ></v-textarea>
        </v-col>
      </v-row>
      <div class="text-body-1" v-if="error">{{ error }}</div>
      <v-btn
        color="primary"
        large
        class="body-1 address-action my-3 rounded-b-1"
        :disabled="!isValid || !(this.text1 && this.text2)"
        @click="checkTexts"
      >
        FIRE
      </v-btn>
    </v-form>
  </div>
</template>

<script>
export default {
  name: "TextChecker",
  props: {
    isLoading: {},
  },
  data() {
    return {
      text1: "",
      text2: "",
      error: "",
      maxLength: 10000,
      isValid: true,
    };
  },
  methods: {
    checkTexts() {
      if (this.text1 && this.text2) {
        this.$emit("checkTexts", this.text1, this.text2);
      } else {
        this.error = "Please type some text";
      }
    },
    checkTextLength(text) {
      const spaceCount = text.split(" ").length - 1;
      return spaceCount > this.maxLength;
    },
  },
};
</script>

<style scoped>
</style>