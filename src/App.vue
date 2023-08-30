`<template>
  <v-app>
    <v-main>
      <v-container fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="8" md="4">
            <v-progress-circular
              :rotate="isSpeaking ? 180 : 0"
              :size="100"
              :width="15"
              :value="50"
              color="blue"
            ></v-progress-circular>
            <AudioCapture @speech-detected="toggleSpeaking" />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import AudioCapture from "./components/AudioCapture.vue";

export default {
  components: {
    AudioCapture,
  },
  data() {
    return {
      isSpeaking: false,
    };
  },
  methods: {
    toggleSpeaking(status) {
      this.isSpeaking = status;
    },
  },
};
</script>`