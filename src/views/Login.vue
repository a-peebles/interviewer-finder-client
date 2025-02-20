<template>
  <div class="h-screen flex">
    <div
      class="flex w-1/2 bg-gradient-to-tr from-green-800 to-green-600 i justify-around items-center"
    >
      <div>
        <h1 class="text-white font-bold text-xl font-sans">Engineer Interviewer Finder</h1>
        <br/>
        <p class="text-white mt-1">
          Post, read, update and delete your interview availability
        </p>
        <br/>
        <p class="text-white mt-1">
          Find Engineers to take on interviews during their specified availability
        </p>
      </div>
    </div>
    <div class="flex items-center w-full max-w-md px-6 mx-auto lg:w-2/6">
      <div class="flex-1">
        <div>
          <h1 class="text-gray-800 font-bold text-2xl mb-1">Hello Again!</h1>

          <p class="mt-3 text-gray-500 dark:text-gray-300">
            Sign in to access your account
          </p>
          
        </div>

        <div class="mt-8">
          <form @submit.prevent="submit">
            <div>
              <label
                for="email"
                class="block mb-2 text-sm text-gray-600 dark:text-gray-200"
                >Email Address</label
              >
              <div
                class="flex items-center border-2 py-2 px-3 rounded-2xl mb-4"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 text-gray-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M16 12a4 4 0 10-8 0 4 4 0 008 0zm0 0v1.5a2.5 2.5 0 005 0V12a9 9 0 10-9 9m4.5-1.206a8.959 8.959 0 01-4.5 1.207"
                  />
                </svg>
                <input
                  class="pl-2 outline-none border-none w-full"
                  type="text"
                  name="email"
                  id="email"
                  placeholder="hello@example.com"
                  v-model="email"
                />
              </div>
            </div>

            <div class="mt-6">
              <div class="flex justify-between mb-2">
                <label
                  for="password"
                  class="text-sm text-gray-600 dark:text-gray-200"
                  >Password</label
                >
                <a
                  href="#"
                  class="text-sm text-gray-400 focus:text-green-500 hover:text-red-500 hover:underline"
                  >Forgot password?</a
                >
              </div>

              <div class="flex items-center border-2 py-2 px-3 rounded-2xl">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 text-gray-400"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                >
                  <path
                    fill-rule="evenodd"
                    d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z"
                    clip-rule="evenodd"
                  />
                </svg>
                <input
                  class="pl-2 outline-none border-none w-full"
                  type="password"
                  name="password"
                  id="password"
                  placeholder="Password"
                  v-model="password"
                />
              </div>
            </div>

            <div class="mt-6">
              <button
                type="submit"
                class="block w-full bg-red-600 hover:bg-green-600 mt-4 py-2 rounded-2xl text-white font-semibold mb-2"
              >
                Login
              </button>
            </div>
          </form>

          <p class="mt-6 text-sm text-center text-gray-400">
            Don&#x27;t have an account yet?
            <router-link to="/register"
              ><span
                class="text-red-500 focus:outline-none focus:text-green-500 focus:underline hover:underline"
                >Sign up</span
              ></router-link
            >
          </p>
          <p class="mt-6 text-sm text-center text-gray-400">
            Back to
            <router-link to="/"
              ><span
                class="text-red-500 focus:outline-none focus:text-green-500 focus:underline hover:underline"
                >Home</span
              ></router-link
            >
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from "vue";
import { useRouter } from "vue-router";
import { useApi } from "../modules/api";

interface LoginPayload {
  email: string;
  password: string;
}

export default defineComponent({
  setup() {
    const { loading, data, post } = useApi("api/auth");

    const router = useRouter();

    const payload = reactive<LoginPayload>({
      email: "",
      password: "",
    });

    const submit = () => {
      post(payload).then(() => {
        localStorage.setItem("token", data.value.token);
        router.push({ name: "dashboard" });
      });
    };

    return {
      loading,
      submit,
      ...toRefs(payload),
    };
  },
});
</script>
