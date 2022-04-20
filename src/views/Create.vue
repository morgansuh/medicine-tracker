<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- Status -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 bg-light-grey rounded-md shadow-lg"
    >
      <p class="text-at-light-pink">
        {{ statusMsg }}
      </p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Creation -->
    <div class="p-8 flex items-start bg-light-grey rounded-md shadow-lg">
      <!-- Forms-->
      <form
        @submit.prevent="createMedication"
        class="flex flex-col gap-y-5 w-full"
      >
        <h1 class="text-2xl text-at-light-pink">Record Medication</h1>
        <!-- Med Names -->
        <div class="flex flex-col">
          <label for="medication-name" class="mb-1 text-sm text-at-light-pink"
            >Medication Name</label
          >
          <input
            type="text"
            required
            class="p-2 text-gray-500 focus:outline-none"
            id="medication-name"
            v-model="medicationName"
          />
        </div>

        <!-- Med Types-->
        <div class="flex flex-col">
          <label for="medication-type" class="mb-1 text-sm text-at-light-pink"
            >Medication Type</label
          >

          <select
            id="medication-type"
            class="p-2 text-gray-500 focus:outline-none"
            required
            @change="medicationChange"
            v-model="medicationType"
          >
            <option value="selection-medication">Select Medication</option>
            <option value="liquid">Liquid</option>
            <option value="capsule">Capsules/Tablets</option>
            <option value="inject">Injections</option>
            <option value="topical">Topical</option>
            <option value="other">Other</option>
          </select>
        </div>

        <!-- Liquid-->
        <div v-if="medicationType === 'liquid'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in medicines"
            :key="index"
          >
            <div class="flex flex-col md:w-1/4">
              <label
                for="medication-name"
                class="mb-1 text-sm text-at-light-pink"
                >Medicine</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.medicine"
              />
            </div>

            <div class="flex flex-col md:w-1/6">
              <label for="dosage" class="mb-1 text-sm text-at-light-pink"
                >Dosage</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.dosage"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="notes" class="mb-1 text-sm text-at-light-pink"
                >Notes</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.notes"
              />
            </div>

            <img
              @click="deleteMedicine(item.id)"
              src="@/assets/images/trashcan.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt="trash"
            />
          </div>

          <button
            @click="addMedication"
            type="button"
            class="
              mt-6
              py-2
              px-6
              rounded-sm
              self-start
              text-sm text-white
              bg-at-light-pink
              duration-200
              border-solid border-2 border-transparent
              hover:border-at-light-pink hover:bg-white hover:text-at-light-pink
            "
          >
            Add Medication
          </button>
        </div>

        <!-- Capsules -->
        <div v-if="medicationType === 'capsule'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in medicines"
            :key="index"
          >
            <div class="flex flex-col md:w-1/4">
              <label
                for="medication-name"
                class="mb-1 text-sm text-at-light-pink"
                >Medicine</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.medicine"
              />
            </div>

            <div class="flex flex-col md:w-1/6">
              <label for="dosage" class="mb-1 text-sm text-at-light-pink"
                >Dosage</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.dosage"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="notes" class="mb-1 text-sm text-at-light-pink"
                >Notes</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.notes"
              />
            </div>

            <img
              @click="deleteMedicine(item.id)"
              src="@/assets/images/trashcan.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt="trash"
            />
          </div>

          <button
            @click="addMedication"
            type="button"
            class="
              mt-6
              py-2
              px-6
              rounded-sm
              self-start
              text-sm text-white
              bg-at-light-pink
              duration-200
              border-solid border-2 border-transparent
              hover:border-at-light-pink hover:bg-white hover:text-at-light-pink
            "
          >
            Add Medication
          </button>
        </div>

        <!-- Inject -->
        <div v-if="medicationType === 'inject'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in medicines"
            :key="index"
          >
            <div class="flex flex-col md:w-1/4">
              <label
                for="medication-name"
                class="mb-1 text-sm text-at-light-pink"
                >Medicine</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.medicine"
              />
            </div>

            <div class="flex flex-col md:w-1/6">
              <label for="units" class="mb-1 text-sm text-at-light-pink"
                >Units</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.units"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="notes" class="mb-1 text-sm text-at-light-pink"
                >Notes</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.notes"
              />
            </div>

            <img
              @click="deleteMedicine(item.id)"
              src="@/assets/images/trashcan.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt="trash"
            />
          </div>

          <button
            @click="addMedication"
            type="button"
            class="
              mt-6
              py-2
              px-6
              rounded-sm
              self-start
              text-sm text-white
              bg-at-light-pink
              duration-200
              border-solid border-2 border-transparent
              hover:border-at-light-pink hover:bg-white hover:text-at-light-pink
            "
          >
            Add Medication
          </button>
        </div>

        <!-- Topical -->
        <div v-if="medicationType === 'topical'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in medicines"
            :key="index"
          >
            <div class="flex flex-col md:w-1/4">
              <label
                for="medication-name"
                class="mb-1 text-sm text-at-light-pink"
                >Name</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.medicine"
              />
            </div>

            <div class="flex flex-col md:w-1/6">
              <label for="dose" class="mb-1 text-sm text-at-light-pink"
                >Dose</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.dose"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="notes" class="mb-1 text-sm text-at-light-pink"
                >Notes</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.notes"
              />
            </div>

            <img
              @click="deleteMedicine(item.id)"
              src="@/assets/images/trashcan.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt="trash"
            />
          </div>

          <button
            @click="addMedication"
            type="button"
            class="
              mt-6
              py-2
              px-6
              rounded-sm
              self-start
              text-sm text-white
              bg-at-light-pink
              duration-200
              border-solid border-2 border-transparent
              hover:border-at-light-pink hover:bg-white hover:text-at-light-pink
            "
          >
            Add Medication
          </button>
        </div>

        <!-- Other -->
        <div v-if="medicationType === 'other'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in medicines"
            :key="index"
          >
            <div class="flex flex-col md:w-1/4">
              <label
                for="medication-name"
                class="mb-1 text-sm text-at-light-pink"
                >Medicine</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.medicine"
              />
            </div>

            <div class="flex flex-col md:w-1/6">
              <label for="units" class="mb-1 text-sm text-at-light-pink"
                >Units</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.units"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label for="notes" class="mb-1 text-sm text-at-light-pink"
                >Notes</label
              >
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.notes"
              />
            </div>

            <img
              @click="deleteMedicine(item.id)"
              src="@/assets/images/trashcan.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt="trash"
            />
          </div>

          <button
            @click="addMedication"
            type="button"
            class="
              mt-6
              py-2
              px-6
              rounded-sm
              self-start
              text-sm text-white
              bg-at-light-pink
              duration-200
              border-solid border-2 border-transparent
              hover:border-at-light-pink hover:bg-white hover:text-at-light-pink
            "
          >
            Add Medication
          </button>
        </div>

        <button
          type="submit"
          class="
            mt-6
            py-2
            px-6
            rounded-sm
            self-start
            text-sm text-white
            bg-at-light-pink
            duration-200
            border-solid border-2 border-transparent
            hover:border-at-light-green hover:bg-white hover:text-at-light-pink
          "
        >
          Record Medication
        </button>
      </form>
    </div>
  </div>
