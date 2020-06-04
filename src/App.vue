<template>
  <v-app id="inspire">
    <v-card-title>
      <v-toolbar dark class="elevation-2" color="cyan">
        <v-toolbar-title>vuetify-number</v-toolbar-title>
      </v-toolbar>
    </v-card-title>
    <v-card-text>
      <v-container>
        <v-row>
          <v-col>
            <v-select v-model="locale" :items="locales" />
          </v-col>
          <v-col cols="4" sm="4" md="4">
            <VuetifyNumber
              v-model="value"
              v-bind:label="label"
              v-bind:readonly="readonly"
              v-bind:disabled="disabled"
              v-bind:outlined="outlined"
              v-bind:clearable="clearable"
              v-bind:options="options"
            />
            Parent v-model: {{ value }}
            <br />
            Parent v-model: {{ value === null ? "null" : "not null" }}
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
  </v-app>
</template>

<script>
import VuetifyNumber from "./components/VuetifyNumber.vue";

const locales = Intl.NumberFormat.supportedLocalesOf(
  [
    "af-ZA",
    "am-ET",
    "ar-AE",
    "ar-BH",
    "ar-DZ",
    "ar-EG",
    "ar-IQ",
    "ar-JO",
    "ar-KW",
    "ar-LB",
    "ar-LY",
    "ar-MA",
    "arn-CL",
    "ar-OM",
    "ar-QA",
    "ar-SA",
    "ar-SY",
    "ar-TN",
    "ar-YE",
    "as-IN",
    "az-Cyrl-AZ",
    "az-Latn-AZ",
    "ba-RU",
    "be-BY",
    "bg-BG",
    "bn-BD",
    "bn-IN",
    "bo-CN",
    "br-FR",
    "bs-Cyrl-BA",
    "bs-Latn-BA",
    "ca-ES",
    "co-FR",
    "cs-CZ",
    "cy-GB",
    "da-DK",
    "de-AT",
    "de-CH",
    "de-DE",
    "de-LI",
    "de-LU",
    "dsb-DE",
    "dv-MV",
    "el-GR",
    "en-029",
    "en-AU",
    "en-BZ",
    "en-CA",
    "en-GB",
    "en-IE",
    "en-IN",
    "en-JM",
    "en-MY",
    "en-NZ",
    "en-PH",
    "en-SG",
    "en-TT",
    "en-US",
    "en-ZA",
    "en-ZW",
    "es-AR",
    "es-BO",
    "es-CL",
    "es-CO",
    "es-CR",
    "es-DO",
    "es-EC",
    "es-ES",
    "es-GT",
    "es-HN",
    "es-MX",
    "es-NI",
    "es-PA",
    "es-PE",
    "es-PR",
    "es-PY",
    "es-SV",
    "es-US",
    "es-UY",
    "es-VE",
    "et-EE",
    "eu-ES",
    "fa-IR",
    "fi-FI",
    "fil-PH",
    "fo-FO",
    "fr-BE",
    "fr-CA",
    "fr-CH",
    "fr-FR",
    "fr-LU",
    "fr-MC",
    "fy-NL",
    "ga-IE",
    "gd-GB",
    "gl-ES",
    "gsw-FR",
    "gu-IN",
    "ha-Latn-NG",
    "he-IL",
    "hi-IN",
    "hr-BA",
    "hr-HR",
    "hsb-DE",
    "hu-HU",
    "hy-AM",
    "id-ID",
    "ig-NG",
    "ii-CN",
    "is-IS",
    "it-CH",
    "it-IT",
    "iu-Cans-CA",
    "iu-Latn-CA",
    "ja-JP",
    "ka-GE",
    "kk-KZ",
    "kl-GL",
    "km-KH",
    "kn-IN",
    "kok-IN",
    "ko-KR",
    "ky-KG",
    "lb-LU",
    "lo-LA",
    "lt-LT",
    "lv-LV",
    "mi-NZ",
    "mk-MK",
    "ml-IN",
    "mn-MN",
    "mn-Mong-CN",
    "moh-CA",
    "mr-IN",
    "ms-BN",
    "ms-MY",
    "mt-MT",
    "nb-NO",
    "ne-NP",
    "nl-BE",
    "nl-NL",
    "nn-NO",
    "nso-ZA",
    "oc-FR",
    "or-IN",
    "pa-IN",
    "pl-PL",
    "prs-AF",
    "ps-AF",
    "pt-BR",
    "pt-PT",
    "qut-GT",
    "quz-BO",
    "quz-EC",
    "quz-PE",
    "rm-CH",
    "ro-RO",
    "ru-RU",
    "rw-RW",
    "sah-RU",
    "sa-IN",
    "se-FI",
    "se-NO",
    "se-SE",
    "si-LK",
    "sk-SK",
    "sl-SI",
    "sma-NO",
    "sma-SE",
    "smj-NO",
    "smj-SE",
    "smn-FI",
    "sms-FI",
    "sq-AL",
    "sr-Cyrl-BA",
    "sr-Cyrl-CS",
    "sr-Cyrl-ME",
    "sr-Cyrl-RS",
    "sr-Latn-BA",
    "sr-Latn-CS",
    "sr-Latn-ME",
    "sr-Latn-RS",
    "sv-FI",
    "sv-SE",
    "sw-KE",
    "syr-SY",
    "ta-IN",
    "te-IN",
    "tg-Cyrl-TJ",
    "th-TH",
    "tk-TM",
    "tn-ZA",
    "tr-TR",
    "tt-RU",
    "tzm-Latn-DZ",
    "ug-CN",
    "uk-UA",
    "ur-PK",
    "uz-Cyrl-UZ",
    "uz-Latn-UZ",
    "vi-VN",
    "wo-SN",
    "xh-ZA",
    "yo-NG",
    "zh-CN",
    "zh-HK",
    "zh-MO",
    "zh-SG",
    "zh-TW",
    "zu-ZA"
  ],
  { localeMatcher: "lookup" }
);

export default {
  name: "app",
  components: {
    VuetifyNumber
  },
  data: () => ({
    value: "-10.0", // "" or "0" or null
    label: "Valor",
    readonly: false,
    disabled: false,
    outlined: true,
    clearable: true,
    // options: {
    //   locale: "pt-BR",
    //   prefix: "R$",
    //   suffix: "",
    //   length: 9,
    //   precision: 2
    // },
    locales,
    locale: "en-CA"
  }),
  computed: {
    options() {
      return {
        locale: this.locale,
        prefix: "",
        suffix: "",
        length: 9,
        precision: 2
      };
    }
  }
};
</script>
