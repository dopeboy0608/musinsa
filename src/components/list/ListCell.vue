<template>
  <div class="q-pa-md items-start q-gutter-md">
    <q-card class="">
      <q-card-section class="bg-primary text-white inner-text">
        <div class="text-subtitle2">name: {{ item.name }}</div>
        <div class="text-subtitle2">aliases: {{ item.aliases }}</div>
        <div class="text-subtitle2">title: {{ item.title }}</div>
        <div class="text-subtitle2">books: {{ item.books }}</div>
        <div class="text-subtitle2">tvSeries: {{ item.tvSeries }}</div>
      </q-card-section>
      <q-card-actions align="center">
        <q-btn flat>삭제</q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>

<script>
export default {
  name: 'ListCell',
  props: {
    item: {
      type: Object,
      default: () => {
        return {
          name: '',
          aliases: '',
          title: '',
          books: '',
          tvSeries: '',
        }
      },
    }
  },
  data() {
    return {
      testNm: 'aa',
    }
  }
}
</script>

<style scoped>
  .inner-text {
    text-align: left;
  }
</style>