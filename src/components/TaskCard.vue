<script setup lang="ts">
import { ref, type PropType } from "vue";
import { Icon } from "@iconify/vue";

const props = defineProps({
  created: {
    type: Number as PropType<number>,
    required: true,
  },
  id: String as PropType<string>,
  task: String as PropType<string>,
  index: Number as PropType<number>,
  editing: Boolean as PropType<boolean>,
  completed: Boolean as PropType<boolean>,
});

const emits = defineEmits(["edit", "save", "completed", "delete"]);

const editedInput = ref(props.task);

function date(timestamp: number) {
  const date = new Date(timestamp);

  const formattedDate = `${("0" + date.getHours()).slice(-2)}:${(
    "0" + date.getMinutes()
  ).slice(-2)} ${getDayOfWeek(date)} (${getMonthAbbreviation(
    date
  )} ${date.getDate()})`;

  function getMonthAbbreviation(date) {
    const months = [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "Jun",
      "Jul",
      "Aug",
      "Sep",
      "Oct",
      "Nov",
      "Dec",
    ];
    return months[date.getMonth()];
  }

  function getDayOfWeek(date: Date) {
    const daysOfWeek = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
    return daysOfWeek[date.getDay()];
  }

  return formattedDate;
}
</script>

<template>
  <div class="card">
    <div class="checkbox">
      <button
        v-if="!props.completed"
        class="done"
        @click="$emit('completed', index)"
      >
        <Icon icon="line-md:circle" />
      </button>

      <button v-else class="done" @click="$emit('completed', index)">
        <Icon icon="line-md:confirm-circle" />
      </button>
      <div>
        <p
          v-if="!props.editing"
          :class="{
            completed: props.completed,
          }"
        >
          {{ props.task }}
        </p>
        <input autofocus type="text" v-else v-model="editedInput" />
        <h2>{{ date(props.created) }}</h2>
      </div>
    </div>

    <div class="Btn-holder">
      <button
        class="Edit"
        v-if="!props.editing"
        @click="$emit('edit', props.index)"
      >
        <Icon icon="mdi:pencil-outline" />
      </button>
      <button
        v-else
        class="Edit"
        @click="$emit('save', props.index, editedInput)"
      >
        <Icon icon="mdi:check-underline" />
      </button>
      <button
        v-if="!props.editing"
        class="Delete"
        @click="$emit('delete', props.id)"
      >
        <Icon icon="mdi:delete" />
      </button>
      <button v-else class="Delete" @click="emits('edit', props.index)">
        <Icon icon="mdi:close-octagon" />
      </button>
    </div>
  </div>
</template>

<style scoped>
.card h2 {
  font-size: 10px;
  font-weight: 200;
}

h1 {
  font-size: 70px;
  font-weight: 900;
}

p {
  font-size: 15px;
}

input {
  border: 0;
  margin: 0;
  padding: 0;
  width: 100%;
  opacity: 0.6;
  outline: none;
  font-size: 15px;
  color: #273b52;
  font-weight: 700;
  box-shadow: none;
  margin-bottom: 5px;
  border-radius: 5px;
  background-color: transparent;
  font-family: "Noto Sans", sans-serif;
}
.card {
  width: 100%;
  display: flex;
  padding: 20px;
  text-align: left;
  margin-bottom: 15px;
  align-items: center;
  border: 1px solid #1a2f20;
  border-radius: 15px;
  background-color: #f9fbfc;
  justify-content: space-between;
}

.card p {
  font-size: 15px;
  font-weight: bold;
  margin-bottom: 10px;
}

.Add,
.Edit,
.Delete {
  width: 30px;
  height: 30px;
  border: none;
  display: flex;
  color: #273b52;
  font-size: 20px;
  border-radius: 50%;
  align-items: center;
  justify-content: center;
  border: 1px solid #1a2f20;
  background-color: #f1f5f8;
}

.Btn-holder {
  gap: 10px;
  display: flex;
  align-items: center;
}

.checkbox {
  gap: 10px;
  display: flex;
  align-items: center;
}
.done {
  border: none;
  color: #273b52;
  font-size: 25px;
  background-color: transparent;
}

.completed {
  text-decoration: line-through;
}
</style>
