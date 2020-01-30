<template>
  <v-form class="mt-4">
    <v-text-field color="deep-purple darken-3" outlined label="House Number" />
    <v-text-field color="deep-purple darken-3" outlined label="Street Name" />
    <v-text-field color="deep-purple darken-3" outlined label="City" />
    <v-text-field color="deep-purple darken-3" outlined label="State" />
    <v-text-field
      color="deep-purple darken-3"
      outlined
      type="tel"
      label="Zip Code"
    />
    <v-btn
      block
      depressed
      dark
      x-large
      color="deep-purple darken-3"
      @click.native="locateMe"
      >Get Geolocation</v-btn
    >
    <span class="subheading">Location:</span>
    <v-row v-if="this.location">
      <v-chip v-if="this.location.coords">
        {{ `Latitude: ${this.location.coords}` }}
      </v-chip>
    </v-row>
  </v-form>
</template>

<script>
export default {
  data: () => ({
    chips: [],
    location: null,
    snackbar: false,
    gettingLocation: false,
    errorStr: null
  }),
  components: {},
  methods: {
    async locateMe() {
      this.gettingLocation = true
      if (!('geolocation' in navigator)) {
        this.snackbar = true
        this.errorStr = 'Geolocation service is not available.'
      } else {
        await navigator.geolocation.getCurrentPosition(
          (success) => {
            this.snackbar = true
            this.location = success
            console.log('success', success)
          },
          (error) => {
            console.log('error', error)
          }
        )
      }
    }
  }
}
</script>
