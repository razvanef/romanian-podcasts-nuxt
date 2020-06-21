<template>
  <div v-if="snackbar" class="fixed right-0 bottom-0 mb-2 mx-2 flex lg:w-5/12 sm:w-full md:w-2/3 max-w-screen-sm justify-center bg-gray-800 text-white items-center items-end px-4 py-2 rounded" role="alert">
    <span class="text-xs block sm:inline flex-1">{{ text }}</span>
    <button @click="setAccepted()" class="hover:bg-gray-700 focus:outline-none text-pink-700 font-semibold py-1 px-4">
      {{ buttonText }}
    </button>
  </div>
</template>

<script>
import * as Cookie from "js-cookie";

const STORAGE_TYPES = {
  local: "localStorage",
  cookies: "cookies"
};

export default {
  name: "CookiesConsent",

  created() {
    if (this.storageType === STORAGE_TYPES.local) {
      // Check for availability of localStorage
      try {
        const test = "__vue-cookielaw-check-localStorage";
        if (typeof window !== "undefined") {
          window.localStorage.setItem(test, test);
          window.localStorage.removeItem(test);
        }
      } catch (e) {
        console.info(
          "Local storage is not supported, falling back to cookie use"
        );
        this.supportsLocalStorage = false;
      }
    }

    if (this.getAccepted()) {
      this.snackbar = false;
    }
  },

  computed: {
    canUseLocalStorage() {
      return (
        this.storageType === STORAGE_TYPES.local && this.supportsLocalStorage
      );
    }
  },

  methods: {
    getAccepted() {
      let accepted = false;
      if (this.canUseLocalStorage) {
        accepted = localStorage.getItem(this.storageName);
      } else {
        accepted = Cookie.get(this.storageName);
      }
      if (typeof accepted === "string") {
        accepted = JSON.parse(accepted);
      }

      return !!accepted;
    },
    setAccepted() {
      if (this.canUseLocalStorage) {
        localStorage.setItem(this.storageName, true);
      } else {
        Cookie.set(this.storageName, true, { expires: 30 });
      }
      this.snackbar = false;
    }
  },

  data: () => ({
    snackbar: true,
    text:
      "Acest site foloseste cookie-uri pentru a iti oferi cea mai buna experienta.",
    buttonText: "Accept",
    timeout: 0,
    snackBarRight: true,
    snackBarBottom: true,
    storageName: "cookieConsent",
    storageType: STORAGE_TYPES.cookies,
    supportsLocalStorage: true
  })
};
</script>
