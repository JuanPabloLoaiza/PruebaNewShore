<template>
  <b-container>
      <b-row class="mt-4">
        <b-col cols="3">
          <img src="../assets/Images/Gryffindor.png" width="100%" @click="Houses('gryffindor')"/>
        </b-col>
        <b-col cols="3">
          <img src="../assets/Images/Slytherin.png" width="100%" @click="Houses('slytherin')"/>
        </b-col>
        <b-col cols="3">
          <img src="../assets/Images/Ravenclaw.png" width="100%" @click="Houses('ravenclaw')"/>
        </b-col>
        <b-col cols="3">
          <img src="../assets/Images/Hufflepuff.png" width="100%" @click="Houses('hufflepuff')"/>
        </b-col>
      </b-row>
      <b-row>
          <b-col cols="3">
              <House :house="houseName" v-if="houseName !== null"></House>
          </b-col>
      </b-row>
  </b-container>
</template>

<script>
import House from '@/components/House';
export default {
    components: {
        House
    },
    data() {
        return {
            houseName: null
        }
    },
    methods: {
        Houses(house) {
            this.houseName = house;
        }
    }
}
</script>

<style>

</style>