<template>
  <v-card class="card">
    <v-card-text class="pa-3">
      <bar-chart :key=chartKey :data="data" xtitle="Cantidad de reportes" ytitle="Colonias" :dataset="{borderWidth: 3}"/>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      data: {
        'Punta Banda 1': 0,
        'Playa Hermosa': 0,
        'Acapulco': 0,
        'Zona Centro': 0,
        },
      chartKey: 0,
    }
  },
  methods:{
    async countReports() {
      var rawDataPunta = await this.$http.get('https://citizen-reports.herokuapp.com/reports?neighborhood__eq=Punta%20Banda$201');
      this.data['Punta Banda 1'] = rawDataPunta.data.length;
      var rawDataPlaya = await this.$http.get('https://citizen-reports.herokuapp.com/reports?neighborhood__eq=Playa%20Hermosa');
      this.data['Playa Hermosa'] = rawDataPlaya.data.length;
      var rawDataAcapulco = await this.$http.get('https://citizen-reports.herokuapp.com/reports?neighborhood__eq=Acapulco');
      this.data['Acapulco'] = rawDataAcapulco.data.length;
      var rawDataZona = await this.$http.get('https://citizen-reports.herokuapp.com/reports?neighborhood__eq=Zona%20Centro');
      this.data['Zona Centro'] = rawDataZona.data.length;
      this.chartKey++
      console.log(rawDataZona.data.length, rawDataAcapulco.data.length, rawDataPlaya.data.length, rawDataPunta.data.length);
      console.log(this.data);
    }
  },
  mounted() {
    this.countReports()
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
