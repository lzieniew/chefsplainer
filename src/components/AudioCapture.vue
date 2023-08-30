<template>
  <div>
    <button @click="startRecording">Start Recording</button>
    <button @click="stopRecording">Stop Recording</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      mediaRecorder: null,
      audioChunks: [],
    };
  },
  methods: {
    async startRecording() {
      const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
      this.mediaRecorder = new MediaRecorder(stream);
      this.audioChunks = [];

      this.mediaRecorder.ondataavailable = (event) => {
        this.audioChunks.push(event.data);
      };

      this.mediaRecorder.onstop = async () => {
        const audioBlob = new Blob(this.audioChunks, { type: "audio/wav" });
        this.sendAudioToWhisper(audioBlob);
      };

      this.mediaRecorder.start();
    },
    stopRecording() {
      if (this.mediaRecorder) {
        this.mediaRecorder.stop();
      }
    },
    async sendAudioToWhisper(audioBlob) {
      const formData = new FormData();
      formData.append("audio", audioBlob, "audio.wav");

      try {
        const response = await axios.post("http://localhost:5000/your-whisper-api-endpoint", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        });
        console.log("Whisper API Response:", response.data);
      } catch (error) {
        console.error("Error sending audio to Whisper:", error);
      }
    },
  },
};
</script>