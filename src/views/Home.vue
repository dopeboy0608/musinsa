<template>
  <div>
    <h3>MUSINSA 과제</h3>
    <SearchFilter
      @FILTER_UPDATE="onFilterUpdate"
    />
    <ListContainer
      :listData="listData"
    />
  </div>
</template>

<script>
import Events from "@/event/Events";

// const createList = (page) => {
//   const tempArr = []
//   for (let i=1; i<=10; i++) {
//     tempArr.push({
//       key: `${page}_${i}`,
//       name: `${page}_${i}`,
//       aliases: [],
//       title: `title`,
//       books: [],
//       tvSeries: [],
//       died: i % 3 === 0 ? '2021/03/06' : '',
//       gender: i % 2 === 0 ? 'Female' : 'Male',
//     })
//   }
//   return tempArr
// }

import axios from 'axios';
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
      maxPage: 10,
      originListData: [],
      listData: [],
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
    this.page = parseInt(page) || 1
    this.startPage = this.page
    this.maxPage = this.page + 10
    // console.log(this.page, this.maxPage)

    EventBus.$on(Events.REMOVE_ITEM, this.onRemoveItem)
    this.onLoadList()
  },
  beforeDestroy() {
    EventBus.$off(Events.REMOVE_ITEM, this.onRemoveItem)
    window.removeEventListener('scroll', this.onScroll)
  },
  methods: {
    async onLoadList() {
      window.removeEventListener('scroll', this.onScroll)

      this.params = {
        ...this.params,
        page: this.page,
      }

      this.$q.loading.show()

      try {
        // const res = await axios.get('/api', { params: this.params })
        const res = await axios.get('https://www.anapioficeandfire.com/api/characters', { params: this.params })
        res.data = res.data.map((item, index) => {
          return {
            ...item,
            key: `${this.page}_${index + 1}`
          }
        })
        this.originListData = this.originListData.concat(res.data)
        // this.originListData = this.originListData.concat(createList(this.page))
        this.listData = this.filterList()
        window.addEventListener('scroll', this.onScroll)
      } catch (e) {
        this.$q.notify({
          message: 'API 통신에 실패했습니다.',
          color: 'red',
          timeout: 1500,
        })
      } finally {
        this.$q.loading.hide()
      }
    },
    onScroll() {
      const currentPosition = document.body.clientHeight - (window.scrollY + window.outerHeight)

      if (currentPosition < 150) {
        if (this.page + 1 < this.maxPage) {
          this.page++
          this.onLoadList()
        } else {
          window.removeEventListener('scroll', this.onScroll)
          this.$q.notify({
            message: '마지막 페이지 입니다.',
            color: 'primary',
            timeout: 1500,
          })
        }

        // this.$q.loading.show()
        // window.removeEventListener('scroll', this.onScroll);
        // setTimeout(() => {
        //   if (this.page + 1 < this.maxPage) {
        //     this.page++
        //     this.onLoadList()
        //     window.addEventListener('scroll', this.onScroll)
        //     this.$q.loading.hide()
        //   } else {
        //     this.$q.loading.hide()
        //     window.removeEventListener('scroll', this.onScroll)
        //     this.$q.notify({
        //       message: '마지막 페이지 입니다.',
        //       color: 'primary',
        //       timeout: 1500,
        //     })
        //   }
        // }, 300)
      }
    },
    onFilterUpdate(filterOptions) {
      // console.log('filterOptions: ', filterOptions)
      const { isAlive, isFemale, isTvSeries } = filterOptions
      this.isAlive = isAlive
      this.isFemale = isFemale
      this.isTvSeries = isTvSeries

      this.listData = this.filterList()
    },
    onRemoveItem(key) {
      this.listData = this.listData.filter(item => item.key !== key)
    },
    filterList() {
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
      return tempList
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
