<template>
  <div class="container_list">
    <div class="center" v-if="notes.length === 0">
      <h1>Список заметок пуст</h1>
    </div>

    <template v-else>
      <note
        v-for="(note, i) in notes"
        :date="note.date"
        :name="note.name"
        :key="i"
        @index-note-delete="DeleteNote(i)"
        @index-note-edit="EditNote(i)"
      />
    </template>
  </div>
</template>

<script>
import Note from "../components/Note.vue";
export default {
  components: { Note },
  props: ["notes"],
  data() {
    return {};
  },
  methods: {
    DeleteNote: function (i) {
      this.$emit("note-to-delete", i);
    },
    EditNote: function (i) {
      this.$emit("note-to-edit", i);
    },
  },
};
</script>

<style lang="scss" scope>
.container_list {
  overflow: auto;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  color: var(--app-text-color);
  margin-bottom: 50px;
  flex-shrink: 5;
  justify-content: center;

  scrollbar-color: var(--app-color) var(--app-text-color);
  scrollbar-width: thin;

  &::-webkit-scrollbar {
    scrollbar-color: var(--app-color) var(--app-text-color);
    scrollbar-width: thin;
  }
}
.center {
  width: 100%;
  display: flex;
  justify-content: center;

  color: var(--app-text-color);
}
</style>
