<template>
  <div v-if="locations">
    <v-card
      outlined
      v-for="(item, index) in locations"
      :key="index"
      class="mb-4"
    >
      <v-card-text class="overline py-2">
        {{ item.firstName }} {{ item.lastName }}
      </v-card-text>

      <v-divider />
      <v-card-text class="subtitle-2">
        {{ item.addressNumber }} {{ item.addressStreet }}<br />{{
          item.addressCity
        }}
        {{ item.addressState }} {{ item.addressZip }}
      </v-card-text>
      <v-card-text v-if="item.notes">
        <h1 class="subtitle-2">Notes:</h1>
        <p>{{ item.notes }}</p>
      </v-card-text>
      <v-list color="grey lighten-3">
        <v-list-item
          target="_blank"
          :href="
            `http://maps.google.com/maps?q=loc:${item.latitude},${item.longitude}`
          "
        >
          <v-list-item-avatar>
            <v-icon>mdi-map-marker</v-icon>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-subtitle
              >Latitude: {{ item.latitude }}</v-list-item-subtitle
            >
            <v-list-item-subtitle
              >Longitude: {{ item.longitude }}</v-list-item-subtitle
            >
          </v-list-item-content>
          <v-list-item-action>
            <v-icon>mdi-open-in-new</v-icon>
          </v-list-item-action>
        </v-list-item>
      </v-list>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({}),
  async asyncData() {
    const locations = await JSON.parse(localStorage.getItem('locations'))

    return { locations }
  }
}
</script>
