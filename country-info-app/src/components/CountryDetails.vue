<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-6">
        <h1>Länderinformation</h1>
        <form @submit.prevent="fetchCountryData">
          <div class="mb-3">
            <label for="countryName" class="form-label">Bitte Ländernamen eingeben</label>
            <input type="text" id="countryName" v-model="countryName" class="form-control" placeholder="Deutschland">
          </div>
          <button :disabled="isLoading" type="submit" class="btn btn-primary">{{ isLoading ? 'Bitte warten...' : 'Informationen anzeigen' }}</button>
        </form>
        <br>
        <div v-if="error" class="alert alert-warning" role="alert">{{ error }}</div>
        <div v-if="country && !isLoading && !error">
          <hr>
          <br>
          <table class="table table-striped table-bordered">
            <tr>
              <th>Name</th>
              <td>{{ country.name.official }} ({{ country.name.common }})</td>
            </tr>
            <tr>
              <th>Hauptstadt</th>
              <td>{{ countryCapitals }}</td>
            </tr>
            <tr>
              <th>Region</th>
              <td>{{ country.region }}</td>
            </tr>
            <tr>
              <th>Unterregion</th>
              <td>{{ country.subregion }}</td>
            </tr>
            <tr>
              <th>Grenzen zu</th>
              <td>{{ countryBorders }}</td>
            </tr>
            <tr>
              <th>Einwohner</th>
              <td>{{ country.population }}</td>
            </tr>
            <tr>
              <th>Sprachen</th>
              <td>{{ countryLanguages }}</td>
            </tr>
            <tr>
              <th>Flagge</th>
              <td><img :src="country.flag" alt="flagge"></td>
            </tr>
            <tr>
              <th>Fläche</th>
              <td>{{ country.area }} km²</td>
            </tr>
            <tr>
              <th>Zeitzonen</th>
              <td>{{ countryTimezones }}</td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      countryName: '',
      country: null,
      error: null,
      isLoading: false
    };
  },
  methods: {
    async fetchCountryData() {
      this.error = null;
      this.isLoading = true;
      try {
        const response = await axios.get(process.env.VUE_APP_API_URL + this.countryName);
        this.country = response.data[0];
      } catch (error) {
        this.error = "Die API von 'restcountries' ist gerade nicht erreichbar. Bitte versuche es erneut.";
      } finally {
        this.isLoading = false;
      }
    }
  },
  computed: {
    countryLanguages() {
      return this.country ? Object.values(this.country.languages).join(', ') : '';
    },
    countryBorders() {
      return this.country ? Object.values(this.country.borders).join(', ') : '';
    },
    countryCapitals() {
      return this.country ? Object.values(this.country.capital).join(', ') : '';
    },
    countryTimezones() {
      return this.country ? Object.values(this.country.timezones).join(', ') : '';
    }
  }
}
</script>