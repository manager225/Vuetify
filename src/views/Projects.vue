<template>
  <div class="projects">
    <h1 class="subheading grey--text text-center">Projects</h1>

    <v-container class="my-5">
      <v-expansion-panels>
        <v-expansion-panel v-for="project in myProjects" :key="project.title">
          <v-expansion-panel-header>
            <div class="py-1">{{ project.title }}</div>
          </v-expansion-panel-header>
          <v-card>
            <v-expansion-panel-content>
              <v-card-text class="px-4 grey--text">
                <div class="font-weight-bold">Due by {{ project.due }}</div>
                <div>{{ project.content }}</div>
              </v-card-text>
            </v-expansion-panel-content>
          </v-card>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-container>
  </div>
</template>


<script>


  import Vue from 'vue';
  import "firebase/app";
  import '../main'
  import 'firebase/database';

  export default {
    data() {
      return {
        projects: []
      }
    },
    computed: {
      myProjects() {
        return this.projects.filter(project => {
          return project.person === this.$store.getters.info.name
        })
      },

    },

    created() {
      Vue.$db.collection('projects').onSnapshot(res => {
        const changes = res.docChanges();
        changes.forEach(change => {
          if (change.type === 'added') {
            this.projects.push({
              ...change.doc.data(),
              id: change.doc.id
            })
          }
        })
      })
    }


  }
</script>
