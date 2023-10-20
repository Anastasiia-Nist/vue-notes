<template>
  <section class="notes" :class="{ full: !grid }">
    <div
      class="note"
      v-for="{
        title,
        description,
        date,
        id,
        priority,
        disabled,
        error,
      } in notes"
      :key="id"
    >
      <form :id="id" @keyup.enter="submit(id)" @keyup.esc="cancelEdit(id)">
        <div>
          <div class="note-header" :class="{ full: !grid }">
            <input
              name="title"
              :value="title"
              :disabled="disabled"
              @input="inputChange"
            />
            <div class="priority">
              <svg
                :class="priority"
                version="1.1"
                id="Layer_1"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                x="0px"
                y="0px"
                viewBox="0 0 426.667 426.667"
                style="enable-background: new 0 0 426.667 426.667"
                xml:space="preserve"
              >
                <polygon
                  style="fill: #fac917"
                  points="213.333,10.441 279.249,144.017 426.667,165.436 320,269.41 345.173,416.226 213.333,346.91 
	81.485,416.226 106.667,269.41 0,165.436 147.409,144.017 "
                />
              </svg>
              <svg
                :class="priority"
                version="1.1"
                id="Layer_1"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                x="0px"
                y="0px"
                viewBox="0 0 426.667 426.667"
                style="enable-background: new 0 0 426.667 426.667"
                xml:space="preserve"
              >
                <polygon
                  style="fill: #fac917"
                  points="213.333,10.441 279.249,144.017 426.667,165.436 320,269.41 345.173,416.226 213.333,346.91 
	81.485,416.226 106.667,269.41 0,165.436 147.409,144.017 "
                />
              </svg>
            </div>
            <p class="note-remove" @click="removeNote(id)">X</p>
          </div>
          <span>{{ error }}</span>
        </div>
        <div class="note-body">
          <textarea
            name="description"
            :value="description"
            :disabled="disabled"
            @input="inputChange"
            rows="5"
          />
          <div>
            <span>{{ date }}</span>
            <p @click="editNote(id)">&#9998;</p>
          </div>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      title: null,
      description: null,
    };
  },
  props: {
    notes: {
      type: Array,
      required: true,
    },
    grid: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    inputChange(event) {
      const { name, value } = event.target;
      if (name === "title") this.title = value;
      if (name === "description")this.description = value;
    },
    removeNote(i) {
      this.$emit("remove", i);
    },
    editNote(i) {
      this.$emit("editNote", i);
    },
    submit(i) {
      this.$emit("submit", i, this.title, this.description);
    },
    cancelEdit(id) {
      this.$emit("cancelEdit", id);
    },
  },
};
</script>

<style lang="scss" >
.notes {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, auto);
  padding: 40px 0;
  &.full {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
.note {
  width: 90%;
  padding: 18px 20px;
  margin-bottom: 20px;
  background-color: #fff;
  transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);
  box-shadow: 0 30px 30px rgba(0, 0, 0, 0.02);
  display: flex;
  justify-content: center;

  form {
    text-align: center;
    width: 84%;
  }
  input,
  textarea {
    padding: 10px;
    color: #000;
  }
  input:disabled,
  textarea:disabled {
    border: none;
    background-color: #fff;
  }
}
.note:hover {
  box-shadow: 0 30px 30px rgba(0, 0, 0, 0.04);
  transform: translate(0, -6px);
  transition-delay: 0s !important;
}
.note-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
  input {
    width: 60%;
    margin-bottom: 0;
    font-size: 22px;
  }
  input:disabled {
    color: #3a23a0;
  }
  &.full {
    gap: 20px;
  }
  h1 {
    font-size: 22px;
  }
  svg {
    width: 24px;
    margin-right: 12px;
    color: #999;
    cursor: pointer;
    &:last-child {
      margin: 0;
    }
    &.active {
      color: #3a23a0;
    }
  }
  .priority {
    display: flex;
    svg {
      display: none;
    }
    .important:last-child {
      display: block;
    }
    .essential {
      display: block;
    }
  }
  span {
    font-size: 10px;
    color: red;
  }
}
.note-body {
  div {
    display: flex;
    align-items: center;
    justify-content: space-between;
    p {
      cursor: pointer;
    }
  }
  span {
    font-size: 14px;
    color: #999;
    padding-left: 10px;
  }
  textarea {
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
    resize: none;
  }
  textarea:disabled {
    color: #000;
    overflow: auto;
  }
}
.note-remove {
  cursor: pointer;
}
</style>