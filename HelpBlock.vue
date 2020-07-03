i<template>
  <div class="help-block" v-click-outside="hideHelp">
    <div class="help-block-button" @click="check">
      <h4>Do you need help?</h4>
      <help-animation></help-animation>
    </div>

    <transition appear mode="out-in" name="help-motion">
      <div v-if="checked" :class="{ checked: checked }" key="dynamic" class="help-block-content">
        <b-button variant="primary" class="rounded-circle icon-close" @click="closeCheck">
          <span class="fas fa-times fa-fw fa-1x"></span>
        </b-button>
        <b-col cols="10" class="offset-1 my-5">
          <b-embed type="video" aspect="16by9" controls allowfullscreen :poster="$loadImage('int-video-cover.jpg')">
            <source src="https://cmrad-public.s3.eu-central-1.amazonaws.com/videos/workflow.mp4" type="video/mp4">
          </b-embed>
        </b-col>

        <h4 class="my-3 q-tittle">{{ $t('cmrad.related_questions') }}</h4>
        <ul v-if="!busy" class="q-related">
          <li class="custom-links" :key="question.id" v-for="question in filteredQuestions">
            <a @click.prevent href="#" v-b-toggle="`question-collapse-${question.id}`">
              {{ question.content[0].text_question }}</a>
            <b-collapse :id="`question-collapse-${question.id}`" class="mt-2">
              <b-container v-html="question.content[0].text_answer" class="answer-content"></b-container>
            </b-collapse>
          </li>
        </ul>
        <b-container v-else>
          <div class="text-primary text-center my-3">
            <b-spinner variant="primary" class="loading-spinner" label="Loading..."></b-spinner>
          </div>
        </b-container>

        <ul class="mt-3">
          <li>
            <a href="/help">Further documentation <i class="far fa-arrow-alt-circle-right"></i></a>
          </li>
        </ul>
      </div>
    </transition>
  </div>
  <!-- morph-button -->
</template>

<script>
  import axios from 'axios';
  import keyBy from 'lodash.keyby';
  import ClickOutside from 'vue-click-outside';
  import HelpAnimation from '../../Atoms/HelpAnimation';

    export default {
    directives: {
      ClickOutside
    },
    components: {
      HelpAnimation
    },
    props: {
      questionIds: Array,
    },

    data() {
      return {
        checked: false,
        questions: {},
        busy: false,
      };
    },

    mounted() {
      this.loadQuestions();
    },

    computed: {
      filteredQuestions() {
        if (this.questionIds.length) {
          return this.questionIds.reduce((acc, questionId) => {
            const question = this.questions[questionId] || null;

            if (question) {
              return [...acc, question];
            }

            return acc;
          }, []);
        }

        return this.questions;
      },
    },

    methods: {
      check() {
        this.checked = !this.checked;
      },
      closeCheck () {
        this.$emit('close-event', (this.checked = !this.checked));
      },

      async loadQuestions() {
        this.busy = true;

        const questions = await axios.get('/get-questions');

        this.questions = keyBy(
          questions.data.reduce((acc, item) => {
            return [...acc, ...item.questions];
          }, []),
          'id'
        );

        this.busy = false;
      },

      hideHelp() {
        if (this.checked) {
          this.checked = false;
        }
      },
    },
  };
</script>

<style lang="scss" scoped>
  // help block appearnce animation
  .help-motion-enter {
    opacity: 0;
    transform: scale(0.5) translateY(-100px);
  }
  .help-motion-enter-active,
  .help-motion-leave-active {
    opacity: 1;
    transition: all 400ms cubic-bezier(0.68, -0.35, 0.265, 1.55);
    transition-property: opacity, transform;
  }
  .help-motion-leave-to {
    opacity: 0;
    transform: scale(0) translateX(100px);
  }
</style>
