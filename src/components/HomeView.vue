<template>
  <div>
    <AddJob @job-added="addJobToList" />
    <FilterNav @search-updated="filterJobs" />

    <ul>
      <li v-for="(job, index) in filteredJobs" :key="job.id">
        <p>
          {{ job.titre }}
          <button @click="selectJob(job)">Voir détails</button>
          <button @click="supprimer(index)">Supprimer</button>
          <button @click="startEdit(job)">Éditer</button>
        </p>
      </li>
    </ul>

    <JobDetail v-if="selectedJob" :job="selectedJob" @close="selectedJob = null" />
    <EditJob v-if="edit" :job="selectedJobE" @job-edited="editJobInList"></EditJob>
  </div>
</template>

<script>
import EditJob from "./EditJob.vue";
import AddJob from "./AddJob.vue";
import JobDetail from "@/components/JobDetail.vue";
import FilterNav from "@/components/FilterNav.vue";

export default {
  components: {
    JobDetail,
    FilterNav,
    AddJob,
    EditJob,
  },
  data() {
    return {
      edit: false,
      selectedJob: null,
      selectedJobE: null,
      jobs: [],
      searchQuery: "",
    };
  },
  computed: {
    filteredJobs() {
      return this.jobs.filter((job) =>
        job.titre.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
  methods: {
    selectJob(job) {
      this.selectedJob = job;
    },
    filterJobs(query) {
      this.searchQuery = query;
    },
    addJobToList(added) {
      this.jobs.push(added);
      localStorage.setItem("jobs", JSON.stringify(this.jobs)); // Sauvegarde immédiate
    },
    supprimer(index) {
      const confirmation = window.confirm("Êtes-vous sûr de vouloir supprimer ce job ?");
      if (confirmation) {
        this.jobs.splice(index, 1);
        localStorage.setItem("jobs", JSON.stringify(this.jobs)); // Mise à jour locale
      }
    },
    editJobInList(updatedJob) {
      const index = this.jobs.findIndex(job => job.id === updatedJob.id);
      if (index !== -1) {
        this.jobs[index] = updatedJob;
        localStorage.setItem("jobs", JSON.stringify(this.jobs)); // Sauvegarde après édition
      }
      this.edit = false;
    },
    startEdit(job) {
      this.selectedJobE = job;
      this.edit = true;
    }
  },
  mounted() {
    this.jobs = JSON.parse(localStorage.getItem("jobs")) || [];
  }
};
</script>
