<template>
  <div id="app">
    <div class="p-3">
      <div class="text-center">
        <span>
          <input type="radio" id="optionSync" value="synchronous" name="renderOption" v-model="optionValue" />
          Synchronous
        </span>
        <span>
          <input type="radio" id="optionDebounced" value="debounced" name="renderOption" v-model="optionValue" />
          Debounced
        </span>
        <span>
          <input disabled type="radio" id="optionAsynchronous" value="asynchronous" name="renderOption"
            v-model="optionValue" />
          Asynchronous
        </span>
      </div>
      <div>
        <input class="border rounded w-full" type="text" v-model="inputValue"
          placeholder="longer you type => the more node it will generate" />
      </div>
      <div>
        current value:
        <span>{{ optionValue === 'debounced' ? debouncedInputValue : inputValue }}</span> =>
        <span>{{ optionValue }}</span>
      </div>
      <div class="container flex flex-wrap">
        <PlotChat class="basis-1/2 overflow-hidden"
          :input="optionValue === 'debounced' ? debouncedInputValue : inputValue" />
        <LineChat class="basis-1/2 overflow-hidden"
          :input="optionValue === 'debounced' ? debouncedInputValue : inputValue" />
        <AreaChat class="basis-1/1 overflow-hidden"
          :input="optionValue === 'debounced' ? debouncedInputValue : inputValue" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref, watch } from "vue"
import debounce from "lodash/debounce";

const inputValue = ref("");
const debouncedInputValue = ref("");
const optionValue = ref < "synchronous" | "debounced" | "asynchronous" > ("synchronous")

const debouncedInputHandler = debounce((newValue: string) => {
  debouncedInputValue.value = newValue
}, 500);

watch([inputValue], () => {
  debouncedInputHandler(inputValue.value);
})

</script>

<style>
.container {
  background-color: black;
}
</style>