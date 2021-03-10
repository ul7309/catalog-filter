<template>
  <form class="filter">
    <div class="filter__item" v-for="item in info" :key="item.unique_id">
      <h2>{{ item.display_name }}</h2>
      <template v-if="item.type == 'slider'">
        <FilterTypeSlider
          :item="item"
          v-model="range"
          v-on:input="setSelectedVariants"
        />
        <VueSimpleRangeSlider
          style="width: 300px"
          :min="item.slider_min_value"
          :max="item.slider_max_value"
          :logarithmic="true"
          v-model="range"
          v-on:input="setSelectedVariants"
        />
      </template>
      <template v-else-if="item.type == 'list'">
        <FilterTypeList
          :item="item"
          v-model="selected"
          v-on:input="setSelectedVariants"
        />
      </template>
    </div>
    <button class="btn" type="button" @click="setDefaultVariants">
      Сбросить
    </button>
  </form>
</template>

<script>
import axios from "axios";
import FilterTypeList from "@/components/FilterTypeList";
import FilterTypeSlider from "@/components/FilterTypeSlider";
import VueSimpleRangeSlider from "vue-simple-range-slider";
import "vue-simple-range-slider/dist/vueSimpleRangeSlider.css";

export default {
  data() {
    return {
      info: null,
      selected: [],
      range: [],
      selectedVariants: {},
    };
  },
  props: ["item"],
  components: {
    FilterTypeList,
    FilterTypeSlider,
    VueSimpleRangeSlider,
  },
  methods: {
    setDefaultVariants() {
      this.selected = [];
      this.range = [
        this.info[1].slider_min_value,
        this.info[1].slider_max_value,
      ];
    },
    setSelectedVariants() {
      this.selectedVariants = {
        variants: this.selected,
        min: this.range[0],
        max: this.range[1],
      };
    },
  },
  created() {
    axios
      .get(
        "https://raw.githubusercontent.com/cscart/apply-for-job/master/frontend-developer/files/002-json-example.json"
      )
      .then((response) => {
        this.info = response.data;
        this.setDefaultVariants();
      });
  },
};
</script>

<style scoped>
h2 {
  margin: 0;
}
.filter {
  margin: auto;
  padding: 20px;
  max-width: 300px;
  box-shadow: 0 0 40px 0 rgb(97 96 96 / 15%);
  border-radius: 10px;
  text-align: left;
}
.filter__item {
  border-bottom: 1px solid #ddd;
  margin-bottom: 10px;
  padding-bottom: 10px;
}
.btn {
  background-color: #037bf7;
  border: 0;
  border-radius: 4px;
  padding: 7px 20px;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}
</style>