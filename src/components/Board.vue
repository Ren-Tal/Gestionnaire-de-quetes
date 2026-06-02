<script>
import Column from './Column.vue';
import Formu from './Form.vue';

export default {
    components: {
        Column,
        Formu
    },
    methods: {
        update({columnId, quests}) {
            const column = this.Columns.find(col => col.id === columnId)
            if (column) {
                column.quests = [...quests]
            }
        },
        deleteQuest({ columnId, questId }) {
            const column = this.Columns.find(col => col.id === columnId);
            if (column) {
                column.quests = column.quests.filter(quest => quest.id !== questId);
            }
        },
        addQuest(newQuest) {
            const column = this.Columns.find(col => col.title === newQuest.status);
            const sameTitleQuest = column.quests.find(quest => quest.title === newQuest.title);
            if (sameTitleQuest) {
                alert('Une quête avec ce titre existe déjà dans cette colonne.');
                return;
            }
            if (column) {                
                column.quests.push({ id: this.counter++, ...newQuest });
            }
        },
        updateQuest({ columnId, updatedQuest, questId }) {
            const column = this.Columns.find(col => col.id === columnId);
            if (column) {
                const questIndex = column.quests.findIndex(quest => quest.id === questId);
                if (questIndex !== -1) {
                    const sameTitleQuest = column.quests.find(quest => quest.title === updatedQuest.title && quest.id !== questId);
                    if (sameTitleQuest) {
                        alert('Une quête avec ce titre existe déjà dans cette colonne.');
                        return;
                    }
                    column.quests[questIndex] = { ...updatedQuest };
                }
            }
        }
    },
    data() {
        return {
            counter: 5,
            Columns : [
                {
                    id: 1,
                    title: 'Disponible',
                    quests: [
                        { id: 1, title: 'Trouver l\'épée magique', description: 'L\'épée magique est cachée dans la forêt enchantée.', status: 'Disponible' },
                        { id: 2, title: 'Sauver la princesse', description: 'La princesse est retenue captive dans le château du dragon.', status: 'Disponible' }
                    ]
                },  
                {
                    id: 2,
                    title: 'En Cours',
                    quests: [
                        { id: 3, title: 'Défendre le château', description: 'Défendre le château contre les attaques ennemies.', status: 'En Cours'}
                    ]
                },
                {
                    id: 3,
                    title: 'Terminé',
                    quests: [
                        { id: 4, title: 'Trouver le trésor caché', description: 'Le trésor est caché dans la grotte mystérieuse.', status: 'Terminé' }
                    ]
                },
                {
                    id: 4,
                    title: 'Abandonné',
                    quests: [
                        { id: 5, title: 'Explorer la forêt interdite', description: 'Explorer la forêt interdite à la recherche de secrets.', status: 'Abandonné' }
                    ]
                }
            ]
        }
    }
}
</script>

<template>
    <div class="board">
        <Column :column="column" v-for="column in Columns" :key="column.id" @delete-quest="deleteQuest" @update-quest="updateQuest" @update-quests="update" />
    </div>
    <Formu @add-quest="addQuest" />
</template>

<style scoped>

.board {
  display: flex;
  gap: 1rem;
  padding: 1rem;
  border: 2px solid #c9922a;
  border-radius: 4px;
  background: #f9e5b6;
  flex-direction: row;
}
</style>