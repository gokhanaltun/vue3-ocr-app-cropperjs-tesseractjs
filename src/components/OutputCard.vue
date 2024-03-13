<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    outputValue: String
})
</script>

<template>
    <div class="card">
        <div class="card-body">
            <h5 class="card-title">Output Text</h5>
            <textarea id="outputText" :value="outputValue" class="form-control" rows="5" readonly></textarea>
        </div>
    </div>
</template>
