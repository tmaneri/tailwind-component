<template>
  <component :is="as" v-bind="bindData">
    <slot />
  </component>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { pickBy } from "lodash-es";
import { twMerge } from "tailwind-merge";

const classesToString = (classes: string | string[]) => {
  if (typeof classes === "string") {
    classes = classes.split(" ");
  } else if (!Array.isArray(classes)) {
    classes = Object.keys(pickBy(classes));
  }

  return classes.flat().join(" ");
};

export default defineComponent({
  props: {
    as: {
      default: "div",
    },
    baseClass: {
      default: "",
    },
    class: {
      default: "",
    },
    size: {
      default: "base",
    },
    sizes: {
      default: Object,
    },
  },

  computed: {
    sizeClass() {
      return this.sizes?.[this.size];
    },
    bindData() {
      return {
        class: twMerge(
          classesToString(this.baseClass),
          classesToString(this.sizeClass),
          classesToString(this.class)
        ),
      };
    },
  },
});
</script>
