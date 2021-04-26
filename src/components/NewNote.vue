<template>
  <div class="input_bar">
    <span class="material-icons date">
      schedule
      <p>{{ Ndate }}</p>
    </span>
    <div class="input_box">
      <input
        class="input_name"
        type="text"
        placeholder="Название заметки"
        v-model="Nname"
        :maxlength="15"
      />
      <p class="length_name">{{ LengthName }}/15</p>
    </div>
    <label class="switch">
      <input type="checkbox" v-model="show_markdown" checked />
    </label>
    <p>Включить редактор Markdown</p>
    <button class="material-icons close btn" @click="$emit('close')">
      close
    </button>
  </div>
  <div class="redactor_window">
    <div class="redactor">
      <textarea
        :maxlength="300"
        placeholder="Текст заметки...."
        v-model="Nnote"
        @input="Update"
        @keyup.enter="AddEnter"
      />
      <p
        class="length"
        :style="
          LengthText == 300 ? 'color:red;' : 'color:var(--app-text-color);'
        "
      >
        Допустимая длинна заметки 300 символов . Текущий размер
        {{ LengthText }} символов
      </p>
    </div>
    <div v-if="show_markdown" class="markdown" v-html="Markdown"></div>
    <div class="toolbar" @click="SaveNewNote">
      <button class="material-icons save btn">save</button>
    </div>
  </div>
  <div class="modal_window" v-if="save_error">
    <div class="modal_body">
      <h1>Название заметки или заметка пустая!</h1>
      <button @click="save_error = false">ОК</button>
    </div>
  </div>
</template>
<script>
var MarkdownParser = require("marked");
export default {
  props: {
    note: {
      type: Object,
    },
  },
  emits: ["close", "push-new-note"],
  data() {
    return {
      Nname: this.note.name || "",
      Ndate: this.note.date || (this.Ndate = new Date().toLocaleString()),
      Nnote: this.note.note || "",
      show_markdown: true,
      save_error: false,
    };
  },
  mounted() {
    setInterval(() => (this.Ndate = new Date().toLocaleString()), 1000);
  },
  methods: {
    SaveNewNote: function () {
      if (this.Nname === "" || this.Nnote === "") {
        this.save_error = true;
      } else {
        let new_note = {
          name: this.Nname,
          date: this.Ndate,
          note: this.Nnote,
        };
        this.$emit("push-new-note", new_note);
        this.$emit("close");
      }
    },
  },
  computed: {
    Markdown: function () {
      return MarkdownParser(this.Nnote);
    },
    LengthText: function () {
      return this.Nnote.length;
    },
    LengthName: function () {
      return this.Nname.length;
    },
  },
};
</script>

<style lang="scss" scoped>
.input_bar {
  width: 100%;
  height: 80px;
  background-color: var(--app-second-color);
  color: var(--app-text-color);

  display: flex;
  align-items: center;
  justify-content: center;
  user-select: text;
  .date {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 30px;
    letter-spacing: 0.5px;
  }
  .input_box {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    padding: 0 20px 0 20px;

    .input_name {
      background-color: var(--app-second-color);
      border: none;
      border-bottom: 2px solid var(--app-text-color);
      outline: none;
      width: 350px;
      height: 60px;
      font-size: 30px;
      margin: 10px 0px 10px 0;
      padding-left: 25px;
      user-select: text;
      color: var(--app-text-color);
      caret-color: var(--app-color);

      &:focus {
        outline: none;
      }
    }
    .length_name {
      align-self: flex-end;
      width: 30px;
      font-size: 20px;
    }
  }
  p {
    font-size: 20px;
    margin: 10px;
  }
}
.redactor_window {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-around;
  width: 100%;
  height: 100%;

  div {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .redactor {
    flex-direction: column;
    flex-grow: 4;
    textarea {
      width: 80%;
      border: 1px solid var(--app-text-color);
      white-space: pre-wrap;
      user-select: text;
      height: 80%;
      padding: 8px 0 0 13px;
      color: var(--app-text-color);

      caret-color: var(--app-color);
      font-weight: 400;
      font-size: 20px;
      resize: none;

      scrollbar-color: var(--app-color) var(--app-text-color);
      scrollbar-width: thin;

      &::-webkit-scrollbar {
        scrollbar-color: var(--app-color) var(--app-text-color);
        scrollbar-width: thin;
      }
      .length {
        align-self: flex-end;
      }
    }
  }
  .markdown {
    display: inline-block;
    word-wrap: break-word;
    flex-grow: 4;
    height: 80%;
    max-width: 500px;
    flex-wrap: wrap;
    padding: 20px;
    border: 1px solid var(--app-text-color);
    margin-right: 20px;
    user-select: text;
  }
  .toolbar {
    width: 20px;
    flex-grow: 1;
    cursor: pointer;

    &:hover {
      background-color: var(--app-color);
    }
  }
}
.btn {
  outline: none;
  border: none;
  height: 44px;
  width: 44px;
  background: var(--app-second-color);
  color: var(--app-text-color);
  font-size: 30px;
  cursor: pointer;
}
.modal_window {
  position: absolute;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
  background-color: rgba(0, 0, 0, 0.719);

  .modal_body {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: rgb(149, 0, 0);
    color: white;
    padding: 20px;

    button {
      margin: 20px;
      outline: none;
      border: none;
      height: 44px;
      width: 100px;
      background: var(--app-second-color);
      color: var(--app-text-color);
      font-size: 30px;
      cursor: pointer;
    }
  }
}
</style>
