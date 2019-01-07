<template>
  <div class="App">
    <h3 class="text-center">{{title}}</h3>
    <form class="m-3">
      <div class="form-group">
        <label>Título</label>
        <input tabindex="1" class="form-control" type="text" v-model="newNote.title">
      </div>

      <div class="form-group">
        <label>Texto</label>
        <textarea tabindex="2" class="form-control" v-model="newNote.text">
        </textarea>
      </div>
      <button :disabled="! isInputValid" tabindex="3" type="button"
        class="btn btn-primary" @click="addNote">Salvar</button>
    </form>

    <div class="col-sm-12">
      <div class="row">
        <div class="col-sm-4 note mb-2"
            v-for="(note, index) in notes" :key="index">
          <div class="colors row" style="border: 1px solid" :key="index" v-if="note.showColors">
            <div class="color col-3" :key="color" v-for="color in availableColors"
              :style="{background: color}" @click="changeColor(index, color)"></div>
          </div>
          <div class="card p-1" :style="{ background: note.color}">
          <div class="options">
            <i class="fas fa-paint-brush" @click="showAvailableColors(index)"></i>
            <i class="fas fa-trash" @click="removeNote(index)"></i>
          </div>
          <div class="card-block">
            <h4 class="card-title">{{note.title}}</h4>
            <h6 class="card-subtitle mb-2 text-muted">{{note.date}}</h6>
            <p class="card-text">{{note.text}}</p>
          </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  
  data() {
    return {
      availableColors: [
        '#EF9A9A', '#F48FB1', '#CE93D8', '#B39DDB',
        '#9FA8DA', '#90CAF9', '#FFFFFF', '#81D4FA',
        '#80DEEA', '#80CBC4', '#A5D6A7', '#C5E1A5',
        '#E6EE9C', '#FFF59D', '#FFE082', '#FFCC80',
        '#FFAB91', '#BCAAA4', '#EEEEEE', '#B0BEC5'
      ],
      title: 'Mestre das Notas',
      newNote: {
        title: '',
        text: '',
        showColors: false
      },
      notes: [
        {
          title: 'Estudar Vue.js',
          text: 'Implementar mais projetos usando Vue.js para consolidar o conhecimento.',
          date: new Date(Date.now()).toLocaleString(),
          showColors: false
        }
      ]
    }
  },

  computed: {
    isInputValid() {
      return this.newNote.text && this.newNote.title
    }
  },

  methods: {
    addNote() {
      let { text, title, showColors } = this.newNote

      let date = new Date(Date.now()).toLocaleString()
      this.notes.push({ title, text, date, showColors })
      this.newNote = Object.assign({}, { title: '', text: '', showColors: false})
    },
    removeNote(index) {
      this.notes.splice(index, 1)
    },
    changeColor(index, color) {
      this.notes[index].color = color
      this.notes[index].showColors = false
    },
    showAvailableColors(index) {
      this.notes[index].showColors = true
    }
  }
}
</script>

<style>
  .options {
    display: flex;
    justify-content: space-between;
  }

  .colors {
    width: auto;
    height: auto;
    position: absolute;
    z-index: 99;
    top: calc(100% - 5rem * 4);
    left: calc(100% - 5rem * 5);
  }

  .color {
    width: 5rem;
    height: 5rem;
  }
</style>
