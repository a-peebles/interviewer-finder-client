<template>
  <section class="text-gray-600 body-font">
    <div class="container px-5 py-24 mx-auto max-w-7x1">
      <div class="flex flex-wrap w-full mb-4 p-4">
        <div class="w-full mb-6 lg:mb-0">
          <h1
            class="sm:text-4xl text-5xl font-medium font-bold title-font mb-2 text-gray-900"
          >
            Search for Interviewer
          </h1>
          <div class="h-1 w-20 bg-red-500 rounded"></div>
          <form
            @submit.prevent="search"
            class="p-6 flex flex-col justify-center"
          >
            <div>
              <label class="block mb-2 text-sm text-gray-600 dark:text-gray-200 font-bold"
                >Grade</label
              >
              <div
                class="w-40 flex items-center border-2 py-2 px-3 rounded-lg mb-4"
              >
                <select v-model="grade"
                  class="pl-2 outline-none border-none w-40"
                  type="text"
                  name="grade"
                  id="grade"
                >
                  <option>Any</option>
                  <option>E0</option>
                  <option>E1</option>
                  <option>E2</option>
                  <option>E3</option>
                  <option>E4</option>
                  <option>E5</option>
                </select>
              </div>
            </div>
            <div>
              <label class="block mb-2 text-sm text-gray-600 dark:text-gray-200 font-bold"
                >Available From</label
              >
              <div>
                <div class="flex flex-col mt-2">
                  <label for="description" class="hidden">Time From</label>
                  <input v-model="dateFrom"
                    type="datetime-local"
                    name="timeFrom"
                    id="timeFrom"
                    class="w-50 mt-2 py-3 px-3 rounded-lg bg-white dark:bg-gray-800 border border-gray-400 dark:border-gray-700 text-gray-800 font-semibold focus:border-red-500 focus:outline-none mb-4"
                  />
                </div>
              </div>
            </div>
            <div>
              <label class="block mb-2 text-sm text-gray-600 dark:text-gray-200 font-bold"
                >Available Until</label
              >
              <div class="flex flex-col mt-2">
                <label for="description" class="hidden">Time Until</label>
                <input v-model="dateUntil"
                  type="datetime-local"
                  name="timeFrom"
                  id="timeUntil"
                  step="600"
                  class="w-50 mt-2 py-3 px-3 rounded-lg bg-white dark:bg-gray-800 border border-gray-400 dark:border-gray-700 text-gray-800 font-semibold focus:border-red-500 focus:outline-none"
                />
              </div>
            </div>

            <div class="flex flex-row flex-wrap gap-5 mt-8 mb-2">
              <ImageCard
                v-for="(theme, index) in themes"
                :key="index"
                :name="theme"
                :onClick="selecTheme"
                :isSelected="theme === selectedTheme"
              />
            </div>
            <button
              type="submit"
              class="md:w-32 bg-red-600 hover:bg-red-dark text-white font-bold py-3 px-6 rounded-lg mt-3 hover:bg-red-500 transition ease-in-out duration-300"
            >
              Search
            </button>
          </form>
          <svg
            viewBox="0 0 20 20"
            enable-background="new 0 0 20 20"
            class="w-6 h-6 inline-block"
          >
            <path
              fill="#FFFFFF"
              d="M16,10c0,0.553-0.048,1-0.601,1H11v4.399C11,15.951,10.553,16,10,16c-0.553,0-1-0.049-1-0.601V11H4.601
                                    C4.049,11,4,10.553,4,10c0-0.553,0.049-1,0.601-1H9V4.601C9,4.048,9.447,4,10,4c0.553,0,1,0.048,1,0.601V9h4.399
                                    C15.952,9,16,9.447,16,10z"
            />
          </svg>
        </div>
      </div>
      <div class="flex flex-wrap -m-4">
        <div
          class="xl:w-1/3 md:w-1/2 p-4"
          v-for="availability in availabilities"
          :key="availability.owner.id"
        >
          <AvailabilityCard
            :id="availability.owner.id"
            :firstName="availability.owner.firstName"
            :lastName="availability.owner.lastName"
            :name="availability.name"
            :theme="availability.theme"
            :description="availability.description"
          />
        </div>
      </div>
      <div class="flex mt-4">
        <button
          class="border border-red-500 text-red-500 block rounded-sm font-bold py-4 px-6 mr-2 flex items-center"
          @click="handleOnPreviousPageClick"
          :disabled="shouldDisablePreviousButton"
          :class="{
            'bg-red-500 hover:bg-red-400 hover:text-white text-white ':
              !shouldDisablePreviousButton,
          }"
        >
          <svg
            class="h-5 w-5 mr-2 fill-current"
            version="1.1"
            id="Layer_1"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            x="0px"
            y="0px"
            viewBox="-49 141 512 512"
            style="enable-background: new -49 141 512 512"
            xml:space="preserve"
          >
            <path
              id="XMLID_10_"
              d="M438,372H36.355l72.822-72.822c9.763-9.763,9.763-25.592,0-35.355c-9.763-9.764-25.593-9.762-35.355,0 l-115.5,115.5C-46.366,384.01-49,390.369-49,397s2.634,12.989,7.322,17.678l115.5,115.5c9.763,9.762,25.593,9.763,35.355,0 c9.763-9.763,9.763-25.592,0-35.355L36.355,422H438c13.808,0,25-11.193,25-25S451.808,372,438,372z"
            ></path>
          </svg>
          Previous page
        </button>
        <button
          class="border border-red-500 text-red-500 block rounded-sm font-bold py-4 px-6 ml-2 flex items-center"
          @click="handleOnNextPageClick"
          :disabled="shouldDisableNextButton"
          :class="{
            'bg-red-500 hover:bg-red-400 hover:text-white text-white':
              !shouldDisableNextButton,
          }"
        >
          Next page
          <svg
            class="h-5 w-5 ml-2 fill-current"
            clasversion="1.1"
            id="Layer_1"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            x="0px"
            y="0px"
            viewBox="-49 141 512 512"
            style="enable-background: new -49 141 512 512"
            xml:space="preserve"
          >
            <path
              id="XMLID_11_"
              d="M-24,422h401.645l-72.822,72.822c-9.763,9.763-9.763,25.592,0,35.355c9.763,9.764,25.593,9.762,35.355,0
            l115.5-115.5C460.366,409.989,463,403.63,463,397s-2.634-12.989-7.322-17.678l-115.5-115.5c-9.763-9.762-25.593-9.763-35.355,0
            c-9.763,9.763-9.763,25.592,0,35.355l72.822,72.822H-24c-13.808,0-25,11.193-25,25S-37.808,422-24,422z"
            />
          </svg>
        </button>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, computed, watch, reactive, toRefs } from "vue";
