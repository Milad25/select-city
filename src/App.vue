<template>
  <div>
    <div class="inputContainer">
      <div class="origin">
        <input
          @click="showOriginsListHandler"
          v-model="origin"
          class="originInput"
          type="text"
          placeholder="مبدا"
        />

        <!-- list of suggested origin cities -->
        <div v-if="showOriginsList" class="suggestedOriginCities">
          <div>
            <div
              v-for="city in cities"
              :key="city.en_name"
              @click="() => updateOriginHandler(city.pe_name)"
              class="suggestedOriginCityItem"
            >
              <p>{{ city.pe_name }}</p>
              <img src="./assets/icons/buildingIcon.svg" alt="city-icon" />
            </div>
          </div>
        </div>
      </div>

      <div class="destination">
        <input
          @click="showDestinationsListHandler"
          v-model="destination"
          class="destinationInput"
          type="text"
          placeholder="مقصد"
        />

        <!-- list of suggested destination cities -->
        <div v-if="showDestinationsList" class="suggestedOriginCities">
          <div>
            <div
              v-for="city in cities"
              :key="city.en_name"
              @click="() => updateDestinationHandler(city.pe_name)"
              class="suggestedOriginCityItem"
            >
              <p>{{ city.pe_name }}</p>
              <img src="./assets/icons/buildingIcon.svg" alt="city-icon" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import cities from "./data/tour_routes.json";

export default {
  name: "App",
  components: {},
  data() {
    return {
      origin: "",
      destination: "",
      showOriginsList: false,
      showDestinationsList: false,
    };
  },

  computed: {
    cities() {
      return cities;
    },
  },

  methods: {
    showOriginsListHandler() {
      this.showOriginsList = !this.showOriginsList;
      this.showDestinationsList = false;
    },

    showDestinationsListHandler() {
      this.showDestinationsList = !this.showDestinationsList;
      this.showOriginsList = false;
    },

    updateOriginHandler(city) {
      this.origin = city !== this.destination ? city : "";
      if (city !== this.destination) {
        this.showOriginsList = false;
        this.showDestinationsList = false;
      }
    },

    updateDestinationHandler(city) {
      this.destination = city !== this.origin ? city : "";
      if (city !== this.origin) {
        this.showDestinationsList = false;
        this.showOriginsList = false;
      }
    },
  },
};
</script>

<style>
input {
  border: 1px solid #ccc;
  padding: 0.5rem 1.5rem;
  border-radius: 8px;
}

.inputContainer {
  display: inline-flex;
}

.origin {
}

.destination {
}

.suggestedOriginCities {
  height: 300px;
  overflow: auto;
  border: 1px solid #f6f6f6;
  box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  border-bottom-left-radius: 12px;
  border-bottom-right-radius: 12px;
}

.suggestedOriginCityItem {
  display: flex;
  flex-direction: row-reverse;
  justify-content: flex-end;
  align-items: center;
  padding: 1.5rem 0.5rem;
  transition: background-color 0.2s ease-out;
  cursor: pointer;
}

.suggestedOriginCityItem:hover {
  background-color: #f3f1f1;
}

.suggestedOriginCityItem > p {
  margin-right: 1rem;
}

.originInput {
}

.destinationInput {
  margin-right: 1rem;
}
</style>
