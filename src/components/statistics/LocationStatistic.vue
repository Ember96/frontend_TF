<template>
  <v-card class="card">
    <v-card-text class="pa-2">
      <pie-chart :key=chartKey :data="data" legend="right" :donut="true"></pie-chart>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      data: {'Vialidades': 90, 'Aceras y alumbrado público': 70, 'Espacios y equipamiento público': 40},
      chartKey: 0,
    }
  },
  methods:{
    async countReports() {
      var rawDataVialidades = await this.$http.get('https://citizen-reports.herokuapp.com/reports?report_type__eq=vialidades');
      this.data.Vialidades = rawDataVialidades.data.length;
      var rawDataEspacios = await this.$http.get('https://citizen-reports.herokuapp.com/reports?report_type__eq=espacios%20y%20equipamiento público');
      this.data['Espacios y equipamiento p\u00FAblico'] = rawDataEspacios.data.length;
      var rawDataAceras = await this.$http.get('https://citizen-reports.herokuapp.com/reports?report_type__eq=aceras%20y%20alumbrado público');
      this.data['Aceras y alumbrado p\u00FAblico'] = rawDataAceras.data.length;
      this.chartKey++
      console.log(rawDataEspacios.data.length, rawDataAceras.data.length, rawDataVialidades.data.length);
      console.log(this.data);
    }
  },
  beforeMount() {
    this.countReports()
    console.log(this.data.Vialidades);
  },
}
</script>

<style>
  .card {
    border-radius: 3px;
    background-clip: border-box;
    border: 1px solid rgba(0, 0, 0, 0.125);
    box-shadow: 1px 1px 1px 1px rgba(0, 0, 0, 0.21);
    background-color: transparent;
  }
</style>
