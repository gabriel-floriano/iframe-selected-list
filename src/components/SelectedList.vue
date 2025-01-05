<template>
  <div>
    <ul>
      <li v-for="email in selectedEmails" :key="email">{{ email }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";

const selectedEmails = ref([]);

const toggleEmailSelection = ({ selected, email }) => {
  const index = selectedEmails.value.indexOf(email);
  if (index !== -1) {
    if (!selected) {
      selectedEmails.value.splice(index, 1);
    }
  } else if (selected) {
    selectedEmails.value.push(email);
  }
};

const bubbleSort = (arr) => {
  let len = arr.length;
  for (let i = 0; i < len; i++) {
    for (let j = 0; j < len - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
};

const handlePostMessage = (event) => {
  if (event.data) {
    if (event.data.type === "TOGGLE_EMAIL" && event.data.email) {
      toggleEmailSelection(event.data.email);
    } else if (event.data.type === "SORT_EMAILS") {
      bubbleSort(selectedEmails.value);
    }
  }
};

onMounted(() => {
  window.addEventListener("message", handlePostMessage);
});

onBeforeUnmount(() => {
  window.removeEventListener("message", handlePostMessage);
});
</script>

<style>
body {
  margin: 0 !important;
  padding: 0 !important;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 8px;
  margin: 4px 0;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
