<template>
  <div class="tableItem">
    <div class="tableColumnName">
      <div>{{ comp.nameCompany }}</div>
    </div>
    <div class="tableColumnName fioDirector">
      <div>{{ comp.director }}</div>
    </div>
    <div class="tableColumnName phoneNumber">
      <div>{{ comp.phone }}</div>
    </div>
    <div class="tableColumnName">
      <button class="itemBTN" @click="deleteItem">X</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TableItem",
  props: {
    comp: {
      type: Object,
      required: true,
    },
  },
  methods: {
    deleteItem() {
      this.$emit("deleteItem", this.comp.id);
    },
  },
};
</script>

<style scoped>

.tableColumnName{
    padding: 0px 10px 0px 10px;
}
.tableItem {
  width: inherit;
  height: 60px;
  display: grid;
  grid-template-columns: 200px 200px 200px 100px;
}

.phoneNumber {
  justify-content: flex-end;
}

.fioDirector {
  justify-content: flex-start;
}

.itemBTN {
  border: solid 1px black;
  background-color: white;
}

.itemBTN:hover {
  background-color: rgb(211, 211, 211);
}

.itemBTN:active {
  background-color: rgb(119, 119, 119);
}
</style>