<script setup lang="ts">
import WelcomeItem from "./WelcomeItem.vue";
import type { Task } from "@/typing";
import { ref } from "vue";

let id:number = 0;

const editingId = ref<number | null>(null);
let inputValue = ref<string>("");
let todoArray = ref<Array<Task>>([]);




//Incrémentation et réinitialisation à l'entrée de l'input
function changeValue(): void {
  id++;
  inputValue.value = "";
}



//Ajouter un élément  tableau
function addList(): void {
  todoArray.value.push({
    id:id++,
    text: `${inputValue.value}`,
    etat: false,
  });

  changeValue();
}



//Modification de l'état en fonction du checkbox
function toggle(todo: Task): void {
  if (todo.etat) {
    todoArray.value.map((t) => {
      if (t.id == todo.id) t.etat = false;
    });
  } else {
    todoArray.value.map((t) => {
      if (t.id == todo.id) t.etat = true;
    });
  }
}



function modif(todo: Task): void {
  editingId.value = todo.id; // Définit l'ID de la tâche en cours de modification
}



function saveModif(todo: Task): void {
  // Utilisez 'todo' pour trouver et mettre à jour la tâche dans 'todoArray'
  const index = todoArray.value.findIndex((t) => t.id === todo.id);
  if (index !== -1) {
    todoArray.value[index] = todo; // Met à jour la tâche avec les nouvelles informations
  }
  editingId.value = null; // Réinitialise l'ID de la tâche en cours de modification
}




function supp(todo: Task): void {
  todoArray.value = todoArray.value.filter((t) => t.id !== todo.id);
}


</script>



<template>
  <div class="contain">
    <h1>My todolist</h1>
    <div class="container">
      <form @submit.prevent="addList">
        <br />
        <input class="modifinput" type="text" required v-model="inputValue" />
        <br />
        <button class="button-62" role="button">Ajouter</button>
      </form>
    </div>

    <div class="table">
      <table>
        <thead>
          <tr>
            <th>Avis</th>
            <th>Tâches</th>
            <th>Etat</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody v-if="todoArray.length == 0">
          <tr>
            <td colspan="4">En attentes de tâches...</td>
          </tr>
        </tbody>
        <tbody v-else>
          <tr v-for="todo in todoArray" :key="todo.id">
            <td>
              <div class="checkbox-wrapper-31">
                <input
                  type="checkbox"
                  @click="toggle(todo)"
                  :checked="todo.etat"
                />
                <svg viewBox="0 0 35.6 35.6">
                  <polyline
                    class="check"
                    points="11.78 18.12 15.55 22.23 25.17 12.87"
                  ></polyline>
                </svg>
              </div>
            </td>
            <td>
              <input
                class="editing"
                v-if="editingId === todo.id"
                type="text"
                v-model="todo.text"
              />
              <span v-else>{{ todo.text }}</span>
            </td>
            <td>{{ !todo.etat ? "Non traitée" : "Traitée" }}</td>
            <td>
              <span class="icons">
                <span
                  class="icon1"
                  @click="modif(todo)"
                  v-if="!todo.etat && editingId !== todo.id"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    height="24px"
                    viewBox="0 -960 960 960"
                    width="24px"
                    fill="#0556f7"
                    title="Modifier"
                  >
                    <path
                      d="M200-200h57l391-391-57-57-391 391v57Zm-80 80v-170l528-527q12-11 26.5-17t30.5-6q16 0 31 6t26 18l55 56q12 11 17.5 26t5.5 30q0 16-5.5 30.5T817-647L290-120H120Zm640-584-56-56 56 56Zm-141 85-28-29 57 57-29-28Z"
                    />
                  </svg>
                </span>
                <button
                  class="button-16"
                  role="button"
                  v-if="editingId === todo.id"
                  @click="saveModif(todo)"
                >
                  Sauvegarder
                </button>
                <span class="icon2" @click="supp(todo)">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    height="24px"
                    viewBox="0 -960 960 960"
                    width="24px"
                    fill="#e30202"
                    title="Supprimer"
                  >
                    <path
                      d="M280-120q-33 0-56.5-23.5T200-200v-520h-40v-80h200v-40h240v40h200v80h-40v520q0 33-23.5 56.5T680-120H280Zm400-600H280v520h400v-520ZM360-280h80v-360h-80v360Zm160 0h80v-360h-80v360ZM280-720v520-520Z"
                    />
                  </svg>
                </span>
              </span>
            </td>
          </tr>
        </tbody>
        <tfoot v-if="todoArray.length !== 0">
          <tr>
            <th colspan="4"></th>
          </tr>
        </tfoot>
      </table>
    </div>
  </div>
