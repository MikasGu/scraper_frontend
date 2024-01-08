<template>
  <div v-if="isOpen" class="modal-overlay" @click="closeModal">
    <div class="modal-container" @click.stop>
      <div class="modal-header">
        <h2>OFFERS FOUND: {{ offers.results.length }}</h2>
        
        <span class="close-button" @click="closeModal">&times;</span>
      </div>
      <div class="modal-body">
        <table class="table">
          <thead>
            <tr>
              <th @click="sortTable('agency')">Agency</th>
              <th @click="sortTable('country')">Country</th>
              <th @click="sortTable('price')">Price</th>
              <th @click="sortTable('description')">Description</th>
              <th>Link</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="offer in offers.results" :key="offer.id">
              <td>
                <img :src="getLogoSrc(offer.agency)" alt="logo" class="agency-logo" />
              </td>
              <td><img :src="flag_img" alt="flag" class="flag-img" /></td>
              <td class="price-cell">{{ offer.price }}</td>
              <td>{{ offer.description }}</td>
              <td>
                <a :href="offer.url" target="_blank" class="external-icon">
                  <i class="fa fa-external-link"></i>
                </a>
              </td>
            </tr>
            <tr v-if="offers.results && offers.results.length === 0">
              <td colspan="5">No offers available</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ModalOffers',
  props: {
    isOpen: Boolean,
    offers: Object,
    flag_img: String,
  },
  methods: {
    closeModal() {
      this.$emit('close');
    },
    sortTable(field) {
      this.$emit('sort', field);
    },
    getLogoSrc(agency) {
      const lowerCaseAgency = agency.toLowerCase();
      if (lowerCaseAgency.includes('makalius')) {
        return 'makalius.png';
      } else if (lowerCaseAgency.includes('teztour')) {
        return 'teztour.png';
      } else if (lowerCaseAgency.includes('airguru')) {
        return 'airguru.png';
      } else {
        return 'makalius.png';
      }
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.763);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-body {
  margin-top: 20px;
  max-height: 400px;
  overflow-y: auto;
  scrollbar-color: #164a3f;
}

.modal-body::-webkit-scrollbar {
  width: 12px;
}

.modal-body::-webkit-scrollbar-track {
  background-color: #ffffff00;
}

.modal-body::-webkit-scrollbar-thumb {
  background-color: #164a3f;
  border-radius: 10px;
  border: 3px solid #ffffff76;
}
.external-icon {
  color: #164a3f;
}
.modal-container {
  background: #00000076;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  max-width: 900px;
  width: 100%;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.close-button {
  cursor: pointer;
  font-size: 20px;
  color: #333;
}

.modal-body {
  margin-top: 20px;
}
.table {
  width: 100%;
  border-collapse: collapse;
}

.table th, .table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.table th {
  background-color: #a687877f;
  text-transform: uppercase;
}

.table tbody tr:hover {
  background-color: #223521;
}

.external-icon {
  color: #00a624;
  text-decoration: none;
}

.table th.sortable {
  cursor: pointer;
  position: relative;
}

.table th.sortable::after {
  content: ' ▾';
  font-size: 12px;
  position: absolute;
  right: 5px;
  opacity: 0.6;
}

.table th.sorted-asc::after {
  content: ' ▲';
}

.table th.sorted-desc::after {
  content: ' ▼';
}

.price-cell {
  white-space: nowrap;
}

.table th, .table td {
  vertical-align: middle;
}

.flag-img {
  width: 30px;
  height: 20px;
  border-radius: 4px;
  margin-left: 10px;
}

.agency-logo {
  width: 80px; 
  height: 40px;
  border-radius: 4px;
}

@media (max-width: 768px) {
  .table th, .table td {
    font-size: 14px;
  }
}
</style>