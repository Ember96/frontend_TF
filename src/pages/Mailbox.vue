<template>
  <v-container fluid grid-list-xl>
    <v-layout row wrap>
      <v-flex d-flex lg12 sm12 xs12>
      <v-card style="margin-bottom: 150px;">
        <v-toolbar color="blue-grey darken-4" dark>
          <v-toolbar-side-icon></v-toolbar-side-icon>

          <v-toolbar-title>Listado de Reportes</v-toolbar-title>

          <v-spacer></v-spacer>
          <v-btn
            class="mx-1"
            fab
            dark
            color="green"
            small
            @click="launchCreation()"
          >
            <v-icon dark>
              fas fa-plus
            </v-icon>
          </v-btn>
          <v-btn icon>
            <v-icon>search</v-icon>
          </v-btn>
        </v-toolbar>

        <v-list two-line :id="ids">
          <template v-for="(item, index) in reports">
            <v-divider
              :key="index"
              :inset="item.inset"
            ></v-divider>

            <v-list-tile
              :key="item.title"
              avatar
              @click="openDetail(item._id)"
            >
              <v-list-tile-avatar>
                <img :src="'https://cdn.vuetifyjs.com/images/lists/' + index + '.jpg'">
              </v-list-tile-avatar>

              <v-list-tile-content>
                <v-list-tile-title v-html="item.title"></v-list-tile-title><span v-if="item.solved" style="color:red"><b>Resuelto</b></span>
                <v-list-tile-sub-title v-html="item.description_short"></v-list-tile-sub-title>
              </v-list-tile-content>
              <v-spacer></v-spacer>
                 
                <v-list-tile-action>
                  <v-list-tile-action-text>En {{ item.neighborhood }}</v-list-tile-action-text>
                  <div>
                     <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="red"
                      small
                      @click="deleteReport(item._id)"
                    >
                      <v-icon dark>
                        far fa-trash-alt
                      </v-icon>
                    </v-btn>
                  <v-icon v-if="item.backings.length == 0" @click="item.isStarred = !item.isStarred" color="grey lighten-1" >star_border</v-icon>
                  <v-icon v-else @click="item.isStarred = !item.isStarred" color="yellow darken-2">star</v-icon>
                  <span v-if="item.backings" style="color:gray">{{item.backings.length}}&nbsp&nbsp</span>
                  </div>
                </v-list-tile-action>
            </v-list-tile>
          </template>
        </v-list>
      </v-card>
      </v-flex>
    </v-layout>
    
    <div class="text-center">
    <v-dialog
      v-model="modal"
      width="500"
    >
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          {{action}}
        </v-card-title>

        <v-card-text>
          <v-text-field
            label="Título del Reporte"
            :value="report.title"
            v-model="report.title"
          ></v-text-field>
          <v-text-field
            label="Autor del Reporte"
            :value="report.author_username"
            v-model="report.author_username"
          ></v-text-field>
          <v-text-field
            label="Colonia de la incidencia"
            v-model="report.neighborhood"
          ></v-text-field>
          <v-text-field
            label="Descripción corta"
            v-model="report.description_short"
          ></v-text-field>
          <v-text-field
            label="Descripción completa"
            v-model="report.description_full"
          ></v-text-field>
          <v-select
            :items="types"
            label="Tipo de Reporte"
            solo
            v-model="report.report_type"
          ></v-select>
          <v-btn
            class="ma-2"
            outlined
            color="green"
            dark
            @click="getLocation()"
          >
            Obtener ubicación
          </v-btn>
          <v-text-field
            :id="ids"
            label="Longitud"
            v-model="report.loc.coordinates[0]"
          ></v-text-field>
          <v-text-field
            :id="ids"
            label="Latitud"
            v-model="report.loc.coordinates[1]"
          ></v-text-field>
          <v-switch
            v-model="report.solved"
            label="¿Está resuelto?"
          ></v-switch>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            v-if="action=='Actualizar Reporte'"
            color="primary"
            text
            @click="updateReport()"
          >
           Actualizar
          </v-btn>
          <v-btn
            v-if="action=='Crear Reporte'"
            color="primary"
            text
            @click="createReport()"
          >
           Crear
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      modal : false,
      action: "Crear Reporte",
      reports: [],
      ids: "0",
      report: {
        title: "",
        neighborhood: "",
        author_username: "",
        report_type: "",
        loc: {
          type: "",
          coordinates:
            [
              0,
              0
            ]
        },
        description_short: "",
        description_full: ""
      },
      types: []
    }
  },

  methods: {
    async getLocation() {
        await this.$getLocation({enableHighAccuracy: true}).then(coordinates => {
          this.report.loc.coordinates[0] = coordinates.lng
          this.report.loc.coordinates[1] = coordinates.lat
          this.ids = this.ids + "1"
          console.log(coordinates, this.report.loc.coordinates);
        })
    },
    async openDetail(id) {
      var response = await this.$http.get('https://citizen-reports.herokuapp.com/reports/' + id);
      console.log(response.data);
      this.report = response.data;
      this.getTypes();
      this.action = "Actualizar Reporte"
      this.modal = true
    },
    async getReports() {
      var response = await this.$http.get('https://citizen-reports.herokuapp.com/reports/');
      this.reports = response.data
    },
    async updateReport() {
      var response = await this.$http.put('https://citizen-reports.herokuapp.com/reports/' + this.report._id, this.report);
      console.log(response, "lo q se envió", this.report);
      this.modal = false
      this.ids = this.ids + "1"
    },
    async getTypes() {
      var response = await this.$http.get("https://citizen-reports.herokuapp.com/report_types/");
      response.data.forEach(element => {
        this.types.push(element.name)
      });
      console.log(this.types, response.data);
    },
    launchCreation() {
      this.action = "Crear Reporte"
      this.modal = true
      this.getTypes()
    },
    async createReport() {
      var response = await this.$http.post('https://citizen-reports.herokuapp.com/reports/', this.report);
      console.log(response, "lo q se envió", this.report);
      this.modal = false
      this.ids = this.ids + "1"
    },
    async deleteReport(id) {
      var response = await this.$http.delete('https://citizen-reports.herokuapp.com/reports/' + id);
    },
  },
  beforeMount() {
    this.getReports()
  }
}
</script>

<style>

</style>
