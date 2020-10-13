<template>
  <div class="big-searcher">
    <b-container>
      <b-row align-v="center" align-h="center" no-gutters>
        <b-col cols="1" class="zoom-icon">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 40" x="0px" y="0px">
            <g data-name="12">
              <path
                d="M28.14,26.73l-6.4-6.4A10,10,0,0,0,6.93,6.93a10,10,0,0,0,13.4,14.81l6.4,6.4a1,1,0,0,0,1.41-1.41ZM8.34,19.66a8,8,0,1,1,11.31,0A7.95,7.95,0,0,1,8.34,19.66Z"
              />
            </g>
          </svg>
        </b-col>
        <b-col cols="5">
          <b-form-input
            :value="value"
            class="big-input-search"
            placeholder="How can we help you?"
            @input="$emit('input', $event)"
          ></b-form-input>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
  //import SearchHelp from '../mixins/SearchHelp';
  export default {
    name: 'BigSearch',

    props: {
      value: String,
    },
  };
</script>

<style lang="scss" scoped>
  @import '../../sass/config/variables';
  .big-searcher {
    margin-top: -15px;
    background: #16a085;
    min-height: 21vh;
    .row {
      min-height: 17vh;
    }
    .big-input-search {
      min-height: 45px;
      border: none;
      border-radius: 0;
      font-size: 1.7rem;
      font-weight: 900;
      letter-spacing: -1px;
      color: white;
      background-color: transparent;
      transition: all 0.25s ease-in-out;
      &:focus {
        color: inherit;
        background-color: white;
        color: $body-color;
      }
    }
    ::placeholder {
      color: white !important;
    }
    .zoom-icon {
      text-align: center;
      border-right: 2px solid #fff;
    }
    svg {
      fill: white;
      width: 55px;
      padding-top: 13px;
    }
  }
</style>
