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
        <li @click="SetTheme(color)" :style="{ backgroundColor: color }"></li>
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
    };
  },
  mounted() {
    if (localStorage.getItem("theme")) {
      document.documentElement.setAttribute(
        "theme",
        localStorage.getItem("theme")
      );
    } else {
      console.log(localStorage.getItem("theme"));
      localStorage.setItem("theme", "#773a9c");
    }
  },
  methods: {
    SetTheme: function (color) {
      localStorage.setItem("theme", color);
      document.documentElement.setAttribute("theme", color);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=PT+Sans&display=swap");
</style>
