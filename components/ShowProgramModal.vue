<template>
  <div v-if="show" class="modal fade show" tabindex="-1" style="display: block;" aria-labelledby="settingsModalLabel" aria-modal="true" role="dialog">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="settingsModalLabel">Load Example Program</h5>
          <button type="button" class="close" @click="closeModal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <form>
            <div class="form-group">
              <label for="loadProgram">Load an example program</label>
              <select class="form-control" id="loadProgram" v-model="loadProgram">
                <option value="-1" :selected="loadProgram == -1">--</option>
                <option :value="program.id" v-for="program in programs" :key="program.id">
                  {{ program.name }}
                </option>
              </select>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" @click="closeModal">Close</button>
          <button type="button" class="btn btn-primary" @click="saveSettings">Load</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type {CCOptions} from "~/utils/objects";

const emit = defineEmits(["update:show", "update:options"]);
const props = defineProps(["show", "options"]);

const compilerVersion = ref(props.options.compilerVersion);
const interruptEnable = ref(props.options.interruptEnable);
const interruptPadding = ref(props.options.interruptPadding);
const loadProgram = ref(props.options.loadProgram);

watch(() => props.options, (newOptions) => {
  compilerVersion.value = newOptions.compilerVersion
  interruptEnable.value = newOptions.interruptEnable
  interruptPadding.value = newOptions.interruptPadding
  loadProgram.value = newOptions.loadProgram
}, { deep: true });

function closeModal() {
  emit("update:show", false);
}
function saveSettings() {
  emit("update:options", {
    compilerVersion: compilerVersion.value,
    interruptEnable: interruptEnable.value,
    interruptPadding: interruptPadding.value,
    loadProgram: loadProgram.value,
  } as CCOptions);

  closeModal();
}
</script>

<style scoped>

</style>
