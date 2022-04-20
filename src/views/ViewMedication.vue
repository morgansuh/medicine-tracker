<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- Error Handling -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 rounded-md shadow-md bg-light-grey"
    >
      <p class="text-at-light-pink">
        {{ statusMsg }}
      </p>
      <p class="text-red-500">
        {{ errorMsg }}
      </p>
    </div>

    <div v-if="dataLoaded">
      <!-- General Info -->
      <div
        class="
          flex flex-col
          items-center
          p-8
          rounded-md
          shadow-md
          bg-light-grey
          relative
        "
      >
        <!-- Delete/Edit Options -->
        <div v-if="user" class="flex absolute left-2 top-2 gap-x-2">
          <div
            class="
              h-7
              w-7
              rounded-full
              flex
              justify-center
              items-center
              cursor-pointer
              bg-at-light-pink
              shadow-lg
            "
            @click="editMode"
          >
            <img
              class="h-3.5 w-auto"
              src="@/assets/images/pencil-light.png"
              alt=""
            />
          </div>
          <div
            @click="deleteMedication"
            class="
              h-7
              w-7
              rounded-full
              flex
              justify-center
              items-center
              cursor-pointer
              bg-at-light-pink
              shadow-lg
            "
          >
            <img
              class="h-3.5 w-auto"
              src="@/assets/images/trashcan.png"
              alt=""
            />
          </div>
        </div>

        <img
          v-if="data.medicineType === 'liquid'"
          class="h-24 w-auto"
          src="@/assets/images/liquid.png"
          alt=""
        />

        <!-- Medicine Type Images -->
        <img
          v-if="data.medicineType === 'capsule'"
          class="h-24 w-auto"
          src="@/assets/images/capsule.png"
          alt=""
        />

        <img
          v-if="data.medicineType === 'inject'"
          class="h-24 w-auto"
          src="@/assets/images/inject.png"
          alt=""
        />

        <img
          v-if="data.medicineType === 'topical'"
          class="h-24 w-auto"
          src="@/assets/images/topical.png"
          alt=""
        />

        <img
          v-if="data.medicineType === 'other'"
          class="h-24 w-auto"
          src="@/assets/images/other.png"
          alt=""
        />

        <span
          class="
            mt-6
            py-1.5
            px-5
            text-xs text-white
            bg-at-light-pink
            rounded-lg
            shadow-md
          "
        >
          {{ data.medicineType }}
        </span>

        <div class="w-full mt-6">
          <input
            v-if="edit"
            type="text"
            class="p-2 w-full text-gray-500 focus:outline-none"
            v-model="data.medicineName"
          />
          <h1 v-else class="text-at-light-pink text-2xl text-center">
            {{ data.medicineName }}
          </h1>
        </div>
      </div>
      <!-- Medicines -->
      <div
        class="
          mt-10
          p-8
          rounded-md
          flex flex-col
          item-center
          bg-light-grey
          shadow-md
        "
      >
        <!-- Liquids -->
        <div
          v-if="data.medicineType === 'liquid'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.medicines"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label
                for="medicine-name"
                class="mb-1 text-sm text-at-light-pink"
              >
                Medicine
              </label>
              <input
                id="medicine-name"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.medicine"
              />
              <p v-else>{{ item.medicine }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="dosage" class="mb-1 text-sm text-at-light-pink">
                Dosage
              </label>
              <input
                id="dosage"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.dosage"
              />
              <p v-else>{{ item.dosage }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="notes" class="mb-1 text-sm text-at-light-pink">
                Notes
              </label>
              <input
                id="notes"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.notes"
              />
              <p v-else>{{ item.notes }}</p>
            </div>
            <img
              v-if="edit"
              @click="deleteMedicine(item.id)"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trashcan.png"
              alt=""
            />
          </div>
          <button
            v-if="edit"
            @click="addMedication"
            type="button"
            class="
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
        <div
          v-if="data.medicineType === 'capsule'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.medicines"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label
                for="medicine-name"
                class="mb-1 text-sm text-at-light-pink"
              >
                Medicine
              </label>
              <input
                id="medicine-name"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.medicine"
              />
              <p v-else>{{ item.medicine }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="dosage" class="mb-1 text-sm text-at-light-pink">
                Dosage
              </label>
              <input
                id="dosage"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.dosage"
              />
              <p v-else>{{ item.dosage }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="notes" class="mb-1 text-sm text-at-light-pink">
                Notes
              </label>
              <input
                id="notes"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.notes"
              />
              <p v-else>{{ item.notes }}</p>
            </div>
            <img
              v-if="edit"
              @click="deleteMedicine(item.id)"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trashcan.png"
              alt=""
            />
          </div>
          <button
            v-if="edit"
            @click="addMedication"
            type="button"
            class="
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
        <div
          v-if="data.medicineType === 'inject'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.medicines"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label
                for="medicine-name"
                class="mb-1 text-sm text-at-light-pink"
              >
                Medicine
              </label>
              <input
                id="medicine-name"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.medicine"
              />
              <p v-else>{{ item.medicine }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="unita" class="mb-1 text-sm text-at-light-pink">
                Units
              </label>
              <input
                id="units"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.units"
              />
              <p v-else>{{ item.units }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="notes" class="mb-1 text-sm text-at-light-pink">
                Notes
              </label>
              <input
                id="notes"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.notes"
              />
              <p v-else>{{ item.notes }}</p>
            </div>
            <img
              v-if="edit"
              @click="deleteMedicine(item.id)"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trashcan.png"
              alt=""
            />
          </div>
          <button
            v-if="edit"
            @click="addMedication"
            type="button"
            class="
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
        <div
          v-if="data.medicineType === 'topical'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.medicines"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label
                for="medicine-name"
                class="mb-1 text-sm text-at-light-pink"
              >
                Medicine
              </label>
              <input
                id="medicine-name"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.medicine"
              />
              <p v-else>{{ item.medicine }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="dose" class="mb-1 text-sm text-at-light-pink">
                Dose
              </label>
              <input
                id="dose"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.dose"
              />
              <p v-else>{{ item.dose }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="notes" class="mb-1 text-sm text-at-light-pink">
                Notes
              </label>
              <input
                id="notes"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.notes"
              />
              <p v-else>{{ item.notes }}</p>
            </div>
            <img
              v-if="edit"
              @click="deleteMedicine(item.id)"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trashcan.png"
              alt=""
            />
          </div>
          <button
            v-if="edit"
            @click="addMedication"
            type="button"
            class="
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
        <div
          v-if="data.medicineType === 'other'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.medicines"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label
                for="medicine-name"
                class="mb-1 text-sm text-at-light-pink"
              >
                Medicine
              </label>
              <input
                id="medicine-name"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.medicine"
              />
              <p v-else>{{ item.medicine }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="units" class="mb-1 text-sm text-at-light-pink">
                Units
              </label>
              <input
                id="units"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.units"
              />
              <p v-else>{{ item.units }}</p>
            </div>
            <div class="flex flex-2 flex-col">
              <label for="notes" class="mb-1 text-sm text-at-light-pink">
                Notes
              </label>
              <input
                id="notes"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.notes"
              />
              <p v-else>{{ item.notes }}</p>
            </div>
            <img
              v-if="edit"
              @click="deleteMedicine(item.id)"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trashcan.png"
              alt=""
            />
          </div>
          <button
            v-if="edit"
            @click="addMedication"
            type="button"
            class="
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
      </div>

      <!-- Updates -->
      <button
        v-if="edit"
        @click="update"
        type="button"
        class="
          mt-10
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
        Update Medication
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { supabase } from "../supabase/init";
import { useRoute, useRouter } from "vue-router";
import store from "../store/index";
import { uid } from "uid";

export default {
  name: "view-medicine",
  setup() {
    // Create data / vars
    const data = ref(null);
    const dataLoaded = ref(null);
    const errorMsg = ref(null);
    const statusMsg = ref(null);
    const route = useRoute();
    const router = useRouter();
    const user = computed(() => store.state.user);

    // Get current Id of route
    const currentId = route.params.medicineId;
    // Get meds data
    const getData = async () => {
      try {
        const { data: medicine, error } = await supabase
          .from("medicine")
          .select("*")
          .eq("id", currentId);
        if (error) throw error;
        data.value = medicine[0];
        dataLoaded.value = true;
      } catch (error) {
        errorMsg.value = error.message;
        setTimeout(() => {
          errorMsg.value = false;
        }, 10000);
      }
    };
    getData();
    // Delete med
    const deleteMedication = async () => {
      try {
        const { error } = await supabase
          .from("medicine")
          .delete()
          .eq("id", currentId);
        if (error) throw error;
        router.push({ name: "Home" });
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };
    // Edit mode
    const edit = ref(null);
    const editMode = () => {
      edit.value = !edit.value;
    };

    // Add medicine
    const addMedication = () => {
      if (data.value.medicineType === "liquid") {
        data.value.medicines.push({
          id: uid(),
          medicine: "",
          dosage: "",
          notes: "",
        });
        return;
      }
      if (data.value.medicineType === "capsule") {
        data.value.medicines.push({
          id: uid(),
          medicine: "",
          dosage: "",
          notes: "",
        });
        return;
      }
      if (data.value.medicineType === "inject") {
        data.value.medicines.push({
          id: uid(),
          medicine: "",
          units: "",
          notes: "",
        });
        return;
      }
      if (data.value.medicineType === "topical") {
        data.value.medicines.push({
          id: uid(),
          medicine: "",
          dose: "",
          notes: "",
        });
        return;
      }
      if (data.value.medicineType === "other") {
        data.value.medicines.push({
          id: uid(),
          medicine: "",
          units: "",
          notes: "",
        });
        return;
      }
    };
    // Delete med
    const deleteMedicine = (id) => {
      if (data.value.medicines.length > 1) {
        data.value.medicines = data.value.medicines.filter(
          (medicines) => medicines.id !== id
        );
        return;
      }
      errorMsg.value =
        "Error: Cannot remove, need to at least have one medicine";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };
    // Update meds
    const update = async () => {
      try {
        const { error } = await supabase
          .from("medicine")
          .update({
            medicineName: data.value.medicineName,
            medicines: data.value.medicines,
          })
          .eq("id", currentId);
        if (error) throw error;
        edit.value = false;
        statusMsg.value = "Success: Medication Updated!";
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value`Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };
    return {
      statusMsg,
      data,
      dataLoaded,
      errorMsg,
      edit,
      editMode,
      user,
      deleteMedication,
      addMedication,
      deleteMedicine,
      update,
    };
  },
};
</script>
