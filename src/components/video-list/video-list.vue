<template>
  <v-container>
    <v-expansion-panels>
      <v-expansion-panel
          v-for="(video,index) in videos"
          :key="index"
      >
        <v-expansion-panel-header>
          <div class="justify-start">
            <span class="font-weight-bold">{{ video.title }} </span>
            <span>{{ video.duration }} sec</span>
          </div>
        </v-expansion-panel-header>
        <v-expansion-panel-content>
          <v-container>
            <v-row>
              <v-col cols="12" md="6">
                <video ref="video" :key="index" :src="video.link" @timeupdate="checkTime" @ended="ended" controls></video>
              </v-col>
              <v-col cols="12" md="6">
                <v-card>
                  <v-card-title>
                    <h3>Tytuł: {{ video.title }}</h3>
                  </v-card-title>
                  <v-card-subtitle>
                    <h4>długość:  {{ video.duration }} sekund</h4>
                  </v-card-subtitle>
                  <v-card-text>
                    <span class="font-italic"><span class="font-weight-bold">Opis: </span> {{ video.description }}</span>
                  </v-card-text>
                  <v-card-actions>
                    <v-btn >Status: {{ videoProgress }}</v-btn>
                  </v-card-actions>
                </v-card>
              </v-col>
            </v-row>
          </v-container>

        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-container>

</template>

<script>
export default {
  data() {
    return {
      videos: [],
      videoProgress: ''
    }
  },

  methods: {
    checkTime() {
      const video = this.$refs.video[this.$children.findIndex(el => el.$el.contains(event.target))];
      const currentTime = video.currentTime;
      const duration = video.duration;
      const percent = (currentTime / duration) * 100;
      if (percent < 25) {
        this.videoProgress = 'Film rozpoczyna się';
        console.log('Film rozpoczyna się');
      } else if (percent >= 25 && percent < 75) {
        this.videoProgress = 'Środek filmu';
        console.log('Środek filmu');
      } else {
        console.log('Końcówka filmu');
        this.videoProgress = 'Końcówka filmu';
      }
    },
    ended() {
      console.log('Film zakończony');
      this.videoProgress = 'Film zakończony';
    }
  },

  // logika do czasu trwania filmu
  mounted() {
    const videos = localStorage.getItem('videos');
    if (videos) {
      this.videos = JSON.parse(videos);
    } else {
      import('./data.js').then(data => {
        this.videos = data.default
        this.videos.forEach(video => {
          const videoEl = document.createElement('video');
          videoEl.src = video.link;
          videoEl.onloadedmetadata = () => {
            video.duration = Math.round(videoEl.duration);
            localStorage.setItem('videos', JSON.stringify(this.videos));
          }
        })
      });
    }
  }
}
</script>

<style scoped>

video {
  width: 100%;
  height: auto;
}

</style>