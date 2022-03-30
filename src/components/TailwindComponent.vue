<template>
  <component :is="as" v-bind="bindData">
    <slot />
  </component>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { twMerge } from "tailwind-merge";

function pickBy(object: { [key: string]: any }) {
  const obj: { [key: string]: any } = {};
  for (const key in object) {
    if (object[key]) {
      obj[key] = object[key];
    }
  }
  return obj;
}

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
