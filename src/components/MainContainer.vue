<template>
  <div>
    <h3>MUSINSA 과제</h3>
    <SearchFilter />
    <ListContainer />
  </div>
</template>

<script>
// import axios from 'axios'
import SearchFilter from "@/components/SearchFilter";
import ListContainer from "@/components/list/ListContainer";
export default {
  name: 'MainContainer',
  components: {
    ListContainer,
    SearchFilter,
  },
  props: {
    msg: String
  },
  async mounted() {
    // const res = await axios.get('/api')
    // console.log(res)
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
