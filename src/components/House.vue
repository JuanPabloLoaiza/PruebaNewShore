<template>
  <b-container fluid>
    <b-row class="mt-5">
      <b-col cols="3">
        <b-form-input
          v-model="findCharacter"
          placeholder="Buscar"
        ></b-form-input>
      </b-col>
      <b-col cols="3" lg="1" align-self="end">
        <p>Ordenar por: </p>
      </b-col>
      <b-col cols="3" lg="1">
        <b-button variant="outline-primary" @click="sortName()">nombre</b-button>
      </b-col>
      <b-col cols="3" lg="1">
        <b-button variant="outline-primary" @click="sortLastName()">apellido</b-button>
      </b-col>
    </b-row>
    <b-row class="mt-5" v-if="findCharacter === '' && $store.state.members.length === 0">
      <b-col cols="6" lg="3" v-for="data in $store.state.houseStudents" :key="data.name" class="mt-3">
        <b-card bg-variant="secondary" tag="article" class="mb-3" style="max-width: 20rem;">
          <b-card-title class="titles">{{ data.name }}</b-card-title>
          <img :src="data.image" class="imagesCharacter" style="margin-bottom: 5px"/>
          <b-card-text class="titleCards">Nombre: {{ data.name }} </b-card-text>
          <b-card-text class="titleCards">Especie: {{ data.species }} </b-card-text>
          <b-card-text class="titleCards">Año de nacimiento: {{ data.yearOfBirth }} </b-card-text>
          <b-card-text class="titleCards">Tipo de sangre:
            <img src="../assets/Images/pure-blood.png" class="imagesBlood" v-if="data.ancestry === 'pure-blood'"/>
            <img src="../assets/Images/half-blood.png" class="imagesBlood" v-else-if="data.ancestry === 'half-blood'"/>
            <img src="../assets/Images/muggleborn.png" class="imagesBlood" v-else-if="data.ancestry === 'muggleborn'"/>
            <img src="../assets/Images/squib.png" class="imagesBlood" v-else />
          </b-card-text>        
        </b-card>
      </b-col>
    </b-row>
    <b-row class="mt-5" v-else-if="findCharacter !== '' || $store.state.members.length > 0">
      <b-col cols="6" lg="3" v-for="data in $store.state.members" :key="data.name" class="mt-3">
        <b-card bg-variant="secondary" tag="article" class="mb-3" style="max-width: 20rem;">
          <b-card-title class="titles">{{ data.name }}</b-card-title>
          <img :src="data.image" class="imagesCharacter" style="margin-bottom: 5px"/>
          <b-card-text class="titleCards;">Nombre: {{ data.name }} </b-card-text>
          <b-card-text class="titleCards">Especie: {{ data.species }} </b-card-text>
          <b-card-text class="titleCards">Año de nacimiento: {{ data.yearOfBirth }} </b-card-text>
          <b-card-text class="titleCards">Tipo de sangre:
            <img src="../assets/Images/pure-blood.png" class="imagesBlood" v-if="data.ancestry === 'pure-blood'"/>
            <img src="../assets/Images/half-blood.png" class="imagesBlood" v-else-if="data.ancestry === 'half-blood'"/>
            <img src="../assets/Images/muggleborn.png" class="imagesBlood" v-else-if="data.ancestry === 'muggleborn'"/>
            <img src="../assets/Images/squib.png" class="imagesBlood" v-else />
          </b-card-text> 
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import axios from "axios";
import store from "vuex";
export default {
  data() {
    return {
      findCharacter: "",
      index: 0
    };
  },
  props: {
    house: String
  },
  watch: {
    findCharacter(value) {
      let names = [];
      let lastNames = [];
      let count = 0;
      this.$store.state.members = [];
      this.$store.state.houseStudents.map((data) => {
        let firstName = data.name.split(" ");
        names.push(firstName[0]);
        lastNames.push(firstName[1]);
      });
      this.$store.state.houseStudents.map((data) => {
        if (value.toLowerCase() === names[count].substring(0, value.length).toLowerCase() ||
        value.toLowerCase() === lastNames[count].substring(0, value.length).toLowerCase() ||
        value.toLowerCase() === data.name.substring(0, value.length).toLowerCase()) {
          this.$store.state.members.push(data);
        }
        count++;
      });
      this.$store.state.members = [...new Set(this.$store.state.members)];
    },
  },
  methods: {
    sortName() {
      let names = [];
      this.$store.state.members = [];
      this.$store.state.houseStudents.map((data) => {
        let firstName = data.name.split(" ");
        names.push(firstName[0]);
      });
      names.sort();
      names.map((data => {
        for (let index = 0; index < this.$store.state.houseStudents.length; index++) {
          let firstName = this.$store.state.houseStudents[index].name.split(" ");
          if (firstName[0] === data) {
            this.$store.state.members.push(this.$store.state.houseStudents[index]);
            break;
          }       
        }
      }));
    },
    sortLastName() {
      let lastNames = [];
      let students = [];
      this.$store.state.members = [];
      this.$store.state.houseStudents.map((data) => {
        let name = data.name.split(" ");
        lastNames.push(name[1]);
        students.push(data);
      });
      lastNames.sort();
      lastNames.map((data) => {
        for (let index = 0; index < students.length; index++) {
          let name = students[index].name.split(" ");
          if (name[1] === data) {
            this.$store.state.members.push(students[index]);
            students.splice(index, 1);
            break;
          }
        }
      });
    }
  }
};
</script>

<style lang="sass">

@font-face
    font-family: 'Harry P'
    src: url("../assets/fonts/HARRYP.TTF") format('truetype')

$font-title: Harry P, Harry P

.imagesCharacter
    object-fit: scale-down
    width: 100%
    height: 30vh

.titles
    font: 200% $font-title

.imagesBlood
  object-fit: scale-down
  width: 20%
  height: 5vh

.titleCards
  font-size: 95%
</style>
