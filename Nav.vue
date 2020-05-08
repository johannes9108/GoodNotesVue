<template>
  <nav class="navButtonContainer">
    <button
      class="navItem"
      v-on:click="buttonClick(buttonKeys.view)"
      v-bind:key="buttonKeys.view"
    >View</button>
    <button
      class="navItem"
      v-on:click="buttonClick(buttonKeys.new)"
      v-bind:key="buttonKeys.new"
    >New Note</button>

    <button
      class="navItem"
      v-on:click="buttonClick(buttonKeys.textColor)"
      v-bind:key="buttonKeys.textColor"
    >
      <input
        v-bind:key="navTextColor"
        v-on:change="updateTextColor(navTextColor)"
        type="color"
        v-model="navTextColor"
      />
    </button>
    <button
      class="navItem"
      v-on:click="buttonClick(buttonKeys.bgColor)"
      v-bind:key="buttonKeys.bgColor"
    >
      <input v-on:change="updateBgColor(navBgColor)" type="color" v-model="navBgColor" />
    </button>
  </nav>
</template>
<script>
export default {
  data() {
    return {
      buttonKeys: {
        view: "view",
        new: "new",
        edit: "edit",
        textColor: "textColor",
        bgColor: "bgColor"
      },
      navTextColor: "#000000",
      navBgColor: "#FFFFFF"
    };
  },
  computed: {
    test() {
      return "test";
    }
  },

  components: {},
  methods: {
    buttonClick(key) {
      console.log(`${key} was clicked! Send to parent`);

      switch (key) {
        case "view":
          this.$emit("toggleViewEvent", "Empty");
          break;
        case "new":
          this.$emit("newNoteEvent", "createNewNote");
          break;
        case "edit":
          this.$emit("editNoteEvent", "editOldNote");
          break;
        case "textColor":
          this.$emit("changeTextColorEvent", "Empty");
          break;
        case "noteColor":
          this.$emit("changeBgColorEvent", "Empty");
          break;
      }
    },
    updateTextColor(color) {
      this.$emit("changeTextColorEvent", color);
    },
    updateBgColor(color) {
      this.$emit("changeBgColorEvent", color);
    }
  }
};
</script>
<style>
.navButtonContainer {
  display: flex;
  background-color: gray;
  justify-content: space-evenly;
}
.navItem {
  font-size: 2rem;
  width: 10%;
  height: 100%;
}
.navItem input {
  width: 100%;
  height: 100%;
}

@media screen and (min-width: 1px) {
  .navButtonContainer {
    background-color: rgb(157, 75, 75);
  }
}
@media screen and (min-width: 768px) {
  .navButtonContainer {
    background-color: rgb(42, 82, 91);
  }
}
@media screen and (min-width: 1024px) {
  .navButtonContainer {
    background-color: rgb(24, 56, 28);
  }
}
</style>