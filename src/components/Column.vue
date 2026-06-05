
<template>
    <div class="column">
        <h2>{{ column.title }} <span class="count">({{ questCount }})</span></h2>
        <VueDraggable class="drag-zone" v-model="localQuests" group="quest" item-key="id" :animation="200" >
            <div v-for="quest in localQuests" :key="quest.id">
                <Card :quest="quest" @delete="deleteQuest" @update="updateQuest" />
            </div>
        </VueDraggable>
    </div>
</template>

<style scoped>
.column {
  flex: 1;
  /*background: #e8d5a3;
  border: 2px solid #c9922a;
  padding: 1rem;
  border-radius: 4px;*/
  min-width: 0;
  max-width: 25%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
  overflow: hidden;
}

.column h2 {
  color: #dd8f77;
  margin: 0 0 1rem;
  font-size: 1.5rem;
  font: bold 1.5rem 'Arial', sans-serif;
  text-shadow: 1px 1px 3px rgba(0,0,0,0.8)
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
    color: #6d4804;
    font-weight: Bold;
}

</style>

<script>
import { VueDraggable } from 'vue-draggable-plus';
import Card from './Card.vue'
export default {
    props: ['column'],
    emits: ['delete-quest', 'update-quest', 'update-quests'],
    components: {
        Card,
        VueDraggable
    },
    data() {
        return {
            localQuests: [...this.column.quests]
        }
    },
    watch : {
        'column.quests': {
            handler(newQuests){
                if (JSON.stringify(newQuests) !== JSON.stringify(this.localQuests)) {
                    this.localQuests = [...newQuests]
                }
            },
            deep: true
        },
        localQuests :  {
            handler(newQuests) {
            const newIds = newQuests.map(q => q.id).join(',')
            const colIds = this.column.quests.map(q => q.id).join(',')
            if (newIds !== colIds) {
                this.$emit('update-quests', { columnId: this.column.id, quests: newQuests })
            }
            },
            deep: true
        }
    },
    methods: {
        deleteQuest(questId) {
            this.$emit('delete-quest', { columnId: this.column.id, questId });
        },
        updateQuest({ updatedQuest, questId }) {
            this.$emit('update-quest', { columnId: this.column.id, updatedQuest, questId });
        }
    },
    computed : {
        questCount() {
            return this.localQuests.length;
        }
    }
}
</script>





