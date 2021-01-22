<template>
  <b-container>
      {{ house }}
  </b-container>
</template>

<script>
import axios from "axios";
import store from "vuex";
export default {
    props: {
        house: String
    },
    created() {
        this.HouseInformation();
    }, 
    methods: {
        async HouseInformation() {
            let data = await axios.get(`http://hp-api.herokuapp.com/api/characters/house/${this.house}`);
            this.$store.state.houseStudents = data.data;
        }
    }
}
</script>

<style>

</style>