<template>
  <body>
  <meta name="viewport" content="width=device-width, initial-scale=2">
  <link rel="icon" href="@/assets/logo.png">


  <img src="@/assets/logo.png" style="width: 150px">

  <div class="container">
    <div v-for="(designGroup, part) in designGroups" :key="part">
      <h3>Wähle {{ part }}-Design:</h3>
      <div class="design-selector">
        <button @click="changeDesign(part, -1)">←</button>
        <img :src="require('@/assets/' + designGroup.designs[designGroup.currentIndex].image)" />
        <button @click="changeDesign(part, 1)">→</button>
      </div>
      <p>{{ designGroup.designs[designGroup.currentIndex].image }}</p>
    </div>
  </div>

  <div class="duplicated-images">
    <div class="images" v-for="(designGroup, part) in designGroups" :key="'duplicated-' + part">
      <img :src="require('@/assets/' + designGroup.designs[designGroup.currentIndex].image)" />
    </div>
  </div>
  <button @click="downloadImage">Bild herunterladen</button>
  <canvas ref="canvas" style="display: none;"></canvas>
</body>
</template>


<script>
export default {
  name: 'vue-moji',
  data() {
    return {
      designGroups: {
        head: {
          currentIndex: 0,
          designs: [
            { name: 'Hair 1', image: 'hair1.png' },
            { name: 'Hair 2', image: 'hair2.png' },
            { name: 'Hair 3', image: 'hair3.png' },
            { name: 'Hair 4', image: 'hair4.png' },
            { name: 'Hair 5', image: 'hair5.png' },
            { name: 'Hair 6', image: 'hair6.png' },
            { name: 'Hair 7', image: 'hair7.png' },
            { name: 'Hair 8', image: 'hair8.png' },
            { name: 'Hair 9', image: 'hair9.png' },
            { name: 'Hair 10', image: 'hair10.png' },

          ]
        },
        hair: {
          currentIndex: 0,
          designs: [
            { name: 'Kopf 1', image: 'head1.png' },
            { name: 'Kopf 2', image: 'head2.png' },
            { name: 'Kopf 3', image: 'head3.png' },
            { name: 'Kopf 4', image: 'head4.png' },
            { name: 'Kopf 5', image: 'head5.png' },
            { name: 'Kopf 6', image: 'head6.png' },
            { name: 'Kopf 7', image: 'head7.png' },
            { name: 'Kopf 8', image: 'head8.png' },
            { name: 'Kopf 9', image: 'head9.png' },
            { name: 'Kopf 10', image: 'head10.png' },
          ]
        },
        body: {
          currentIndex: 0,
          designs: [
            { name: 'Body 1', image: 'body1.png' },
            { name: 'Body 2', image: 'body2.png' },
            { name: 'Body 3', image: 'body3.png' },
            { name: 'Body 4', image: 'body4.png' },
            { name: 'Body 5', image: 'body5.png' },
            { name: 'Body 6', image: 'body6.png' },
            { name: 'Body 7', image: 'body7.png' },
            { name: 'Body 8', image: 'body8.png' },
            { name: 'Body 9', image: 'body9.png' },
            { name: 'Body 10', image: 'body10.png' },
          ]
        },
        feet: {
          currentIndex: 0,
          designs: [
            { name: 'Feet 1', image: 'feet1.png' },
            { name: 'Feet 2', image: 'feet2.png' },
            { name: 'Feet 3', image: 'feet3.png' },
            { name: 'Feet 4', image: 'feet4.png' },
            { name: 'Feet 5', image: 'feet5.png' },
            { name: 'Feet 6', image: 'feet6.png' },
            { name: 'Feet 7', image: 'feet7.png' },
            { name: 'Feet 8', image: 'feet8.png' },
            { name: 'Feet 9', image: 'feet9.png' },
            { name: 'Feet 10', image: 'feet10.png' },
          ]
        }
      }
    };
  },
  methods: {
    changeDesign(part, direction) {
      let designGroup = this.designGroups[part];
      designGroup.currentIndex += direction;

      if (designGroup.currentIndex >= designGroup.designs.length) {
        designGroup.currentIndex = 0; 
      } else if (designGroup.currentIndex < 0) {
        designGroup.currentIndex = designGroup.designs.length - 1; 
      }
    },

    drawImageOnCanvas() {
  const canvas = this.$refs.canvas;
  const ctx = canvas.getContext('2d');
  canvas.width = 300; 
  canvas.height = 500; 

  const imagesToDraw = Object.values(this.designGroups).map(group => {
    return require('@/assets/' + group.designs[group.currentIndex].image);
  });

  let accumulatedHeight = 0; 

  const drawImage = (image, index) => {
    const img = new Image();
    img.onload = () => {
      ctx.drawImage(img, (canvas.width - img.width) / 2, accumulatedHeight);
      accumulatedHeight += img.height; 

      if (index === imagesToDraw.length - 1) this.downloadCanvas(canvas);
    };
    img.src = image;
  };

  imagesToDraw.forEach(drawImage);
},


    downloadCanvas(canvas) {
      const image = canvas.toDataURL("image/png").replace("image/png", "image/octet-stream");
      const link = document.createElement('a');
      link.download = 'mein-maennchen.png';
      link.href = image;
      link.click();
    },

    downloadImage() {
      this.drawImageOnCanvas();
    },

  }
}
</script>

<style scoped>
body {
  color: #01003D;
  background-color: #01003D;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.duplicated-images {
  display: flex;
  flex-direction: column;
  align-items: center; 
  justify-content: center; 
  background-color: whitesmoke;
  width: 150px;
  margin: 0 auto; 
}

.duplicated-images .images {
  display: flex; 
  justify-content: center; 
  align-items: center;
  width: 100%; 
  overflow: hidden; 
}

.duplicated-images img {
  max-width: 200%; 
  height: auto; 
}
</style>