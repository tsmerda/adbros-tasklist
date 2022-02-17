<script setup>
import Done from '../components/icons/Done.vue'

defineProps({
  data: {
    type: Object,
    required: true
  }
})

function getStatus (status) {
  return status === 'todo' ? 'Nesplněno' : 'Splněno'
}

</script>

<template>
  <div class="task-tile">
    <div class="task-tile__text">
      <div class="task-tile__text__done"
           :style="{ background: data.status === 'done' ? '#00ff00' : 'transparent'}"
           @click="$emit('on-done', data.id)">
        <Done/>
      </div>

      <span class="task-tile__text__title">{{ data.title }}</span>
    </div>

    <div class="task-tile__actions">
    <span class="task-tile__actions__status"
          :style="{ color: data.status === 'done' ? '#0ed50e' : 'darkgray'}">
      {{ getStatus(data.status) }}
    </span>

      <button class="task-tile__actions__close"
              @click="$emit('on-remove', data.id)">
        Smazat
      </button>
    </div>
  </div>
</template>

<style scoped>
.task-tile {
  width: 100%;
  padding: 1.5rem 1rem;
  border-radius: .5rem;
  display: flex;
  align-items: center;
}

.task-tile__text {
  display: flex;
  align-items: center;
  width: 60%;
}

.task-tile__text__title {
  font-size: 1.1rem;
  font-weight: 500;
  margin-left: 1rem;
  word-break: break-word;
}

.task-tile__actions {
  display: flex;
  width: 40%;
  justify-content: space-between;
  align-items: center;
}

.task-tile__actions__status {
  font-size: .9rem;
  margin-left: 1rem;
}

.task-tile__text__done {
  cursor: pointer;
  width: 34.5px;
  height: 34.5px;
  min-width: 34.5px;
  min-height: 34.5px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #a7a7a7;
  border-radius: 50%;
  transition: background .3s ease;
}

.task-tile__text__done svg {
  fill: #2c3e50;
  height: 100%;
}

.task-tile__actions__close {
  border: 1px solid #a7a7a7;
  margin-left: auto;
}

@media (max-width: 568px) {
  .task-tile {
    flex-direction: column;
    row-gap: 1rem;
  }

  .task-tile__actions {
    width: 100%;
  }

  .task-tile__text {
    width: 100%;
  }

  .task-tile__actions__status {
    margin-left: 0;
  }
}

</style>
