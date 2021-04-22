<template>
  <div class="root_container">
    <div class="top_bar">
      <img class="logo" src="./assets/logo.svg" alt="logo" />
      <a href="#"
        ><img class="githublogo" src="./assets/github_logo.png" alt="gitlogo" />
      </a>
    </div>
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
    <div class="container_center">
      <h1>Список заметок пуст</h1>
      <button class="add_note">Добавить заметку</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      colors: [
        "#773a9c",
        "#3a619c",
        "#9c3a3a",
        "#3a9c85",
        "#FFA07A",
        "#778899",
      ],
      current_theme: "",
    };
  },
  mounted() {
    this.SetTheme();
  },
  methods: {
    SetNewTheme: function (color) {
      this.current_theme = color;
      localStorage.setItem("theme", color);
      document.documentElement.setAttribute("theme", color);
    },
    SetTheme: function () {
      this.current_theme = localStorage.getItem("theme");
      if (this.current_theme) {
        document.documentElement.setAttribute("theme", this.current_theme);
      } else {
        this.current_theme = "#773a9c";
        localStorage.setItem("theme", this.current_theme);
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=PT+Sans&display=swap");
</style>
