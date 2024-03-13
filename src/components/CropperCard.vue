<script setup>
import { defineProps, defineEmits, onMounted, ref, watch } from 'vue';

const props = defineProps({
    imageSrc: String,
    onFileChanged: Function,
    defaultRotateDegree: {
        type: Number,
        default: 0,
        required: false
    }
})

const emit = defineEmits(["imageRefUpdated", "rotate"])

const imageRef = ref(null)
const rotateDegree = ref(null)

onMounted(() => {
    rotateDegree.value = props.defaultRotateDegree
    emit("imageRefUpdated", imageRef.value)
})

watch(() => props.defaultRotateDegree, newVal => rotateDegree.value = newVal)

const rotate = (e) => {
    const {value} = e.target
    rotateDegree.value = value
    emit("rotate", value)
}
</script>

<template>
    <div class="card card-input mb-3">
        <div class="card-body">
            <h5 class="card-title">Cropper</h5>
            <div class="mb-3">
                <img class="crop-image" ref="imageRef" :src="props.imageSrc" alt="Croppable Image">
                <div class="range-container mt-3">
                    <label for="rotateRange" class="form-label">Rotate: ({{ rotateDegree }})</label>
                    <input type="range" class="mt-3" min="-180" max="180" :value="rotateDegree" id="rotateRange"
                        @input="rotate">
                </div>
            </div>
        </div>
        <div class="card-footer">
            <div>
                <label for="imageInput" class="form-label">Choose an image</label>
                <input type="file" class="form-control-file" @change="props.onFileChanged" id="imageInput">
            </div>
        </div>
    </div>
</template>

<style scoped>
.range-container {
    display: flex;
    flex-direction: column;
}

input[type="range"] {
    width: 100%;
}
</style>