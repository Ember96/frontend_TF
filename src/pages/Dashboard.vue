<template>
  <v-container fluid grid-list-xl>
    <v-layout row wrap>
      <!-- Widgets-->
      <v-flex d-flex lg6 sm6 xs12>
        <widget icon="domain" title="Mes con menor cantidad de reportes" :subTitle=bestMonth[0] :supTitle='bestMonth[1]' color="#00b297"/>
      </v-flex>
      <v-flex d-flex lg6 sm6 xs12>
        <widget icon="money_off" title="Mes con mayor cantidad de reportes" :subTitle=criticalMonth[0] :supTitle='criticalMonth[1]' color="#dc3545"/>
      </v-flex>
      <!-- Widgets Ends -->
      <!-- Statistics -->
      <v-flex d-flex lg6 sm8 xs12>
        <site-view-statistic/>
      </v-flex>
      <v-flex d-flex lg6 sm8 xs12>
        <location-statistic/>
      </v-flex>
      <!-- Statistics Ends -->
      <!-- DataTable&TimeLine Starts -->
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      lorem: `Lorem ipsum dolor sit amet, mel at clita quando.`,
      reportsByMonth: [['Enero', 0], ['Febrero', 0], ['Marzo', 0], ['Abril', 0], ['Mayo', 0], ['Junio', 0], ['Julio', 0], ['Agosto', 0], ['Septiembre', 0], ['Octubre', 0], ['Noviembre', 0], ['Diciembre', 0],],
      criticalMonth: [],
      bestMonth: [],
    }
  },
  methods:{
    async getNumReportsByMonth() {
      var response = await this.$http.get('https://citizen-reports.herokuapp.com/reports/');
      response.data.forEach(element => {
        this.reportsByMonth[parseInt(element.createdAt.slice(element.createdAt.indexOf('-') + 1, element.createdAt.lastIndexOf('-')))][1]++
      });
      console.log(this.reportsByMonth);
      var mayor = 0;
      var monthIndex = 10;
      for (let index = 0; index < this.reportsByMonth.length; index++) {
        if(this.reportsByMonth[index][1] > mayor) {
          mayor = this.reportsByMonth[index][1];
          monthIndex = index
        }
      }
      this.criticalMonth = this.reportsByMonth[monthIndex];
      monthIndex = 10
      var menor = 12
      for (let index = 0; index < this.reportsByMonth.length; index++) {
        if(this.reportsByMonth[index][1] < menor) {
          menor = this.reportsByMonth[index][1];
          monthIndex = index
        }
      }
      this.bestMonth = this.reportsByMonth[monthIndex]
    }
  },
  beforeMount(){
    this.getNumReportsByMonth()
  }
}
</script>

<style>

</style>
