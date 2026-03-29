<script setup lang="ts">
import DownloadButton from "@/components/document/DownloadButton.vue";
import IconForLevel from "@/components/icons/IconForLevel.vue";
import {computed} from "vue";
import {VerdictCalculator} from "@/Calculator";
import DocumentName from "@/components/document/DocumentName.vue";
import type { IDocumentData} from "@/interfaces";

const props = defineProps<{
  document: IDocumentData,
  fs: string,
  onReference?: (document: IDocumentData) => void,
}>()
const worstLevel = computed(() =>
    VerdictCalculator.getWorstAnnotationLevel(props.document.annotations)
)
</script>

<template>
  <div class="is-flex is-flex-direction-row is-align-items-center" style="gap: 0.5rem;">
    <IconForLevel :level="worstLevel"/>
    <DocumentName :document="document"/>
    <div class="tags mb-0" v-if="document.tags && document.tags.length > 0">
      <span v-for="tag in document.tags" :key="tag" class="tag is-light">{{ tag }}</span>
    </div>
    <DownloadButton type="button" :filename="document.filename" :fs="fs"/>
    <button v-if="onReference" class="button is-small ml-1" type="button"
            @click="onReference(document)">
      Referenzieren
    </button>
  </div>
</template>
