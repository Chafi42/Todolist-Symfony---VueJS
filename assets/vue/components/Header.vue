<template>
  <header class="z-30 mx-auto w-full max-w-screen-md bg-transparent py-3 lg:max-w-screen-lg relative inset-0">
    <div class="px-4">
      <div class="flex items-center justify-between">
        <!-- Logo -->
        <div class="flex shrink-0">
          <router-link to="/" class="flex items-center">
            <img
              class="h-7 w-auto"
              src="@/assets/images/chafi.png"
              alt="Logo"
            />
            <p class="sr-only">Website Title</p>
          </router-link>
        </div>

        <!-- Navigation -->
        <div class="md:flex md:items-center md:justify-center md:gap-5">
          <router-link
            to="/"
            class="inline-block rounded-lg px-2 py-1 text-sm font-medium text-[#222222] transition-all duration-200 hover:bg-gray-100 hover:text-[#222222]"
          >
            Accueil
          </router-link>

          <router-link
            to="/contact"
            class="inline-block rounded-lg px-2 py-1 text-sm font-medium text-[#222222] transition-all duration-200 hover:bg-gray-100 hover:text-[#222222]"
          >
            Contact
          </router-link>
        </div>

        <!-- Réseaux sociaux -->
        <div class="flex items-center gap-2 justify-end">
          <a
            href="https://www.linkedin.com/in/chafi-aad-5838431a6/"
            target="_blank"
            rel="noopener noreferrer"
            class="items-center justify-center bg-transparent px-1 sm:inline-flex hover:border-b-2 py-1 hover:border-[#020C69]"
          >
            <img
              class="h-5 w-auto"
              src="@/assets/images/linkedin.png"
              alt="LinkedIn"
            />
          </a>

          <a
            href="https://portfolio-chafi.netlify.app/"
            target="_blank"
            rel="noopener noreferrer"
            class="items-center justify-center bg-transparent px-1 sm:inline-flex hover:border-b-2 py-1 hover:border-[#020C69]"
          >
            <img
              class="h-5 w-auto"
              src="@/assets/images/dossier-portfolio.png"
              alt="Portfolio"
            />
          </a>

          <a
            href="https://github.com/"
            target="_blank"
            rel="noopener noreferrer"
            class="items-center justify-center bg-transparent px-1 sm:inline-flex hover:border-b-2 py-1 hover:border-[#020C69]"
          >
            <img
              class="h-5 w-auto"
              src="@/assets/images/github.png"
              alt="GitHub"
            />
          </a>
        </div>
      </div>
    </div>
  </header>
  <div class="border-b-1 mt-4 w-full border-[#020C69]"></div>
</template>

<script>
export default {
  name: "Header",
};
</script>