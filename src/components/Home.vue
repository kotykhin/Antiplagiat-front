<template>
  <v-container class="mt-5">
    <v-row v-if="!isChecked">
      <v-col cols="6">
        <div class="text-subtitle-1 mr-2">Choose texts language:</div>
        <v-btn-toggle v-model="form.language" :disabled="isLoading">
          <v-btn :value="languages[0].value">
            <div>{{ languages[0].text }}</div>
          </v-btn>
          <v-btn :value="languages[1].value">
            <div>{{ languages[1].text }}</div>
          </v-btn>
          <v-btn :value="languages[2].value">
            <div>{{ languages[2].text }}</div>
          </v-btn>
        </v-btn-toggle>
      </v-col>
      <v-col cols="6">
        <div class="text-subtitle-1 mr-2">Choose texts topic:</div>
        <v-combobox
          solo
          v-model="form.topic"
          :items="topics"
          :disabled="isLoading"
        ></v-combobox>
      </v-col>
    </v-row>
    <TextChecker
      @checkTexts="checkTexts"
      :isLoading="isLoading"
      v-if="!isChecked"
      transition="fade-transition"
    />
    <CheckResults
      :conclusion="conclusion"
      v-else
      transition="fade-transition"
      @checkMore="reset"
    />
  </v-container>
</template>

<script>
import TextChecker from "@/components/TextChecker";
import CheckResults from "@/components/CheckResults";
export default {
  name: "Home",
  data: () => ({
    form: {
      topic: "",
      language: "eng",
    },
    topics: [
      {
        text: "topic1",
        value: "topic1",
      },
      {
        text: "topic2",
        value: "topic2",
      },
      {
        text: "topic3",
        value: "topic3",
      },
      {
        text: "topic4",
        value: "topic4",
      },
    ],
    languages: [
      {
        text: "Ukrainian",
        value: "ukr",
      },
      {
        text: "English",
        value: "eng",
      },
      {
        text: "Russian",
        value: "rus",
      },
    ],
    isLoading: false,
    isChecked: false,
    conclusion: {
      percent: 0,
      time: 0,
      conclusion: "yess",
    },
  }),
  methods: {
    checkTexts(tex1, tex2) {
      this.form.text1 = tex1;
      this.form.text2 = tex2;
      this.isLoading = true;
      console.log(this.form);
      return this.axios
        .post("/check", this.form)
        .then((response) => {
          console.log(response.data);
          //parse answer from api
          this.conclusion.percent = response.data.percent;
          this.conclusion.time = response.data.time;
          this.conclusion.conclusion = response.data.conclusion;
          this.isChecked = true;
        })
        .catch((err) => {
          console.log(err);
        })
        .finally(() => {
          this.isLoading = false;
          this.isChecked = true;
        });
    },
    initTopics() {
      this.isLoading = true;
      return this.axios
        .get("/topics/" + this.form.language)
        .then((response) => {
          console.log(response.data);
          this.topics = response.data.message;
        })
        .catch((err) => {
          console.log(err);
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
    reset() {
      this.isChecked = false;
    },
  },
  mounted() {
    this.initTopics(); //uncomment for loading topics on load
  },
  watch: {
    "form.language": {
      handler() {
        this.initTopics();
      },
      deep: true,
    },
  },

  components: {
    CheckResults,
    TextChecker,
  },
};
</script>
