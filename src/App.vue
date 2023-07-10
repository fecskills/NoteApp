<script setup lang="ts">
import { ref, reactive } from "vue";

const showModal = ref(false)
const errorMessage = ref("")
const addNote = () => {
  if (newNote.content.length < 10) {

    return errorMessage.value = "Note can only be 10 characters";
  }

  notes.push({
    id: Math.floor(Math.random() * 100000),
    color: pickColor(),
    date: new Date(),
    excerpt: newNote.content.substring(0, 100),
    content: newNote.content,
  })
  showModal.value = false;
  newNote.content = "";
}

const pickColor = () => {
  return "hsl(" + (Math.random() * 360) + ", 100%, 75%)"
}

const newNote = reactive({
  id: "",
  color: "",
  date: "",
  excerpt: "",
  content: "",

})

interface INote {
  id: number
  color: string
  date: Date
  excerpt: string
  content: string
}

const notes = reactive([]) as INote[]

</script>
<template>
  <div>
    <div v-if="showModal" class="overlay">
      <div class="custom-modal">
        <textarea v-model.trim="newNote.content" name="note" placeholder="Click to add a new note" id="note" cols="30"
          rows="10"></textarea>
        <p v-if="errorMessage" class="text-danger mb-0"> {{ errorMessage }}</p>
        <button @click="addNote" class="btn btn-primary my-2">Add Note</button>
        <button @click="showModal = false" class="btn btn-danger">Close Note</button>
      </div>
    </div>
    <section>
      <div class="container">
        <header class="py-5 justify-content-between align-items-center d-flex">
          <h1 class="">Notes</h1>
          <button @click="showModal = true" class="btn btn-secondary w-50px rounded-circle">+</button>
        </header>
        <section>
          <div class="container">
            <div class="d-flex">
              <div v-for="(note) in notes" :key="note.id" class="note me-2" :style="{ backgroundColor: note.color }">
                <p class="flex-fill text-muted">{{ note.excerpt.substring(0, 100) }}</p>
                <p class="fs-11 mb-0 fw-bold">{{ note.date.toLocaleDateString() }}</p>
              </div>
            </div>
          </div>
        </section>
      </div>
    </section>
  </div>
</template>

<style>
.custom-pad {
  padding-left: 150px !important;
  padding-right: 150px !important;
}

.fs-11 {
  font-size: 11px;
}

.modal-content {
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 500px;
  height: 500px;
  padding: 10px;
  margin-bottom: 10px;
  transition: all 0.3s ease-in-out;
  background-color: orange;
  display: flex;
  flex-direction: column;
}

.note {
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 200px;
  height: 200px;
  padding: 10px;
  margin-bottom: 10px;
  transition: all 0.3s ease-in-out;
  background-color: orange;
  display: flex;
  flex-direction: column;
}

.note:hover {
  box-shadow: 3px 2px 5px 0px #ccc;
  cursor: pointer;
}

.overlay {
  position: fixed;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.465);
  z-index: 10;
  display: flex;
}

.custom-modal {
  width: 500px;
  height: 300px;
  background-color: #fff;
  margin: auto;
  padding: 20px;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
}
</style>