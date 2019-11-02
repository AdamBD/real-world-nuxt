<template>
  <div>
    <div class="event-header">
          <span class="eyebrow">
            @{{ event.time }} on {{ event.date }}
          </span>
      <h1 class="title">
        {{ event.title }}
      </h1>
      <h5>Organized by {{ event.organizer ? event.organizer.name : '' }}</h5>
      <h5>Category: {{ event.category }}</h5>
    </div>

    <span name="map">
          <h2>Location</h2>
        </span>

    <address>{{ event.location }}</address>

    <h2>Event details</h2>
    <p>{{ event.description }}</p>

    <h2>
      Attendees
      <span class="badge -fill-gradient">
            {{ event.attendees ? event.attendees.length : 0 }}
          </span>
    </h2>
    <ul class="list-group">
      <li v-for="(attendee, index) in event.attendees" :key="index" class="list-item">
        <b>{{ attendee.name }}</b>
      </li>
    </ul>
  </div>

</template>

<script>
import { mapState } from 'vuex'
export default {
  head() {
    return {
      title: 'Event ' + this.event.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'What you need to know about event' + this.event.title
        }
      ]
    }
  },
  computed: mapState({
    event: (state) => state.events.event
  }),
  async fetch(context) {
    try {
      await context.store.dispatch(
        'events/fetchEvent',
        context.params.id
      )
    } catch (e) {
      context.error({
        statusCode: 503,
        message: 'Unable to fetch event ' + context.params.id
      })
    }
  }
}
</script>
<style></style>
