<template >
  <div class="q-pa-md items-start q-gutter-md" >
    <q-card class="">
      <q-card-section class="bg-primary text-white inner-text">
        <div class="text-subtitle2">key: {{ item.key }}</div>
        <div class="text-subtitle2">name: {{ item.name }}</div>
        <div class="text-subtitle2">died: {{ item.died }}</div>
        <div class="text-subtitle2">gender: {{ item.gender }}</div>
        <div class="text-subtitle2">aliases: {{ item.aliases.join(',') }}</div>
        <div class="text-subtitle2">title: {{ item.title }}</div>
        <div class="text-subtitle2">books: {{ item.books.length }}</div>
        <div class="text-subtitle2">tvSeries: {{ item.tvSeries.length }}</div>
      </q-card-section>
      <q-card-actions align="center">
        <q-btn flat @click="onRemove">삭제</q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>

<script>
import EventBus from "@/event/EventBus";
import Events from "@/event/Events";

export default {
  name: 'ListCell',
  props: {
    item: {
      type: Object,
      default: () => {
        return {
          key: '',
          name: '',
          aliases: '',
          title: '',
          died: '',
          gender: '',
          books: [],
          tvSeries: [],
          isRemove: false,
        }
      },
    }
  },
  methods: {
    onRemove() {
      EventBus.$emit(Events.REMOVE_ITEM, this.item.key)
    }
  }
}
</script>

<style scoped>
  .inner-text {
    text-align: left;
  }
</style>