<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <errorMessage v-if="errorMessage" :errorMessage="errorMessage" />
          <newNote :note="note" @addNote="addNote" />
          <div class="note-header">
            <h1>{{ title }}</h1>
            <search
              :value="search"
              placeholder="Find your note"
              @search="search = $event"
            />
          <iconsGrid :grid="grid" @gridToogle="handleGridToogle" />
          </div>
          <notesList
            :notes="notesFilter"
            :grid="grid"
            @remove="removeNote"
            @editNote="editNote"
            @submit="submit"
            @cancelEdit="cancelEdit"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import errorMessage from "@/components/errorMessage.vue";
import notesList from "@/components/NotesList.vue";
import newNote from "@/components/NewNote.vue";
import search from "@/components/Search.vue";
import iconsGrid from './components/iconsGrid.vue';

export default {
  components: {
    errorMessage,
    notesList,
    newNote,
    search,
    iconsGrid
  },
  data() {
    return {
      title: "Notes App",
      search: "",
      errorMessage: null,
      grid: true,
      note: {
        title: "",
        description: "",
        disabled: true,
        error: "",
        priorities: [
          { alias: "easy", title: "Simple" },
          { alias: "medium", title: "Hard" },
          { alias: "hard", title: "Extra hard" },
        ],
        selected: "easy",
      },
      notes: [
        {
          title: "First Note",
          description: "Description for first note",
          date: new Date(Date.now()).toLocaleString(),
          id: Math.random() + " First Note",
          disabled: true,
          error: "",
          selected: "easy",
        },
        {
          title: "Second Note",
          description: "Description for second note",
          date: new Date(Date.now()).toLocaleString(),
          id: Math.random() + " Second Note",
          disabled: true,
          error: "",
          selected: "easy",
        },
        {
          title: "Third Note",
          description: "Description for third note",
          date: new Date(Date.now()).toLocaleString(),
          id: Math.random() + " Third Note",
          disabled: true,
          error: "",
          selected: "easy",
        },
      ],
    };
  },
  computed: {
    notesFilter() {
      let array = this.notes;
      let search = this.search;
      if (!search) return array;
      search = search.trim().toLowerCase();
      array = array.filter((item) => {
        if (item.title.toLowerCase().indexOf(search) !== -1) return item;
      });
      return array;
    },
  },
  methods: {
    addNote() {
      if (this.note.title === "") {
        this.errorMessage = "Error: Title can`t be blank!";
        return false;
      }
      this.notes.push({
        ...this.note,
        date: new Date(Date.now()).toLocaleString(),
        id: Math.random() + " " + this.note.title,
        error: "",
      });
      this.errorMessage = null;
      this.note.title = "";
      this.note.description = "";
      this.note.disabled = true;
      this.note.selected = "easy";
    },
    removeNote(i) {
      let searchNote = this.notes.find((note) => note.id === i);
      this.notes.splice(this.notes.indexOf(searchNote), 1);
    },
    editNote(i) {
      this.notes.map((note) => {
        if (note.id === i) {
          note.disabled = false;
          return note;
        }
        return note;
      });
    },
    submit(id, title, description) {
      this.notes.map((note) => {
        if (note.id === id) {
          if (title === "") {
            note.error = "Error: Title can`t be blank!";
            return false;
          } else {
            note.disabled = true;
            if (title !== null) note.title = title;
            if (description !== null) note.description = description;
            note.date = new Date(Date.now()).toLocaleString();
            return note;
          }
        }
        return note;
      });
    },
    cancelEdit(id) {
      this.notes.map((note) => {
        if (note.id === id) {
          note.disabled = true;
          return note;
        }
        return note;
      });
    },
    handleGridToogle(grid) {
      this.grid = !grid;
      }
  },
};
</script>
