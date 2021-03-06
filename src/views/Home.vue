<template>
  <div>
    <h3>MUSINSA 과제</h3>
    <SearchFilter
      @FILTER_UPDATE="onFilterUpdate"
    />
    <ListContainer />
  </div>
</template>

<script>
// import axios from 'axios'
import SearchFilter from '@/components/SearchFilter';
import ListContainer from '@/components/list/ListContainer';
export default {
  name: 'MainContainer',
  components: {
    ListContainer,
    SearchFilter,
  },
  props: {
    msg: String
  },
  mounted() {
    const { page } = this.$route.query
    console.log('page: ', page)
    // const res = await axios.get('/api')
    // console.log(res)
    window.addEventListener('scroll', this.onScroll)
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.onScroll);
  },
  methods: {
    onScroll() {
      console.log(window.scrollY)
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
