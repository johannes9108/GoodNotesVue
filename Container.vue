<template>
  <section>
    <article v-if="!editMode" class="displayView" v-bind:class="{listView: listView}">
      <NoteVue
        @updateNote="updateNote"
        @editModeViewEvent="toggleEditMode"
        v-on:removeThisNote="removeNote"
        v-bind:post="post"
        v-bind:key="post.id"
        v-for="post in database"
      />
    </article>
    <article
      v-bind:key="item.id"
      v-if="editMode"
      class="editView"
      v-bind:style="{color: item.textcolor, backgroundColor: item.bgcolor, borderColor: item.textcolor}"
    >
      <div class="header">
        <h1>Title</h1>
        <button v-on:click.stop="saveItem">&#128394;</button>
      </div>
      <input name="title" type="text" v-model="item.title" />
      <h1>Content</h1>
      <textarea name="content" type="text" v-model="item.content" />
      <!-- <button v-on:click.stop="saveItem">&#128190;</button> -->
    </article>
  </section>
</template>
<script>
import NoteVue from "./Note.vue";
import Note from "./Note.js";

export default {
  data() {
    return {
      //Flytta till APP istället database
      editMode: false,
      item: {
        title: "",
        content: "",
        textcolor: "",
        bgcolor: ""
      }
    };
  },

  components: {
    NoteVue: NoteVue
  },
  // computed:{
  //   getItemForEdit(noteId){

  //     return
  //   }
  // },

  watch: {
    bgColor(bgcolor) {
      console.log("bg changed");
      console.log(bgcolor);
      this.item.bgcolor = bgcolor;
    },
    textColor(textcolor) {
      console.log("textcolor changed");
      console.log(textcolor);
      this.item.textcolor = textcolor;
    }
  },

  methods: {
    removeNote(id) {
      this.$emit("removeNote", id);
    },
    toggleEditMode(noteId) {
      console.log(noteId);

      this.editMode = !this.editMode; // Turns on edit mode(selecting different view)
      let editProperty = `post${noteId}`;
      let editNote = this.database[editProperty];
      this.item.id = noteId;
      this.item.title = editNote.title;
      this.item.content = editNote.content;
      this.item.textcolor = editNote.textcolor;
      this.item.bgcolor = editNote.bgcolor;
      this.$emit("updateColorsEvent", {
        textcolor: this.item.textcolor,
        bgcolor: this.item.bgcolor
      });
      console.log(editNote);
      console.log(this.item);
    },
    updateNote(note) {
      let editProperty = `post${note.id}`;
      let editNote = this.localDatabase[editProperty];
      console.log(editNote);
      editNote.title = note.title;
      editNote.content = note.content;
    },
    saveItem() {
      console.log("saveitem");

      let note = new Note(
        this.item.id,
        this.item.title,
        this.item.content,
        this.item.textcolor,
        this.item.bgcolor
      );
      this.$emit("updateNoteEvent", note);
      this.editMode = !this.editMode;
      console.log("Database after saveItem: ");
      console.log(this.database);
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
.displayView {
  background-color: white;
  padding: 0 1rem;
  display: grid;
  grid-template-columns: auto auto auto;
}
.editView {
  height: 80vh;
  display: grid;
  grid-template-rows: 5vh 5vh 5vh auto;
}
.editView > :nth-child(odd) {
  border-bottom-width: 1px;
  border-bottom-style: solid;
}

input,
textarea,
h1,
label,
button,
div,
p {
  color: inherit;
  background-color: inherit;
}
.header {
  display: flex;
  justify-content: space-between;
}
.header button {
  font-size: 2rem;
}

textarea {
  width: 100%;
  resize: false;
}
.listView {
  grid-template-columns: auto;
}
</style>
