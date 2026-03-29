<script setup>
import SingleSkill from "./Cards/SingleSkill.vue";
import { frontendSkills, backendSkills, tools } from "../helpers/skillHelper";
import { ref } from "vue";

const frontend = ref(frontendSkills);
const backend = ref(backendSkills);
const seTools = ref(tools);
</script>
<template>
  <div>
    <h1>My Skills</h1>
    <hr />
    <h2>Frontend Skills</h2>
    <ul className="skill-set__list">
      <template v-for="skill in frontend" :key="skill.name">
        <SingleSkill
          :name="skill.name"
          :date="skill.date"
          :img="skill.image"
          :num="skill.num"
        />
      </template>
    </ul>

    <h2>Backend Skills</h2>
    <ul className="skill-set__list">
      <template v-for="skill in backend" :key="skill.name">
        <SingleSkill
          :name="skill.name"
          :date="skill.date"
          :img="skill.image"
          :num="skill.num"
        />
      </template>
    </ul>

    <h2>Tools</h2>
    <ul className="skill-set__list">
      <template v-for="skill in seTools" :key="skill.name">
        <SingleSkill
          :name="skill.name"
          :date="skill.date"
          :img="skill.image"
          :num="skill.num"
        />
      </template>
    </ul>
  </div>
</template>

<style scoped>
h1 {
  color: orangered;
  margin-bottom: 10px;
}
h2 {
  margin-top: 50px;
  text-align: center;
  margin-bottom: 15px;
  color: orangered;
}
.skill-set__list {
  width: 100%;
  padding-left: 0px;
}
@media screen and (max-width: 800px) {
  .skill-set__list {
    width: 100%;
  }
  .skill-set__list:last-child {
    margin-top: 1.2rem;
  }
}
</style>
