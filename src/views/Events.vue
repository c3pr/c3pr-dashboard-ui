<template>
  <div class="about">

    <h1>Committed Changes per Project</h1>

    <table>
      <thead>
      <tr>
        <th>Project Name</th>
        <th>Committed Changes</th>
        <th>Last modification</th>
        <th>-</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="ccpp of changesCommittedPerProject">
        <td>{{ (projects.find(p => p.uuid === ccpp._id.project_uuid) || {}).name }}</td>
        <td>{{ ccpp.count }}</td>
        <td :class="{highlight: isToday(ccpp.last_modified)}">{{ formatarData(ccpp.last_modified) }}</td>
        <td>
          [<router-link :to="{ name: 'events-per-project', params: { project_uuid: ccpp._id.project_uuid }}">commit events</router-link>]
        </td>
      </tr>
      </tbody>
    </table>

    <br>

    <h1>Processing Events ({{ eventsProcessing.length }})</h1>
    <event-list :events="eventsProcessing"></event-list>

    <br>

    <h1>Unprocessed Events ({{ eventsUnprocessed.length }})</h1>
    <event-list :events="eventsUnprocessed"></event-list>

  </div>
</template>

<script>
  import {mapActions, mapGetters} from 'vuex';
  import {
    EVENTS,
    FETCH_CHANGES_COMMITTED_PER_PROJECT,
    FETCH_EVENTS_UNPROCESSED_AND_PROCESSING,
    GET_CHANGES_COMMITTED_PER_PROJECT,
    GET_EVENTS_PROCESSING,
    GET_EVENTS_UNPROCESSED,
  } from "../store/modules/events";
  import {PROJECTS, FETCH_ALL_PROJECTS, GET_ALL_PROJECTS} from "../store/modules/projects";
  import EventDetail from '../components/EventDetail.vue';
  import EventList from '../components/EventList.vue';
  import {formatarData, isToday} from "../app/data";

  export default {
    name: "Events",

    components: {EventDetail, EventList},

    data() {
      return {
      };
    },

    computed: {
      ...mapGetters(EVENTS, {
        changesCommittedPerProject: GET_CHANGES_COMMITTED_PER_PROJECT,
        eventsUnprocessed: GET_EVENTS_UNPROCESSED,
        eventsProcessing: GET_EVENTS_PROCESSING,
      }),
      ...mapGetters(PROJECTS, {projects: GET_ALL_PROJECTS})
    },

    created() {
      this.fetchChangesCommittedPerProject();
      this.fetchProjects();
      this.fetchEventsUnprocessedAndProcessing();
    },

    methods: {
      ...mapActions(EVENTS, {
        fetchChangesCommittedPerProject: FETCH_CHANGES_COMMITTED_PER_PROJECT,
        fetchEventsUnprocessedAndProcessing: FETCH_EVENTS_UNPROCESSED_AND_PROCESSING
      }),
      ...mapActions(PROJECTS, {fetchProjects: FETCH_ALL_PROJECTS}),
      isToday,
      formatarData
    }
  };
</script>

<style scoped>
</style>
