<template>
  <InferenceWidget
    v-model="sourceText"
    title="Translation"
    subtitle="t5-base (English to German)"
    button-action="Translate"
    :is-loading="isLoading"
    @submit="onTranslate"
  >
    <div v-if="predictionText !== ''">
      <br />
      <p><b>Prediction Text</b></p>
      <br />
      <p>{{ predictionText }}</p>
    </div>
  </InferenceWidget>
</template>
<script>
import InferenceWidget from "./InferenceWidget.vue";
import axios from "axios";

export default {
  components: {
    InferenceWidget
  },
  data() {
    return {
      inferenceURL: "https://api-inference.huggingface.co/models/t5-base",
      isLoading: false,
      sourceText:
        "The Statue of Liberty Enlightening the World was a gift of friendship from the people of France to the United States and is recognized as a universal symbol of freedom and democracy. The Statue of Liberty was dedicated on October 28, 1886. It was designated as a National Monument in 1924.",
      predictionText: ""
    };
  },
  methods: {
    onTranslate: async function() {
      this.predictionText = "";
      this.isLoading = true;
      try {
        const response = await axios.post(
          this.inferenceURL,
          JSON.stringify(this.sourceText)
        );
        this.predictionText = response.data[0].translation_text;
      } catch (error) {
        this.$buefy.toast.open({
          duration: 5000,
          message: `Translation failed, please try again.`,
          position: "is-bottom",
          type: "is-danger"
        });
      }
      this.isLoading = false;
    }
  }
};
</script>
