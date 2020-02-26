<template>
  <div>
    <v-card flat>
      <v-card-title class="title font-weight-regular justify-space-between">
        <span>{{ currentTitle }}</span>
        <v-avatar
          color="primary lighten-2"
          class="subheading white--text"
          size="24"
          v-text="step"
        ></v-avatar>
      </v-card-title>
      <v-window v-model="step">
        <v-window-item :value="1">
          <v-card-text>
            <v-text-field
              color="deep-purple darken-3"
              outlined
              v-model="customerLocation.addressNumber"
              label="House Number"
            />
            <v-text-field
              color="deep-purple darken-3"
              outlined
              v-model="customerLocation.addressStreet"
              label="Street Name"
            />
            <v-text-field
              color="deep-purple darken-3"
              outlined
              label="City"
              v-model="customerLocation.addressCity"
            />
            <v-text-field
              color="deep-purple darken-3"
              outlined
              label="State"
              v-model="customerLocation.addressState"
            />
            <v-text-field
              color="deep-purple darken-3"
              outlined
              type="tel"
              label="Zip Code"
              v-model="customerLocation.addressZip"
            />
          </v-card-text>
        </v-window-item>

        <v-window-item :value="2">
          <v-card-text>
            <v-text-field
              color="deep-purple darken-3"
              outlined
              v-model="customerLocation.firstName"
              label="First Name"
            />
            <v-text-field
              color="deep-purple darken-3"
              outlined
              v-model="customerLocation.lastName"
              label="Last Name"
            />
            <v-text-field
              color="deep-purple darken-3"
              outlined
              label="Email"
              v-model="customerLocation.customerEmail"
            />
            <v-text-field
              color="deep-purple darken-3"
              outlined
              label="Phone"
              v-model="customerLocation.customerPhone"
            />
          </v-card-text>
        </v-window-item>

        <v-window-item :value="3">
          <v-btn
            large
            block
            dark
            :loading="gettingLocation"
            depressed
            color="deep-purple darken-3"
            @click="locateMe"
            >Get Geolocation</v-btn
          >
          <v-list v-if="customerLocation.latitude">
            <v-list-item>
              <v-list-item-avatar>
                <v-icon color="green">mdi-latitude</v-icon>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-subtitle>Latitude:</v-list-item-subtitle>
                <v-list-item-title>{{
                  customerLocation.latitude
                }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item>
              <v-list-item-avatar>
                <v-icon color="green">mdi-longitude</v-icon>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-subtitle>Longitude:</v-list-item-subtitle>
                <v-list-item-title>{{
                  customerLocation.longitude
                }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
          <v-textarea
            color="deep-purple darken-3"
            outlined
            label="Notes"
            class="mt-4"
            v-model="customerLocation.notes"
          />
        </v-window-item>

        <v-window-item :value="4">
          <v-list>
            <v-list-item>
              <v-list-item-avatar>
                <v-icon>mdi-account</v-icon>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-subtitle>Customer Name:</v-list-item-subtitle>
                <v-list-item-title
                  >{{ customerLocation.firstName }}
                  {{ customerLocation.lastName }}</v-list-item-title
                >
              </v-list-item-content>
            </v-list-item>
            <v-list-item>
              <v-list-item-avatar>
                <v-icon>mdi-map</v-icon>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-subtitle>Customer Address:</v-list-item-subtitle>
                <v-list-item-title
                  >{{ customerLocation.addressNumber }}
                  {{ customerLocation.addressStreet }}</v-list-item-title
                >
              </v-list-item-content>
            </v-list-item>
            <v-list-item v-if="customerLocation.latitude">
              <v-list-item-avatar>
                <v-icon>mdi-map-marker</v-icon>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-subtitle
                  >Latitude:
                  {{ customerLocation.latitude }}</v-list-item-subtitle
                >
                <v-list-item-subtitle
                  >Longitude:
                  {{ customerLocation.longitude }}</v-list-item-subtitle
                >
              </v-list-item-content>
            </v-list-item>
          </v-list>
          <v-card flat v-if="customerLocation.notes">
            <v-subheader>Notes:</v-subheader>
            <v-card-text>{{ customerLocation.notes }}</v-card-text>
          </v-card>
        </v-window-item>
      </v-window>
      <v-divider></v-divider>

      <v-card-actions>
        <v-row justify="space-between" align="center">
          <v-btn :disabled="step === 1" text @click="step--">
            Back
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn
            :disabled="step === 4"
            v-if="step < 4"
            color="deep-purple darken-3"
            x-large
            dark
            depressed
            @click="step++"
          >
            Next
            <v-icon large right>mdi-chevron-right</v-icon>
          </v-btn>
          <v-btn
            v-if="step === 4"
            color="primary"
            depressed
            @click="addLocation"
          >
            Submit
          </v-btn>
        </v-row>
      </v-card-actions>
      <!-- <v-card-text class="pb-0">
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
      </v-card-actions> -->
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
    locations: [],
    customerLocation: {
      firstName: null,
      lastName: null,
      customerEmail: null,
      customerPhone: null,
      addressNumber: null,
      addressStreet: null,
      notes: null,
      addressState: null,
      addressCity: null,
      addressZip: null,
      latitude: null,
      longitude: null
    },
    firstName: null,
    lastName: null,
    customerEmail: null,
    customerPhone: null,
    addressNumber: null,
    addressStreet: null,
    notes: null,
    addressState: null,
    addressCity: null,
    addressZip: null,
    chips: [],
    loading: false,
    location: null,
    step: 1,
    snackbar: false,
    snackbarColor: null,
    snackbarText: null,
    gettingLocation: false,
    errorStr: null
  }),
  components: {},
  computed: {
    currentTitle() {
      switch (this.step) {
        case 1:
          return 'Address'
        case 2:
          return 'Customer Information'
        case 3:
          return 'Geolocation'
        case 4:
          return 'Final Check'
        default:
          return 'Account created'
      }
    }
  },
  methods: {
    addLocation() {
      // const location = this.customerLocation
      // this.locations.push(location)
      this.saveLocations()
    },
    saveLocations() {
      let tempLocations = []
      if (localStorage.getItem('locations')) {
        const parsedLocations = JSON.parse(localStorage.getItem('locations'))
        tempLocations = parsedLocations
      }
      tempLocations.push(this.customerLocation)
      const parsed = JSON.stringify(tempLocations)
      localStorage.setItem('locations', parsed)
      this.$router.push('locations')
    },
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
            this.customerLocation.latitude = success.coords.latitude
            this.customerLocation.longitude = success.coords.longitude
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
