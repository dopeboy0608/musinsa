<template>
  <div>
    <h3>MUSINSA 과제</h3>
    <SearchFilter
      @FILTER_UPDATE="onFilterUpdate"
    />
    <ListContainer
      :listData="characterList"
    />
  </div>
</template>

<script>
import Events from "@/event/Events";

// const createList = (page) => {
//   const tempArr = []
//   for (let i=1; i<=10; i++) {
//     tempArr.push({
//       name: `${page}_${i}`,
//       aliases: `aliases`,
//       title: `title`,
//       books: [],
//       tvSeries: [],
//       died: '',
//       gender: '',
//     })
//   }
//   return tempArr
// }

import axios from 'axios'
import SearchFilter from '@/components/SearchFilter';
import ListContainer from '@/components/list/ListContainer';
import EventBus from "@/event/EventBus";

export default {
  name: 'MainContainer',
  components: {
    ListContainer,
    SearchFilter,
  },
  data() {
    return {
      startPage: 0,
      page: 0,
      maxPage: 2,
      originListData: [],
      isAlive: false,
      isFemale: false,
      isTvSeries: false,
      params: {
        page: 0,
        pageSize: 10,
      }
    }
  },
  mounted() {
    const { page } = this.$route.query
    this.page = page || 1
    this.startPage = this.page
    EventBus.$on(Events.REMOVE_ITEM, this.onRemoveItem)
    this.onLoadList()
  },
  beforeDestroy() {
    EventBus.$off(Events.REMOVE_ITEM, this.onRemoveItem)
    window.removeEventListener('scroll', this.onScroll)
  },
  computed: {
    characterList() {
      let tempList = this.originListData
      if (this.isAlive) {
        tempList = tempList.filter(item => !item.died)
      }
      if (this.isFemale) {
        tempList = tempList.filter(item => item.gender === 'Female')
      }
      if (this.isTvSeries) {
        tempList = tempList.filter(item => !item.tvSeries.length)
      }
      // console.log(tempList)
      return tempList
    }
  },
  methods: {
    async onLoadList() {
      window.removeEventListener('scroll', this.onScroll)
      this.params = {
        ...this.params,
        page: this.page,
      }

      this.$q.loading.show()
      const res = await axios.get('/api', { params: this.params })
      res.data = res.data.map((item, index) => {
        return {
          ...item,
          key: `${this.page}_${index + 1}`
        }
      })
      this.$q.loading.hide()
      // createList(this.page)
      this.originListData = this.originListData.concat(res.data)
      window.addEventListener('scroll', this.onScroll)
    },
    onScroll() {
      const currentPosition = document.body.clientHeight - (window.scrollY + window.outerHeight)

      if (currentPosition < 150) {
        console.log('next page!!');
        if (this.page + 1 <= this.maxPage) {
          this.page++
          this.onLoadList()
        } else {
          window.removeEventListener('scroll', this.onScroll)
          this.$q.notify({
            message: '마지막 페이지 입니다.',
            color: 'primary'
          })
        }

        // this.$q.loading.show()
        // window.removeEventListener('scroll', this.onScroll);
        // setTimeout(() => {
        //   console.log('page complete, add scroll')
        //   this.page++
        //   this.onLoadList()
        //   window.addEventListener('scroll', this.onScroll)
        //   this.$q.loading.hide()
        // }, 300)
      }
    },
    onFilterUpdate(filterOptions) {
      console.log('filterOptions: ', filterOptions)
      const { isAlive, isFemale, isTvSeries } = filterOptions
      this.isAlive = isAlive
      this.isFemale = isFemale
      this.isTvSeries = isTvSeries
    },
    onRemoveItem(key) {
      this.listData = this.listData.filter(item => item.key !== key)
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
