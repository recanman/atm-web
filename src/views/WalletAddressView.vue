<script setup lang="ts">
import { RouterLink, useRouter } from 'vue-router'
import { ref, onUnmounted } from 'vue'
import { useSessionStore } from '@/stores/session'
import { useLanguageStore } from '@/stores/language'

let seconds = ref(15)
const router = useRouter()
const sessionStore = useSessionStore()
const languageStore = useLanguageStore()

const content = languageStore.getContent('wallet')
const buttons = languageStore.getContent('buttons')

const intervalId = setInterval(() => {
  seconds.value--
  if (seconds.value === 0) {
    clearInterval(intervalId)
    router.push({ name: 'Payment' })
  }
}, 1000)

onUnmounted(() => {
  clearInterval(intervalId)
})
</script>

<template>
  <div class="flex flex-col">
    <div class="flex flex-col justify-center items-center flex-grow">
      <p class="text-lg font-semibold text-monero-grey m-3">{{ content.title }}</p>
      <input
        readonly
        class="input bg-monero-grey text-white rounded-3xl py-2 px-4 w-11/12 text-xl text-center"
        :value="sessionStore.walletAddress"
        data-testid="wallet-address"
      />
    </div>
    <div class="flex justify-between items-center m-5">
      <RouterLink
        class="hover:brightness-90 rounded-3xl bg-white border border-black py-1 px-3 text-xl text-monero-grey"
        :to="{ name: 'Error', params: { errorType: 'cancelled' } }"
        data-testid="cancel-transaction-button"
      >
        {{ buttons.cancel }}
      </RouterLink>
      <RouterLink
        class="hover:bg-opacity-75 rounded-3xl bg-monero-orange py-1 px-3 text-xl text-white"
        :to="{ name: 'Payment' }"
        data-testid="continue-transaction-button-wallet"
      >
        {{ buttons.continue }} ({{ seconds }}{{ buttons.seconds }})
      </RouterLink>
    </div>
  </div>
</template>
