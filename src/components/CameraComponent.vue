
<template>
  <div class="camera-container">

    <!-- Take Photo Button -->
    <ion-button
      expand="block"
      @click="takePhoto"
    >
      <ion-icon
        slot="start"
        :icon="camera"
      ></ion-icon>

      Take Photo
    </ion-button>


    <!-- Choose Photo Button -->
    <ion-button
      expand="block"
      fill="outline"
      @click="choosePhoto"
    >
      <ion-icon
        slot="start"
        :icon="imageOutline"
      ></ion-icon>

      Choose from Gallery
    </ion-button>

  </div>
</template>


<script setup lang="ts">

import {
  IonButton,
  IonIcon
} from '@ionic/vue';

import {
  camera,
  imageOutline
} from 'ionicons/icons';

import {
  Camera,
  CameraResultType,
  CameraSource
} from '@capacitor/camera';


// ========================================
// EMIT
// ========================================

const emit = defineEmits<{
  (event: 'photo-taken', photo: string): void;
}>();


// ========================================
// TAKE PHOTO
// ========================================

const takePhoto = async () => {

  try {

    const photo = await Camera.getPhoto({

      quality: 90,

      allowEditing: false,

      resultType: CameraResultType.DataUrl,

      source: CameraSource.Camera

    });


    if (photo.dataUrl) {

      emit(
        'photo-taken',
        photo.dataUrl
      );

    }

  } catch (error) {

    console.error(
      'Camera error:',
      error
    );

  }

};


// ========================================
// CHOOSE PHOTO
// ========================================

const choosePhoto = async () => {

  try {

    const photo = await Camera.getPhoto({

      quality: 90,

      allowEditing: false,

      resultType: CameraResultType.DataUrl,

      source: CameraSource.Photos

    });


    if (photo.dataUrl) {

      emit(
        'photo-taken',
        photo.dataUrl
      );

    }

  } catch (error) {

    console.error(
      'Gallery selection error:',
      error
    );

  }

};

</script>


<style scoped>

.camera-container {

  padding: 20px;

}

.camera-container ion-button {

  margin-bottom: 12px;

}

</style>
