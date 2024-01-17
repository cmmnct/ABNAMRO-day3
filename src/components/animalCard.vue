<template>
  <div class="card">
    <button class="edit-btn" @click="enableEditMode" v-if="!editMode">e</button>
    <div v-if="editMode"><label>Name:</label><input type="text" v-model="animal.name" /></div>

    <h1 v-else>{{ name }}</h1>
    <hr />
    <img :src="`/assets/img/${type}.jpg`" />
    <div v-if="editMode">
      <label>Type</label><input type="text" v-model="animal.type" /><br />
      <label>Latin name:</label> <input type="text" v-model="animal.latin" /><br />
      <label>With us since:</label><input type="text" v-model="animal.since" /><br />
      <label>Diet:</label>
      <select name="food" multiple id="food">
        <option
          v-for="(kind, index) in food"
          :key="index"
          value="index"
          :selected="returnSelectedFood(index)"
        >
          {{ food[index] }}
        </option>
      </select>
      <br />
      <button @click="updateAnimal">Submit</button>
    </div>
    <div v-else>
      <p>
        <span class="type">{{ type }}</span> (<span class="latin">{{ latin }}</span
        >)
      </p>
      <p></p>
      <p>{{ since }}</p>
      <p>{{ returnDiet(diet) }}</p>
    </div>
    <br class="clearfloat" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'
import type { Animal } from './zooList.vue'

const { id, name, type, since, latin, diet } = defineProps<Animal>()
const emit = defineEmits<{
  (e: 'update', value: Animal): void
}>()

//const myString = defineModel();

const animal: Ref<Animal> = ref({
  id,
  name,
  type,
  latin,
  since,
  diet
})
const food: Ref<string[]> = ref([
  'rice',
  'wheat',
  'corn',
  'meat',
  'dogfood',
  'catfood',
  'lettuce',
  'vegetables',
  'peel of vegetables and fruits',
  'grass',
  'leaves',
  'herbs',
  'scrub',
  'heather'
])
let editMode = ref(false)
//const editFood:Ref<number[]> = ref([])

function returnSelectedFood(index: number): boolean {
  if (animal.value.diet.indexOf(index) > -1) {
    return true
  } else return false
}

function returnDiet(arr: number[]): string {
  let myFood: string = ''
  arr.forEach((item) => {
    myFood += ` ${food.value[item] as string}`
  })
  return myFood
}

function enableEditMode() {
  editMode.value = true
}

function updateAnimal() {
  //
  emit('update', animal.value)
  editMode.value = false
}
</script>

<style scoped>
.card {
  width: 30vw;
  padding: 15px;
  border: grey 1px solid;
  border-radius: 5px;
  box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.3);
  margin: 20px;
}
.card img {
  width: 64px;
  aspect-ratio: 1/1;
  float: left;
  margin: 5px;
}
.latin {
  font-style: italic;
}
.type {
  font-weight: bold;
}

hr {
  margin-bottom: 10px;
}

.clearfloat {
  clear: both;
}

.edit-btn {
  float: right;
}
</style>
