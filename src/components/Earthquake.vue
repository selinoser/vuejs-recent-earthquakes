<template>
  <div class="container">
      <div class="navbar navbar-light bg-light mb-2" v-if="this.items.length > 0">
        <input type="text" class="form-control" v-model="search" placeholder="Lokasyona göre ara..."/>
      </div>
      <Spinner :start="this.items.length == 0" />
      <table class="table table-hover" v-if="this.items.length > 0">
        <thead class="thead-light">
          <tr>
            <th>Tarih</th>
            <th>Yer</th>
            <th>Enlem</th>
            <th>Boylam</th>
            <th>Derinlik</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in filteredList" :key="item.hash">
            <td>{{item.date}}</td>
            <td>{{item.lokasyon}}</td>
            <td>{{item.lat}}</td>
            <td>{{item.lng}}</td>
            <td>{{item.depth}}</td>
            <td class="earthquakeColor" :style="getBg(item.mag)"><strong style="font-size:16px">{{item.mag}}</strong></td>
          </tr>
        </tbody>
      </table>
  </div>
</template>

<script>
import axios from 'axios';
import Spinner from './Spinner'
export default {
  components: {
    Spinner
  },
  created() {
    this.getEarthquake();
  },
  data() {
    return {
      items: [],
      search: '',
      magBg : '',
      loading : false
    };
  },
  methods: {
    getEarthquake() {
      axios
        .get("https://api.orhanaydogdu.com.tr/deprem/live.php?limit=200")
        .then(response => {
          this.items = response.data.result;
        })
        .catch(error => {
          this.errors.push(error);
        });
    },
    getBg(mag) {
        if(mag >= 0 && mag <= 2) return { 'background-color' : '#f1f129' };
        if(mag > 2 && mag <= 4) return { 'background-color' : '#eca71d' };
        if(mag > 4 && mag <= 5) return { 'background-color' : '#f48700' };
        return { 'background-color' : '#e72525' };
    }
  },
  computed : {
    filteredList() {
      return this.items.filter(item => {
        return item.lokasyon.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  }
}
</script>

<style scoped>
.earthquakeColor {
  text-align: center;
}
</style>