<template>
  <div v-if="currentMata_pelajaran" class="edit-form">
    <h4>Tutorial</h4>
    <form>
      <div class="form-group">
        <label for="mapel">Mapel</label>
        <input type="text" class="form-control" id="mapel"
          v-model="currentMata_pelajaran.mapel"
        />
      </div>
      <div class="form-group">
        <label for="nilai">Nilai</label>
        <input type="text" class="form-control" id="nilai"
          v-model="currentMata_pelajaran.nilai"
        />
      </div>

      <div class="form-group">
        <label><strong>Status:</strong></label>
        {{ currentMata_pelajaran.published ? "Published" : "Pending" }}
      </div>
    </form>

    <button class="badge badge-primary mr-2"
      v-if="currentMata_pelajaran.published"
      @click="updatePublished(false)"
    >
      UnPublish
    </button>
    <button v-else class="badge badge-primary mr-2"
      @click="updatePublished(true)"
    >
      Publish
    </button>

    <button class="badge badge-danger mr-2"
      @click="deleteMata_pelajaran"
    >
      Delete
    </button>

    <button type="submit" class="badge badge-success"
      @click="updateMata_pelajaran"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Please click on a Tutorial...</p>
  </div>
</template>

<script>
import Mata_pelajaranDataService from "../services/Mata_pelajaranDataService";

export default {
  name: "mata_pelajaran",
  data() {
    return {
      currentMata_pelajaran: null,
      message: ''
    };
  },
  methods: {
    getMata_pelajaran(id) {
      Mata_pelajaranDataService.get(id)
        .then(response => {
          this.currentMata_pelajaran = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updatePublished(status) {
        var data = {
        id: this.currentMata_pelajaran.id,
        mapel: this.currentMata_pelajaran.mapel,
        nilai: this.currentMata_pelajaran.nilai,
        published: status
      };

      Mata_pelajaranDataService.update(this.currentMata_pelajaran.id, data)
        .then(response => {
          console.log(response.data);
          this.currentMata_pelajaran.published = status;
          this.message = 'The status was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    updateMata_pelajaran() {
      Mata_pelajaranDataService.update(this.currentMata_pelajaran.id, this.currentMata_pelajaran)
        .then(response => {
          console.log(response.data);
          this.message = 'The tutorial was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteMata_pelajaran() {
      Mata_pelajaranDataService.delete(this.currentMata_pelajaran.id)
        .then(response => {
          console.log(response.data);
          this.$router.push ({ name: "mata_pelajarans" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getMata_pelajaran(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>