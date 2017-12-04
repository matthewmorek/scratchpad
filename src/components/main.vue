<template>
  <div class="flex flex-auto">
    <aside class="flex flex-auto flex-column w-30 w-25-m mw6" v-show="notes.length">
      <div class="flex-auto">
        <div v-for="note in notes" :key="note.id"
          @click="loadNote(note)"
          :class="{ active: editedNote === note }"
          class="notes--item pa3">

          <list-item :note="note" @remove-item="removeNote">
          </list-item>
        </div>
      </div>
    </aside>

    <section class="flex flex-auto flex-column w-70 w-75-m h-100">
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
      editedNote: {}
    };
  },
  watch: {
    notes: {
      handler: function (notes) {
        notesStorage.save(notes);
      },
      deep: true
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
        var draft = {
          id: (this.editedNote.id ? this.editedNote.id : notesStorage.uid++),
          content: this.editedNote.content
        };

        if (!this.editedNote.id && this.editedNote.content && this.notes.indexOf(draft.id) === -1) {
          var lastId = this.notes.push(draft);
          this.editedNote = this.notes[lastId - 1];
        } else {

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
