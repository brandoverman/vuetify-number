<template>
  <div>
    <v-text-field
      v-model="cmpValue"
      v-bind:label="label"
      v-bind:readonly="readonly"
      v-bind:disabled="disabled"
      v-bind:outlined="outlined"
      v-bind:clearable="clearable"
      v-bind:backgroundColor="backgroundColor"
      v-bind:prefix="options.prefix"
      v-bind:suffix="options.suffix"
      v-on:keypress="keyPress"
    ></v-text-field>
  </div>
</template>

<script>
export default {
  model: { prop: "value", event: "input" },
  props: {
    value: {
      // type: String,
      type: [String, Number],
      default: "0"
    },
    label: {
      type: String,
      default: "Value"
    },
    readonly: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    outlined: {
      type: Boolean,
      default: true
    },
    clearable: {
      type: Boolean,
      default: true
    },
    backgroundColor: {
      type: String,
      default: "white"
    },
    options: {
      type: Object,
      default: function() {
        return {
          locale: "pt-BR",
          prefix: "",
          suffix: "",
          length: 11,
          precision: 2
        };
      }
    }
  },
  data: () => ({}),
  /*
   v-model="cmpValue": Dessa forma, ao digitar, o valor é atualizado automaticamente no componente pai.
   O valor digitado entra pelo newValue do Set e é emitido para o componente pai.
   the-vue-mask nao funciona corretamente ao incluir valores existentes no componente pai.
  */
  computed: {
    cmpValue: {
      get: function() {
        console.log("cmpValue", this.localeHasLeadingNegativeSign);
        return this.value !== null
          ? this.humanFormat(this.value.toString())
          : null;
      },
      set: function(newValue) {
        this.$emit("input", this.machineFormat(newValue));
      }
    },
    localeHasLeadingNegativeSign() {
      const format = new Intl.NumberFormat(this.options.locale).format(-1);

      return format.startsWith("-");
    }
  },
  methods: {
    humanFormat: function(number) {
      if (isNaN(number)) {
        number = ""; // 0 anteriormente
      } else {
        // number = Number(number).toLocaleString(this.options.locale, {maximumFractionDigits: 2, minimumFractionDigits: 2, style: 'currency', currency: 'BRL'});
        number = Number(number).toLocaleString(this.options.locale, {
          maximumFractionDigits: this.options.precision,
          minimumFractionDigits: this.options.precision
        });
      }

      return number;
    },
    machineFormat(number) {
      if (number) {
        number = this.cleanNumber(number);
        // Ajustar quantidade de zeros à esquerda
        number = number.padStart(parseInt(this.options.precision) + 1, "0");
        // Incluir ponto na casa correta, conforme a precisão configurada
        number =
          number.substring(
            0,
            number.length - parseInt(this.options.precision)
          ) +
          "." +
          number.substring(
            number.length - parseInt(this.options.precision),
            number.length
          );
        if (isNaN(number)) {
          number = null; // "" anteriormente, 0 anteriormente
        }
      } else {
        number = null; // "" anteriormente, 0 anteriormente
      }
      if (this.options.precision === 0) {
        number = this.cleanNumber(number);
      }

      return number;
    },
    // Only allow numeric digits or the leading negative number to be
    // entered.
    keyPress($event) {
      const key = $event.key;

      if (key === "-") {
        if (
          this.localeHasLeadingNegativeSign &&
          $event.target.selectionStart === 0
        ) {
          // do nothing
        } else if (
          !this.localeHasLeadingNegativeSign &&
          $event.target.selectionStart === this.value.length
        ) {
          // do nothing
        } else {
          $event.preventDefault();
        }
      } else {
        if (!key.match(/[0-9]/)) {
          $event.preventDefault();
        }
        if (this.targetLength()) {
          $event.preventDefault();
        }
      }
    },
    // Retira todos os caracteres não numéricos e zeros à esquerda
    cleanNumber: function(value) {
      let result = "";
      if (value) {
        let flag = false;
        let arrayValue = value.toString().split("");
        for (var i = 0; i < arrayValue.length; i++) {
          if (i === 0 && arrayValue[i] === "-") {
            result = "-";
          } else {
            if (this.isInteger(arrayValue[i])) {
              if (!flag) {
                // Retirar zeros à esquerda
                if (arrayValue[i] !== "0") {
                  result = result + arrayValue[i];
                  flag = true;
                }
              } else {
                result = result + arrayValue[i];
              }
            }
          }
        }
      }

      return result;
    },
    isInteger(value) {
      let result = false;
      if (Number.isInteger(parseInt(value))) {
        result = true;
      }
      return result;
    },
    targetLength() {
      if (
        Number(this.cleanNumber(this.value).length) >=
        Number(this.options.length)
      ) {
        return true;
      } else {
        return false;
      }
    }
  }
};
</script>
