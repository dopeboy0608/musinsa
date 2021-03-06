<template>
  <div>
    <ListCell
      v-for="(item, index) of listData"
      :key="index"
      :item="item"
    />
<!--    <ListCell />-->
<!--    <ListCell />-->
<!--    <ListCell />-->
<!--    <ListCell />-->
<!--    <ListCell />-->
<!--    <ListCell />-->
<!--    <ListCell />-->
<!--    <ListCell />-->
<!--    <ListCell />-->
  </div>
</template>

<script>
import ListCell from '@/components/list/ListCell'

export default {
  name: 'ListContainer',
  components: {
    ListCell,
  },
  props: {
    listData: {
      type: Array,
      default: () => [],
    }
  },
  data() {
    return {
    }
  }
}
</script>

<style scoped>

</style>