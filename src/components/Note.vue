<template>
  <div class="box">
    <div class="discription">
      <p>{{ date }}</p>
      <p>{{ name }}</p>
    </div>
    <button class="btn_edit material-icons" @click="$emit('index-note-edit')">
      edit
    </button>
    <button class="btn_remove" @click="$emit('index-note-delete')">
      <span class="material-icons"> delete_forever </span>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    date: String,
    name: String,
  },
  emits: ["index-note-delete", "index-note-edit"],
  data() {
    return {};
  },
};
</script>

<style lang="scss" scoped>
.box {
  color: var(--app-text-color);
  width: 400px;
  height: 75px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  border: 4px solid var(--app-second-color);
  border-radius: 5px;
  margin: 30px 40px 0 30px;
  background-color: var(--app-second-color);

  .discription {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    width: 220px;
    margin-left: 12px;

    p {
      padding: 0;
      margin: 0;
    }
    p:nth-child(1) {
      font-size: 15px;
      font-weight: 300;
    }
    p:nth-child(2) {
      font-size: 25px;
      font-weight: 600;
    }
  }

  .btn_edit {
    display: flex;
    justify-content: center;
    align-items: center;
    border: 0 solid;
    height: 35px;
    width: 35px;
    color: white;
    cursor: pointer;
    background-color: var(--app-color);
    margin-right: 20px;

    &:hover {
      background-color: rgb(151, 151, 151);
    }
  }
  .btn_remove {
    color: #a10000;
    height: 100%;
    width: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    border: 0 solid;
    border-radius: 0 3px 3px 0;
    background-color: white;
    transition: 0.3s;

    &:hover {
      color: var(--app-text-color);
      background-color: #dd00004d;
      width: 100px;
    }
    span {
      font-size: 25px;
    }
  }
}
</style>
