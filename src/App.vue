<template>
  <v-app>
    <v-app-bar app color="#000000" dark>
      <div class="d-flex align-center"></div>
      <v-spacer></v-spacer>
      <v-btn color="secendary" href="#">
        <span class="mr-2">Movies List | Task 2</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main >
      <video-list/>
    </v-main>
  </v-app>
</template>


// q: jak cofnąć commitowane zmiany?
// a: git reset --hard HEAD~1
<script>
import VideoList from "@/components/video-list/video-list.vue";


export default {
  name: 'App',

  components: {
    VideoList,
  },

  data: () => ({
    //
  }),

};
</script>
