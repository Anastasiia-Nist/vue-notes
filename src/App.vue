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
            <div class="icons">
              <svg
                :class="{ active: grid }"
                @click="grid = true"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
              </svg>
              <svg
                :class="{ active: !grid }"
                @click="grid = false"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3" y2="6"></line>
                <line x1="3" y1="12" x2="3" y2="12"></line>
                <line x1="3" y1="18" x2="3" y2="18"></line>
              </svg>
            </div>
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

export default {
  components: {
    errorMessage,
    notesList,
    newNote,
    search,
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
        priority: "ordinary",
        disabled: true,
        error: "",
      },
      notes: [
        {
          title: "First Note",
          description: "Description for first note",
          date: new Date(Date.now()).toLocaleString(),
          id: Math.random() + " First Note",
          priority: "ordinary",
          disabled: true,
          error: "",
        },
        {
          title: "Second Note",
          description: "Description for second note",
          date: new Date(Date.now()).toLocaleString(),
          id: Math.random() + " Second Note",
          priority: "ordinary",
          disabled: true,
          error: "",
        },
        {
          title: "Third Note",
          description: "Description for third note",
          date: new Date(Date.now()).toLocaleString(),
          id: Math.random() + " Third Note",
          priority: "ordinary",
          disabled: true,
          error: "",
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
        error: ''
      });
      this.errorMessage = null;
      this.note.title = "";
      this.note.description = "";
      this.note.priority = "ordinary";
      this.note.disabled = true;
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
  },
};
</script>
