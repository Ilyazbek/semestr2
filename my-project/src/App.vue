<template>
  <div>
    <h2>Лабораторная работа #11</h2>
    <form>
      <label for="name">Имя:</label>
      <input type="text" name="name" v-model="name" required
        :class="{ invalid: isNameInvalid, warning: isNameWarning }" />
    </form>

    <p v-show="name.length > 0">Текст появился в поле</p>

    <p v-show="name.length > 0">
      Количество символов:
      <span :class="{
        'invalid-text': isNameInvalid,
        'warning-text': isNameWarning,
      }">
        {{ isNameInvalid ? maxNameLength - name.length : nameCounter }} / {{ maxNameLength }}
      </span>
    </p>

    <button @click="addEntry" :disabled="isFormEmpty">Добавить запись</button>

    <p>
      {{ entries.length === 0 ? 'Нет записей' : 'Записи' }}
    </p>


    <ul>
      <li v-for="entry in entries" :key="entry.name">
        {{ entry.name }} -- {{ entry.time }}
      </li>
    </ul>

    <p v-show="entries.length > 0">Количество записей: {{ entries.length }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      submitted: false,
      maxNameLength: 20,
      entries: [],
    };
  },
  computed: {
    isFormEmpty() {
      return !this.name;
    },
    nameCounter() {
      return this.name.length;
    },
    isNameInvalid() {
      return this.name.length > this.maxNameLength;
    },
    isNameWarning() {
      return this.name.length > 0 && this.name.length <= 10;
    },
  },
  methods: {
    addEntry() {
      if (this.name) {
        const date = new Date();
        const formattedTime = new Intl.DateTimeFormat("ru", {
          year: "numeric",
          month: "numeric",
          day: "numeric",
          hour: "numeric",
          minute: "numeric",
        }).format(date);

        const newEntry = {
          id: this.entries.length + 1,
          name: this.name,
          time: formattedTime,
        };

        this.entries.push(newEntry);
        this.name = "";
      }
    },
  },
};
</script>
<style>
@import './app.css';
</style>