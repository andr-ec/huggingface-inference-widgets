<template>
  <div class="box">
    <div class="media-content">
      <p class="title is-4">
        {{ title }}
      </p>
      <p class="subtitle is-6">
        {{ subtitle }}
      </p>
      <b-field label="Source Text">
        <b-input
          v-model="sourceText"
          type="textarea"
          @keyup="$emit('update:sourceText', sourceText)"
        />
      </b-field>
      <b-button
        :loading="isLoading"
        :type="isLoading ? '' : 'is-primary'"
        @click="onInference"
      >
        {{ buttonAction }}
      </b-button>
      <br />
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: { default: "", type: String },
    subtitle: { default: "", type: String },
    buttonAction: { default: "", type: String },
    value: { default: "", type: String },
    isLoading: Boolean
  },
  computed: {
    // used to allow prop to be mutated
    sourceText: {
      get() {
        return this.value;
      },
      set(sourceText) {
        this.$emit("input", sourceText);
      }
    }
  },
  methods: {
    onInference: function() {
      this.$emit("submit");
    }
  }
};
</script>

<style></style>
