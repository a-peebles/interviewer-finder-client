<template>
  <section class="w-full px-6 pb-12 antialiased bg-white">
    <div class="mx-auto max-w-7xl">
      <nav class="relative z-50 h-24 select-none" x-data="{ showMenu: false }">
        <div
          class="container relative flex flex-wrap items-center justify-between h-24 mx-auto overflow-hidden font-medium border-b border-gray-200 md:overflow-visible lg:justify-center sm:px-4 md:px-2"
        >
          <div class="flex items-center justify-start w-1/4 h-full pr-4">
            <a href="/" class="inline-block py-4 md:py-0">
              <span class="p-1 text-xl font-black leading-none text-gray-900"
                ><span>Interviewer Finder</span
                ><span class="text-red-600">.</span></span
              >
            </a>
          </div>
          <div
            class="top-0 left-0 items-start hidden w-full h-full p-4 text-sm bg-gray-900 bg-opacity-50 md:items-center md:w-3/4 md:absolute lg:text-base md:bg-transparent md:p-0 md:relative md:flex"
            :class="{ 'flex fixed': true }"
          >
            <div
              class="flex-col w-full h-auto overflow-hidden bg-white rounded-lg md:bg-transparent md:overflow-visible md:rounded-none md:relative md:flex md:flex-row"
            >
              <a
                href="#_"
                class="inline-flex items-center block w-auto h-16 px-6 text-xl font-black leading-none text-gray-900 md:hidden"
                >tails<span class="text-green-600">.</span></a
              >
              <div
                class="flex flex-col items-start justify-center w-full space-x-6 text-center lg:space-x-8 md:w-2/3 md:mt-0 md:flex-row md:items-center"
              ></div>
              <div
                class="flex flex-col items-start justify-end w-full pt-4 md:items-center md:w-1/3 md:flex-row md:py-0"
              >
                <a
                  href="/login"
                  class="w-full px-6 py-2 mr-0 text-red-700 md:px-0 lg:pl-2 md:mr-4 lg:mr-5 md:w-auto hover:text-green-600"
                  >Sign In</a
                >
                <a
                  href="/register"
                  class="inline-flex items-center w-full px-6 py-3 text-sm font-medium leading-4 text-white bg-red-600 md:px-3 md:w-auto md:rounded-full lg:px-5 hover:bg-green-600 focus:outline-none md:focus:ring-2 focus:ring-0 focus:ring-offset-2"
                  >Sign Up</a
                >
              </div>
            </div>
          </div>
          <div
            @click="showMenu = !showMenu"
            class="absolute right-0 flex flex-col items-center items-end justify-center w-10 h-10 bg-white rounded-full cursor-pointer md:hidden hover:bg-gray-100"
          >
            <svg
              class="w-6 h-6 text-gray-700"
              x-show="!showMenu"
              fill="none"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              viewBox="0 0 24 24"
              stroke="currentColor"
              x-cloak=""
            >
              <path d="M4 6h16M4 12h16M4 18h16"></path>
            </svg>
            <svg
              class="w-6 h-6 text-gray-700"
              x-show="showMenu"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
              x-cloak=""
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M6 18L18 6M6 6l12 12"
              ></path>
            </svg>
          </div>
        </div>
      </nav>

      <!-- Main Hero Content -->
      <div
        class="container max-w-lg px-4 py-32 mx-auto text-left md:max-w-none md:text-center"
      >
        <h1
          class="text-5xl font-extrabold leading-10 tracking-tight text-left text-gray-900 md:text-center sm:leading-none md:text-6xl lg:text-7xl"
        >
          <span class="inline md:block">Find Available Engineers</span>
          <span
            class="relative mt-2 text-transparent bg-clip-text bg-gradient-to-br from-red-600 to-red-500 md:inline-block"
            >For Tech Interviews</span
          >
        </h1>
        <div
          class="mx-auto mt-5 text-gray-500 md:mt-12 md:max-w-lg md:text-center lg:text-lg"
        >
          See their preferred interview timeslots
        </div>
        <div class="flex flex-col items-center mt-12 text-center">
          <span class="relative inline-flex w-full md:w-auto">
            <a
              href="/register"
              class="inline-flex items-center justify-center w-full px-8 py-4 text-base font-bold leading-6 text-white bg-red-600 border border-transparent rounded-full md:w-auto hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-600"
            >
              Sign Up
            </a>
            <span
              class="absolute top-0 right-0 px-2 py-1 -mt-3 -mr-6 text-xs font-medium leading-tight text-white bg-green-600 rounded-full"
              >now!</span
            >
          </span>
          <a href="/login" class="mt-3 text-sm text-red-500 hover:text-green-600">Sign In</a>
        </div>
      </div>
      <!-- End Main Hero Content -->
    </div>
  </section>
</template>