</template>


<script>
import { ref } from "vue";
import { uid } from "uid";
import { supabase } from "../supabase/init";
export default {
  name: "create",
  setup() {
    // data types
    const medicationName = ref("");
    const medicationType = ref("select-medicine");
    const medicines = ref([]);
    // errorHandling
    const statusMsg = ref(null);
    const errorMsg = ref(null);
    // adding medicines

    const addMedication = () => {
      if (medicationType.value === "liquid") {
        medicines.value.push({
          id: uid(),
          medicine: "",
          dosage: "",
          notes: "",
        });
      }
      if (medicationType.value === "capsule") {
        medicines.value.push({
          id: uid(),
          medicine: "",
          dosage: "",
          notes: "",
        });
      }
      if (medicationType.value === "inject") {
        medicines.value.push({
          id: uid(),
          medicine: "",
          units: "",
          notes: "",
        });
      }
      if (medicationType.value === "topical") {
        medicines.value.push({
          id: uid(),
          medicine: "",
          dose: "",
          notes: "",
        });
      }
      if (medicationType.value === "other") {
        medicines.value.push({
          id: uid(),
          medicine: "",
          units: "",
          notes: "",
        });
        return;
      }
    };
    // deleting medicine
    const deleteMedicine = (id) => {
      if (medicines.value.length > 1) {
        medicines.value = medicines.value.filter(
          (medicine) => medicine.id !== id
        );
        return;
      }
      //must have min one med
      errorMsg.value =
        "Error: Cannot remove, need to at least have one medicine";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };

    // listens for changing of medicine type input (don't see inputs on change)
    const medicationChange = () => {
      medicines.value = [];
      addMedication();
    };
    // create medicine
    const createMedication = async () => {
      try {
        const { error } = await supabase.from("medicine").insert([
          {
            medicineName: medicationName.value,
            medicineType: medicationType.value,
            medicines: medicines.value,
          },
        ]);
        if (error) throw error;
        statusMsg.value = "Success: Medication Created!";
        medicationName.value = null;
        medicationType.value = "select-medicine";
        medicines.value = [];
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };
    return {
      medicationName,
      medicationType,
      medicines,
      statusMsg,
      errorMsg,
      addMedication,
      medicationChange,
      deleteMedicine,
      createMedication,
    };
  },
};
</script>
