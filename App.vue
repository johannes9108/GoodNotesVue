<template>
  <div class="wrapper">
    <header>
      <h1>Goodnotes</h1>
      <div class="mobileMenu"></div>
    </header>
    <Nav
      v-on:toggleViewEvent="handleToggleView"
      v-on:newNoteEvent="handleNewNoteEvent"
      v-on:editNoteEvent="handleEditNoteEvent"
      v-on:changeTextColorEvent="handleChangeTextColorEvent"
      v-on:changeBgColorEvent="handleChangeBgColorEvent"
    />

    <Container
      v-bind:database="database"
      v-bind:listView="listView"
      v-bind:textColor="textColor"
      v-bind:bgColor="bgColor"
      ref="container"
    />
  </div>
</template>
<script>
import n from "./Nav";
import c from "./Container";

function NoteConstructor(id, title, content, textColor, bgColor) {
  (this.id = id),
    (this.title = title),
    (this.content = content),
    (this.textColor = textColor),
    (this.bgColor = bgColor);
}
export default {
  name: "app",
  data() {
    return {
      notesId: 0,
      listView: false,
      textColor: "#000000",
      bgColor: "#FFFFFF",
      database: {}
    };
  },
  components: {
    Container: c,
    Nav: n
  },
  methods: {
    handleToggleView() {
      this.listView = !this.listView;
      this.$refs.container.toggleView();
    },
    handleNewNoteEvent() {
      // this.components.c.createNewNote();
      console.log(this.database);
      const newPost = `post${++this.notesId}`;

      const newNote = new NoteConstructor(
        this.notesId,
        "",
        "",
        this.textColor,
        this.bgColor
      );

      console.log();
      this.$set(this.database, newPost, newNote);
    },
    handleEditNoteEvent(payload) {
      console.log(payload);
    },
    handleChangeTextColorEvent(color) {
      this.textColor = color;
    },
    handleChangeBgColorEvent(color) {
      this.bgColor = color;
    }
  },
  props: {}
};
</script>
<style>
* {
  padding: 0;
  margin: 0;
  font-size: 16px;
  line-height: 1.6;
  box-sizing: border-box;
}
body {
  padding: 0 2rem;
  margin: 0 auto;
  max-width: 1200px;
  background-color: aquamarine;
  /* border: 1px solid black; */
}

.wrapper {
  min-height: 100vh;
  display: grid;
  background-color: red;
}
@media screen and (min-width: 350px) {
  .wrapper {
    grid-template-rows: 10vh 10vh auto;
  }
}
@media screen and (min-width: 768px) {
  .wrapper {
    grid-template-rows: 25px 100px minmax(70vh, auto);
  }
}
</style>

