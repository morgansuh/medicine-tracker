<template>
  <div v-if="dataLoaded" class="container mt-10 px-4">
    <!-- Empty -->
    <div v-if="data.length === 0" class="w-full flex flex-col items-center">
      <h1 class="text-2xl">No Medication Added Yet</h1>
      <router-link
        class="
          mt-6
          py-2
          px-6
          rounded-sm
          text-sm text-white
          bg-at-light-pink
          duration-200
          border-solid border-2 border-transparent
          hover:border-at-light-green hover:bg-white hover:text-at-light-green
        "
        :to="{ name: 'Create' }"
        >Add New Medication</router-link
      >
    </div>

    <!-- Data Display (Non Empty) -->
    <div
      v-else
      class="
        grid grid-cols-1
        sm:grid-cols-2
        md:grid-cols-3
        lg:grid-cols-4
        gap-6
      "
    >
      <router-link
        class="
          flex flex-col
          items-center
          bg-light-grey
          p-8
          shadow-md
          cursor-pointer
        "
        :to="{ name: 'ViewMedication', params: { medicineId: medicine.id } }"
        v-for="(medicine, index) in data"
        :key="index"
      >
        <!-- Liquid image -->
        <img
          v-if="medicine.medicineType === 'liquid'"
          src="@/assets/images/liquid.png"
          class="h-24 w-auto"
          alt=""
        />

        <!-- Capsule image -->
        <img
          v-if="medicine.medicineType === 'capsule'"
          src="@/assets/images/capsule.png"
          class="h-24 w-auto"
          alt=""
        />

        <!-- Inject image -->
        <img
          v-if="medicine.medicineType === 'inject'"
          src="@/assets/images/inject.png"
          class="h-24 w-auto"
          alt=""
        />

        <!-- Topical image -->
        <img
          v-if="medicine.medicineType === 'topical'"
          src="@/assets/images/topical.png"
          class="h-24 w-auto"
          alt=""
        />

        <!-- Other image -->
        <img
          v-if="medicine.medicineType === 'other'"
          src="@/assets/images/other.png"
          class="h-24 w-auto"
          alt=""
        />
        <p
          class="
            mt-6
            py-1
            px-3
            text-xs text-white
            bg-at-light-pink
            shadow-md
            rounded-lg
          "
        >
          {{ medicine.medicineType }}
        </p>

        <h1 class="mt-8 mb-2 text-center text-xl text-at-light-green">
          {{ medicine.medicineName }}
        </h1>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";

export default {
  name: "Home",
  components: {},
  setup() {
    // creating data / vars
    const data = ref([]);
    const dataLoaded = ref(null); //display markup once we have data

    // getting data
    const getData = async () => {
      try {
        const { data: medicine, error } = await supabase
          .from("medicine")
          .select("*");
        if (error) throw error;
        data.value = medicine;
        dataLoaded.value = true;
      } catch (error) {
        console.warn(error.message);
      }
    };

    // data function
    getData();
    return { data, dataLoaded };
  },
};
</script>
