<template>
  <div>
    <div class="inputContainer">
      <div class="origin">
        <input
          @change="originChangeHandler"
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
              v-for="city in listOfOrigins ? listOfOrigins : cities"
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
          :disabled="origin === ''"
          @click="showDestinationsListHandler"
          v-model="destination"
          class="destinationInput"
          :class="{ disabledInput: origin === '' }"
          type="text"
          placeholder="مقصد"
        />

        <!-- list of suggested destination cities -->
        <div v-if="showDestinationsList" class="suggestedOriginCities">
          <div>
            <div
              v-for="city in listOfDestinations
                ? listOfDestinations
                : destinations"
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
      listOfDestinations: undefined,
      listOfOrigins: undefined,
    };
  },

  computed: {
    cities() {
      return cities;
    },
    destinations() {
      const values = cities
        .map((item) => Object.values(item.destinations))
        .flat();
      const ids = values.map((item) => item.id);

      const destinations = Array.from(new Set(ids)).map((id) =>
        values.find((item) => item.id === id)
      );

      return destinations;
    },
  },

  methods: {
    originChangeHandler(e) {
      this.origin = e.target.value;
      this.listOfOrigins = cities.filter((item) => {
        return (
          item.pe_name.includes(e.target.value) ||
          item.en_name.toLowerCase().includes(e.target.value.toLowerCase())
        );
      });
    },

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
      this.destination = "";
      const selectedCity = cities.filter((item) => item.pe_name === city);
      const foundDestinations = Object.values(selectedCity[0].destinations);
      this.listOfDestinations = foundDestinations;
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

.disabledInput {
  cursor: not-allowed;
}
</style>
