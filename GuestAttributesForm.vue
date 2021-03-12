<template>
  <div class="guest-attribute-form">

    <!-- non field errors -->

    <b-alert
        class="guest-attribute-alert"
        variant="danger"
        size="sm"
        :show="hasErrors('nonFieldErrors')">
      <div
          v-for="msg in getErrors('nonFieldErrors')"
          :key="msg">
        {{ msg }}
      </div>
    </b-alert>

    <!-- form -->

    <b-form
        class="mb-0"
        autocomplete="nope"
        v-on:submit.prevent>

      <div
          v-if="checkedGuestEmailDoesNotExist"
          class="alert alert-warning is-non-existent-email-feedback d-flex align-items-center">
        <svgicon
            class="mr-2"
            name="warning-o"
            width="15"
            height="15"/>
        <div>Email address does not exist. Please ask the chatter again.</div>
      </div>

      <div class="form-row">
        <div
            :class="getCssClass('first_name')"
            class="col">
          <label class="form-label d-flex align-items-center m-0">First Name</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add first name"
              v-model="form.first_name"
              size="sm"
              :state="getInputFormState('first_name')"
              @focus.native="onInputFocus"
              @input.native="$v.form.first_name.$touch()"
          />
          <div
              v-if="hasErrors('first_name')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('first_name')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
          <div
              v-else-if="!$v.form.first_name.required"
              class="invalid-feedback">
            <small>First name is required</small>
          </div>
        </div>
        <div
            :class="getCssClass('last_name')"
            class="col">
          <label class="form-label d-flex align-items-center m-0">Last Name</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add last name"
              v-model="form.last_name"
              size="sm"
              :state="getInputFormState('last_name')"
              @focus.native="onInputFocus"
              @input.native="$v.form.last_name.$touch()"
          />
          <div
              v-if="hasErrors('last_name')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('last_name')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
          <div
              v-else-if="!$v.form.last_name.required"
              class="invalid-feedback">
            <small>Last name is required</small>
          </div>
        </div>
      </div>

      <div class="form-row">
        <div
            :class="getCssClass('email')"
            class="col">
          <label class="form-label m-0">Email</label>
          <b-form-input
              class="form-input"
              :class="{ 'non-existent-email': checkedGuestEmailDoesNotExist == true }"
              autocomplete="nope"
              placeholder="Add email"
              v-model.trim="form.email"
              size="sm"
              :state="getInputFormState('email')"
              @focus.native="onInputFocus"
              @input.native="$v.form.email.$touch()"
          />
          <div
              v-if="hasErrors('email')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('email')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
          <div
              v-else-if="!$v.form.email.required"
              class="invalid-feedback">
            <small>Email is required</small>
          </div>
          <div
              v-else-if="$v.form.email.$invalid"
              class="invalid-feedback">
            <small>Enter a valid email</small>
          </div>
        </div>
        <div
            :class="getCssClass('phone')"
            class="col">
          <label class="form-label m-0">Phone</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add phone"
              v-model="form.phone"
              size="sm"
              :state="getInputFormState('phone')"
              @focus.native="onInputFocus"
              @input.native="$v.form.phone.$touch()"
          />
          <div
              v-if="hasErrors('phone')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('phone')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>
      </div>

      <div class="form-row">
        <div
            :class="getCssClass('company')"
            class="col">
          <label class="form-label m-0">Company</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add company name"
              v-model="form.company"
              size="sm"
              :state="getInputFormState('company')"
              @focus.native="onInputFocus"
              @input.native="$v.form.company.$touch()"
          />
          <div
              v-if="hasErrors('company')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('company')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>
        <div
            :class="getCssClass('street_address')"
            class="col">
          <label class="form-label m-0">Address</label>
          <textarea
              ref="inputStreetAddress"
              class="form-input form-control col"
              autocomplete="nope"
              placeholder="Add street address"
              v-model="form.street_address"
              size="sm"
              :state="getInputFormState('street_address')"
              @focus="onInputFocus"
              @input="$v.form.street_address.$touch()"
          />
          <div
              v-if="hasErrors('street_address')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('street_address')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>
      </div>

      <div class="form-row">
        <div
            :class="getCssClass('city')"
            class="col">
          <label class="form-label m-0">City</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add city"
              v-model="form.city"
              size="sm"
              :state="getInputFormState('city')"
              @focus.native="onInputFocus"
              @input.native="$v.form.city.$touch()"
          />
          <div
              v-if="hasErrors('city')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('city')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>

        <div
            :class="getCssClass('state')"
            class="col">
          <label class="form-label m-0">State</label>
          <b-form-input
              class="form-input col"
              autocomplete="nope"
              placeholder="Add state"
              v-model="form.state"
              size="sm"
              :state="getInputFormState('state')"
              @focus.native="onInputFocus"
              @input.native="$v.form.state.$touch()"
          />
          <div
              class="invalid-feedback"
              v-if="hasErrors('state')">
            <div
                v-for="msg in getErrors('state')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>
      </div>

      <div class="form-row">
        <div
            :class="getCssClass('zipcode')"
            class="col">
          <label class="form-label m-0">Zipcode</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add zipcode"
              v-model="form.zipcode"
              size="sm"
              :state="getInputFormState('zipcode')"
              @focus.native="onInputFocus"
              @input.native="$v.form.zipcode.$touch()"
          />
          <div
              v-if="hasErrors('zipcode')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('zipcode')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>

        <div
            :class="getCssClass('country')"
            class="col">
          <label class="form-label m-0">Country</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add country"
              v-model="form.country"
              size="sm"
              :state="getInputFormState('country')"
              @focus.native="onInputFocus"
              @input.native="$v.form.country.$touch()"
          />
          <div
              v-if="hasErrors('country')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('country')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>
      </div>

      <div
          v-if="showAltFieldRow"
          class="form-row">
        <div
            class="col"
            :class="getCssClass('phone_2')">
          <label class="form-label m-0">Alt Phone</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add alternate phone"
              v-model="form.phone_2"
              size="sm"
              :state="getInputFormState('phone_2')"
              @focus.native="onInputFocus"
              @input.native="$v.form.phone_2.$touch()"
          />
          <div
              v-if="hasErrors('phone_2')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('phone_2')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>
        <div
            class="col"
            :class="getCssClass('email_2')">
          <label class="form-label m-0">Alt Email</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add alternate email"
              v-model.trim="form.email_2"
              size="sm"
              :state="getInputFormState('email_2')"
              @focus.native="onInputFocus"
              @input.native="$v.form.email_2.$touch()"
          />
          <div
              v-if="($v.form.email_2.$invalid)"
              class="invalid-feedback">
            <small>Enter a valid email</small>
          </div>
        </div>
      </div>
      <div
          v-if="showWebsiteFieldRow"
          class="form-row"
          :class="getCssClass('website')">
        <div class="col">
          <label class="form-label m-0">Website</label>
          <b-form-input
              class="form-input"
              autocomplete="nope"
              placeholder="Add website"
              v-model="form.website"
              size="sm"
              :state="getInputFormState('website')"
              @focus.native="onInputFocus"
              @input.native="$v.form.website.$touch()"
          />
        </div>
        <div
            v-if="hasErrors('website')"
            class="invalid-feedback">
          <div
              v-for="msg in getErrors('website')"
              :key="msg">
            <small>{{ msg }}</small>
          </div>
        </div>
      </div>

      <div class="form-row">
        <b-btn
            v-if="!showAltFieldRow"
            class="alt-field-btn-link"
            size="sm"
            variant="link"
            @click="showAltFieldRow = true">
          + Alt phone &amp; email
        </b-btn>
        <b-btn
            v-if="!showWebsiteFieldRow"
            class="alt-field-btn-link"
            size="sm"
            variant="link"
            @click="showWebsiteFieldRow = true">
          + Website
        </b-btn>
      </div>

      <!-- Custom intake fields -->

      <div v-if="customQuestionFields.length">
        <hr class="mt-2" />
        <div
            class="form-row"
            v-for="f in customQuestionFields"
            :key="f.key">
          <template>
            <div class="col">
              <label class="form-label m-0">{{ f.label }}</label>
              <b-form-input
                  class="form-input"
                  autocomplete="nope"
                  :placeholder="`Add ${f.label}`"
                  v-model="form.custom_fields[f.key]"
                  size="sm"
                  @focus.native="onInputFocus"
              />
            </div>
          </template>
        </div>
      </div>

      <div v-if="formattedMultipleChoiceFields.length">
        <hr />
        <div
            class="form-row"
            v-for="f in formattedMultipleChoiceFields"
            :key="f.key">
          <div class="col">
            <label class="form-label m-0">{{ f.label }}</label>
            <b-form-select
                class="form-input"
                autocomplete="nope"
                v-model="form.custom_fields[f.key]"
                :options="f.answer_options"
                size="sm"
                @focus.native="onInputFocus"
            />
          </div>
        </div>
      </div>

      <hr />

      <div
          class="form-row"
          :class="getCssClass('guest_type')">
        <div class="col">
          <label class="form-label m-0">Type</label>
          <b-form-select
              class="form-select mb-0 pr-4"
              size="sm"
              v-model="form.guest_type"
              :options="GUEST_TYPE_OPTIONS"
              :state="getInputFormState('guest_type')"
              @focus.native="onInputFocus"
              @input.native="$v.form.guest_type.$touch()"
          />
        </div>
      </div>

      <div
          class="form-row"
          :class="getCssClass('notes')">
        <div class="col">
          <label class="form-label m-0 mt-1">Notes</label>
          <textarea
              ref="inputNotes"
              class="form-input form-control notes-textarea"
              autocomplete="nope"
              placeholder="Add notes"
              v-model="form.notes"
              :state="getInputFormState('notes')"
              @focus="onInputFocus"
              @input="$v.form.notes.$touch()"
          />
          <div class="notes-character-counter mr-2 mt-1">
            {{ (form.notes) ? form.notes.trim().length : 0 }} characters
          </div>
          <div
              v-if="hasErrors('notes')"
              class="invalid-feedback">
            <div
                v-for="msg in getErrors('notes')"
                :key="msg">
              <small>{{ msg }}</small>
            </div>
          </div>
        </div>
      </div>
    </b-form>

    <!-- Buttons -->

    <div class="d-flex justify-content-end mt-3" v-if="showActions">
      <b-button
          class="mr-1"
          variant="outline-secondary"
          size="sm"
          :disabled="this.isSubmitting"
          @click="onCancel">
        Cancel
      </b-button>
      <b-button
          class="ml-1"
          variant="primary"
          size="sm"
          :disabled="($v.form.$invalid || this.isSubmitting)"
          @click="onSubmit">
        {{ this.isSubmitting ? 'Saving' : 'Save' }}
      </b-button>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import autosize from 'autosize'
