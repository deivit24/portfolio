<script setup>
import SingleProject from "./SingleProject.vue";
import { projects } from "../../helpers/projectHelper";
import { ref } from "vue";

const allProjects = ref(projects);
</script>
<template>
  <div>
    <h1>My Projects</h1>
    <hr />
    <v-row class="pa-3">
      <template v-for="i in allProjects" :key="i.name">
        <v-col cols="6">
          <SingleProject
            :name="i.name"
            :img="i.image"
            :stack="i.stack"
            :desc="i.desc"
            :git="i.git"
            :url="i.url"
          />
        </v-col>
      </template>
    </v-row>
  </div>
</template>

<style scope>
h1 {
  color: orangered;
  margin-bottom: 10px;
}
</style>
