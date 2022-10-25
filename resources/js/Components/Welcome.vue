<script setup>
import ApplicationLogo from "@/Components/ApplicationLogo.vue";
import AgGridVue from "@/Components/AgGridComponent.vue";
</script>

<template>
  <div class="p-6 sm:px-20 bg-white border-b border-gray-200">
    <div>
      <ApplicationLogo class="block h-12 w-auto" />
    </div>

    <div class="mt-8 text-2xl">Ag grid</div>

    <div class="mt-6 text-gray-500">
      <AgGridVue
        :parentColumns="columnDefs"
        :parentData="rowData"
        @selectedRow="logResult"
      >
        <template #title>
          <span>Number of rows: </span>
        </template>
      </AgGridVue>
    </div>
  </div>
</template>
<script>
import { defineComponent } from "vue";
export default defineComponent({
  data() {
    return {
      columnDefs: [
        {
          headerName: "",
          checkboxSelection: true,
          // headerCheckboxSelection: true,
          width: 50,
        },
        {
          field: "minID",
          headerName: "ID",
          sortable: true,
          filter: true,
        },
        {
          field: "Felt1",
          headerName: "Felt En",
          sortable: true,
          filter: true,
        },
        {
          field: "Felt2",
          headerName: "Felt To",
          filter: false,
          sortable: true,
        },
      ],
      rowData: [
        { minID: "01", Felt1: "test", Felt2: "test2" },
        { minID: "02", Felt1: "testing", Felt2: "feltet høyre" },
        { minID: "03", Felt1: "neida", Felt2: "yeyeyeye" },
      ],
    };
  },
  methods: {
    logResult(e) {
      console.log(e);
    },
  },
});
</script>