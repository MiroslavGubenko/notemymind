<template>
  <div class="root_container">
    <div class="top_bar">
      <img class="logo" src="./assets/logo.svg" alt="logo" />
      <a href="https://github.com/MiroslavGubenko/notemymind" target="_blank"
        ><img class="githublogo" src="./assets/github_logo.png" alt="gitlogo" />
      </a>
      <div class="color_piker">
        <ul v-for="(color, i) in colors" :key="i">
          <li @click="SetNewTheme(color)" :style="{ backgroundColor: color }">
            <p
              :class="{ active: current_theme == color }"
              class="current_theme"
            ></p>
          </li>
        </ul>
      </div>
    </div>

    <list-notes
      @note-to-edit="EditNote"
      @note-to-delete="DeleteNote"
      :notes="notes"
      v-if="!add_new"
    />

    <button
      @click="(add_new = !add_new), (edit_note = {})"
      v-if="!add_new"
      class="add_note_btn"
    >
      <p>Добавить заметку</p>
    </button>

    <new-note
      v-if="add_new"
      :note="edit_note"
      @close="add_new = !add_new"
      @push-new-note="PushNewNote"
    />
  </div>
</template>
<script>
import ListNotes from "./components/ListNotes.vue";
import NewNote from "./components/NewNote.vue";
export default {
  name: "App",
  components: { ListNotes, NewNote },
  data() {
    return {
      colors: [
        "#4c0464",
        "#3a61ca",
        "#a85279",
        "#3a9c85",
        "#e7a129",
        "#424642",
      ],
      add_new: false,
      current_theme: "#4c0464",
      notes: [],
      edit_note: { name: "", date: "", note: "" },
      id_edit: "",
    };
  },
  mounted() {
    this.SetThemeInFirstStart();
    this.GetNotes();
  },
  methods: {
    GetNotes: function () {
      let notes = localStorage.getItem("notes");
      if (notes) {
        this.notes = JSON.parse(notes);
      }
    },
    SaveNotesInLocalstore: function (note) {
      localStorage.removeItem("notes");
      localStorage.setItem("notes", JSON.stringify(note));
    },
    SetNewTheme: function (color) {
      this.current_theme = color;
      localStorage.setItem("theme", color);
      document.documentElement.setAttribute("theme", color);
    },
    SetThemeInFirstStart: function () {
      this.current_theme = localStorage.getItem("theme");
      if (this.current_theme) {
        document.documentElement.setAttribute("theme", this.current_theme);
      } else {
        this.current_theme = "#4c0464";
        localStorage.setItem("theme", this.current_theme);
      }
    },
    DeleteNote: function (i) {
      this.notes.splice(i, 1);
      this.SaveNotesInLocalstore(this.notes);
    },
    EditNote: function (i) {
      this.id_edit = i;
      this.edit_note = this.notes[i];
      this.add_new = true;
    },
    PushNewNote: function (new_note) {
      if (this.id_edit !== "") {
        this.notes.splice(this.id_edit, 1);
      }
      this.notes.push(new_note);
      this.SaveNotesInLocalstore(this.notes);
      this.id_edit = "";
    },
  },
};
</script>
