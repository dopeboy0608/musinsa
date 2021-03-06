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
const createList = (page) => {
  const tempArr = []
  for (let i=1; i<=10; i++) {
    tempArr.push({
      name: `${page}_${i}`,
      aliases: `aliases`,
      title: `title`,
      books: [],
      tvSeries: [],
    })
  }
  return tempArr
}
// import axios from 'axios'
import SearchFilter from '@/components/SearchFilter';
import ListContainer from '@/components/list/ListContainer';
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
      listData: [],
    }
  },
  mounted() {
    const { page } = this.$route.query
    this.page = page || 1
    this.startPage = this.page
    this.onLoadList()

    window.addEventListener('scroll', this.onScroll)
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.onScroll);
  },
  methods: {
    onLoadList() {
      // const res = await axios.get('/api')
      // console.log(res)
      // this.$q.loading.show()
      this.listData = this.listData.concat(createList(this.page))
      console.log(this.listData)
    },
    onScroll() {
      // console.log(document.body.clientHeight, document.body.offsetHeight, document.body.scrollHeight)
      // console.log(window.scrollY, window.screen.availHeight, window.outerHeight)
      const currentPosition = document.body.clientHeight - (window.scrollY + window.outerHeight)
      console.log(currentPosition)

      if (currentPosition < 150) {
        console.log('next page!!');
        this.$q.loading.show()
        window.removeEventListener('scroll', this.onScroll);
        setTimeout(() => {
          console.log('page complete, add scroll')
          this.page++
          this.onLoadList()
          window.addEventListener('scroll', this.onScroll)
          this.$q.loading.hide()
        }, 300)
      }
    },
    onFilterUpdate() {
      console.log('onFilterUpdate')
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