import { mapActions, mapGetters } from 'vuex'
import {
  cloneDeep as _cloneDeep,
  filter as _filter,
  forOwn as _forOwn,
  get as _get,
  has as _has,
  includes as _includes,
  some as _some,
  sortBy as _sortBy,
  uniqBy as _uniqBy,
} from 'lodash'
import { required, email } from 'vuelidate/lib/validators'

import {
  ANSWER_TYPE_FREETEXT,
  ANSWER_TYPE_MULTIPLE_CHOICE,
  GUEST_TYPE_OPTIONS,
} from '../constants'
import { checkEmailExists, updateGuestAttributes } from '../api'
import { FormMixin } from '../mixins'
import {
  phone,
  uniqueEmailInService,
  uniquePhoneInService,
} from '../validators/validators.js'

const SYSTEM_EMAIL_REGEX = /@fb.smith.ai|@smschat.smith.ai|@thumbtack.smith.ai/

export default {
  name: 'guest-attributes-form',
  mixins: [FormMixin],
  props: {
    guestData: {
      type: Object,
      required: true
    },
    requiredFields: {
      type: Array,
      default: () => [],
    },
    serviceId: {
      type: String,
      required: true,
    },
    guestFieldUpdates: {
      type: Object,
    },
    // Be careful removing this as it's used by 'uniqueEmailInService', and 'uniquePhoneInService'.
    channelId: {
      type: String,
      required: true,
    },
  },

  data () {
    const service = this.$store.getters['service/getServiceById'](this.serviceId)
    let form = {}
    let formBackup = _cloneDeep(this.guestData)

    // Ensure guest's custom field keys match currently set fields for the service.
    formBackup.custom_fields = formBackup.custom_fields || {}
    service.custom_fields.forEach(({ key }) => {
      // If the key is missing or undefined, use null to allow the b-form-select elements
      // to render their default options in the form.
      if (!formBackup.custom_fields[key]) {
        formBackup.custom_fields[key] = null
      }
    })

    form = _cloneDeep(formBackup)

    // Ensure that if a system generated email is provided, we obfuscate it from the UI.
    if (SYSTEM_EMAIL_REGEX.test(form.email)) {
      form.email = null
    }

    return {
      form,
      formBackup,
      showActions: false,
      submitClicked: false,
      isSubmitting: false,
      isCheckingGuestEmailExists: false,
      guestEmailExists: false,
      showAltFieldRow: false,
      showWebsiteFieldRow: false,
    }
  },

  validations () {
    let form = {
      first_name: { required },
      last_name: { },
      email: { email, unique: uniqueEmailInService('email') },
      email_2: { email },
      phone: { phone: phone('phone'), unique: uniquePhoneInService('phone') },
      phone_2: { phone: phone('phone_2') },
      company: { },
      street_address: { },
      city: { },
      state: { },
      zipcode: { },
      country: { },
      website: { },
      guest_type: { },
      notes: { },
    }

    if (!this.hasSystemGeneratedEmailBackup) {
      // If a system generated email is detected, do not require the email field.
      form.email.required = required
    }

    return { form }
  },

  computed: {
    ...mapGetters({
      getServiceById: 'service/getServiceById',
    }),

    service () {
      return this.getServiceById(this.serviceId) || {}
    },

    customFields () {
      return this.service.custom_fields || []
    },
    customQuestionFields () {
      return _filter(this.customFields, ['answer_type', this.ANSWER_TYPE_FREETEXT_CODE])
    },
    customMultipleChoiceFields () {
      return _filter(this.customFields, ['answer_type', this.ANSWER_TYPE_MULTIPLE_CHOICE_CODE])
    },
    formattedMultipleChoiceFields () {
      return this.customMultipleChoiceFields.map(f => {
        // For each field in the service's definition, format its answer options to contain
        // a default, a preset based on the last saved value, and the field's default options.
        let options = []

        // If a value is already set for this custom intake field, add it as an option.
        if (this.formBackup.custom_fields[f.key]) {
          const previousValue = this.formBackup.custom_fields[f.key]
          options.push({
            value: previousValue,
            text: previousValue
          })
        }

        // Add the predefined options for this custom intake field.
        const fieldOptions = _cloneDeep(f.answer_options.map(o => ({ value: o, text: o })))
        options = options.concat(fieldOptions)

        options = _uniqBy(options, 'text')
        options = _sortBy(options, ['text'])

        // Add the default placeholder option
        options.unshift({
          value: null,
          text: `Add ${f.label}`,
          disabled: true
        })

        return { ...f, answer_options: options }
      })
    },

    hasSystemGeneratedEmailBackup () {
      return SYSTEM_EMAIL_REGEX.test(this.formBackup.email)
    },
    checkedGuestEmailDoesNotExist () {
      return (
        !this.$v.form.$anyDirty &&
        !this.isCheckingGuestEmailExists &&
        !this.guestEmailExists
      )
    }
  },

  methods: {
    ...mapActions('channel', ['updateGuestName', 'updateGuestAttributes']),

    // Form utils

    _resetForm () {
      let formData = _cloneDeep(this.formBackup)

      if (this.hasSystemGeneratedEmailBackup) {
        // Ensure that if a system generated email is provided, we obfuscate it from the UI.
        formData.email = null
      }

      this.form = formData
      this.resetValidation()
      this.$emit('guestAttributesDirty', false)
      this.$nextTick(() => {
        autosize.update(this.$refs.inputStreetAddress)
        autosize.update(this.$refs.inputNotes)
      })
    },
    _getFormPayload () {
      const formData = _cloneDeep(this.form)

      if (formData.website && !formData.website.startsWith('http')) {
        // Ensure that if not provided, we manually add a protocol for the website url.
        formData.website = 'http://' + formData.website
      }
      if (!formData.email && this.hasSystemGeneratedEmailBackup) {
        // Ensure that the system generated email is used if one is not provided.
        formData.email = this.formBackup.email
      }

      return formData
    },
    checkGuestEmailExists () {
      if (this.isCheckingGuestEmailExists) {
        return
      }

      this.isCheckingGuestEmailExists = true
      return checkEmailExists({ email: this.form.email })
        .then(() => {
          this.guestEmailExists = true
        })
        .catch(err => {
          if (err && (err.code === 'ECONNABORTED')) {
            // If the request times out do not render the warning message.
            this.guestEmailExists = true
          } else {
            const errorData = _get(err, 'response.data.email[0]')
            // Only show the warning message if the error in is the expected format.
            this.guestEmailExists = (errorData !== 'This email address does not exist.')
          }
        })
        .finally(() => {
          this.isCheckingGuestEmailExists = false
        })
    },

    // Field utils

    getCssClass (fieldName) {
      return _includes(this.requiredFields, fieldName) ? 'required' : ''
    },

    // Handlers

    onInputFocus () {
      this.showActions = true
    },
    onSubmit () {
      if (this.$v.form.$invalid) {
        this.$v.form.$touch()
        return
      }

      const guestId = this.guestData.id
      const payload = this._getFormPayload()

      this.isSubmitting = true
      updateGuestAttributes(guestId, payload)
        .then(({ data }) => {
          this.showActions = false
          this.formBackup = data
          this.$emit('guestAttributesUpdated', data)
          this.updateGuestName(data)
          this.updateGuestAttributes({
            'channelId': this.channelId,
            'data': data,
          })
          this._resetForm()
        })
        .catch(({ response }) => {
          this.handleErrorResponse(response)
        })
        .finally(() => {
          this.checkGuestEmailExists()
          this.isSubmitting = false
        })
    },
    onCancel () {
      this.showActions = false
      this._resetForm()
    },
  },

  watch: {
    guestFieldUpdates (newVal) {
      if (!newVal) return

      _forOwn(newVal, (val, key) => {
        if (_has(this.form, key)) {
          // A standard guest field update
          Vue.set(this.form, key, val)
          Vue.set(this.formBackup, key, val)
        } else if (_some(this.customFields, { key })) {
          // A custom field guest update
          Vue.set(this.form.custom_fields, key, val)
          Vue.set(this.formBackup.custom_fields, key, val)
        }
      })
    },

    '$v.$anyDirty' (newVal) {
      this.$emit('guestAttributesDirty', newVal)
    },
  },

  created () {
    this.GUEST_TYPE_OPTIONS = GUEST_TYPE_OPTIONS
    this.ANSWER_TYPE_FREETEXT_CODE = ANSWER_TYPE_FREETEXT.code
    this.ANSWER_TYPE_MULTIPLE_CHOICE_CODE = ANSWER_TYPE_MULTIPLE_CHOICE.code
    this.checkGuestEmailExists()
  },
  mounted () {
    autosize(this.$refs.inputStreetAddress)
    autosize(this.$refs.inputNotes)
  },
  beforeDestroy () {
    autosize.destroy(this.$refs.inputStreetAddress)
    autosize.destroy(this.$refs.inputNotes)
  },
}
</script>

