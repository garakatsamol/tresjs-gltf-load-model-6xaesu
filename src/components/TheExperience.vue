<script setup lang="ts">
import { ref } from 'vue';
import { Color } from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

import { useLoader } from '@tresjs/core';
import { OrbitControls, GLTFModel } from '@tresjs/cientos';

const bgColor = new Color('#82DBC5');

const meshPosition = [0, 3, 0];

const lightRef = ref(null);

const { scene } = await useLoader(
  GLTFLoader,
  'https://raw.githubusercontent.com/Tresjs/assets/main/models/gltf/aku-aku/AkuAku.gltf'
);

scene.position.y = -80;
</script>

<template>
  <Suspense>
    <TresCanvas
      :clear-color="bgColor"
      shadows
      alpha
      window-size
      power-preference="high-performance"
      preserve-drawing-buffer
    >
      <OrbitControls />
      <TresPerspectiveCamera
        :position="[1, 4, 7]"
        :fov="75"
        :near="0.1"
        :far="1000"
      />
      <TresScene :fog="bgColor">
        <TresMesh v-bind="scene" />
        <TresDirectionalLight
          ref="lightRef"
          :position="[-4, 8, 4]"
          :intensity="1.5"
          :look-at="akuAkuRef"
        />
        <TresDirectionalLightHelper v-if="false" :args="[lightRef, 5]" />
        <TresAxesHelper />
      </TresScene>
    </TresCanvas>
  </Suspense>
</template>
