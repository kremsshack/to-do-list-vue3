<template>
  <div class="gestion-liste">
    <h2>todoList</h2>

    <label>
      Nouvelle Tâche :
      <input
        type="text"
        placeholder="ecrire ici"
        v-model="saisie"
        @keyup.enter="ajouterTache"
      />
    </label>

    <button @click="ajouterTache" :disabled="!saisie.trim()">
      Ajouter la Tâche
    </button>

    <hr />

    <button @click="effacerTout" :disabled="!todoList.length">
      Tout suppr
    </button>

    <p class="count-message">
      Tâches en cours : {{ tachesRestantes }} / {{ todoList.length }}
    </p>

    <h3>Tâches noté</h3>

    <ul v-if="todoList.length">
      <li
        v-for="(tache, index) in todoList"
        :key="index"
        :class="{ done: tache.done }"
      >
        {{ index + 1 }}. {{ tache.text }}

        <div class="actions">
          <button @click="toggleTache(index)" class="btn-toggle">
            {{ tache.done ? "Refaire" : "Fait" }}
          </button>
          <button @click="supprimerTache(index)" class="btn-delete">X</button>
        </div>
      </li>
    </ul>

    <p v-else class="empty-message">Vide</p>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const saisie = ref("");

const todoList = ref([]);

function ajouterTache() {
  const texte = saisie.value.trim();
  if (texte !== "") {
    todoList.value.push({
      text: texte,
      done: false,
    });
    saisie.value = "";
  }
}

function toggleTache(index) {
  todoList.value[index].done = !todoList.value[index].done;
  console.log(`Tâche ${index + 1} basculée: ${todoList.value[index].text}`);
}

function supprimerTache(index) {
  const tacheSupprimee = todoList.value[index].text;

  todoList.value.splice(index, 1);
  console.log(`Tâche supprimée : ${tacheSupprimee}`);
}

function effacerTout() {
  console.log("--- Contenu du Tableau avant effacement ---");
  console.table(todoList.value);
  console.log("-----------------------------------------");

  todoList.value = [];
}

const tachesRestantes = computed(function () {
  return todoList.value.filter(function (tache) {
    return !tache.done;
  }).length;
});
</script>

<style scoped>
.gestion-liste {
  max-width: 500px;
  margin: 30px auto;
  padding: 20px;
  border: 1px solid #3498db;
  border-radius: 8px;
  background-color: #1043c2;
}
input {
  padding: 8px;
  margin-right: 10px;
}
button {
  padding: 8px 15px;
  background-color: #3498db;
  color: rgb(241, 233, 233);
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-left: 5px;
}
button:disabled {
  background-color: #b10606;
  cursor: not-allowed;
}
ul {
  list-style-type: none;
  padding: 0;
  margin-top: 15px;
}
li {
  background-color: #0695b9;
  padding: 8px;
  margin-bottom: 5px;
  border-left: 4px solid #3498db;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li.done {
  text-decoration: line-through;
  opacity: 1.6;
  border-left: 4px solid #2ecc71;
}
.btn-toggle {
  background-color: #2ecc71;
}
.btn-delete {
  background-color: #e74c3c;
}
.empty-message {
  color: #e74c3c;
  font-style: italic;
}
.count-message {
  font-weight: bold;
  margin-top: 10px;
  color: #2ecc71;
}
</style>
