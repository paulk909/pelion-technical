<template>
  <div class="home">
    <q-layout view="lHh Lpr lFf">
      <q-header elevated>
        <q-toolbar>
          <q-toolbar-title> Activity Suggestion App </q-toolbar-title>
          <q-btn
            flat
            dense
            round
            label="Filters"
            aria-label="Menu"
            @click="rightDrawerOpen = !rightDrawerOpen"
          />
        </q-toolbar>
      </q-header>
      <q-drawer
        side="right"
        v-model="rightDrawerOpen"
        bordered
        content-class="bg-grey-1"
      >
        <q-scroll-area class="fit">
          <q-form @submit="updateFilters" class="q-gutter-md" ref="filterForm">
            <q-expansion-item
              group="filterGroup"
              expand-separator
              v-model="typeFilter"
              label="Filter by Activity Type"
            >
              <q-card>
                <q-card-section>
                  <q-select
                    outlined
                    v-model="typeModel"
                    :options="types"
                    label="Activity Type"
                  />
                </q-card-section>
              </q-card>
            </q-expansion-item>

            <q-expansion-item
              group="filterGroup"
              expand-separator
              v-model="participantFilter"
              label="Filter by Number of Participants"
            >
              <q-card>
                <q-card-section>
                  <q-select
                    outlined
                    v-model="participantModel"
                    :options="participants"
                    label="Number of Participants"
                  />
                </q-card-section>
              </q-card>
            </q-expansion-item>

            <q-expansion-item
              group="filterGroup"
              expand-separator
              v-model="accessibiltyFilter"
              label="Filter by Accessibilty"
            >
              <q-card>
                <q-card-section>
                  <q-slider
                    v-model="accessibiltyModel"
                    :min="0"
                    :max="1"
                    :step="0.01"
                    label
                    :marker-labels="fnMarkerLabel"
                    :markers="0.1"
                  />
                </q-card-section>
              </q-card>
            </q-expansion-item>

            <q-expansion-item
              group="filterGroup"
              expand-separator
              v-model="accessibiltyRangeFilter"
              label="Filter by Accessibility Range"
            >
              <q-card>
                <q-card-section>
                  <q-range
                    v-model="accessibiltyRangeModel"
                    :min="0"
                    :max="1"
                    :step="0.01"
                    color="green"
                    label
                    :marker-labels="fnMarkerLabel"
                    :markers="0.25"
                  />
                </q-card-section>
              </q-card>
            </q-expansion-item>

            <q-expansion-item
              group="filterGroup"
              expand-separator
              v-model="priceFilter"
              label="Filter by Price"
            >
              <q-card>
                <q-card-section>
                  <q-slider
                    v-model="priceModel"
                    :min="0"
                    :max="1"
                    :step="0.01"
                    label
                    :marker-labels="fnMarkerLabel"
                    :markers="0.25"
                  />
                </q-card-section>
              </q-card>
            </q-expansion-item>

            <q-expansion-item
              group="filterGroup"
              expand-separator
              v-model="priceRangeFilter"
              label="Filter by Price Range"
            >
              <q-card>
                <q-card-section>
                  <q-range
                    v-model="priceRangeModel"
                    :min="0"
                    :max="1"
                    :step="0.01"
                    color="green"
                    label
                    :marker-labels="fnMarkerLabel"
                    :markers="0.25"
                  />
                </q-card-section>
              </q-card>
            </q-expansion-item>

            <!--<q-btn label="Submit" type="submit" color="primary" />
            <div>
                <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
              </div>-->
          </q-form>
        </q-scroll-area>
      </q-drawer>

      <q-page-container>
        <div class="q-pa-md q-gutter-sm">
          <q-btn
            color="purple"
            @click="getSuggestion()"
            label="Get Suggestion"
          />
          <q-btn
            color="red"
            @click="clearSuggestions()"
            label="Clear Suggestions"
          />
          <!--<div v-if="suggestion">-->
          <q-table
            style="margin-top: 15px"
            title="Suggestions"
            :rows="rows"
            :columns="columns"
            :pagination="pagination"
            row-key="name"
          />
        </div>
      </q-page-container>
      <!--</div>-->
    </q-layout>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "HomeView",
  setup() {
    const filterForm = ref(null);
    return {
      filterForm,
      typeFilter: ref(false),
      participantFilter: ref(false),
      accessibiltyFilter: ref(false),
      accessibiltyRangeFilter: ref(false),
      priceFilter: ref(false),
      priceRangeFilter: ref(false),
      fnMarkerLabel: (val) => parseFloat(val).toFixed(2),
      accessibiltyModel: ref(0.4),
      priceModel: ref(0.5),
      accessibiltyRangeModel: ref({
        min: 0.2,
        max: 0.7,
      }),
      priceRangeModel: ref({
        min: 0.2,
        max: 0.7,
      }),
      typeModel: ref("busywork"),
      participantModel: ref(1),
      rightDrawerOpen: ref(false),
      types: [
        "busywork",
        "diy",
        "education",
        "social",
        "recreational",
        "relaxation",
        "music",
        "cooking",
        "charity",
      ],
      participants: [1, 2, 3, 4, 5, 6, 7, 8],
      pagination: {
        rowsPerPage: 0,
        sortBy: "id",
        descending: true,
      },
      columns: [
        {
          name: "id",
          required: true,
          label: "#",
          align: "left",
          field: "id",
          sortable: true,
        },
        {
          name: "activity",
          label: "Activity",
          align: "left",
          field: "activity",
          sortable: true,
        },
        {
          name: "accessibility",
          align: "center",
          label: "Accessibility",
          field: "accessibility",
          sortable: true,
        },
        { name: "type", label: "Type", field: "type", sortable: true },
        {
          name: "participants",
          label: "Participants",
          field: "participants",
          sortable: true,
        },
        { name: "price", label: "Price", field: "price", sortable: true },
        { name: "key", label: "Key", field: "key", sortable: true },
      ],
    };
  },
  data() {
    return {
      rows: [],
    };
  },
  methods: {
    clearSuggestions() {
      this.rows = [];
      localStorage.clear();
    },
    getSuggestion() {
      const filter = this.updateFilters();
      console.log({ filter });
      let apiUrl = "http://www.boredapi.com/api/activity";
      if (Object.keys(filter).length) {
        let apiSuffix = "";
        switch (filter.name) {
          case "typeFilter":
            apiSuffix = `?type=${filter.value}`;
            break;
          case "participantFilter":
            apiSuffix = `?participants=${filter.value}`;
            break;
          case "accessibiltyFilter":
            apiSuffix = `?accessibility=${filter.value}`;
            break;
          case "accessibiltyRangeFilter":
            apiSuffix = `?minaccessibility=${filter.value.min}&maxaccessibility=${filter.value.max}`;
            break;
          case "priceRangeFilter":
            apiSuffix = `?minprice=${filter.value.min}&maxprice=${filter.value.max}`;
            break;
          case "priceFilter":
            apiSuffix = `?price=${filter.value}`;
            break;
          default:
            apiSuffix = "";
        }
        apiUrl = apiUrl + apiSuffix;
      }
      fetch(apiUrl)
        .then((res) => res.json())
        .then((data) => {
          let id = this.rows.length + 1;
          data.id = id;
          data.activity ? this.rows.push(data) : console.log("No data");
          localStorage.clear();
          localStorage.setItem("rows", JSON.stringify(this.rows));
        })
        .catch((err) => console.log(err));
    },
    updateFilters() {
      let activeFilter = {};
      const filterList = [
        {
          name: "typeFilter",
          active: this.typeFilter,
          value: this.typeModel,
        },
        {
          name: "participantFilter",
          active: this.participantFilter,
          value: this.participantModel,
        },
        {
          name: "accessibiltyFilter",
          active: this.accessibiltyFilter,
          value: this.accessibiltyModel,
        },
        {
          name: "accessibiltyRangeFilter",
          active: this.accessibiltyRangeFilter,
          value: this.accessibiltyRangeModel,
        },
        {
          name: "priceRangeFilter",
          active: this.priceRangeFilter,
          value: this.priceRangeModel,
        },
        {
          name: "priceFilter",
          active: this.priceFilter,
          value: this.priceModel,
        },
      ];
      filterList.forEach((filter) => {
        if (filter.active) activeFilter = filter;
      });
      console.log({ activeFilter });
      return activeFilter;
    },
  },
  mounted() {
    // this.getSuggestion();
    let storedRows = localStorage.getItem("rows");
    if (storedRows !== null) {
      this.rows = JSON.parse(storedRows);
    } else {
      this.getSuggestion();
    }
    console.log(storedRows);
  },
};
</script>
