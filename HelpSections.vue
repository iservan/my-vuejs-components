<template>
  <b-row class="help-sections py-5 px-3">
    <fragment v-if="groupedQs && groupedQs.length">
      <b-col v-for="(section, index) in groupedQs" :key="section.id" :index="index" cols="12" md="4" class="mb-3">
        <slot name="sectionIcon">
          <i :id="`section-id-${section.id}`" ref="icon" :key="section.id" :class="[iconName, 'fas fa-3x icon']"></i>
        </slot>
        <h4>{{ section.name }}</h4>
        <ul class="list-unstyled">
          <li v-for="(question, qindex) in section.questions" :key="question.id" :index="qindex">
            <inertia-link :href="$route('help-question', { help_question: question.id })" @click.stop>
              {{ question.question }}
            </inertia-link>
          </li>
        </ul>
      </b-col>
    </fragment>

    <b-col v-else-if="isLoading" cols="12">
      <empty-state title="Loading" class="border-0">
        <template #image>
          <div class="loader position-relative text-center p-3">
            <b-spinner variant="primary" class="loading-spinner" label="Loading..."></b-spinner>
          </div>
        </template>
      </empty-state>
    </b-col>

    <b-col v-else-if="!isLoading && groupedQs.length === 0" cols="12">
      <empty-state
        class="border-0"
        svg-icon
        :font-icon="false"
        title="Whoops! We couldn't find any article. Please try to search by a single term  i.e. TAGS"
      >
        <template name="svg">
          <no-results-icon></no-results-icon>
        </template>
      </empty-state>
    </b-col>
  </b-row>
</template>

<script>
  import $ from 'jquery';
  import EmptyState from '../Atoms/EmptyState';
  import NoResultsIcon from '../Atoms/svgIcons/NoResultsIcon';

  export default {
    props: {
      questions: Array,
      iconName: String,
      search: String,
    },

    components: {
      EmptyState,
      NoResultsIcon,
    },

    computed: {
      isLoading() {
        return !this.groupedQs || (!this.groupedQs.length && !this.search);
      },

      groupedQs: function() {
        return Object.values(
          this.questions
            .filter(item => {
              if (!this.search || this.search == '') {
                return true;
              }
              if (!item.question || !item.answer) {
                return false;
              }

              return item.question.match(this.search) || item.answer.match(this.search);
            })
            .reduce((acc, item) => {
              let foundSection = acc[item.section.id];
              if (!foundSection) {
                foundSection = item.section;
              }
              let questions = foundSection.questions || [];
              questions.push(item);
              return {
                ...acc,
                [item.section.id]: { ...foundSection, questions },
              };
            }, {})
        );
      },
    }, //end computed

    updated() {
      $('#section-id-1').addClass(' fa-tags');
      $('#section-id-2').addClass(' fa-paste');
      $('#section-id-3').addClass(' fa-mobile-alt');
      $('#section-id-4').addClass(' fa-bell');
      $('#section-id-5').addClass(' fa-thumbs-up');
      $('#section-id-6').addClass(' fa-hospital');
      $('#section-id-7').addClass(' fa-brain');
    },
  };
</script>

<style lang="scss" scoped>
  .icon {
    color: #16a085;
    margin-bottom: 1rem;
  }
  li {
    line-height: 1.25rem;
    margin-bottom: 0.75rem;
  }
  .help-sections {
    margin-top: -5vh;
    background: #fff;
    box-shadow: 0 -15px 30px -16px rgba(50, 50, 93, 0.25), 0 -20px 27px -24px rgba(0, 0, 0, 0.3);
    border-radius: 8px;
  }
  body.dark {
    .help-sections {
      background: linear-gradient(180deg, #333 30%, #000 100%);
      .list-unstyled li a {
        color: #aaa;
        &:hover {
          color: #fff;
        }
      }
    }
  }
</style>
