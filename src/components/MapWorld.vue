<template>
  <div id="svgMap" ref="map" @click="openModal">
  </div>
  <ModalOffers :isOpen="isModalOpen" :openModal="openModal" :flag_img="flag_img" :offers="offers" @close="closeModal" />
</template>

<script>
import svgMap from "svgmap";
import 'svgmap/dist/svgMap.min.css';
import { onMounted, ref } from "vue";
import ModalOffers from "./ModalOffers.vue";

export default {
  name: "MapWorld",
  components: {
    ModalOffers,
  },
  setup() {
    const mapElement = ref(null);
    const isModalOpen = ref(false);
    const offers = ref([]);
    const flag_img = ref('');

    const openModal = async (ev) => {
      if (!ev.target.dataset.id) {
        return;
      }
      await fetchOffers(ev);
      flag_img.value = ev.target.ownerDocument.images[0].currentSrc;
      isModalOpen.value = true;
    };

    const fetchOffers = async (ev) => {
      try {
        const response = await fetch(`https://mikasgu.pythonanywhere.com/offers/${ev.target.dataset.id}`);
        console.log(ev.target.dataset.id);
        offers.value = await response.json();
      } catch (error) {
        console.error("Error fetching offers:", error);
      }
    };

    const closeModal = () => {
      isModalOpen.value = false;
    };
    
    onMounted(async () => {
      const response = await fetch("https://mikasgu.pythonanywhere.com/total_offers/all");
      const responseData = await response.json();

      const formattedValues = Object.entries(responseData.results).reduce((acc, [countryCode, value]) => {
        acc[countryCode] = { countresidents: value };
        return acc;
      }, {});
      const mapInstance = new svgMap({
        targetElementID: "svgMap",
        mouseWheelZoomEnabled: true,
        hideFlag: false,
        noDataText: "",
        colorNoData: "#ffffff0",
        data: {
          data: {
            countresidents: {
              format: "{0}",
              thousandSeparator: ",",
              thresholdMax: 30,
              thresholdMin: 0,
            },
          },  
          applyData: "countresidents",
          values: {
            ...formattedValues,
        },
        },
      });

      mapElement.value = mapInstance.targetElement;
    });

    return { mapElement, isModalOpen, closeModal, openModal, offers, flag_img };
  },
};
</script>

<style>

.svgMap-map-wrapper {
  position: relative;
  width: 100%;
  overflow: hidden;
  background: #0000006f;
  color: #ffffff33;
}

@media (hover:hover) {
  .svgMap-map-wrapper .svgMap-country:hover {
    stroke: #c64343;
    stroke-width: 1.5;
  }
}

.svgMap-tooltip {
  box-shadow: 0 0 3px rgba(0, 0, 0, 0.2);
  position: absolute;
  z-index: 2;
  border-radius: 2px;
  background: #ffffff46;
  transform: translate(-50%, -100%);
  border-bottom: 1px solid #000;
  display: none;
  pointer-events: none;
  min-width: 60px;
}

.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.374);
  background-color: rgba(0, 0, 0, 0.044);
}

.modal-content {
  background-color: #fefefe4b;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

</style>
