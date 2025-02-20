<template>
  <Modal
    v-if="isModalVisible"
    :onConfirm="deleteAvailability"
    :message="'Are you sure you want to delete this availability?'"
  />
  <section class="text-gray-600 body-font">
    <div class="container px-5 py-24 mx-auto max-w-7x1">
      <div class="flex flex-wrap w-full mb-4 p-4">
        <div class="w-full mb-6 lg:mb-0">
          <h1
            class="sm:text-4xl text-5xl font-medium font-bold title-font mb-2 text-gray-900"
          >
            Availability
          </h1>
          <div class="h-1 w-20 bg-red-500 rounded"></div>
        </div>
      </div>
      <div class="flex flex-wrap -m-4" v-if="data">
        <div class="xl:w-1/3 md:w-1/2 p-4">
          <div class="bg-white p-6 rounded-lg">
            <h3
              class="tracking-widest text-red-500 text-xs font-medium title-font"
            >
              {{ `${data.owner.firstName} ${data.owner.lastName}` }}
            </h3>
            <h2 class="text-lg text-gray-900 font-medium title-font mb-4">
              {{ data.name }}
            </h2>
            <p class="leading-relaxed text-base">
              {{ data.description }}
            </p>
            <div>
              <button
                v-if="
                  !contributors
                    .map((contributor) => contributor.id)
                    .includes(user.id)
                "
                @click="contributeAvailability"
                class="text-white px-4 mt-8 w-auto h-10 bg-green-500 rounded-full hover:bg-green-700 active:shadow-lg mouse shadow transition ease-in duration-200 focus:outline-none mr-4"
              >
                <span class="title-font font-medium">Contribute</span>
              </button>
              <router-link :to="'/availabilities/edit/' + props.id">
                <button
                  v-if="user.id === data.owner.id"
                  class="text-white px-4 mt-8 w-auto h-10 bg-yellow-500 rounded-full hover:bg-yellow-700 active:shadow-lg mouse shadow transition ease-in duration-200 focus:outline-none mr-4"
                >
                  <span class="title-font font-medium">Edit</span>
                </button>
              </router-link>
              <button
                v-if="user.id === data.owner.id"
                @click="toggleModal"
                class="text-white px-4 mt-8 w-auto h-10 bg-red-500 rounded-full hover:bg-red-700 active:shadow-lg mouse shadow transition ease-in duration-200 focus:outline-none"
              >
                <span class="title-font font-medium">Delete</span>
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="flex flex-wrap w-full mb-4 p-4">
        <div class="w-full mb-6 lg:mb-0">
          <h1
            class="sm:text-4xl text-5xl font-medium font-bold title-font mb-2 text-gray-900"
          >
            Contributors
          </h1>
          <div class="h-1 w-20 bg-red-500 rounded"></div>
        </div>
      </div>
      <UsersTable
        v-if="data && contributors.length > 0"
        :columns="['Name', 'Email', 'Status', 'Action']"
        :data="contributors"
        :action="removeContributor"
        :currentUserId="currentUserId"
      />
      <Notification
        v-if="data && !contributors.length"
        :header="'No contributors...yet!'"
        :body="'Contribute to ' + data.name + ' now!'"
      />
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, computed, watchEffect, ref } from "vue";
import { useRouter } from "vue-router";
import { useApiWithAuth } from "../modules/api";
import { useModal } from "../modules/modal";
import UsersTable from "../components/UsersTable.vue";
import Modal from "../components/Modal.vue";
import Notification from "../components/Notification.vue";
import { useAuth, loadUser } from "../modules/auth";

interface ContributeAvailabilityPayload {
  userId: string;
}

export default defineComponent({
  components: { UsersTable, Modal, Notification },
  props: ["id"],
  setup(props) {
    const router = useRouter();
    loadUser();
    const { user } = useAuth();
    const { toggleModal, isModalVisible } = useModal();
    const { loading, data, get } = useApiWithAuth(
      `/api/availabilities/${props.id}`
    );
    const currentUserId = computed(() => user?.value && user.value.id);

    get();

    const contributors = ref();

    watchEffect(
      () => data.value && (contributors.value = data.value.contributors)
    );

    const deleteAvailability = () => {
      const { del } = useApiWithAuth(`/api/availabilities/${props.id}`);
      del().then(() => {
        router.push({ name: "dashboard" });
      });
    };

    const contributeAvailability = () => {
      const { post } = useApiWithAuth(
        `/api/availabilities/${props.id}/contributors`
      );
      const paylod: ContributeAvailabilityPayload = {
        userId: user?.value?.id || "",
      };
      post(paylod).then(() => {
        contributors.value = [
          ...contributors.value,
          {
            id: user?.value?.id,
            email: user?.value?.email,
            firstName: user?.value?.firstName,
            lastName: user?.value?.lastName,
            status: user?.value?.status,
            avatar: user?.value?.avatar,
          },
        ];
      });
    };

    const removeContributor = () => {
      const { del } = useApiWithAuth(
        `/api/availabilities/${props.id}/contributors/${currentUserId.value}`
      );
      del().then(() => {
        contributors.value = contributors.value.filter(
          (contributor: {
            id: string;
            email: string;
            firstName: string;
            lastName: string;
          }) => contributor.id !== user?.value?.id
        );
      });
    };
    return {
      user,
      data,
      loading,
      deleteAvailability,
      contributeAvailability,
      removeContributor,
      currentUserId,
      contributors,
      props,
      isModalVisible,
      toggleModal,
    };
  },

  onMounted() {
    // loadUser();
  },
});
</script>
