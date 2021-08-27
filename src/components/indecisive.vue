<template>
  <figure class="absolute h-screen" v-if="image">
    <img :src="image" alt="bg" class="w-screen h-screen" />
  </figure>
  <div class="absolute top-0 bottom-0 left-0 right-0 bg-dark"></div>
  <section class="relative z-10 h-screen">
    <div
      class="
        relative
        flex flex-col
        justify-center
        w-full
        pt-16
        px-2
        mx-auto
        sm:w-[65%]
        md:w-[45%]
        lg:w-[35%]
      "
    >
      <input
        type="text"
        class="flex-1 w-full px-4 py-2 text-base text-black placeholder-gray-400 border-0 border-gray-300 rounded-lg shadow-sm outline-none  dark:text-white focus:ring-2 focus:ring-purple-500 dark:bg-gray-800"
        name="name"
        placeholder="Your question"
        v-model="question"
      />
    </div>
    <p class="py-2 text-3xl text-center">
      Remenber to finish with a sign of interrogaton (?)
    </p>
    <div
      class="relative flex flex-col items-center justify-center text-center  h-80"
      v-show="isValidQuestion"
    >
      <h2 class="text-2xl">
        {{ question }}
      </h2>
      <h1 class="text-4xl font-semibold">{{ answer }}!</h1>
    </div>
  </section>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import { watch } from "@vue/runtime-core";

const question = ref("");
const answer = ref("");
const image = ref("");
const isValidQuestion = ref(false);

const getAnswer = async () => {
  answer.value = `Thinking about it...`;
  const url = `https://yesno.wtf/api`;
  const res = await fetch(url);
  const data = await res.json();
  console.log(data);
  return data;
};

watch(question, (question, previousQuestion) => {
  isValidQuestion.value = false;
  if (!question.includes("?")) return;

  isValidQuestion.value = true;

  getAnswer().then((data) => {
    answer.value = data.answer.toUpperCase();
    image.value = data.image;
  });
});
</script>

<style></style>
