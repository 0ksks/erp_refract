<template>
  <el-container class="outer-container">
    <el-header height="80px" class="outer-header">
      <BannerHeader
        :componentRouteMapping="[
          ['User', '/user'],
          ['House', '/'],
        ]"
        :headingString="$sentenceCase(entity)"
      />
    </el-header>
    <el-container class="inner-container">
      <el-aside width="300px" class="inner-aside">
        <AsideLayout
          :entity            = "entity"
          :filters           = "filters"
          :tableData         = "tableData"
          :optionColumns     = "['materialID']"
          @update-table-data = "updateTableData"
          @select-entity     = "updateMainForm"
        />
      </el-aside>
      <el-main class="inner-content">
        <MainLayout
          :entity             = "entity"
          :titleFieldsMapping = "titleFieldsMapping"
          :form               = "mainForm"
        />
      </el-main>
    </el-container>
    <el-footer height="80px"></el-footer>
  </el-container>
</template>

<script>
import { ref,toRaw } from "vue"
import AsideLayout from "@/layout/AsideLayout.vue"
import MainLayout from "@/layout/MainLayout.vue"

export default {
  components: {
    AsideLayout,
    MainLayout,
  },
  setup() {
    const entity = "stock"
    const filters = ref({
      materialID           : false,
      plant                : false,
      storageLocation      : false,
      quantity             : false,
      unitOfMeasure        : false,
      stockType            : false,
      valuationType        : false,
      batch                : false,
      specialStockIndicator: false,
      companyCode          : false,
      userID               : true,  // Keep this true as per instruction
      stockID              : true  // Keep this true as per instruction
    })
    const tableData = ref([])

    const titleFieldsMapping = [
      [
        "Location",
        ["plant", "storageLocation"],
        ["desc", "desc"]
      ],
      [
        "Quantity Data",
        ["materialID", "quantity", "unitOfMeasure", "stockType", "valuationType"],
        ["desc", "desc", "desc", "desc", "desc"]
      ],
      [
        "Batch Information",
        ["batch", "specialStockIndicator"],
        ["desc", "desc"]
      ],
      [
        "Company Data",
        ["companyCode"],
        ["desc"]
      ]
    ]

    const mainForm = ref({});
    Object.keys(filters.value).forEach(key => {
      if (key!==entity+"ID" && key!=="userID") {
        mainForm[key] = "";
      }
    });

    const updateTableData = (newData) => {
      tableData.value = newData
      console.log("newData:",toRaw(newData))
      console.log("mainForm:",toRaw(mainForm))
    };

    const updateMainForm = (newForm) => { 
      mainForm.value = newForm
    }

    return {
      entity,
      mainForm,
      filters,
      tableData,
      titleFieldsMapping,
      updateTableData,
      updateMainForm
    }
  },
}
</script>

<style scoped>
.inner-container {
  display         : flex;
  height          : 100vh;
  width           : 100%;
  overflow        : hidden;
  padding-left    : var(--window-margin);
  padding-right   : var(--window-margin);
}
.inner-aside {
  background-color: #f5f4f4;
  padding         : 20px;
  overflow-y      : scroll;
  height          : 100%;
  flex-grow       : 1;
  border-radius   : 10px 0 0 10px;
}
.inner-content {
  background-color: #fff;
  overflow-y      : scroll;
  height          : 100%;
  flex-grow       : 1;
  border-radius   :  0 10px 10px 0;
}
</style>
