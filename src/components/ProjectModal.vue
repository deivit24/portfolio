<script setup>
import { ref } from "vue";

const dialog = ref(false);
const props = defineProps({
  name: String,
  stack: String,
  git: String,
  url: String,
  desc: String,
});
</script>
<template>
  <div class="text-center">
    <v-dialog v-model="dialog" width="500">
      <template v-slot:activator="{ props: activatorProps }">
        <v-btn
          class="project-button"
          color="primary"
          dark
          v-bind="activatorProps"
        >
          Learn More
        </v-btn>
      </template>
      <v-card>
        <v-card-title style="display: flex">
          {{ props.name }}
          <v-spacer></v-spacer>
          <v-btn color="error" variant="outlined" @click="dialog = false">
            Close
          </v-btn>
        </v-card-title>
        <h3 class="ml-6">Stack:</h3>
        <v-card-text>
          {{ props.stack }}
        </v-card-text>
        <h3 class="ml-6">Description:</h3>
        <v-card-text>
          {{ props.desc }}
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text :href="props.url">
            <v-icon class="mr-2">mdi-web</v-icon>
            Visit
          </v-btn>
          <v-btn color="default" text :href="props.git">
            <v-icon class="mr-2">mdi-github</v-icon>
            Git Repo
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<style></style>
