<template>
  <div>
    <b-btn
        class="d-flex align-items-center"
        variant="link"
        size="sm"
        @click="goBack">
        <svgicon
            class="d-block mr-2"
            name="arrow"
            width="18"
            height="18"/>
        Back
    </b-btn>
    <form>
      <div class="row d-flex justify-content-center">
        <!-- Quick replies editing -->
        <div class="col-8 border-bottom pb-3">
          <label class="field-label">Title</label>
          <length-limited-input-text
              id="label"
              class="mb-2"
              v-model.trim="form.label"
              :state="getInputFormState('label')"
              :maxLength="100"
              :rows="1"
              @input.native="$v.form.label.$touch()"
          />
          <label class="field-label">Body</label>
          <length-limited-input-text
              id="body"
              v-model.trim="form.body"
              :state="getInputFormState('body')"
              :maxLength="180"
              :rows="3"
              @input.native="$v.form.body.$touch()"
          />
        </div>
        <div class="col-8 py-3 justify-content-between">
        <b-btn
            v-if="editMode"
            class="ml-auto"
            size="sm"
            variant="outline-danger"
            @click="showQRDeleteModal">
          Delete
        </b-btn>
        <div class="float-right">
          <b-btn
              class="mr-2"
              size="sm"
              variant="outline-secondary"
              @click="goBack">
            Cancel
          </b-btn>
          <b-btn
              v-if="editMode"
              size="sm"
              variant="success"
              @click="onUpdateQuickReply">
            Save QR
          </b-btn>
          <b-btn
              v-else
              size="sm"
              variant="primary"
              @click="onCreateQuickReply">
            Add new QR
          </b-btn>
        </div>
        </div>
      </div>
    </form>
    <!-- Delete quick reply modal -->
    <b-modal
        ref="deleteQRModal"
        title="Delete quick reply">
      <p class="m-0">Are you sure you want to delete this quick reply?.</p>
      <template slot="modal-footer">
        <b-btn
            variant="danger"
            @click="onDeleteQuickReply">
          Delete
        </b-btn>
      </template>
    </b-modal>
  </div>
</template>

<script>
import { required } from 'vuelidate/lib/validators'
import { FormMixin, LeaveGuardMixin } from '../mixins'
import {
  fetchQuickReply as fetchQuickReplyAPI,
  deleteQuickReply as deleteQuickReplyAPI,
  updateQuickReply as updateQuickReplyAPI,
  createQuickReply as createQuickReplyAPI,
} from 'Api'

export default {
  name: 'knowledge-base-quick-replies-view',
  mixins: [FormMixin, LeaveGuardMixin],

  props: {
    service_id: {
      type: String,
      required: true,
    },
    quick_reply_id: {
      type: String,
      required: false,
      default: '',
    },
  },

  data () {
    return {
      form: {
        label: '',
        body: ''
      },
      quickReply: null,
    }
  },

  validations: {
    form: {
      label: {
        required
      },
      body: {
        required
      },
    }
  },

  computed: {
    // Used by the LeaveGuardMixin.
    canLeave () {
      return !this.$v.form.$anyDirty
    },
    editMode () {
      return !!this.quick_reply_id
    }
  },

  methods: {
    goBack () {
      // Navigate back one state.
      this.$router.go(-1)
    },

    populateForm (data) {
      this.form.label = data.label
      this.form.body = data.body
      this.quickReply = data
    },

    onUpdateQuickReply () {
      if (this.isSubmitting) {
        return
      } else if (this.$v.form.$invalid) {
        this.$v.form.$touch()
        return
      }
      this.isSubmitting = true

      updateQuickReplyAPI(this.service_id, this.quick_reply_id, this.form)
        .then(() => {
          this.resetValidation()
          this.$snotify.success('Quick reply updated successfully.')
        })
        .catch((error) => {
          this.$snotify.error('Quick reply update failed.')
          this.handleErrorResponse(error.response)
        })
        .finally(() => {
          this.isSubmitting = false
        })
    },

    onCreateQuickReply () {
      createQuickReplyAPI(this.service_id, {
        service: this.service_id,
        ...this.form
      })
        .then(({ data }) => {
          this.resetValidation()
          this.$snotify.success('Quick reply created successfully.')
        })
        .catch((error) => {
          this.$snotify.error('Quick reply creation failed.')
          this.handleErrorResponse(error.response)
        })
        .finally(() => {
          this.isSubmitting = false
        })
    },

    showQRDeleteModal () {
      this.$refs.deleteQRModal.show()
    },

    onDeleteQuickReply () {
      deleteQuickReplyAPI(this.service_id, this.quick_reply_id)
        .catch(() => {
          this.$snotify.error('Failed to delete quick reply.')
        })
        .finally(() => {
          this.goBack()
        })
    },

    fetchQuickReply (serviceId, quickReplyId) {
      fetchQuickReplyAPI(serviceId, quickReplyId)
        .then(({ data }) => {
          this.populateForm(data)
        })
        .catch(() => {
          this.$snotify.error('Failed to load quick reply.')
        })
    },
  },

  beforeRouteEnter (to, from, next) {
    const serviceId = to.params.service_id
    const quickReplyId = to.params.quick_reply_id
    if (quickReplyId) {
      next(vm => {
        vm.fetchQuickReply(serviceId, quickReplyId)
      })
    } else {
      next()
    }
  },

}
</script>

<style lang="scss" scoped>
  @import '../styles/variables/colors';

</style>
