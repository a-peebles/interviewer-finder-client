<template>
  <section class="text-gray-600 body-font">
    <div class="container px-5 py-24 mx-auto max-w-7x1">
      <div class="flex flex-wrap w-full mb-4 p-4">
        <div class="w-full mb-6 lg:mb-0">
          <h1
            class="sm:text-4xl text-5xl font-medium font-bold title-font mb-2 text-gray-900"
          >
            Interviewer Finder
          </h1>
          <div class="h-1 w-20 bg-red-500 rounded"></div>
          <router-link to="/add-availability">
            <button
              class="text-white px-4 mt-8 w-auto h-10 bg-red-500 rounded-full hover:bg-red-700 active:shadow-lg mouse shadow transition ease-in duration-200 focus:outline-none"
            >
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
              <span class="title-font font-medium">Add availability</span>
            </button>
          </router-link>
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
            :description="availability.description"
          />
        </div>
      </div>
      <div class="month">
  <ul>
    <li class="prev">&#10094;</li>
    <li class="next">&#10095;</li>
    <li class ="">{{month[date.getMonth()]}}<br><span style="font-size:18px">{{date.getFullYear()}}</span></li>
  </ul>
</div>

<ul class="weekdays">
  <li>Mo</li>
  <li>Tu</li>
  <li>We</li>
  <li>Th</li>
  <li>Fr</li>

</ul>

<ul class="days">
  <li v-for="index in 31" :key="index">{{index}}</li>

</ul>
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

interface DashboardState {
  currentPage: number;
  currentAvailabilities: Availability[];
}

export default defineComponent({
  components: { AvailabilityCard },
  setup() {
    loadUser();
    const { user } = useAuth();
    const { setItemsCount, itemsCount } = usePagination();
    const dashboardState = reactive<DashboardState>({
      currentPage: 1,
      currentAvailabilities: [],
    });
    const date = new Date();
    const month = ["January","February","March","April","May","June","July","August","September","October","November","December"];
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
      month,
      date,
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
<style>


/* Month header */
.month {
  padding: 70px 25px;
  width: 100%;
  background: #16a34a;
  text-align: center;
}

/* Month list */
.month ul {
  margin: 0;
  padding: 0;
}

.month ul li {
  color: white;
  font-size: 20px;
  text-transform: uppercase;
  letter-spacing: 3px;
}

/* Previous button inside month header */
.month .prev {
  float: left;
  padding-top: 10px;
}

/* Next button */
.month .next {
  float: right;
  padding-top: 10px;
}

/* Weekdays (Mon-Sun) */
.weekdays {
  margin: 0;
  padding: 10px 0;
  background-color:#ddd;
}

.weekdays li {
  display: inline-block;
  width: 20%;
  color: #666;
  text-align: center;
}

/* Days (1-31) */
.days {
  padding: 10px 0;
  background: #eee;
  margin: 0;
}

.days li {
  list-style-type: none;
  display: inline-block;
  width: 20%;
  height: 20%;
  text-align: center;
  margin-bottom: 5px;
  font-size:12px;
  color: #777;
}

/* Highlight the "current" day */
.days li .active {
  padding: 5px;
  background: #1abc9c;
  color: white !important
}</style>
