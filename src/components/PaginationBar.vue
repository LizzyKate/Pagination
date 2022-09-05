<template lang="">
  <div>
    <div class="pagination-row">
      <button class="pagination-button" @click="changePage(pageNumber - 1)" :disabled="pageNumber <= 1">&lt;-</button>
      <div v-for="(item, index) in new Array(numberOfPages)" :key="index">
        <button class="pagination-button" @click="changePage(index + 1)" :class="pageNumber === index + 1 ? 'active' : ''">{{ index + 1 }}</button>
      </div>
      <button class="pagination-button" @click="changePage(pageNumber + 1)" :disabled="pageNumber >= numberOfPages">-&gt;</button>
    </div>
    <h1>Lizzy Repos</h1>
    <div class="__repos">
      <div v-for="(repo, i) in repos" :key="i">
        <div class="repo" @click="numberOfPages()">
          {{ repo.name }}
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "PaginationBar",
  data() {
    return {
      repos: [],
      pageNumber:1,
      totalRepos:111,
      perPage:10
    };
  },
  mounted() {
   this.getRepos();
  },
  methods:{
    getRepos(){
      axios
      .get(`https://api.github.com/users/LizzyKate/repos?page=${this.pageNumber}&per_page=${this.perPage}`)
      .then(response => {
        this.repos = response.data;
      })
      .catch(error => {
        console.log(error);
      });
    },
    changePage(page){
      this.pageNumber = page;
      this.getRepos();
    },
  },
  computed:{
    numberOfPages(){
      return Math.ceil(this.totalRepos / this.perPage);
    }
  },
};
</script>
<style scoped>
.pagination-row {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}
.pagination-button {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 5px 10px;
  margin: 0 5px;
  cursor: pointer;
}
.pagination-button:hover {
  background-color: #eee;
}
h1{
  text-align: center;
}
.__repos {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 20px;
}
.repo {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 5px 10px;
  margin: 0 5px;
  cursor: pointer;
  margin-bottom: 10px;
}
.active {
  background-color: #eee;
  cursor: auto;
} 

:disabled{
  cursor: auto;
}

</style>
