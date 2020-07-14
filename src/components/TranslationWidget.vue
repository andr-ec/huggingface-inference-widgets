<template>
  <div class="box">
    <div class="media-content">
      <p class="title is-4">
        Translation
      </p>
      <p class="subtitle is-6">
        t5-base
      </p>
      <b-field label="Source Text">
        <b-input v-model="sourceText" />
      </b-field>
      <b-button
        :loading="isLoading"
        :type="isLoading ? '': 'is-primary'"
        @click="onTranslate"
      >
        Translate
      </b-button>
      <br>
      <br>
      <p>{{ predictionText }}</p>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      inferenceURL: 'https://api-inference.huggingface.co/models/t5-base',
      isLoading: false,
      sourceText: 'My name is Julien and I am an engineer who lives in NYC.',
      predictionText: ''
    }
  },
  methods: {
    onTranslate: async function() {
      this.predictionText = ''
      this.isLoading = true
      try {
        const response = await axios.post(this.inferenceURL, JSON.stringify(this.sourceText))
        this.predictionText = response.data[0].translation_text
      } catch (error) {
        this.$buefy.toast.open({
          duration: 5000,
          message: `Translation failed, please try again.`,
          position: 'is-bottom',
          type: 'is-danger'
          })
      }
      this.isLoading = false

    }
  }
}
</script>