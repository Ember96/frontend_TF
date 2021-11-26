<template>
  <v-footer height="auto" dark>
    <v-card
      flat
      tile
      :color="$root.themeColor"
      class="text-xs-center">
      <v-card-text>
        <v-btn
          v-for="icon in icons"
          :key="icon"
          class="mx-3 white--text"
          icon>
          <v-icon size="24px">{{ icon }}</v-icon>
        </v-btn>
      </v-card-text>

      <v-card-text class="white--text pt-0">
        Todos los derechos reservados, entonces todos los zurdos son extrovertidos jajajajajajajaja. No, ya en serio, propiedad intelectual del Centro de Altos Estudios CICESE, favor de respetar el reglamento interno del sitio accesible desde la barra superior, justo de la derecha del rating que agradecería se tome un segundo para valorar. Para mayor información contácteme en mis redes sociales arriba expuestas.
      </v-card-text>

      <v-divider></v-divider>

      <v-card-text class="white--text">
        &copy;2021 — <strong>Ember</strong>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
export default {
  data() {
    return {
      icons: [
        'fab fa-facebook',
        'fab fa-twitter',
        'fab fa-telegram',
      ]
    }
  }
}
</script>

<style>

</style>
