<script setup>
import { onMounted, reactive } from 'vue';
import CropperCard from './CropperCard.vue';
import PreviewCard from './PreviewCard.vue'
import OutputCard from './OutputCard.vue'
import Cropper from 'cropperjs';
import "cropperjs/dist/cropper.css"
import { createWorker } from 'tesseract.js';


const imageSrc = "/image.jpg"

const state = reactive({
    imageRef: null,
    cropperInstance: null,
    process: "",
    output: "",
    defaultRotation: -21
})

onMounted(() => {
    state.cropperInstance = new Cropper(state.imageRef,
        {
            preview: ".preview",
            dragMode: "move",
            rotatable: true,
        },

    );

    setTimeout(() => { state.cropperInstance.rotateTo(state.defaultRotation) }, 2000)
})

const onImageRefUpdated = (ref) => { state.imageRef = ref }

const onFileChanged = (e) => {
    const file = e.target.files[0]
    const reader = new FileReader();
    reader.onload = () => {
        state.cropperInstance.replace(reader.result)
        state.defaultRotation = 0
    };

    reader.readAsDataURL(file);
}

const onRotate = (degree) => {
    state.cropperInstance.rotateTo(degree)
}

const recognize = () => {
    state.cropperInstance.getCroppedCanvas().toBlob((blob) => {
        (async () => {
            const worker = await createWorker(undefined, 1, {
                logger: log => state.process = log.status
            })
            const ret = await worker.recognize(URL.createObjectURL(blob));
            state.output = ret.data.text
            state.process = "Done"
            await worker.terminate();
        })();
    })
}
</script>

<template>
    <div class="container-fluid">
        <div class="row justify-content-center mt-5">
            <!-- Cropper Card -->
            <div class="col-lg-6">
                <CropperCard @imageRefUpdated="onImageRefUpdated" :imageSrc="imageSrc" :onFileChanged="onFileChanged"
                    @rotate="onRotate" :defaultRotateDegree="state.defaultRotation" />
            </div>
            <!-- Preview Card -->
            <div class="col-lg-6">
                <PreviewCard :processLabelData="state.process" :recognizeButtonClick="recognize" />

            </div>
            <!-- Output Text Card -->
            <div class="col-lg-12 mt-3 mb-5">
                <OutputCard :outputValue="state.output" />
            </div>
        </div>
    </div>
</template>
