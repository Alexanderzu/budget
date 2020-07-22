<template>
  <div class="total-value" :class="classTotal">Balance: {{total}}</div>
</template>

<script>
export default {
  name: "TotalBalance",
  props: {
    total: {
      type: Number,
      default: 0
    }
  },
  computed: {
    classTotal() {
      if (this.total === 0) return "";
      return this.total > 0 ? "green" : "red";
    }
  }
};
</script>

<style scoped>
.total-value {
  margin: 10px;
  text-align: center;
  font-size: 40px;
}

.total-value.red {
  color: red;
}

.total-value.green {
  color: green;
}
</style>

