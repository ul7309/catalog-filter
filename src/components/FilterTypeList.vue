<template>
  <div>
    <input
      type="text"
      v-model="filter"
      placeholder="Найти"
      class="filter__search"
    />
    <div
      v-for="variant in filtered"
      :key="variant.unique_id"
      class="filter__variant"
    >
      <input
        type="checkbox"
        :id="variant.unique_id"
        :value="variant.unique_id"
        v-model="checked"
      />
      <label
        :for="variant.unique_id"
        v-if="variant.unique_id !== editInput.id"
        >{{ variant.display_name }}</label
      >
      <input
        class="input-edit"
        type="text"
        v-if="variant.unique_id === editInput.id"
        v-model="editInput.name"
      />
      <button
        class="btn-edit"
        type="button"
        v-if="variant.unique_id !== editInput.id"
        @click="editVariant(variant.display_name, variant.unique_id)"
      >
        Edit
      </button>
      <button
        class="btn-save"
        type="button"
        v-if="variant.unique_id === editInput.id"
        @click="saveVariant(variant.unique_id)"
      >
        Save
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["item", "value"],
  data() {
    return {
      filter: "",
      editInput: {},
    };
  },
  computed: {
    filtered() {
      return this.item.list_variants.filter((variant) =>
        variant.display_name.includes(this.filter)
      );
    },
    checked: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit("input", value);
      },
    },
  },
  methods: {
    editVariant(name, id) {
      this.editInput = { name: name, id: id };
    },
    saveVariant(id) {
      this.filtered.filter((variant) => {
        if (variant.unique_id == id) {
          variant.display_name = this.editInput.name;
          this.editInput = { name: null, id: null };
        }
      });
    },
  },
};
</script>

<style scoped>
.filter__search {
  margin: 20px 0 10px;
  width: 100%;
  background-color: #f5f2f2;
  border: 0;
  border-radius: 4px;
  padding: 10px;
  box-sizing: border-box;
}

.filter__variant {
  margin-bottom: 7px;
  position: relative;
}

.filter__variant:hover .btn-edit {
  display: block;
}

.btn-save {
  position: absolute;
  top: 0;
  right: 0;
  background-image: url("data:image/svg+xml,%3Csvg version='1.1' xmlns='http://www.w3.org/2000/svg' x='0' y='0' viewBox='0 0 100 100' xml:space='preserve'%3E%3Cpath d='M11 51.4v33.4c0 2.3 1.9 4.2 4.2 4.2h69.6c2.3 0 4.2-1.9 4.2-4.2V34.1c0-2.1-.8-4.2-2.4-5.7l-15.1-15C70 11.8 68 11 65.9 11H15.2c-2.3 0-4.2 1.9-4.2 4.2v18.5' fill='%23f47e60' stroke-linecap='round' stroke='%23333' stroke-width='3.5' stroke-miterlimit='10'/%3E%3Cpath fill='%23f47e60' stroke-linecap='round' stroke='%23333' stroke-width='3.5' stroke-miterlimit='10' d='M11 42v1.7'/%3E%3Cpath d='M75.2 64v-7.7c0-3.5-2.8-6.4-6.4-6.4H31.1c-3.5 0-6.4 2.8-6.4 6.4V89h50.5V78.5' fill='%23e0e0e0' stroke='%23333' stroke-width='3.5' stroke-linecap='round' stroke-miterlimit='10'/%3E%3Cpath d='M49.4 36.5h9.9c1.8 0 3.3-1.5 3.3-3.3V11H24.8v22.2c0 1.8 1.5 3.3 3.3 3.3h5.3' fill='%23e0e0e0' stroke='%23333' stroke-width='3.5' stroke-linecap='round' stroke-miterlimit='10'/%3E%3Cpath fill='%23e0e0e0' stroke='%23333' stroke-width='3.5' stroke-linecap='round' stroke-miterlimit='10' d='M40.5 36.5H42'/%3E%3Cpath fill='%23e0e0e0' stroke='%23333' stroke-width='3.5' stroke-linecap='round' stroke-miterlimit='10' d='M75.2 71.4v-1.1'/%3E%3C/svg%3E");
  border: 0;
  background-repeat: no-repeat;
  background-position: center;
  background-color: transparent;
  font-size: 0;
  width: 20px;
  height: 20px;
  cursor: pointer;
}

.btn-edit {
  display: none;
  position: absolute;
  top: 0;
  right: 0;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='20' fill='%2336c' height='20' viewBox='0 0 20 20'%3E%3Cpath d='M16.77 8l1.94-2a1 1 0 0 0 0-1.41l-3.34-3.3a1 1 0 0 0-1.41 0L12 3.23zm-5.81-3.71L1 14.25V19h4.75l9.96-9.96-4.75-4.75z'/%3E%3C/svg%3E%0A");
  border: 0;
  background-repeat: no-repeat;
  background-position: center;
  background-color: transparent;
  font-size: 0;
  width: 20px;
  height: 20px;
  cursor: pointer;
}
.input-edit {
  background-color: #f5f2f2;
  border: 0;
  border-radius: 4px;
  padding: 2px 10px;
  box-sizing: border-box;
}
</style>