<template>
  <div class="wrapper">
    <header>
      <h1>GoodNotes</h1>
      <button @click="showMenu = !showMenu" class="mobileMenuButton">Menu</button>

      <Nav
        v-if="showMenu"
        @toggleViewEvent="handleToggleView"
        @newNoteEvent="handleNewNoteEvent"
        @editNoteEvent="handleEditNoteEvent"
        @changeTextColorEvent="handleChangeTextColorEvent"
        @changeBgColorEvent="handleChangeBgColorEvent"
        v-bind:textcolor="textcolor"
        v-bind:bgcolor="bgcolor"
      />
    </header>
    <Nav
      class="regularMenu"
      v-on:toggleViewEvent="handleToggleView"
      v-on:newNoteEvent="handleNewNoteEvent"
      v-on:editNoteEvent="handleEditNoteEvent"
      v-on:changeTextColorEvent="handleChangeTextColorEvent"
      v-on:changeBgColorEvent="handleChangeBgColorEvent"
      v-bind:textcolor="textcolor"
      v-bind:bgcolor="bgcolor"
    />

    <Container
      @updateColorsEvent="handleUpdateColors"
      @removeNote="removeNote"
      @updateNoteEvent="updateNote"
      v-bind:database="database"
      v-bind:listView="listView"
      v-bind:textColor="textcolor"
      v-bind:bgColor="bgcolor"
      ref="container"
    />
  </div>
</template>
<script>
import n from "./Nav";
import c from "./Container";
import Note from "./Note";
import Api from "./api";

export default {
  name: "app",
  data() {
    return {
      showMenu: false,
      notesId: 0,
      listView: false,
      textcolor: "#000000",
      bgcolor: "#FFFFFF",
      database: {},
      endpoint: "http://localhost:8080/notes",
      apiHandler: new Api(this.endpoint)
    };
  },

  created() {
    console.log("Fetch DB");
    let x = this.apiHandler.getAll();

    Promise.resolve(x)
      .then(posts => {
        console.log("Got data in MAIN");
        console.log(posts);
        this.notesId = posts.length;
        for (let item of posts) {
          const newPost = `post${item.id}`;
          const newNote = new Note(
            item.id,
            item.title,
            item.content,
            item.textcolor,
            item.bgcolor
          );
          this.$set(this.database, newPost, newNote);
        }
        console.log(this.database);
      })
      .catch(() => console.log("Failed DATA in MAIN"));
    console.log("Created");
  },

  computed: {
    size() {
      return "HEJ";
    }
  },
  components: {
    Container: c,
    Nav: n
  },
  watch: {
    textcolor() {
      // console.log("TC CHANGED!");
    }
  },
  methods: {
    handleUpdateColors(colors) {
      this.handleChangeTextColorEvent(colors.textcolor);
      this.handleChangeBgColorEvent(colors.bgcolor);
    },

    handleToggleView() {
      this.listView = !this.listView;
    },
    removeNote(id) {
      console.log(id);
      const deletePost = `post${id}`;
      console.log("DeletP " + deletePost);

      let res = this.apiHandler.delete(id);

      Promise.resolve(res)
        .then(() => {
          console.log(
            "The post with id " + id + " was deleted from the database"
          );
          this.$delete(this.database, deletePost);
        })
        .catch("Couldn't delete the post with id " + id);
    },
    updateNote(note) {
      // console.log(note);
      const existingPost = `post${note.id}`;

      let res = this.apiHandler.update(note);

      Promise.resolve(res)
        .then(() => {
          console.log("The post was updated!");
          this.$set(this.database, existingPost, note);
        })
        .catch("Couldn't update the post! ");
    },
    handleNewNoteEvent() {
      console.log(this.database);

      const newNote = new Note("", "", "", this.textcolor, this.bgcolor);

      let res = this.apiHandler.create(newNote);

      Promise.resolve(res)
        .then(data => {
          console.log("The post was created!");
          console.log(data);
          const newPost = `post${data.id}`;
          console.log("DataID: " + data.id);
          newNote.id = data.id;
          this.$set(this.database, newPost, newNote);
        })
        .catch("Couldn't created the post! ");
    },
    handleEditNoteEvent(payload) {
      console.log(payload);
    },
    handleChangeTextColorEvent(color) {
      console.log(color);
      this.textcolor = color;
    },
    handleChangeBgColorEvent(color) {
      console.log(color);
      this.bgcolor = color;
    }
  },
  props: {}
};
</script>
<style>
* {
  color: white;
  padding: 0;
  margin: 0;
  font-size: 16px;
  line-height: 1.6;
  box-sizing: border-box;
}

body {
  background-color: black;
  padding: 0 2rem;
  margin: 0 auto;
  max-width: 1200px;
  /* border: 1px solid black; */
}
header {
  border-bottom: 1px solid white;
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1 {
  font-size: 2rem;
}
button {
  background: transparent;
  border: none;
}
.wrapper {
  display: grid;
}

@media screen and (min-width: 1px) {
  .wrapper {
    grid-template-rows: 10vh auto;
  }
  .regularMenu {
    display: none;
  }
}
@media screen and (min-width: 768px) {
  .regularMenu {
    display: grid;
  }
  .wrapper {
    grid-template-rows: 10vh 5vh auto;
  }
  .mobileMenuButton {
    display: none;
  }
}
@media screen and (min-width: 1024px) {
  .wrapper {
  }
}
</style>

