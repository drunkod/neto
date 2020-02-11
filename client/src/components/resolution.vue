<template>
  <vue-context class="context" ref="context">
    <template v-for="(conf, i) in configurations">
      <li v-for="(rate, j) in conf.rates" :key="`${i}-${j}`" @click="screenSet(conf.width, conf.height, rate)">
        <i class="fas fa-desktop"></i>
        <span>{{ conf.width }}x{{ conf.height }}</span>
        <small>{{ rate }}</small>
      </li>
    </template>
  </vue-context>
</template>

<style lang="scss" scoped>
  .context {
    background-color: $background-floating;
    background-clip: padding-box;
    border-radius: 0.25rem;
    display: block;
    margin: 0;
    padding: 5px;
    min-width: 150px;
    z-index: 1500;
    position: fixed;
    list-style: none;
    box-sizing: border-box;
    max-height: calc(100% - 50px);
    overflow-y: auto;
    color: $interactive-normal;
    user-select: none;
    box-shadow: $elevation-high;
    scrollbar-width: thin;
    scrollbar-color: $background-secondary transparent;

    &::-webkit-scrollbar {
      width: 8px;
    }

    &::-webkit-scrollbar-track {
      background-color: transparent;
    }

    &::-webkit-scrollbar-thumb {
      background-color: $background-secondary;
      border: 2px solid $background-floating;
      border-radius: 4px;
    }

    &::-webkit-scrollbar-thumb:hover {
      background-color: $background-floating;
    }

    > li {
      margin: 0;
      position: relative;
      align-content: center;
      display: flex;
      flex-direction: row;
      padding: 8px 5px;
      cursor: pointer;

      i {
        margin-right: 10px;
      }

      span {
        flex-grow: 1;
      }

      small {
        font-size: 0.7em;
        justify-self: flex-end;
        align-self: flex-end;
      }

      &:hover,
      &:focus {
        text-decoration: none;
        background-color: $background-modifier-hover;
        color: $interactive-hover;
      }

      &:focus {
        outline: 0;
      }
    }

    &:focus {
      outline: 0;
    }
  }
</style>

<script lang="ts">
  import { Component, Ref, Watch, Vue } from 'vue-property-decorator'
  import { Member } from '~/neko/types'

  // @ts-ignore
  import { VueContext } from 'vue-context'

  @Component({
    name: 'neko-resolution',
    components: {
      'vue-context': VueContext,
    },
  })
  export default class extends Vue {
    @Ref('context') readonly context!: any

    get configurations() {
      return this.$accessor.video.configurations
    }

    open(event: MouseEvent) {
      this.context.open(event)
    }

    screenSet(width: number, height: number, rate: number) {
      this.$accessor.video.screenSet({ width, height, rate })
    }
  }
</script>