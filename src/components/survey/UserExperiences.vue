<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <ul>
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
    };
  },
  methods: {
    async loadExperiences(){
      const BASE_API_URL = 'https://vue-http-project-f36ed-default-rtdb.europe-west1.firebasedatabase.app/';
      const url = new URL('surveys.json',BASE_API_URL);
      const response = await fetch(url);
      if(response.ok){
      const resData = await response.json();
  
      for(const key in resData){
        resData[key].id = Math.random();
        this.results.push(resData[key]);        
      }
      console.log(this.results); 
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