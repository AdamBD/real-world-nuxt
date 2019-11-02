<template>
  <div>
    <h1>Events</h1>
    <EventCard v-for="event in this.events" v-bind:event="event" />
  </div>
</template>

<script>
import EventCard from '../components/EventCard.vue'
import { mapState } from 'vuex'
export default {
  components: {
    EventCard
  },
  head() {
    return {
      title: 'Event listing '
    }
  },
  async fetch(context) {
    try {
      await context.store.dispatch('events/fetchEvents')
    } catch (e) {
      context.error({
        statusCode: 503,
        message: 'Something went wrong. Sorry.'
      })
    }
  },
  computed: mapState({
    events: (state) => state.events.events
  })
}
</script>

<style></style>
