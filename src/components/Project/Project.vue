<template>
  <div class="space-y-6 text-center">
    <!-- Project description -->
    <h1 class="project__title">{{ project_description }}</h1>

    <!-- Toggle add feature form & button -->
    <button
      @click="toggle_feature_form = !toggle_feature_form"
      v-if="!toggle_feature_form || getStartFormDisabledState"
      class="btn__base btn__new__feature"
      :class="getStartFormDisabledState ? 'opacity-50' : ''"
      :disabled="getStartFormDisabledState"
      :title="
        getStartFormDisabledState
          ? 'Please save your ongoing feature first before adding a new one'
          : 'Click to add a new feature'
      "
    >
      {{ getStartFormDisabledState ? 'Save your work...' : 'NEW FEATURE' }}
    </button>

    <SegmentStartForm
      v-else
      @toggle-feature-button="toggleAddFeatureButton"
      class="max-w-5xl mx-auto"
      :project_id="this.project_id"
    />

    <!-- features -->
    <div class="max-w-5xl mx-auto">
      <div class="space-y-3">
        <!-- If no features exist -->
        <div v-if="getFeatures.length === 0" class="font-thin text-center">
          💡 Please add a feature...
        </div>

        <!-- Print features -->
        <Feature
          v-for="feature in this.getFeatures"
          :key="feature.id"
          :project_id="this.project_id"
          :feature_id="feature.id"
          :description="feature.description"
          :segments="feature.segments"
        />
      </div>
    </div>
  </div>
</template>

<script>
  import SegmentStartForm from './components/SegmentStartForm.vue';
  import Feature from './components/Feature.vue';

  export default {
    components: { SegmentStartForm, Feature },
    data() {
      return {
        project_id: this.$route.params.id,
        project_description: '',
        toggle_feature_form: false,
      };
    },
    created() {
      this.project_description = this.getDescription;
    },
    methods: {
      toggleAddFeatureButton() {
        this.toggle_feature_form = !this.toggle_feature_form;
      },
    },
    computed: {
      getFeatures() {
        // Returns all the features of a specific project, project ID is send as payload
        return this.$store.getters.getFeatures({ project_id: this.project_id });
      },
      getDescription() {
        return this.$store.getters.getProject({ project_id: this.project_id })
          .description;
      },
      getStartFormDisabledState() {
        return this.$store.getters.getProject({ project_id: this.project_id })
          .disable_start_form;
      },
    },
  };
</script>

<style scoped>
  .project {
    @apply px-4 py-2 rounded-md shadow-md cursor-pointer;
    @apply bg-white hover:bg-gray-100;
    @apply border-black border-solid;
    @apply dark:text-black;
  }
  .project__title {
    @apply py-4 text-xl italic font-thin text-center text-black;
    @apply border-b border-dashed border-gray-500 rounded-t-3xl;
    @apply dark:text-white;
  }

  .btn__new__feature {
    @apply rounded-md;
    @apply md:max-w-xs;
  }
</style>
