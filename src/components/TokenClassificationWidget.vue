<template>
  <div>
    <InferenceWidget
      v-model="sourceText"
      title="Token Classification"
      subtitle="dbmdz/bert-large-cased-finetuned-conll03-english"
      button-action="Classify"
      :is-loading="isLoading"
      @submit="onClassify"
    >
      <div v-if="hasResponse">
        <br>
        <span
          v-for="(wordObject, index) in classifiedWords"
          :key="index"
          :class="wordObject.class !== null ? wordObject.class.entity_group + ' all-entities' : ''"
        >
          <span>
            {{ wordObject.word }}
          </span>
          <span
            v-if="wordObject.class !== null"
            class="entity-group"
          >{{ wordObject.class.entity_group }}</span>
        </span>
      </div>
    </InferenceWidget>
  </div>
</template>
<script>
import InferenceWidget from "./InferenceWidget.vue";
import axios from 'axios'

export default {
  components: {
    InferenceWidget
  },
  data() {
    return {
      inferenceURL: 'https://api-inference.huggingface.co/models/dbmdz/bert-large-cased-finetuned-conll03-english',
      isLoading: false,
      sourceText: 'When Sebastian Thrun started working on self-driving cars at Google in 2007, few people outside of the company took him seriously. “I can tell you very senior CEOs of major American car companies would shake my hand and turn away because I wasn’t worth talking to,” said Thrun, now the co-founder and CEO of online higher education startup Udacity, in an interview with Recode earlier this week. A little less than a decade later, dozens of self-driving startups have cropped up while automakers around the world clamor, wallet in hand, to secure their place in the fast-moving world of fully automated transportation.',
      hasResponse: false,
      classifiedWords: []
    }
  },
  methods: {
    onClassify: async function() {
      this.hasResponse = false
      this.isLoading = true
      try {
        const response = await axios.post(this.inferenceURL, JSON.stringify(this.sourceText))
        this.classifiedWords = this.classifyWords(response.data)
        this.hasResponse = true
      } catch (error) {
        console.log(error)
        this.$buefy.toast.open({
          duration: 5000,
          message: `Classification failed, please try again.`,
          position: 'is-bottom',
          type: 'is-danger'
          })
      }
      this.isLoading = false
    },
    classifyWords: function(classes) {
      // const words = this.sourceText.split(/(\s+)/)
      // let usedClassifiedWordIndices = new Set()
      let latestIndex = 0
      const reducer = (accumulatedWords, currentValue) => {
        const wordsFromLatest = this.sourceText.substring(latestIndex, this.sourceText.length)
        const startIndex = wordsFromLatest.indexOf(currentValue.word)
        if (startIndex == -1) {
          return accumulatedWords
        }
        const currentWordStartIndex = startIndex + latestIndex
        const currentWordEndIndex = currentWordStartIndex + currentValue.word.length

        const wordsUpToCurrent = this.sourceText.substring(latestIndex, currentWordStartIndex - 1)
        // words with no entity
        const wordsUpToCurrentObject = { word: wordsUpToCurrent, class: null}

        const currentWordsInClass = this.sourceText.substring(currentWordStartIndex, currentWordEndIndex)
        // word with current entity
        const currentWordsObject = { word: currentWordsInClass, class: currentValue}

        latestIndex = currentWordEndIndex

        return accumulatedWords.concat([wordsUpToCurrentObject,currentWordsObject])
      }

      const groupedWordsWithClasses = classes.reduce(reducer, [])
      if (latestIndex < this.sourceText.length - 1) {
        // include last words
        const finalWords = this.sourceText.substring(latestIndex, this.sourceText.length - 1)
        return groupedWordsWithClasses.concat([ {word: finalWords, class: null}])
      } else {
        return groupedWordsWithClasses
      }
    },
    
  }
}
</script>
<style scoped>
.all-entities {
  padding: 0.2em 0.3em;
  margin: 0 0.25em;
  line-height: 1;
  display: inline-block;
  border-radius: 0.25em;
  border-radius: 5px;
}
.B-PER{
  border: 2px solid rgb(36, 101, 46);
  background-color: rgba(36, 101, 46, 0.16);
}
.I-PER{
  border: 2px solid rgb(55, 0, 255);
  background-color: rgba(55, 0, 255, 0.16);
}
.BLOC {
  border: 2px solid rgb(132, 27, 148);
  background-color: rgba(132, 27, 148, 0.16);
}
.I-LOC{
  border: 2px solid rgb(148, 35, 27);
  background-color: rgba(148, 35, 27, 0.16);
}
.B-ORG{
  border: 2px solid rgb(27, 126, 148);
  background-color: rgba(27, 126, 148, 0.16);
}
.I-ORG{
  border: 2px solid rgb(148, 148, 27);
  background-color: rgba(148, 148, 27, 0.16);
}
.B-MISC{
  border: 2px solid rgb(53, 27, 148);
  background-color: rgba(53, 27, 148, 0.16);
}
.I-MISC{
  border: 2px solid rgb(232, 225, 24);
  background-color: rgba(232, 225, 24, 0.16);
}
.entity-group {
  box-sizing: border-box;
    content: attr(data-entity);
    font-size: 0.55em;
    line-height: 1;
    padding: 0.35em 0.35em;
    border-radius: 0.35em;
    text-transform: uppercase;
    display: inline-block;
    vertical-align: middle;
    margin: 0 0 0.15rem 0.5rem;
    background: #fff;
    font-weight: bold;
}
</style>