<template>
  <div>
    <v-card flat>
      <v-card-text class="pb-0">
        <v-form>
          <v-text-field
            color="deep-purple darken-3"
            outlined
            label="House Number"
          />
          <v-text-field
            color="deep-purple darken-3"
            outlined
            label="Street Name"
          />
          <v-text-field color="deep-purple darken-3" outlined label="City" />
          <v-text-field color="deep-purple darken-3" outlined label="State" />
          <v-text-field
            color="deep-purple darken-3"
            outlined
            type="tel"
            label="Zip Code"
          />
        </v-form>
      </v-card-text>
      <template v-if="location">
        <v-divider />
        <v-subheader>Location:</v-subheader>
        <v-card-actions class="pt-0">
          <v-chip class="mr-4">Lat: {{ location.coords.latitude }}</v-chip>
          <v-chip>Long: {{ location.coords.longitude }}</v-chip>
          <v-spacer />
          <v-btn icon color="red"><v-icon>mdi-close</v-icon></v-btn>
        </v-card-actions>
        <v-divider />
      </template>

      <v-card-actions v-else>
        <v-btn
          block
          depressed
          color="deep-purple darken-3"
          dark
          @click="locateMe"
          >Get geolocation</v-btn
        >
      </v-card-actions>

      <v-card-actions class="mt-4">
        <v-spacer />
        <v-btn depressed large dark color="deep-purple darken-3">Save</v-btn>
      </v-card-actions>
    </v-card>
    <v-snackbar v-model="snackbar" multi-line :color="snackbarColor">
      <span>
        {{ snackbarText }}
      </span>
    </v-snackbar>
  </div>
</template>

<script>
export default {
  data: () => ({
    chips: [],
    loading: false,
    location: null,
    snackbar: false,
    snackbarColor: null,
    snackbarText: null,
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
            this.snackbarColor = 'green darken-1'
            this.snackbarText = `Your location data is ${success.coords.latitude}, ${success.coords.longitude}`
            this.location = success
            this.gettingLocation = false
            console.log('success', success)
          },
          (error) => {
            console.log('error', error)
            this.snackbar = true
            this.snackbarColor = 'red darken-1'
            if (error.code === 1) {
              this.snackbarText =
                'Please allow location services to use this feature.'
            } else if (error.code === 2) {
              this.snackbarText = "Can't get GPS coordinates."
            } else if (error.code === 3) {
              this.snackbarText =
                'It took too long to get coordinates. Please try again.'
            }
            this.errorStr = error
            this.gettingLocation = false
          }
        )
      }
    }
  }
}
</script>
