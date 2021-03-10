<template>
  <div>
    <div class="price__wrap">
      <label for="minValue">
        {{ this.item.slider_value_prefix }}
        <input
          class="price__input"
          :id="minValue"
          type="text"
          ref="minValue"
          :value="value[0]"
          @input="updateValue($event.target.value)"
          @change="minValue($event.target.value)"
        />
      </label>
      <label for="maxValue">
        {{ this.item.slider_value_prefix }}
        <input
          class="price__input"
          :id="maxValue"
          type="text"
          ref="maxValue"
          :value="value[1]"
          @input="updateValue($event.target.value)"
          @change="maxValue($event.target.value)"
        />
      </label>
    </div>
  </div>
</template>

<script>
export default {
  props: ["item", "value"],
  computed: {},
  methods: {
    updateValue() {
      this.$emit("input", [
        this.$refs.minValue.value,
        this.$refs.maxValue.value,
      ]);
    },
    minValue(value) {
      if (
        value < this.item.slider_min_value ||
        value > this.item.slider_max_value
      ) {
        this.$refs.minValue.value = this.item.slider_min_value;
      }
      this.updateValue();
    },
    maxValue(value) {
      if (
        value < this.item.slider_min_value ||
        value > this.item.slider_max_value
      ) {
        this.$refs.maxValue.value = this.item.slider_max_value;
      }
      this.updateValue();
    },
  },
};
</script>

<style scoped>
.price__wrap {
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
}
.price__input {
  width: 50px;
  background-color: #f5f2f2;
  border: 0;
  border-radius: 4px;
  padding: 10px;
  box-sizing: border-box;
}
</style>