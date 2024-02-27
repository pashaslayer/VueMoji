<template>
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
            { name: 'Kopf 1', image: 'kopf1.png' },
            { name: 'Kopf 2', image: 'kopf1.png' },
          ]
        },
        hair: {
          currentIndex: 0,
          designs: [
            { name: 'Haar 1', image: 'kopf1.png' },
            { name: 'Haar 2', image: 'kopf1.png' },
          ]
        },
        body: {
          currentIndex: 0,
          designs: [
            { name: 'Body 1', image: 'kopf1.png' },
            { name: 'Body 2', image: 'kopf1.png' },
          ]
        },
        feet: {
          currentIndex: 0,
          designs: [
            { name: 'Fuß 1', image: 'kopf1.png' },
            { name: 'Fuß 2', image: 'kopf1.png' },
          ]
        }
      }
    };
  },
  methods: {
    changeDesign(part, direction) {
      let designGroup = this.designGroups[part];
      designGroup.currentIndex += direction;

      // Überprüfen, ob der Index innerhalb der Grenzen des Arrays liegt
      if (designGroup.currentIndex >= designGroup.designs.length) {
        designGroup.currentIndex = 0; // Zurück zum Anfang
      } else if (designGroup.currentIndex < 0) {
        designGroup.currentIndex = designGroup.designs.length - 1; // Zum letzten Design
      }
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column; /* Stapelt die Kinder vertikal */
  align-items: center; /* Zentriert die Kinder horizontal */
  justify-content: center; /* Zentriert die Kinder vertikal, wenn der Container mehr Höhe hat */
  min-height: 100vh; /* Stellt sicher, dass der Container mindestens so hoch ist wie der Viewport */
}
h3 {
  margin: 40px 0 0;
}
.design-selector {
  display: flex;
  align-items: center; 
}
button {
  margin: 0 5px;
}
img {
  width: 100px; /* oder die gewünschte Größe */
  cursor: pointer;
}
</style>
