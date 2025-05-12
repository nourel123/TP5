<template>
  <div>
    <h1>Éditer un emploi</h1>
    
    <input v-model="search" placeholder="Rechercher un emploi..." @input="filterJobs" />
    
    <ul v-if="filteredJobs.length">
      <li v-for="job in filteredJobs" :key="job.id" @click="selectJob(job)">
        {{ job.titre }}
      </li>
    </ul>

    <form v-if="new_job.id" @submit.prevent="editJob">
      <input v-model="new_job.titre" placeholder="Titre" required />
      <input v-model="new_job.salaire" placeholder="Salaire" required />
      <textarea v-model="new_job.description" placeholder="Description"></textarea>
      <button type="submit">Modifier</button>
    </form>

    <p v-else>Aucun emploi sélectionné.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      jobs: [], // Liste complète des emplois
      new_job: { id: "", titre: "", salaire: "", description: "" }, // Emploi sélectionné
      search: "", // Champ de recherche
      filteredJobs: [] // Liste filtrée
    };
  },
  created() {
    const jobs = JSON.parse(localStorage.getItem("jobs")) || [];
    this.jobs = jobs;
    this.filteredJobs = jobs; // Afficher tous les emplois au départ

    const jobId = Number(this.$route.params.id);
    const jobToEdit = jobs.find(job => job.id === jobId);
    
    if (jobToEdit) {
      this.new_job = { ...jobToEdit };
    }
  },
  methods: {
    editJob() {
      let jobs = JSON.parse(localStorage.getItem("jobs")) || [];
      const index = jobs.findIndex(job => job.id === this.new_job.id);

      if (index !== -1) {
        jobs[index] = this.new_job;
        localStorage.setItem("jobs", JSON.stringify(jobs));
        alert("Emploi modifié !");
        this.$router.push('/');
      } else {
        alert("Erreur lors de la modification.");
      }
    },
    filterJobs() {
      this.filteredJobs = this.jobs.filter(job => 
        job.titre.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    selectJob(job) {
      this.new_job = { ...job };
    }
  }
};
</script>
