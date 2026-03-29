<script setup>
import CardBio from "./Cards/CardBio.vue";
import CardSkills from "./Cards/CardSkills.vue";
import CardProjects from "./Cards/CardProjects.vue";
import CardContact from "./Cards/CardContact.vue";
import { ref } from "vue";
const tab = ref(null);
const tabs = ref([
  {
    title: "BIO",
    icon: "mdi-account-circle",
    component: CardBio,
  },
  {
    title: "SKILLS",
    icon: "mdi-tools",
    component: CardSkills,
  },
  {
    title: "PROJECTS",
    icon: "mdi-application-brackets-outline",
    component: CardProjects,
  },
  {
    title: "Contact",
    icon: "mdi-card-account-mail",
    component: CardContact,
  },
]);
const text = ref("Hello");
</script>

<template>
  <v-card max-width="620">
    <v-tabs
      v-model="tab"
      bg-color="blue-grey darken-2"
      align-tabs="center"
      dark
      stacked
      icons-and-text
    >
      <template v-for="tab in tabs" :key="tab.title">
        <v-tab :value="tab.title">
          {{ tab.title }}
          <v-icon>
            {{ tab.icon }}
          </v-icon>
        </v-tab>
      </template>
    </v-tabs>

    <v-tabs-window v-model="tab">
      <v-tabs-window-item v-for="i in tabs" :key="i" :value="i.title">
        <v-card class="tabs" color="#e4e4e4" flat>
          <v-card-text><component :is="i.component"></component> </v-card-text>
        </v-card>
      </v-tabs-window-item>
    </v-tabs-window>
  </v-card>
</template>

<style>
.tabs {
  max-height: 600px;
  overflow: hidden;
  overflow-y: scroll;
}
</style>
