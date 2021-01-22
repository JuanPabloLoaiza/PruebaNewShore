<template>
  <b-container>
    <b-row class="mt-5">
      <b-col cols="3">
        <b-form-input
          v-model="findCharacter"
          placeholder="Buscar"
        ></b-form-input>
      </b-col>
      <b-col cols="2" align-self="end">
        <p>Ordenar por: </p>
      </b-col>
      <b-col cols="1">
        <b-button variant="outline-primary" @click="sortName()">nombre</b-button>
      </b-col>
      <b-col cols="1">
        <b-button variant="outline-primary" @click="sortLastName()">apellido</b-button>
      </b-col>
    </b-row>
    <b-row class="mt-5" v-if="findCharacter === '' && $store.state.members.length === 0">
      <b-col cols="3" v-for="data in $store.state.houseStudents" :key="data.name" class="mt-3">
        <b-card tag="article" class="mb-3" style="max-width: 20rem;">
          <b-card-title class="titulos">{{ data.name }}</b-card-title>
          <img :src="data.image" class="imagenes" />
          <b-card-text class="mt-3">Hola, soy {{ data.name }} </b-card-text>
        </b-card>
      </b-col>
    </b-row>
    <b-row class="mt-5" v-else-if="findCharacter !== '' || $store.state.members.length > 0">
      <b-col cols="3" v-for="data in $store.state.members" :key="data.name" class="mt-3">
        <b-card tag="article" class="mb-3" style="max-width: 20rem;">
          <b-card-title class="titulos">{{ data.name }}</b-card-title>
          <img :src="data.image" class="imagenes" />
          <b-card-text class="mt-3">Hola, soy {{ data.name }} </b-card-text>
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

.imagenes
    object-fit: scale-down
    width: 100%
    height: 30vh

.titulos
    font: 200% $font-title
</style>