<style lang="scss" scoped>
  @import '../styles/variables/colors';

  .form-group {
    margin-bottom: 0;
  }

  .form-group.required .form-label:after {
    content: " *";
    color: $red;
  }

  .guest-attribute-alert {
    font-size: .75rem;
    margin-bottom: 1rem;
    padding: .25rem .5rem;
  }

  .guest-attribute-form {
    .form-row {

      .form-label {
        color: $text-muted;
        font-size: 0.6875rem;
        font-weight: 600;
        letter-spacing: 0.4px;
        text-transform: uppercase;
      }

      .form-input,
      .form-select {
        background-color: white;
        color: $text-primary;
        font-size: .75rem;
        height: 1.5rem;
        letter-spacing: .6px;
        line-height: 1.5rem;
        min-height: 27px;
        padding: 0 .5rem;

        &::placeholder {
          color: transparentize($black, .59);
        }

        &:hover {
          cursor: pointer;
        }

        &:focus,
        &:active,
        &.is-invalid,
        &.is-valid {
          border-width: 1px;
          margin: 0;
        }
        &.non-existent-email {
          border-color: $orange;
          border-width: 1px;
          &:focus {
            box-shadow: 0 0 0 0.2em rgba(242, 163, 44, 0.2);
          }
        }
      }
    }

    .is-non-existent-email-feedback {
      color: darken($orange, 12);
      font-size: 13px;
      line-height: 1;
      margin-bottom: 0.5rem;
      padding: 0.55rem 1.5rem;
    }

    .notes-character-counter {
      font-size: .75rem;
      text-align: right;
    }

    .invalid-feedback {
      right: 6px;
      letter-spacing: -0.15px;
      margin: 0;
      position: absolute;
      text-align: right;
      top: 4px;
    }
    hr {
      margin-bottom: 0.5rem;
    }
  }
  .alt-field-btn-link {
    font-size: 13px;
    padding: 0.25rem;
  }
</style>
