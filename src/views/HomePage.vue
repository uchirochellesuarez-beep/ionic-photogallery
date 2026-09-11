
<template>

  <ion-page>

    <!-- Header -->
    <ion-header>

      <ion-toolbar>

        <ion-title>
          Photo Gallery
        </ion-title>

      </ion-toolbar>

    </ion-header>


    <!-- Content -->
    <ion-content
      :fullscreen="true"
    >

      <!-- Welcome -->
      <div class="page-header">

        <h1>
          My Photo Gallery
        </h1>

        <p>
          Capture and view your favorite photos.
        </p>

      </div>


      <!-- Camera Component -->
      <CameraComponent
        @photo-taken="addPhoto"
      />


      <!-- Divider -->
      <div class="divider"></div>


      <!-- Photo Gallery Component -->
      <PhotoGalleryComponent
        :photos="photos"
        @delete-photo="deletePhoto"
      />

    </ion-content>

  </ion-page>

</template>


<script setup lang="ts">

import { ref } from 'vue';

import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent
} from '@ionic/vue';


// ========================================
// COMPONENTS
// ========================================

import CameraComponent
  from '../components/CameraComponent.vue';

import PhotoGalleryComponent
  from '../components/PhotoGalleryComponent.vue';


// ========================================
// PHOTO TYPE
// ========================================

interface Photo {

  id: string;

  data: string;

}


// ========================================
// PHOTO LIST
// ========================================

const photos =
  ref<Photo[]>([]);


// ========================================
// ADD PHOTO
// ========================================

const addPhoto = (
  photoData: string
) => {

  const newPhoto: Photo = {

    id: Date.now().toString(),

    data: photoData

  };


  photos.value.unshift(
    newPhoto
  );

};


// ========================================
// DELETE PHOTO
// ========================================

const deletePhoto = (
  id: string
) => {

  photos.value =
    photos.value.filter(
      photo => photo.id !== id
    );

};

</script>


<style scoped>

/* ========================================
   PAGE HEADER
======================================== */

.page-header {

  padding: 24px 20px 10px;

}

.page-header h1 {

  margin: 0;

  font-size: 28px;

  font-weight: 700;

}

.page-header p {

  margin-top: 8px;

  color:
    var(--ion-color-medium);

}


/* ========================================
   DIVIDER
======================================== */

.divider {

  height: 1px;

  background:
    var(--ion-color-light);

  margin:
    10px 20px;

}

</style>

