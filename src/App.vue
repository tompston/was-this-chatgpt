<script setup lang="ts">
import { ref } from 'vue';

function very_long_response_time(time: number) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('resolved')
    }, time)
  })
}

const response_texts: string[] = [
  "Maybe",
  "Idk, what do you think?",
  "It depends... who's asking?",
  "Hmm, hard to say",
]

/** Get a random message */
const rand_response_txt = () => {
  return response_texts[Math.floor(Math.random() * response_texts.length)]
}

const message_is_shown = ref(false)
const is_fetching = ref(false)
const response_msg = ref("")
const input_txt = ref("")

async function submitText() {
  message_is_shown.value = false
  is_fetching.value = true
  await very_long_response_time(7700)
  is_fetching.value = false
  message_is_shown.value = true
  response_msg.value = rand_response_txt()
}

</script>

<template>
  <div class="bg-zinc-800 text-gray-200 h-full w-full">
    <main class="flex-center w-full h-full">
      <div class="max-width-4">
        <!--  -->
        <div class="opacity-60 fs-9 text-center mb-2 xs:mb-0">
          State of the art Machine Learning AI Model that tests if the text is generated by ChatGPT. 98.1% accuracy.
        </div>
        <div class="flex-center h-full gap-2 xs:flex-col xs:gap-5">
          <div class="grid grid-rows-[1fr_auto] h-full">
            <h3 class="fw-700 mb-3 mt-6 xs:mt-3 leading-none xs:text-center">Was this text generated by ChatGPT?</h3>
            <div class="flex gap-5">
              <button @click="submitText()" id="submit_btn" :disabled="is_fetching|| input_txt.length < 25"
                class="border fw-700 fs-10 py-2 px-5 border-rad-5 transition-opacity disabled:opacity-40">SUBMIT</button>

              <div class="flex-center" v-if="is_fetching">
                <span id="loading_spinner" class="loader w-3 h-3"></span>
              </div>

            </div>
          </div>
          <textarea placeholder="Input text" id="input_text" autofocus type="text" v-model="input_txt"
            class="w-full border border-white/20 placeholder:text-white/60 placeholder:text-base p-3  fs-9 border-rad-4 h-[150px] max-h-[150px] min-h-[150px] transition-colors bg-black/0 focus:border-white/40"></textarea>
        </div>

        <transition name="fade">
          <div id="response_box" class="flex-center relative" v-if="message_is_shown">
            <div
              class="mt-24 absolute text-white disable-text-select bg-emerald-700 py-1 px-7 fw-500 border-rad-4 fs-9">
              {{ response_msg }}
            </div>
          </div>
        </transition>

      </div>
    </main>

    <a aria-label="Link to source code" href="https://github.com/tompston/was-this-chatgpt" target="_blank"
      rel="noopener noreferrer" class="absolute right-5 bottom-5 opacity-20 hover:opacity-80 transition-opacity">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 1024 1024" fill="none">
        <path fill-rule="evenodd" clip-rule="evenodd"
          d="M8 0C3.58 0 0 3.58 0 8C0 11.54 2.29 14.53 5.47 15.59C5.87 15.66 6.02 15.42 6.02 15.21C6.02 15.02 6.01 14.39 6.01 13.72C4 14.09 3.48 13.23 3.32 12.78C3.23 12.55 2.84 11.84 2.5 11.65C2.22 11.5 1.82 11.13 2.49 11.12C3.12 11.11 3.57 11.7 3.72 11.94C4.44 13.15 5.59 12.81 6.05 12.6C6.12 12.08 6.33 11.73 6.56 11.53C4.78 11.33 2.92 10.64 2.92 7.58C2.92 6.71 3.23 5.99 3.74 5.43C3.66 5.23 3.38 4.41 3.82 3.31C3.82 3.31 4.49 3.1 6.02 4.13C6.66 3.95 7.34 3.86 8.02 3.86C8.7 3.86 9.38 3.95 10.02 4.13C11.55 3.09 12.22 3.31 12.22 3.31C12.66 4.41 12.38 5.23 12.3 5.43C12.81 5.99 13.12 6.7 13.12 7.58C13.12 10.65 11.25 11.33 9.47 11.53C9.76 11.78 10.01 12.26 10.01 13.01C10.01 14.08 10 14.94 10 15.21C10 15.42 10.15 15.67 10.55 15.59C13.71 14.53 16 11.53 16 8C16 3.58 12.42 0 8 0Z"
          transform="scale(64)" fill="#ffff" />
      </svg>
    </a>
  </div>

</template>

<style>
.fade-enter-active {
  transition: opacity 7s ease-in;
}

.fade-leave-active {
  transition: opacity 0.13s ease-out;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>