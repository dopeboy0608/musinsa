<template>
  <div>
    <ListCell
      v-for="(item, index) of listData"
      :key="index"
      :item="item"
    />
  </div>
<!--  <q-virtual-scroll-->
<!--    style="max-height: 300px;"-->
<!--    :items="listData"-->
<!--    separator-->
<!--  >-->
<!--    <template v-slot="{ item, index }">-->
<!--      <q-item-->
<!--        :key="index"-->
<!--        dense-->
<!--      >-->
<!--      </q-item>-->
<!--    </template>-->
<!--  </q-virtual-scroll>-->
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