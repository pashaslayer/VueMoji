<template>
  <div class="outer-box">
    <img src="@/assets/logo.png" style="width: 150px" class="logo">

    <div class="inner-box">
      <div class="container">
        <div v-for="(designGroup, part) in designGroups" :key="part" class="design-group">
          <h3>{{ translatePart(part) }}</h3>
          <div class="design-selector">
            <button @click="changeDesign(part, -1)" class="arrow-button">←</button>
            <img :src="require('@/assets/' + designGroup.designs[designGroup.currentIndex].image)" alt="Design" class="design-image" />
            <button @click="changeDesign(part, 1)" class="arrow-button">→</button>
          </div>
          <p>{{ designGroup.designs[designGroup.currentIndex].image }}</p>
        </div>
      </div>

      <div class="duplicated-images">
        <div class="images" v-for="(designGroup, part) in designGroups" :key="'duplicated-' + part">
          <img :src="require('@/assets/' + designGroup.designs[designGroup.currentIndex].image)" alt="Duplicated Design" class="duplicated-design" />
        </div>
      </div>

      <button @click="downloadImage" class="download-button">Bild herunterladen</button>
      <canvas ref="canvas" style="display: none;"></canvas>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      designGroups: {
        hair: {
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
        head: {
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
  canvas.width = 60;
  canvas.height = 80;

  // Set canvas background to white
  ctx.fillStyle = '#FFFFFF'; // Set the fill style to white
  ctx.fillRect(0, 0, canvas.width, canvas.height); // Fill the canvas with white color

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
      link.download = 'mein-avatar.png';
      link.href = image;
      link.click();
    },

    downloadImage() {
      this.drawImageOnCanvas();
    },

    translatePart(part) {
      const translations = {
        hair: 'Haare:',
        head: 'Gesicht:',
        body: 'Oberkörper:',
        feet: 'Unterkörper:'
      };
      return translations[part] || part;
    }
  }
};
</script>

<style scoped>
.outer-box {
  border: 5px solid #007bff;
  padding: 15px;
  display: inline-block;
  border-radius: 10px;
  background-color: white;
}

.inner-box {
  background-color: white;
  padding: 15px;
}

.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
}

.design-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 15px;
}

.design-group h3 {
  margin-bottom: 10px;
  height: 30px;
}

.design-selector {
  display: flex;
  align-items: center;
  margin: 20px 0;
}

.arrow-button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  margin: 0 10px;
}

.design-image {
  max-width: 200px;
  height: auto;
  margin: 0 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.duplicated-images {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: white;
  width: 150px;
  margin: 20px auto;
}

.duplicated-images img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 0;
  padding: 0;
}

.duplicated-images img {
  max-width: 100%;
  height: auto;
}

.download-button {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin: 20px 20px 10px 20px;
  transition: background-color 0.3s;
}

.download-button:hover {
  background-color: #218838;
}
</style>
