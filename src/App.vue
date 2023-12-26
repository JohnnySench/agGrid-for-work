<script>
import {onMounted, ref} from 'vue';
import "ag-grid-community/styles/ag-grid.css"; // Core CSS
import "ag-grid-community/styles/ag-theme-quartz.css"; // Theme
import { AgGridVue } from "ag-grid-vue3"; // Vue Grid Logic
import AutocompleteFIlter from "./components/AutocompleteFIlter.vue";

export default {
  name: "App",
  components: {
    AgGridVue, // Add AG Grid Vue3 component
    AutocompleteFIlter
  },
  setup(props) {
    const data = ref(null)
    const gridApi = ref();
    const rowData = ref([]);
    const colDefs = ref([
      { field: "mission" },
      { field: "company" },
      { field: "location" },
      { field: "date" },
      { field: "price" },
      { field: "successful" },
      { field: "rocket" }
    ]);
    const defaultColDef = ref({
      filter: true,
      floatingFilter: true,
      editable: true,
    });
    const onGridReady = (param) => {
      gridApi.value = param.api
    }
    const fetchData = async () => {
      const response = await fetch('https://www.ag-grid.com/example-assets/space-mission-data.json');
      return response.json();
    };
    onMounted(async () => {
      rowData.value = await fetchData();
    });

    const externalFilterChanged = (filter) => {
      data.value = filter;
      gridApi.value.onFilterChanged();
    }

    const doesExternalFilterPass = (node) => {
      if (node.data) {
        return node.data.mission.includes(data.value)
      }
      return true;
    };
    const isExternalFilterPresent = () => {
      return data.value !== null;
    };


    return {rowData, colDefs, defaultColDef, externalFilterChanged, onGridReady, doesExternalFilterPass, isExternalFilterPresent}
  },
};
</script>

<template>
  <div>
    <ag-grid-vue
        style="height: 500px"
        class="ag-theme-quartz"
        :rowData="rowData"
        :columnDefs="colDefs"
        :defaultColDef="defaultColDef"
        @grid-ready="onGridReady"
        :isExternalFilterPresent="isExternalFilterPresent"
        :doesExternalFilterPass="doesExternalFilterPass"
    >
    </ag-grid-vue>
    <AutocompleteFIlter style="width: 300px" @saveFilter="externalFilterChanged"/>
  </div>
</template>

<style lang="scss" scoped>

</style>
