<template>
  <section class="footer-column">
    <FooterSubsection
      v-for="el in elements"
      :key="el.title"
      :title="el.title"
      :elements="el.elements"
    />
  </section>
</template>

<script lang="ts">
import Vue from 'vue'
import { Component, Prop } from 'vue-property-decorator'
import FooterSubsection from './FooterSubsection.vue'

@Component({
  components: {
    FooterSubsection
  }
})
export default class extends Vue {
  @Prop(Array) elements
}
</script>

<style lang="scss" scoped>
.footer-column {
  flex: 1;
  margin-right: $layout-03;

  &:last-child {
    margin-right: 0;
  }

  @include mq($until: medium) {
    margin-top: $layout-03;
    margin-right: 0;
  }
}
</style>
