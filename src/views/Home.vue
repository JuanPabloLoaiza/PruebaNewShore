<template>
  <b-container>
      <b-row>
          <b-col cols="3">

          </b-col>
      </b-row>
  </b-container>
</template>

<script>
import axios from "axios";
export default {
    data() {
        return {
            gryffindorHouse: null,
            slytherinHouse: null,
            ravenclawHouse: null,
            hufflepuffHouse: null
        }
    },
    created() {
        this.Houses();
    },
    methods: {
        async Houses() {
            let data = await axios.get('http://hp-api.herokuapp.com/api/characters/house/gryffindor');
            this.gryffindorHouse = data.data;
            data = await axios.get('http://hp-api.herokuapp.com/api/characters/house/slytherin');
            this.slytherinHouse = data.data;
            data = await axios.get('http://hp-api.herokuapp.com/api/characters/house/ravenclaw');
            this.ravenclawHouse = data.data;
            data = await axios.get('http://hp-api.herokuapp.com/api/characters/house/hufflepuff');
            this.hufflepuffHouse = data.data;
        }
    }
}
</script>

<style>

</style>