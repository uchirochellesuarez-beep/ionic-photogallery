```vue
<template>

  <div class="gallery-container">

    <!-- Gallery Header -->
    <div class="gallery-header">

      <div>

        <h2>
          My Photos
        </h2>

        <p>
          {{ photos.length }}
          photo{{ photos.length !== 1 ? 's' : '' }}
        </p>

      </div>

    </div>


    <!-- Empty State -->
    <div
      v-if="photos.length === 0"
      class="empty-state"
    >

      <ion-icon
        :icon="imagesOutline"
        class="empty-icon"
      ></ion-icon>

      <h3>
        No Photos Yet
      </h3>

      <p>
        Your photos will appear here.
      </p>

    </div>


    <!-- Photo Gallery -->
    <div
      v-else
      class="photo-grid"
    >

      <div
        v-for="(photo, index) in photos"
        :key="photo.id"
        class="photo-card"
        @click="viewPhoto(photo)"
      >

        <!-- Photo -->
        <img
          :src="photo.data"
          :alt="'Photo ' + (index + 1)"
        />


        <!-- Delete Button -->
        <button
          class="delete-button"
          @click.stop="deletePhoto(photo.id)"
        >

          <ion-icon
            :icon="trashOutline"
          ></ion-icon>

        </button>

      </div>

    </div>


    <!-- Fullscreen Photo Viewer -->
    <ion-modal
      :is-open="isViewerOpen"
      @didDismiss="closeViewer"
    >

      <ion-header>

        <ion-toolbar>

          <ion-title>
            Photo
          </ion-title>

          <ion-buttons slot="end">

            <ion-button
              @click="closeViewer"
            >
              Close
            </ion-button>

          </ion-buttons>

        </ion-toolbar>

      </ion-header>


      <ion-content class="viewer-content">

        <img
          v-if="selectedPhoto"
          :src="selectedPhoto.data"
          class="full-photo"
          alt="Selected photo"
        />

      </ion-content>

    </ion-modal>

  </div>

</template>


<script setup lang="ts">

import { ref } from 'vue';

import {
  IonIcon,
  IonModal,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonButtons,
  IonButton,
  IonContent
} from '@ionic/vue';

import {
  imagesOutline,
  trashOutline
} from 'ionicons/icons';


// ========================================
// PHOTO TYPE
// ========================================

export interface Photo {

  id: string;

  data: string;

}


// ========================================
// PROPS
// ========================================

defineProps<{

  photos: Photo[];

}>();


// ========================================
// EMIT
// ========================================

const emit = defineEmits<{

  (
    event: 'delete-photo',
    id: string
  ): void;

}>();


// ========================================
// VIEWER
// ========================================

const isViewerOpen =
  ref(false);

const selectedPhoto =
  ref<Photo | null>(null);


// ========================================
// VIEW PHOTO
// ========================================

const viewPhoto = (
  photo: Photo
) => {

  selectedPhoto.value =
    photo;

  isViewerOpen.value =
    true;

};


// ========================================
// CLOSE VIEWER
// ========================================

const closeViewer = () => {

  isViewerOpen.value =
    false;

  selectedPhoto.value =
    null;

};


// ========================================
// DELETE PHOTO
// ========================================

const deletePhoto = (
  id: string
) => {

  emit(
    'delete-photo',
    id
  );

};

</script>


<style scoped>

/* ========================================
   GALLERY
======================================== */

.gallery-container {

  width: 100%;

}


/* ========================================
   HEADER
======================================== */

.gallery-header {

  padding: 20px;

}

.gallery-header h2 {

  margin: 0;

  font-size: 24px;

  font-weight: 700;

}

.gallery-header p {

  margin: 5px 0 0;

  color:
    var(--ion-color-medium);

}


/* ========================================
   PHOTO GRID
======================================== */

.photo-grid {

  display: grid;

  grid-template-columns:
    repeat(2, 1fr);

  gap: 4px;

  padding: 4px;

}


/* ========================================
   PHOTO CARD
======================================== */

.photo-card {

  position: relative;

  width: 100%;

  aspect-ratio: 1 / 1;

  overflow: hidden;

  background: #eeeeee;

  cursor: pointer;

}


/* ========================================
   PHOTO
======================================== */

.photo-card img {

  width: 100%;

  height: 100%;

  object-fit: cover;

  display: block;

}


/* ========================================
   DELETE
======================================== */

.delete-button {

  position: absolute;

  top: 8px;

  right: 8px;

  width: 34px;

  height: 34px;

  border: none;

  border-radius: 50%;

  background:
    rgba(0, 0, 0, 0.65);

  color: white;

  display: flex;

  align-items: center;

  justify-content: center;

  cursor: pointer;

}

.delete-button ion-icon {

  font-size: 18px;

}


/* ========================================
   EMPTY STATE
======================================== */

.empty-state {

  min-height: 50vh;

  display: flex;

  flex-direction: column;

  align-items: center;

  justify-content: center;

  text-align: center;

  padding: 30px;

}

.empty-icon {

  font-size: 80px;

  color:
    var(--ion-color-primary);

  margin-bottom: 15px;

}

.empty-state h3 {

  margin: 0 0 5px;

  font-size: 22px;

}

.empty-state p {

  margin: 0;

  color:
    var(--ion-color-medium);

}


/* ========================================
   PHOTO VIEWER
======================================== */

.viewer-content {

  --background: #000000;

}

.full-photo {

  width: 100%;

  height: 100%;

  object-fit: contain;

  display: block;

}


/* ========================================
   TABLET
======================================== */

@media (min-width: 600px) {

  .photo-grid {

    grid-template-columns:
      repeat(3, 1fr);

    gap: 6px;

    padding: 6px;

  }

}


/* ========================================
   DESKTOP
======================================== */

@media (min-width: 900px) {

  .photo-grid {

    grid-template-columns:
      repeat(4, 1fr);

    max-width: 1200px;

    margin: 0 auto;

  }

}

</style>
