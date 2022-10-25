<script setup>
import ApplicationLogo from "@/Components/ApplicationLogo.vue";

import "ag-grid-community/dist/styles/ag-grid.css";
import "ag-grid-community/dist/styles/ag-theme-alpine.css";
import { AgGridVue } from "ag-grid-vue3";
</script>

<template>
  <div class="text-black">
    <div class="flex mb-2">
      <div>
        <slot  name="title" ></slot>
        <select v-on:change="onPageSizeChanged()" id="page-size">
          <option value="5">5</option>
          <option value="10" selected="">10</option>
          <option value="50">50</option>
          <option value="100">100</option>
        </select>
      </div>
      <div>
        <input
          class="ml-2"
          type="text"
          v-on:input="onQuickFilterChanged()"
          id="quickFilter"
          placeholder="Search"
        />
      </div>
    </div>
    <ag-grid-vue
      style="width: 1050px; height: 650px"
      class="ag-theme-alpine"
      :columnDefs="columnDefs"
      :pagination="true"
      :paginationPageSize="paginationPageSize"
      :paginationNumberFormatter="paginationNumberFormatter"
      :rowData="rowData"
      @grid-ready="onGridReady"
    >
    </ag-grid-vue>
    <button
      class="
        mt-2
        inline-block
        px-6
        py-2.5
        bg-gray-800
        text-white
        font-medium
        text-xs
        leading-tight
        uppercase
        rounded
        shadow-md
        hover:bg-gray-900 hover:shadow-lg
        focus:bg-gray-900 focus:shadow-lg focus:outline-none focus:ring-0
        active:bg-gray-900 active:shadow-lg
        transition
        duration-150
        ease-in-out
      "
      v-on:click="SelectedNode()"
    >
      Open selected row
    </button>
  </div>
</template>
<script>
import { defineComponent } from "vue";

export default defineComponent({
  emits: ["selectedRow"],
  props: ["parentColumns", "parentData"],
  setup() {},
  mounted() {
    if (this.parentColumns) {
      this.columnDefs = this.parentColumns;
    }
    if (this.parentData) {
      this.rowData = this.parentData;
    }
  },
  data() {
    return {
      columnDefs: [],
      gridApi: null,
      columnApi: null,
      paginationPageSize: 25,
      paginationNumberFormatter: null,
      defaultColDef: {
        resizable: true,
      },
      rowData: [],
      selectedArr: [],
    };
  },
  methods: {
    // Størrelsen på kolonnene endres automatisk etter størrelse på skjerm
    onFirstDataRendered(params) {
      // params.api.sizeColumnsToFit();
      // params.api.paginationGoToPage(0);
    },
    onGridReady(params) {
      this.gridApi = params.api;
      this.columnApi = params.columnApi;
    },
    //Endre hvor mange linjer per side i dropdown menyen
    onPageSizeChanged() {
      var value = document.getElementById("page-size").value;
      this.gridApi.paginationSetPageSize(Number(value));
    },
    // Søkefunksjon
    onQuickFilterChanged() {
      this.gridApi.setQuickFilter(document.getElementById("quickFilter").value);
    },
    SelectedNode() {
      var selectedNode = this.gridApi.getSelectedNodes()[0]; // single selection
      if (!selectedNode) {
        console.log("No nodes selected!");
        return;
      } else {
        const selectedNodes = this.gridApi.getSelectedNodes();
        const selectedData = selectedNodes.map((node) => node.data);
        // console.log(selectedData[0]);
        // document.getElementById("targetModal").classList.remove("hidden");
        this.selectedArr = selectedData[0];
        this.$emit("selectedRow", selectedData[0]);
      }
    },
  },
});
</script>