</template>
<style scoped>
.contain {
  width: 900px;
  background-color: beige;
  height: 500px;
  text-align: center;
  box-shadow: rgb(38, 57, 77) 0px 20px 30px -10px;
}

.table {
  border: solid 0.5px;
  display: flex;
  justify-content: space-around;
  margin: 0 auto;
  width: 70%;
}

.editing {
  border: none;
  padding: 5px;
  background-color: #addb17;
}

.button-16 {
  background-color: #1577d8;
  border: 1px solid #117eec;
  border-radius: 5px;
  font-family: arial, sans-serif;
  font-size: 14px;
  height: 36px;
  line-height: 27px;
  min-width: 54px;
  padding: 16px;
  text-align: center;
  user-select: none;
  white-space: pre;
}

.button-16:hover {
  background-color: #6cbe45;
  border-color: #080b10;
  color: beige;
}

.button-16:focus {
  border-color: #4285f4;
  outline: none;
}

.modifinput {
  border: none;
  border-radius: 10px;
  box-shadow: rgba(60, 64, 67, 0.3);
  width: 300px;
  height: 25px;
  margin: 10px;
}

label {
  font-weight: bold;
  font-size: 18px;
}

h1 {
  color: blue;
}

table th,
table td {
  padding-inline: 15px;
  padding-block: 5px;
  border: solid 1px;

  border-collapse: collapse;
}

table {
  border-collapse: collapse;
  width: 100%;
}

.button-62 {
  background: yellow;
  border: 0;
  border-radius: 12px;
  color: #44da2d;
  cursor: pointer;
  display: inline-block;
  font-size: 16px;
  user-select: none;
  -webkit-user-select: none;

  margin: 10px;
}

.button-62:not([disabled]):focus {
  box-shadow: -0.125rem -0.125rem 1rem rgba(46, 174, 247, 0.5);
}

.button-62:not([disabled]):hover {
  box-shadow: -0.125rem -0.125rem 1rem rgba(239, 71, 101, 0.5);
}

.checkbox-wrapper-31:hover .check {
  stroke-dashoffset: 0;
}

.checkbox-wrapper-31 {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 40px;
}
.checkbox-wrapper-31 .background {
  fill: #ccc;
  transition: ease all 0.6s;
  -webkit-transition: ease all 0.6s;
}
.checkbox-wrapper-31 .stroke {
  fill: none;
  stroke: #fff;
  stroke-miterlimit: 10;
  stroke-width: 2px;
  stroke-dashoffset: 100;
  stroke-dasharray: 100;
  transition: ease all 0.6s;
  -webkit-transition: ease all 0.6s;
}
.checkbox-wrapper-31 .check {
  fill: none;
  stroke: #fff;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-width: 2px;
  stroke-dashoffset: 22;
  stroke-dasharray: 22;
  transition: ease all 0.6s;
  -webkit-transition: ease all 0.6s;
}
.checkbox-wrapper-31 input[type="checkbox"] {
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  margin: 0;
  opacity: 0;
  -appearance: none;
}
.checkbox-wrapper-31 input[type="checkbox"]:hover {
  cursor: pointer;
}
.checkbox-wrapper-31 input[type="checkbox"]:checked + svg .background {
  fill: #6cbe45;
}
.checkbox-wrapper-31 input[type="checkbox"]:checked + svg .stroke {
  stroke-dashoffset: 0;
}
.checkbox-wrapper-31 input[type="checkbox"]:checked + svg .check {
  stroke-dashoffset: 0;
}

.icons {
  display: flex;
  justify-content: space-around;
}

.icon1 {
  transition: transform 0.2s;
  cursor: pointer;
}

.icon2 {
  transition: transform 0.1s;
  cursor: pointer;
}

.icon1:hover {
  transform: scale(1.1);
}

.icon2:hover {
  transform: scale(1.1);
}
</style>