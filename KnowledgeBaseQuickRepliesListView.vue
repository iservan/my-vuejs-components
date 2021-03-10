<template>
  <div>
    <div
        v-if="!fetchingQuickReplies"
        class="row">

      <div class="chat-panel-form col-8">

        <div
            class="px-1"
            v-for="quickReply in paginatedItems"
            :key="quickReply.id">
          <div
              class="py-3 d-flex justify-content-between border-bottom">
            <!-- Quick replies list -->
            <div class="col-7">
              <span class="field-label">{{ quickReply.label }}</span>
              <p class="quick-reply-body mb-0">{{ quickReply.body }}</p>

            </div>
            <div class="mr-3 action-btns">
              <b-btn
                  class="p-0"
                  variant="link"
                  @click="quickReplyEditItem(quickReply)">
                <svgicon
                    class="edit-icon"
                    name="edit"
                    height="20"
                    width="20"
                />
              </b-btn>
            </div>
          </div>
        </div>

        <!-- Pagintation controls -->
        <b-pagination
            v-if="quickReplies.length > pageSize"
            class="mt-5"
            size="sm"
            next-text="Next ›"
            prev-text="‹ Prev"
            limit="3"
            :total-rows="quickReplies.length"
            v-model="currentPage"
            :per-page="pageSize"
            hide-goto-end-buttons
            hide-ellipsis
        />
      </div>

      <!-- Add new QR form -->
      <div class="col-2 p-0 mt-1 float-right">
        <b-btn
            size="md"
            variant="primary"
            @click="showAddQRform">
          + New QR
        </b-btn>
      </div>
    </div>

    <div
        v-else
        class="d-flex align-items-center justify-content-center my-4">
      <loading-indicator
          size="20"
          class="mr-2"
          color="#618190"
      />
      <p class="mb-0">
        <small>Loading quick replies</small>
      </p>
    </div>

  </div>
</template>

<script>
import {
  fetchQuickReplies as fetchQuickRepliesAPI,
} from '../api'

export default {
  name: 'knowledge-base-quick-replies-view',

  props: {
    service_id: {
      type: String,
      required: true,
    },
    panel: {
      type: Object,
      required: true
    },
  },

  data () {
    return {
      quickReplies: [],
      fetchingQuickReplies: false,
      currentPage: 1,
      pageSize: 5,
    }
  },

  computed: {
    quickRepliesList () {
      return this.quickReplies.length
    },
    paginatedItems () {
      if (!this.quickReplies || !this.currentPage) return []

      const firstIndex = (this.currentPage - 1) * this.pageSize
      const lastIndex = (this.currentPage * this.pageSize)

      return this.quickReplies.slice(firstIndex, lastIndex)
    },
  },

  methods: {
    showAddQRform () {
      this.$router.push({
        name: 'knowledge-base-quick-replies-item-view'
      })
    },

    quickReplyEditItem ({ id }) {
      // Move to question edit view on row click.
      this.$router.push({
        name: 'knowledge-base-quick-replies-item-view',
        params: {
          quick_reply_id: id
        }
      })
    },

    fetchQuickReplies (serviceId) {
      this.fetchingQuickReplies = true
      fetchQuickRepliesAPI(serviceId)
        .then(({ data }) => {
          this.quickReplies = data
        })
        .catch(() => {
          this.$snotify.error('Failed to load quick replies.')
        })
        .finally(() => {
          this.fetchingQuickReplies = false
        })
    },
  },

  beforeRouteEnter (to, from, next) {
    const serviceId = to.params.service_id
    next(vm => {
      vm.fetchQuickReplies(serviceId)
    })
  },

}
</script>

<style lang="scss" scoped>
  @import '../styles/variables/colors';

  .quick-reply-body {
    color: $text-muted;
    font-size: 14px;
    letter-spacing: 0.1px;
    line-height: 1.43;
    max-height: 40px;
    overflow: hidden;
  }
</style>
