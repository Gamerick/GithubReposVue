<template>
  <b-container class="hello">
    <h1>Github Public Repos</h1>

    <b-row>
      <b-col md>
        <label for="search-box">Search: </label>
        <input id="search-box" v-model="filterString" />
      </b-col>
      <b-col md>
        <b-pagination
          v-model="currentPage"
          :per-page="perPage"
          :total-rows="rows"
          aria-controls="my-table"
          align="right"
        />
      </b-col>
    </b-row>

    <b-table
      id="my-table"
      :items="repos"
      :per-page="perPage"
      :current-page="currentPage"
      :filter="filterString"
      :fields="fields"
      small
      striped
      hover
    />
  </b-container>
</template>

<script>
import axios from 'axios';
/**
 * Queries the Github API to get the list of all repos
 * By default 100 entries are returned
 */

export default {
  name: 'RepoList',
  props: {
    apiURI: String
  },
  data () {
    return {
      currentPage: 1, //The current page being viewed.
      perPage: 10,
      rows: 100, // The number of items returned when requesting a list of public repositories
      filterString: "",
      repos: [],
      fields: [
        {key: 'name', sortable: true},
        {key: 'description', sortable: false},
        {key: 'owner', sortable: true},
      ]
    }
  },
  mounted () {
    this.getRepoList();
  },
  methods : {
    getRepoList: function() {
      console.log('Updating table');
      axios.get(`${this.apiURI}/repositories`)
        .then((response) => {
        // The response is an array of repos
        // The following pulls out only that info we care about
        var parsedRepos = [];
        response.data.forEach(element => {
          parsedRepos.push({
            name: element.name,
            description: element.description,
            owner: element.owner.login,
            avatar_URL: element.owner.avatar_url // Couldn't figure out turning urls into images in a v-table
          })
        });
        this.repos = parsedRepos;
      }).catch((error) => {
        console.log(error);
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
