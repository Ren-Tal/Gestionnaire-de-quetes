<script>
export default {}
</script>

<template>
    <table>
        <tr>
            <th>To Do</th>
            <th>In Progress</th>
            <th>Done</th>
        </tr>
        <tr>
            <td>
                <Card :quest="quest" v-for="quest in columns[0].quests" :key="quest.id" />
            </td>
            <td>
                <Card :quest="quest" v-for="quest in columns[1].quests" :key="quest.id" />
            </td>
            <td>
                <Card :quest="quest" v-for="quest in columns[2].quests" :key="quest.id" />
            </td>
        </tr>
    </table>
</template>