<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>MediaCapture Vue 3</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div>
        <pre>{JSON.stringify(videoInfo, null, 2)}</pre>
      </div>
      <ion-button @click="takeVideo">TAKE VIDEO</ion-button>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonButton,
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar
} from "@ionic/vue";
import { MediaCapture } from "@ionic-native/media-capture";
import { ref } from "vue";
import { Capacitor, CapacitorPlatforms } from "@capacitor/core";
import { Filesystem, Directory } from "@capacitor/filesystem";

export default {
  name: "Other",
  components: {
    IonButton,
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar
  },
  setup() {
    const videoInfo = ref<any>(null);

    const takeVideo = async () => {
      try {
        console.log("start take video");
        const options = { limit: 1, duration: 30, quality: 1 };
        const result = await MediaCapture.captureVideo(options);
        videoInfo.value = (result as any)[0];

        console.log("videoInfo", JSON.stringify(videoInfo.value));

        // GET THE BLOB OF THE FILE...
        const blob = await fetch(
          Capacitor.convertFileSrc(videoInfo.value.fullPath)
        ).then(r => r.blob());
        console.log(videoInfo.value.fullPath, blob);

        // GET A LIST OF THE FILES STORED IN TEMP DIR ON IOS
        const files = await Filesystem.readdir({
          path: videoInfo.value.fullPath.substring(
            0,
            videoInfo.value.fullPath.lastIndexOf("/")
          )
        });
        console.log("files", JSON.stringify(files));
      } catch (error) {
        console.log("takeVideo error", error);
      }
    };

    return {
      takeVideo,
      videoInfo
    };
  }
};
</script>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>