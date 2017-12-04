<template>
  <div class="flex">
    <div class="flex-auto">
      <h4 class="mv1 f6">{{ getTitle() }}</h4>
      <p class="mv1">
        <span class="tag" v-for="tag in getTags()">{{ tag }}</span>
      </p>
    </div>
    <div class="flex items-center">
      <button class="btn-rm" @click.stop="remove">&times;</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    note: Object
  },
  methods: {
    getTitle: function () {
      // Extract title from the first line with RegEx, then leave only 52 chars.
      return this.note.content.match(/^(.*)$/m)[0].substr(0, 52) + '…';
    },
    getTags: function () {
      // Extract tags from the note with RegEx
      // TODO: Check for duplicates when matching
      return this.note.content.match(/#(\w+)/g);
    },
    remove: function () {
      this.$emit('remove-item', this.note.id);
    }
  }
};
</script>
