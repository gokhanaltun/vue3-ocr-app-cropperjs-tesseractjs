<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    recognizeButtonClick: Function,
    processLabelData: String
})
</script>

<template>
    <div class="card card-input mb-3">
        <div class="card-body">
            <h5 class="card-title">Preview</h5>
            <div id="preview" class="preview"></div>
        </div>
        <div class="card-footer">
            <div>
                <button id="cropButton" class="btn btn-primary btn-lg" @click="props.recognizeButtonClick">
                    Recognize
                </button>
                <span>&nbsp;&nbsp;</span>
                <label for="cropButton" class="form-label">{{ props.processLabelData }}</label>

            </div>
        </div>
    </div>
</template>
