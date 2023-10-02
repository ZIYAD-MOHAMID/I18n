<template>
  <select @change="switchLanguage">
    <option
      v-for="sLocale in supportedLocales"
      :key="`locale-${sLocale}`"
      :value="sLocale"
      :selected="locale === sLocale"
    >
      {{ t(`locale.${sLocale}`) }}
    </option>
  </select>
</template>

<script setup>
import { useRouter } from "vue-router";
const router = useRouter();

import { useI18n } from "vue-i18n";
const { t, locale } = useI18n();

import Tr from "@/i18n/translation";
const supportedLocales = Tr.supportedLocales;

const switchLanguage = async (event) => {
  const newLocale = event.target.value;
  await Tr.switchLanguage(newLocale);

  try {
    await router.replace({ params: { locale: newLocale } });
  } catch (e) {
    console.log(e);
    router.push("/");
  }
};
</script>
