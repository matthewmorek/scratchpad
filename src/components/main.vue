<template>
  <div class="flex flex-auto">
    <aside class="flex flex-auto flex-column w-30 w-25-m mw6" v-show="notes.length">
      <div class="pa3 tc">
        <button class="btn-new" @click="createNote">New note</button>
      </div>
      <div class="flex-auto">
        <div v-for="note in notes" :key="note.id"
          @click="loadNote(note)"
          :class="{ active: editedNote === note }"
          class="notes--item pa3">

          <list-item :note="note" @remove-item="removeNote">
          </list-item>
        </div>
      </div>
      <div class="pa3 tc self-end w-100" v-show="notes.length">
        <a download="export.json" class="btn-new" id="export-btn">Export to file</a>
      </div>
    </aside>

    <section class="flex flex-auto flex-column w-70 w-75-m">
      <textarea
        class="editor--content pa3 pa4-ns"
        placeholder="Type your note here…"
        autofocus
        autocomplete="off"
        ref="editor"
        v-model="editedNote.content"
        @keyup.prevent="saveNote">
      </textarea>
    </section>
  </div>
</template>

<script>
import _ from 'lodash';
import axios from 'axios';
import ListItem from '@/components/list-item';

// localStorage persistence
const STORAGE_KEY = 'scratchpad';
var notesStorage = {
  fetch: function () {
    var notes = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
    notes.forEach(function (note, index) {
      note.id = index;
    });
    notesStorage.uid = notes.length;
    return notes;
  },
  save: function (notes) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(notes));
  }
};

export default {
  components: { ListItem },
  // Initial data
  data: function () {
    return {
      notes: notesStorage.fetch(),
      editedNote: {},
      errors: []
    };
  },
  watch: {
    notes: {
      handler: function (notes) {
        notesStorage.save(notes);
        var archive = JSON.stringify(this.notes);
        var button = document.querySelector('#export-btn');
        button.href = 'data:text/json;charset=utf-8,' + escape(archive);
      },
      // Ensure the watcher goes deep
      deep: true
    }
  },
  created: function () {
    var self = this;
    if (this.notes.length === 0) {
      axios.get('/static/import.json').then(function (response) {
        response.data.forEach(function (item) {
          self.notes.push(item);
        });
      }).catch(function (e) {
        self.errors.push(e);
      });
    }
  },
  methods: {
    createNote: function () {
      this.editedNote = {};
      this.$refs.editor.focus();
    },
    // Use lodash's debounce to introduce rate limiting
    saveNote: _.debounce(
      function () {
        var self = this;
        // Check if a note being typed is an edit or a new note
        if (!this.notes.find(function (el) { return self.editedNote.id === el.id; })) {
          var draft = {
            id: (this.editedNote.id ? this.editedNote.id : notesStorage.uid++),
            content: this.editedNote.content
          };
          // Check if the editor is not empty before adding a new note
          if (this.editedNote.content !== undefined && this.editedNote.content.length > 0) {
            var lastId = this.notes.push(draft);
            this.editedNote = this.notes[lastId - 1];
          }
        }
      },
      500
    ),
    loadNote: function (note) {
      this.editedNote = note;
      this.$refs.editor.focus();
    },
    removeNote: function (id) {
      this.notes.splice(this.notes.indexOf(id), 1);
      this.editedNote = {};
      this.$refs.editor.focus();
    }
  }
};
</script>
