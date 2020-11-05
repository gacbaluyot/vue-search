<template>
  <div>
    <div class="row">
      <div class="col-md-12">
        <form class="form-inline" @submit.prevent="onSubmit">
          <div class="form-group ">
            <input name="search" v-model="search" @input="onSearch()" placeholder="Search NPM Package"/>
          </div>
<!--          <button type="submit" class="btn btn-sm btn-primary mb-2">Search</button>-->
        </form>
      </div>
    </div>
    <div v-if="isResult" class="row">
      <div class="col-md-12">
        <div class="searchResult" transition="expand">
          <div v-for="elem in wikiObj" :key="elem.id">
            <ul class="list-group">
              <li class="list-group-item list-group-item-action">
                <a :href="elem.package.links.npm">
                  <div class="float-right"> {{ elem.package.version }}</div>

                  <strong> {{ elem.package.name }} </strong> <br/>
                  {{ elem.package.description }}
                </a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
    <div v-if="submitted" class="row">
      <div class="form-group row">
        Showing All Results
      </div>
      <div class="form-group row">
        <div class="col-md-12">
          <div v-for="elem in wikiObj" :key="elem.id">
            <ul class="list-group">
              <li class="list-group-item list-group-item-action">
                <div class="float-right"> {{ elem.package.version }}</div>
                <strong> {{ elem.package.name }} </strong> <br/>
                {{ elem.package.description }}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      search: null,
      wikiObj: null,
      isResult: false,
      apiUrl: "https://api.npms.io/v2/search?q=",
      submitted: false,
    }
  },
  methods: {
    async onSearch() {
      if (this.search) {
        const apiResult = await this.getApiData();
        const {data} = apiResult;
        this.wikiObj = data.results.slice(0, 8);
        this.isResult = true;
      } else {
        this.wikiObj = [];
      }
    },
    async onSubmit() {
      this.isResult = false;
      this.submitted = true;
      if (this.search) {
        const apiResult = await this.getApiData();
        const {data} = apiResult;
        this.wikiObj = data.results;
      }
    },

    async getApiData() {
      let reqURL = this.apiUrl + this.search;
      return await this.axios.get(reqURL);
    }
  }
};
</script>

<style scoped>


</style>
