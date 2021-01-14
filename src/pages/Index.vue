<template>
  <q-page class="flex flex-center q-ma-md">
    <FullCalendar :options="calendarOptions" />
  </q-page>
</template>

<script>
import FullCalendar from '@fullcalendar/vue'
import TimeGridPlin from '@fullcalendar/timegrid'
import dayGridPlugin from '@fullcalendar/daygrid'
import interactionPlugin from '@fullcalendar/interaction'

export default {
  name: 'PageIndex',
  components: { FullCalendar },
  data () {
    return {
      calendarOptions: {
        plugins: [dayGridPlugin, TimeGridPlin, interactionPlugin],
        initialView: 'dayGridMonth',
        height: 'auto',
        locale: 'pt-br',
        headerToolbar: {
          start: 'title',
          center: 'dayGridMonth,timeGridWeek,timeGridDay',
          end: 'today,prevYear,prev,next,nextYear'
        },
        buttonText: {
          today: 'hoje',
          month: 'mês',
          week: 'semana',
          day: 'dia',
          list: 'list'
        },
        events: 'https://fullcalendar.io/demo-events.json',
        currentEvents: [],
        editable: true,
        selectable: true,
        selectMirror: true,
        dayMaxEvents: true,
        weekends: true,
        select: this.handleDateSelect,
        eventClick: this.handleEventClick,
        eventsSet: this.handleEvents
      }
    }
  },
  methods: {
    handleWeekendsToggle () {
      this.calendarOptions.weekends = !this.calendarOptions.weekends // update a property
    },
    handleDateSelect (selectInfo) {
      const title = prompt('Please enter a new title for your event')
      const calendarApi = selectInfo.view.calendar
      calendarApi.unselect() // clear date selection
      if (title) {
        calendarApi.addEvent({
          id: 1,
          title,
          start: selectInfo.startStr,
          end: selectInfo.endStr,
          allDay: selectInfo.allDay
        })
      }
    },
    handleEventClick (clickInfo) {
      if (
        confirm(
          `Are you sure you want to delete the event '${clickInfo.event.title}'`
        )
      ) {
        clickInfo.event.remove()
      }
    },
    handleEvents (events) {
      this.currentEvents = events
    }
  }
}
</script>
<style lang="css" scoped>
.fc-button-primary {
  background-color: black !important;
}
</style>
