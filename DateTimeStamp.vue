<template>
  <div class="date my-2"><i class="far fa-clock mr-1"></i>{{ stampDate }}</div>
</template>

<script>
  import { format, formatDistanceToNow, addDays, isPast, parseISO } from 'date-fns';
  export default {
    name: 'TimeStamp',

    props: {
      date: String,
    },

    computed: {
      //current date-time
      now() {
        return new Date();
      },
      parsedDate() {
        return parseISO(this.date);
      },
      //a week from date
      aWeekFromDate() {
        return addDays(this.parsedDate, 7);
      },
      isDatePast() {
        return isPast(this.aWeekFromDate);
      },
      stampDate() {
        if (this.isDatePast == true) {
          return format(this.parsedDate, 'dd-MMM-yyyy HH:mm', { weekStartsOn: 1 });
        }
        return formatDistanceToNow(this.parsedDate, { includeSeconds: true }) + ' ago';
      },
    },
  };
</script>

<style lang="scss" scoped>
  @import '../../sass/config/variables';
  @import '~bootstrap/scss/mixins/breakpoints';
  .now {
    color: cadetblue;
  }
  .date,
  .date far {
    font-size: 0.75rem;
    font-weight: 600;
  }
  .reply-date {
    margin-top: 1rem;
    margin-left: calc(35px + 1rem);
  }
  @include media-breakpoint-down(md) {
    .reply-date {
      margin: 0 !important;
      position: absolute;
      top: 0;
      right: 1rem;
    }
  }
</style>
