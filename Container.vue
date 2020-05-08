<template>
  <section class="gridContainer" v-bind:class="{listView}">
    <Note
      @editModeViewEvent="editModeView"
      @exitEditModeViewEvent="exitEditModeView"
      v-on:removeThisNote="removeNote"
      v-bind:post="post"
      v-bind:key="post.id"
      v-for="post in database"
    />
  </section>
</template>
<script>
import Note from "./Note";

export default {
  data() {
    return {
      localDatabase: this.database //Flytta till APP istället
    };
  },
  components: {
    Note: Note
  },
  methods: {
    createNewNote() {
      // console.log("BG: " + this.bgColor);
      // console.log(this.localDatabase);
      // const newPost = `post${++this.notesId}`;
      // const newNote = new NoteConstructor(
      //   this.notesId,
      //   "",
      //   "",
      //   this.textColor,
      //   this.bgColor
      // );
      // console.log();
      // this.$set(this.localDatabase, newPost, newNote);
    },

    toggleView() {
      // if (this._props.listView) {
      //   this.$el.style.gridTemplateColumns = "auto";
      // } else {
      //   this.$el.style.gridTemplateColumns = "auto auto auto";
      // }
    },

    removeNote(id) {
      console.log(id);
      const deletePost = `post${id}`;
      console.log("DeletP " + deletePost);
      // delete this.database[deletePost];
      this.$delete(this.localDatabase, deletePost);
    },
    editModeView(noteId) {
      this.$el.style.gridTemplateColumns = "auto";
      console.log("Dölj alla barn utan: " + noteId);
      for (let note of this.$children) {
        if (note.noteId != noteId) {
          note.toggleHidden();
        }
      }
    },
    exitEditModeView(noteId) {
      console.log("Current: " + this.listView);
      if (!this.listView) this.$el.style.gridTemplateColumns = "auto auto auto";

      console.log("Visa alla barn utan: " + noteId);
      for (let note of this.$children) {
        if (note.noteId != noteId) {
          note.toggleHidden();
        }
      }
    }
  },
  props: {
    database: {},
    listView: Boolean,
    textColor: String,
    bgColor: String
  }
};
</script>
<style>
.gridContainer {
  padding: 0 1rem;
  display: grid;
  grid-template-columns: auto auto auto;

  background-color: gainsboro;
}

.listView {
  grid-template-columns: auto;
}
</style>
