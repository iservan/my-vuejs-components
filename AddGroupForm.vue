<template>
  <div>
    <b-form novalidate :disabled="busy" @submit.prevent="onSubmit">
      <b-row class="align-items-center mb-3">
        <b-col cols="3">
          <avatar-group-image :size="150"></avatar-group-image>
          <btn
            v-b-tooltip.hover
            pill
            icon
            icon-name="fa-pencil-alt"
            size="md"
            variant="outline-primary"
            title="Add a group profile picture"
          ></btn>
        </b-col>
        <b-col cols="9" sm class="mb-2">
          <text-input
            id="groupName"
            label="Name"
            required
            placeholder="Group name"
            autofocus
            v-model="name"
          ></text-input>
        </b-col>
      </b-row>
      <b-row>
        <b-col sm>
          <rich-editor-input
            ref="editor"
            output="html"
            :label="$t('cmrad.group_description')"
            :enable-images="false"
            v-model="description"
            :placeholder="$t('cmrad.group_description')"
          ></rich-editor-input>
        </b-col>
        <b-col sm>
          <user-selector label="Users"></user-selector>
        </b-col>
      </b-row>
      <submit-btn></submit-btn>
    </b-form>
    <pre>
      {{ output }}
    </pre>
  </div>
</template>

<script>
  import TextInput from '../Atoms/Form/Inputs/TextInput';
  import RichEditorInput from '../Molecules/Forms/RichEditorInput';
  import UserSelector from '../Organisms/UserSelector';
  import AvatarGroupImage from '../Atoms/AvatarGroupImage';
  import md5 from 'md5';
  import uniqid from 'uniqid';
  import axios from 'axios';
  import { CaseGroupResource } from '../api-resources';

  export default {
    name: 'AddGroup',

    components: {
      RichEditorInput,
      TextInput,
      UserSelector,
      AvatarGroupImage,
    },

    data() {
      return {
        info: null,
        name: '',
        description: '',
        output: '',
        //members: '',
      };
    },

    props: {
      group: CaseGroupResource,
      busy: {
        type: Boolean,
        default: false,
      },
      //members: [],
      members: CaseGroupResource,
    },

    mounted() {
      //axios.get('https://www.cmrad.local/api/x1.0/groups').then(response => (this.info = response.data));
    },

    methods: {
      onSubmit() {
        let { currentObj } = this;
        axios
          .post('/api/1.0/groups', {
            name: this.name,
            description: this.description,
            members: this.members,
          })
          .then(function(response) {
            currentObj.output = response.data;
            console.log(response);
          })
          .catch(function(error) {
            currentObj.output = error;
          });
      },
    },

    remember: {
      data: ['name', 'description', 'members'],
      key: () => (window ? `add-group-${md5(window.location.href)}` : uniqid()),
    },
  };
</script>

<style lang="scss" scoped></style>
