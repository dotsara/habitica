<template lang="pug">
li
  ul
    li 
      strong {{task.text}}
    li(v-if="task.type === 'habit'") up: {{task.up}}, down: {{task.down}}
    li value: {{task.value}}
    template(v-if="task.type === 'daily' || task.type === 'todo'")
      li completed: {{task.completed}}
      li
        span checklist
        ul 
          li(v-for="checklist in task.checklist") {{checklist.text}}
    template(v-if="task.type === 'daily'") 
      li streak: {{task.streak}}
      li repeat: {{task.repeat}}
    li(v-if="task.type === 'todo'") due date: {{task.date}}
    li attribute {{task.attribute}}
    li difficulty {{task.priority}}
    li tags {{getTagsFor(task)}}
</template>

<script>
import { mapState, mapGetters } from 'client/libs/store';

export default {
  props: ['task'],
  computed: {
    ...mapState({user: 'user.data'}),
    ...mapGetters({getTagsFor: 'tasks:getTagsFor'}),
  },
};
</script>