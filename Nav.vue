<template>
  <nav class="navButtonContainer">
    <a class="navItem" v-on:click="buttonClick(buttonKeys.view)" v-bind:key="buttonKeys.view">View</a>
    <a class="navItem" v-on:click="buttonClick(buttonKeys.new)" v-bind:key="buttonKeys.new">New Note</a>

    <a
      class="navItem"
      v-on:click="buttonClick(buttonKeys.textcolor)"
      v-bind:key="buttonKeys.textcolor"
    >
      <input
        v-bind:key="navTextColor"
        @change="updateTextColor(navTextColor)"
        type="color"
        v-model="navTextColor"
      />
    </a>
    <a class="navItem" v-on:click="buttonClick(buttonKeys.bgcolor)" v-bind:key="buttonKeys.bgcolor">
      <input @change="updateBgColor(navBgColor)" type="color" v-model="navBgColor" />
    </a>
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
        textcolor: "textcolor",
        bgcolor: "bgcolor"
      },
      navTextColor: this.textcolor,
      navBgColor: this.bgcolor
    };
  },
  computed: {
    test() {
      return "test";
    }
  },
  watch: {
    textcolor(textcolor) {
      console.log("TC CHANGED!");
      console.log(textcolor);
      this.navTextColor = this.textcolor;
    },
    bgcolor(bgcolor) {
      console.log("bg CHANGED!");
      console.log(bgcolor);
      this.navBgColor = this.bgcolor;
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
      }
    },
    updateTextColor(color) {
      this.$emit("changeTextColorEvent", color);
    },
    updateBgColor(color) {
      this.$emit("changeBgColorEvent", color);
    }
  },
  props: {
    textcolor: String,
    bgcolor: String
  }
};
</script>
<style>
.navButtonContainer {
  background-color: rgb(72, 71, 71);
}
input {
  border: transparent;
  background: transparent;
  height: 100%;
  width: 100%;
}
.navItem {
  font-size: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
}
.navItem:hover {
  background-color: rgb(120, 119, 119);
  border-bottom: 1px solid white;
}

@media screen and (min-width: 1px) {
  .navButtonContainer {
    position: absolute;
    /* display: flex;
    flex-flow: column; */
    display: grid;
    grid-template-rows: repeat(4, 1fr);
    height: 200px;
    width: 200px;
    top: 100%;
    right: 0;
  }
}
@media screen and (min-width: 768px) {
  .navButtonContainer {
    position: unset;
    height: unset;
    width: 100%;
    gap: 2rem;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: auto;
  }
}
</style>