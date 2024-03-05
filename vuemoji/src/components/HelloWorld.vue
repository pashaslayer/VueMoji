<template>
    <div class="container mt-5">
      <div class="row justify-content-center">
      <div
        class="col-md-2 bg-info bg-opacity-25 border border-2 border-white rounded-start border-end align-items-center d-flex justify-content-center"
      >
        <label for="username" class="text-center">Username:</label>
      </div>
      <div
        class="col-md-2 m-0 p-0 bg-white border border-2 border-white border-start-0 rounded-end"
      >
        <input
          v-model="postData.username"
          type="text"
          class="form-control"
          id="username-label"
          @input="validateUsername"
        />
      </div>
    </div>

    <div class="row justify-content-center">
      <div
        class="col-md-2 bg-info bg-opacity-25 border border-2 border-top-0 border-white rounded-start border-end align-items-center d-flex justify-content-center"
      >
        <label for="password" class="text-center">Password: </label>
      </div>
      <div
        class="col-md-2 m-0 p-0 bg-white border border-2 border-white border-top-0 border-start-0 rounded-end"
      >
        <input
          v-model="postData.password"
          type="password"
          class="form-control"
          id="password-label"
          aria-describedby="passwordHelp"
          required
          @input="validatePassword"
        />
      </div>
    </div>

    <div class="row">
      <div
        class="col-12 col-md-6 d-flex justify-content-md-end justify-content-center mb-2"
      >
        <button type="submit" class="btn btn-primary" @click="postLogin">
          Bestätigen
        </button>
      </div>
      <div
        class="col-12 col-md-6 d-flex justify-content-md-start justify-content-center mb-2"
      >
        <button type="submit" class="btn btn-secondary">Abbrechen</button>
      </div>
    </div>
  </div>


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
  <!-- Duplicated images outside of the loop -->
  <div class="duplicated-images">
    <div class="images" v-for="(designGroup, part) in designGroups" :key="'duplicated-' + part">
      <img :src="require('@/assets/' + designGroup.designs[designGroup.currentIndex].image)" />
    </div>
  </div>
</template>


<script>
export default {
  name: 'vue-moji',
  data() {
    return {
      postData: {
        username: "",
        password: "",
      },
      designGroups: {
        head: {
          currentIndex: 0,
          designs: [
            { name: 'Kopf 1', image: 'hair1.png' },
            { name: 'Kopf 2', image: 'hair2.png' },
          ]
        },
        hair: {
          currentIndex: 0,
          designs: [
            { name: 'Haar 1', image: 'head1.png' },
            { name: 'Haar 2', image: 'head2.png' },
          ]
        },
        body: {
          currentIndex: 0,
          designs: [
            { name: 'Body 1', image: 'body1.png' },
            { name: 'Body 2', image: 'body2.png' },
          ]
        },
        feet: {
          currentIndex: 0,
          designs: [
            { name: 'Fuß 1', image: 'feet1.png' },
            { name: 'Fuß 2', image: 'feet2.png' },
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
  justify-content:center;
  align-items: center;
}

.duplicated-images {
  display: flex;
  flex-direction: column;
 }

.duplicated-images img {
  margin-top: 0px; 
  width: auto; /* Breite des Bildes */
  height: auto; 
}

.duplicated-images img {
  display: block; 
  margin: 0px;
  padding: 0px;
  width: auto;
  height: auto;
}

.images {
  align-self: center;
  margin: 0px; /* Stellt sicher, dass auch die Container keinen Außenabstand haben */
  padding: 0px; /* Entfernt den Innenabstand vom Container */
}

</style>
