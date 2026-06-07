<template>
  <div class="column">
    <h2>
      {{ column.title }} <span class="count">({{ questCount }})</span>
    </h2>
    <VueDraggable
      class="drag-zone"
      v-model="localQuests"
      group="quest"
      item-key="id"
      :animation="300"
      :fallback-class="true"
      :fallback-tolerance="5"
    >
      <div v-for="quest in localQuests" :key="quest.id">
        <Card :quest="quest" @delete="deleteQuest" @update="updateQuest" />
      </div>
    </VueDraggable>
  </div>
</template>

<style scoped>
@media (max-width: 768px) {
  .column {
    width: 100%;
    max-width: 100%;
    min-width: 0;
  }

  .drag-zone {
    width: 100%;
  }
}

.column {
  flex: 1;
  min-width: 0;
  max-width: 25%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
  overflow: hidden;
}

.column h2 {
  color: #d8a563;
  margin: 1rem 0;
  font-size: 1.5rem;
  font: bold 1.5rem;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.8);
}
.card {
  margin-top: 1rem;
}

.drag-zone {
  min-height: 100px; /* ← zone de drop toujours visible */
  width: 100%;
}

.count {
  font-size: 0.9rem;
  color: #442d03;
  font-weight: Bold;
}
</style>

<script>
import { VueDraggable } from 'vue-draggable-plus'
import Card from './Card.vue'
export default {
  props: ['column'],
  emits: ['delete-quest', 'update-quest', 'update-quests'],
  components: {
    Card,
    VueDraggable,
  },
  data() {
    return {
      localQuests: [...this.column.quests],
    }
  },
  watch: {
    'column.quests': {
      handler(newQuests) {
        if (JSON.stringify(newQuests) !== JSON.stringify(this.localQuests)) {
          this.localQuests = [...newQuests]
        }
      },
      deep: true,
    },
    localQuests: {
      handler(newQuests) {
        const newIds = newQuests.map((q) => q.id).join(',')
        const colIds = this.column.quests.map((q) => q.id).join(',')
        if (newIds !== colIds) {
          this.$emit('update-quests', { columnId: this.column.id, quests: newQuests })
        }
      },
      deep: true,
    },
  },
  methods: {
    deleteQuest(questId) {
      this.$emit('delete-quest', { columnId: this.column.id, questId })
    },
    updateQuest({ updatedQuest, questId }) {
      this.$emit('update-quest', { columnId: this.column.id, updatedQuest, questId })
    },
  },
  computed: {
    questCount() {
      return this.localQuests.length
    },
  },
}
</script>
