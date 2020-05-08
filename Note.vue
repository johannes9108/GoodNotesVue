<template>
  <div
    v-if="isVisible"
    class="note"
    v-bind:style="styleObject"
    @mouseover="hover = true"
    @mouseleave="hover = false"
    :class="{ highlight: hover }"
    @click="expand()"
  >
    <!-- <h3 contenteditable>{{title}}</h3> -->
    <div class="header">
      <input v-model="noteTitle" type="text" placeholder="New Title" />
      <button v-if="isExpanded" v-on:click.stop="saveItem">❌</button>
      <button v-on:click="removeItem">❌</button>
    </div>
    <!-- <p contenteditable>{{content}}</p> -->
    <textarea v-model="noteContent" placeholder="New Content"></textarea>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isVisible: true,
      isExpanded: false,
      hover: false,
      noteTitle: this.post.title,
      noteContent: this.post.content,
      noteId: this.post.id,
      styleObject: {
        color: this.post.textColor,
        backgroundColor: this.post.bgColor
      }
    };
  },
  components: {},
  methods: {
    removeItem() {
      console.log("Remove this note");
      this.$emit("removeThisNote", this.noteId);
    },
    highlightOn() {
      console.log("input");
      //   this.$el.classList.add("highlight");
    },
    highlightOff() {
      console.log("textarea");
      //   this.$el.classList.remove("highlight");
    },
    expand() {
      if (!this.isExpanded) {
        this.isExpanded = true;
        this.$emit("editModeViewEvent", this.noteId);
      }
    },
    saveItem() {
      console.log("Editing finished");
      this.isExpanded = false;
      this.$emit("exitEditModeViewEvent", this.noteId);
    },
    toggleHidden() {
      this.isVisible = !this.isVisible;
      console.log("Toggla synbarhet");
    }
  },
  props: {
    post: Object
  }
};
</script>
<style>
.note {
  box-shadow: 5px 5px 15px grey;
  border: 1px dashed white;
  padding: 1rem;
  margin: 1rem;
  overflow: auto;
  max-height: 150px;
}
::placeholder {
  font-size: 1.5rem;
}

.note input,
.note textarea,
.note button {
  width: 100%;
  background: transparent;
  border: none;
  resize: none;
}
.note input,
textarea {
  color: inherit;
}
.note .header {
  display: grid;
  grid-template-columns: 1fr auto auto;
  justify-content: flex-start;
  border-bottom: 1px solid black;
}
.note button {
}

.note.highlight {
  transform: scale(1.05);
  box-shadow: 10px 10px 0px grey;
}
</style>
