<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && (!results || results.length===0)">No stored experiences found. Start adding some survey results first.</p>
      <ul v-else-if="!isLoading && results && results.length>0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.userName"
          :rating="result.rating"
        ></survey-result>
        
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';
export default {
  components: {
    SurveyResult,
  },
  data(){
    return {
      results:[],
      isLoading: false,
    };
  },
  methods: {
    async loadExperiences(){
      this.isLoading = true;
      const BASE_API_URL = 'https://vue-http-project-f36ed-default-rtdb.europe-west1.firebasedatabase.app/';
      const url = new URL('surveys.json',BASE_API_URL);
      const response = await fetch(url);
      if(response.ok){
      const resData = await response.json();
      this.isLoading =false;

      for(const key in resData){
        resData[key].id = Math.random();
        this.results.push(resData[key]);        
      }
      console.log(this.results); 
      } else {
        this.isLoading = true;
      }
    }
  },
  mounted() {
    this.loadExperiences();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>