<template>
  <div>
    <h1>Ajouter un emploi</h1>
    <form @submit.prevent="addJob">
      <input v-model="new_job.titre" placeholder="Titre" required />
      <input v-model="new_job.salaire" placeholder="Salaire" required />
      <textarea v-model="new_job.description" placeholder="Description"></textarea>
      <button type="submit">Ajouter</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      new_job: { titre: "", salaire: "", description: "" }
    };
  },
  methods: {
    addJob() {
      const jobToAdd = { ...this.new_job, id: Date.now() };

      // Récupérer les anciens emplois
      let jobs = JSON.parse(localStorage.getItem("jobs")) || [];
      jobs.push(jobToAdd);

      // Sauvegarder dans localStorage
      localStorage.setItem("jobs", JSON.stringify(jobs));

      // Émettre l'événement vers HomeView
      this.$emit("job-added", jobToAdd);

      alert("Emploi ajouté !");
      
      // Réinitialiser le formulaire
      this.new_job = { titre: "", salaire: "", description: "" };

      // Rediriger vers la page d'accueil
      this.$router.push("/");
    }
  }
};
</script>