import { useApiWithAuth } from "../modules/api";
import AvailabilityCard from "../components/AvailabilityCard.vue";
import { useAuth, loadUser } from "../modules/auth";
import constants from "../modules/constants";
import { Availability } from "../interfaces/types";
import { usePagination } from "../modules/pagination";

interface dashboardState {
  currentPage: number;
  currentAvailabilities: Availability[];
  grade?: string;
  dateFrom: Date;
  dateUntil: Date;
}

export default defineComponent({
  components: { AvailabilityCard },
  setup() {
    loadUser();
    const { user } = useAuth();
    const { setItemsCount, itemsCount } = usePagination();
    const dashboardState = reactive<dashboardState>({
      currentPage: 1,
      currentAvailabilities: [],
    });

    const fetchData = (): void => {
      const { get, data } = useApiWithAuth(
        `/api/availability?pageSize=${constants.PAGE_SIZE.toString()}&page=${
          dashboardState.currentPage
        }`
      );
      get().then(
        (data) => (dashboardState.currentAvailabilities = data.availabilities)
      );
      watch([data], () => {
        if (data.value) {
          setItemsCount(data.value.availabilitiesCount);
        }
      });
    };

    fetchData();
    // const initials = computed(() => user?.value && user.value.email);

    const availabilities = computed(() => dashboardState.currentAvailabilities);

    const handleOnNextPageClick = () => {
      dashboardState.currentPage++;
      fetchData();
    };

    const handleOnPreviousPageClick = () => {
      dashboardState.currentPage--;
      fetchData();
    };

    const shouldDisablePreviousButton = computed(
      () => dashboardState.currentPage === 1
    );

    const shouldDisableNextButton = computed(
      () =>
        Math.ceil(itemsCount.value / constants.PAGE_SIZE) ===
        dashboardState.currentPage
    );

    return {
      user,
      handleOnNextPageClick,
      handleOnPreviousPageClick,
      availabilities,
      shouldDisablePreviousButton,
      shouldDisableNextButton,
      ...toRefs(dashboardState),
    };
  },
  onMounted() {
    // loadUser();
  },
});
</script>
