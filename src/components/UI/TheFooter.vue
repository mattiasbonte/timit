<template>
  <footer class="container p-2 mx-auto">
    <div class="apply-footer">
      <div class="md:order-2 flex justify-center space-x-6">
        <!-- MAIL -->
        <a
          href="mailto:info@mattiasbonte.dev"
          target="_blank"
          class="apply-icon"
          title="Say Hi!"
        >
          <span class="sr-only">Say hi to me!</span>
          <svg
            class="w-6 h-6"
            stroke-width="1.1"
            stroke="currentColor"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <line x1="10" y1="14" x2="21" y2="3" />
            <path
              d="M21 3l-6.5 18a0.55 .55 0 0 1 -1 0l-3.5 -7l-7 -3.5a0.55 .55 0 0 1 0 -1l18 -6.5"
            />
          </svg>
        </a>
        <!-- GITHUB -->
        <a
          href="https://github.com/mattiasbonte/"
          class="apply-icon"
          title="GitHub"
        >
          <span class="sr-only">GitHub</span>
          <svg
            class="w-6 h-6"
            stroke-width="1.1"
            stroke="currentColor"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <path
              d="M9 19c-4.3 1.4 -4.3 -2.5 -6 -3m12 5v-3.5c0 -1 .1 -1.4 -.5 -2c2.8 -.3 5.5 -1.4 5.5 -6a4.6 4.6 0 0 0 -1.3 -3.2a4.2 4.2 0 0 0 -.1 -3.2s-1.1 -.3 -3.5 1.3a12.3 12.3 0 0 0 -6.2 0c-2.4 -1.6 -3.5 -1.3 -3.5 -1.3a4.2 4.2 0 0 0 -.1 3.2a4.6 4.6 0 0 0 -1.3 3.2c0 4.6 2.7 5.7 5.5 6c-.6 .6 -.6 1.2 -.5 2v3.5"
            />
          </svg>
        </a>
        <!-- LINKEDIN -->
        <a
          href="https://www.linkedin.com/in/mattiasbonte/"
          target="_blank"
          class="apply-icon"
          title="LinkedIn"
        >
          <span class="sr-only">LinkedIn</span>
          <svg
            class="w-6 h-6"
            stroke-width="1.1"
            stroke="currentColor"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <rect x="4" y="4" width="16" height="16" rx="2" />
            <line x1="8" y1="11" x2="8" y2="16" />
            <line x1="8" y1="8" x2="8" y2="8.01" />
            <line x1="12" y1="16" x2="12" y2="11" />
            <path d="M16 16v-3a2 2 0 0 0 -4 0" />
          </svg>
        </a>
        <!-- BUY ME WINE -->
        <a
          href="https://www.buymeacoffee.com/mattiasbonte"
          target="_blank"
          class="apply-icon"
          title="Buy me some wine!"
        >
          <span class="sr-only">Buy me a glass of wine</span>
          <svg
            class="w-6 h-6"
            stroke-width="1.1"
            stroke="currentColor"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <line x1="8" y1="21" x2="16" y2="21" />
            <line x1="12" y1="15" x2="12" y2="21" />
            <path d="M16 4l1 6a5 5 0 0 1 -10 0l1 -6z" />
          </svg>
        </a>
      </div>

      <!-- COPYRIGHT TEXT -->
      <div class="md:mt-0 md:order-1 mt-4 font-thin">
        <p class="dark:text-gray-300 text-center text-gray-600">
          &copy; 2020
          <a
            href="https://mattiasbonte.dev"
            rel="noreferrer"
            class="hover:underline hover:text-black dark:hover:text-white"
            title="Check out my portflio!"
            target="_blank"
            >Mattias Bonte</a
          >. All rights reserved.
        </p>
      </div>
    </div>
  </footer>
</template>

<script>
  export default {};
</script>

<style scoped>
  .apply-footer {
    @apply md:flex md:items-center md:justify-between py-4;
  }

  .apply-icon {
    @apply text-gray-600 dark:text-gray-300;
    @apply transform hover:scale-110;
    @apply hover:text-black;

    /* dark */
    @apply dark:hover:text-white;
  }
</style>
