<script lang="ts">
// @ts-nocheck
import { defineComponent, ref } from "vue";
import MeanigList from "./MeanigList.vue";
import Example from "./Example.vue";
import Synonym from "./Synonym.vue";
import Antonym from "./Antonym.vue";
import ShareIcon from "./Icons/ShareIcon.vue";
import WhatsappIcon from "./Icons/WhatsappIcon.vue";
import InputGroup from "./DaisyUI/InputGroup.vue";

import { generateRandomID } from "../utils";
import { LinkIcon } from "@heroicons/vue/24/solid";

export default defineComponent({
  name: "ResultCard",
  setup() {
    const modalCompartilhar = ref(false);
    const id = ref(generateRandomID());
    const compartilhar = () => {
      modalCompartilhar.value = !modalCompartilhar.value;
    };

    const base_url = import.meta.env.VITE_BASE_URL;

    return {
      compartilhar,
      modalCompartilhar,
      id,
      base_url
    };
  },
  props: {
    result: {
      required: true,
    },
  },
  components: {
    ShareIcon,
    MeanigList,
    Example,
    Synonym,
    Antonym,
    InputGroup,
    LinkIcon,
    WhatsappIcon,
  },
});
</script>
<template>
  <div
    class="rounded-md text-green-kelp-900 bg-green-kelp-400 py-4 px-2 shadow-md"
    v-if="result"
  >
    <div class="flex justify-between">
      <h1 class="text-2xl">{{ result.palavra }}</h1>
      <div class="flex items-center">
        <div>
          <ShareIcon @click="compartilhar" class="w-6" />
          <input
            type="checkbox"
            :checked="modalCompartilhar"
            @click="compartilhar"
            :id="id"
            class="modal-toggle"
          />
          <label :for="id" class="modal cursor-pointer">
            <label class="modal-box relative dark:bg-white">
              <h3 class="text-lg font-bold">Compartilhar esse verbete</h3>
              <p class="space-y-7 py-4">
                <div>
                  <a target="_blank" :href="`https://wa.me?text=Veja só o que a palavra *${result.palavra}* significa em *Nheengatu*: ${base_url}/palavra/${result.slug}`" class=" inline-block border border-green-300 p-2 rounded-full cursor-pointer">
                    <WhatsappIcon class="h-10 w-10 " />
                  </a>
                </div>
                <InputGroup :readonly="true" :value="`${base_url}/palavra/${result.slug}`">
                  <template #icon>
                    <div class="h-full w-full p-2">
                      <LinkIcon />
                    </div>
                  </template>
                </InputGroup>
              </p>
            </label>
          </label>
        </div>
      </div>
    </div>
    <div class="space-x-1 mt-4 ">
      <span class="border border-supernova-700 rounded-md px-2 py-1" v-for="s in result.significados">{{ s.palavra }}</span>
    </div>
    <!-- <h3 class="mt-4 text-2xl font-bold">Significado e definição:</h3> -->
    <!-- <MeanigList mean="{response}" /> -->
    <!-- <h3 class="mt-4 text-2xl font-bold">Exemplo:</h3> -->
    <!-- <Example mean="{response}" /> -->
    <!-- <h3 class="mt-4 text-2xl font-bold">Sinônimo:</h3> -->
    <!-- <Synonym mean="{response}" /> -->
    <!-- <h3 class="mt-4 text-2xl font-bold">Antônimo:</h3> -->
    <!-- <Antonym mean="{response}" /> -->
  </div>
</template>

<style>
.modal-box{
  @apply w-auto;
}
</style>