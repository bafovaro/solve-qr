<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>
      <video ref="scanner"></video>
      <canvas ref="qrcanvas"></canvas>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import QrScanner from "qr-scanner";
import {onMounted, ref} from "vue";
import QRCode from "qrcode";
import ScanResult = QrScanner.ScanResult;

const scanner = ref(null);
const qrcanvas = ref(null);

onMounted(() => {
  
  console.log('mounted');
  
  const qrScanner = new QrScanner(
      scanner.value, (result: ScanResult) => {
        console.log(result);
        qrScanner.stop();

        QRCode.toCanvas(qrcanvas.value, result.data, function (error) {
          if (error) console.error(error)
          console.log('success!');
        })
      },
      { /* your options or returnDetailedScanResult: true if you're not specifying any other options */ },
  );
  
  qrScanner.start();
});

</script>
