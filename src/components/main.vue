<template>
  <div>
    <aside>
      <div>
        <div v-for="note in notes" :key="note.id"
          @click="loadNote(note)"
          :class="{ active: editedNote === note }"
          class="notes--item">

          <list-item :note="note" @remove-item="removeNote">
          </list-item>
        </div>
      </div>
    </aside>

    <section>
      <textarea
        class="editor--content"
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
import ListItem from '@/components/list-item';

export default {
  components: { ListItem },
  // Initial data
  data: function () {
    return {
      notes: [],
      editedNote: {}
    };
  },
  methods: {
    createNote: function () {
      this.editedNote = {};
      this.$refs.editor.focus();
    },
    // Use lodash's debounce to introduce rate limiting
    saveNote: function () {
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
