<template>
  <div>
    <component
      :is="children.component"
      v-for="(item, index) in internalValue"
      :key="`${children.component}-${item.username}-${index}`"
      v-model="internalValue[index]"
      v-bind="children"
    />
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: [Array, String],
      default: () => [],
    },
    children: {
      type: Object,
      default: () => ({}),
    },
  },
  data: () => ({
    internalValue: [],
  }),
  watch: {
    value(val) {
      const extra = [...val, {}]

      if (JSON.stringify(extra) === JSON.stringify(this.internalValue)) {
        return
      }

      this.internalValue = extra
    },
    internalValue: {
      deep: true,
      handler(val) {
        let filtered = []

        if (val) {
          filtered = val.filter((item) => item && Object.keys(item).length)
        }

        this.$emit('input', filtered)
      },
    },
  },
  mounted() {
    const val = this.value || []
    const extra = [...val, {}]

    this.internalValue = extra
  },
}
</script>
