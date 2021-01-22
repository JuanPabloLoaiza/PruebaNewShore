<template>
  <b-container>
    <b-row class="mt-4">
      <b-col cols="3">
        <img
          src="../assets/Images/Gryffindor.png"
          width="100%"
          @click="Houses('gryffindor')"
        />
      </b-col>
      <b-col cols="3">
        <img
          src="../assets/Images/Slytherin.png"
          width="100%"
          @click="Houses('slytherin')"
        />
      </b-col>
      <b-col cols="3">
        <img
          src="../assets/Images/Ravenclaw.png"
          width="100%"
          @click="Houses('ravenclaw')"
        />
      </b-col>
      <b-col cols="3">
        <img
          src="../assets/Images/Hufflepuff.png"
          width="100%"
          @click="Houses('hufflepuff')"
        />
      </b-col>
    </b-row>
    <House :house="houseName" v-if="houseName !== null"></House>
  </b-container>
</template>

<script>
import axios from "axios";
import House from "@/components/House";
export default {
  components: {
    House,
  },
  data() {
    return {
      houseName: null
    };
  },
  methods: {
    async Houses(house) {
      this.$store.state.members = [];
      this.houseName = house;
      let data = await axios.get(
        `http://hp-api.herokuapp.com/api/characters/house/${this.houseName}`
      );
      this.$store.state.houseStudents = data.data;
    },
  },
};
</script>

<style lang="sass">
</style>
