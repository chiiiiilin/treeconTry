<template>
    <div class="progress-bar">
        <div
            class="progress-bar-step"
            v-for="step in 4"
            :class="{ thisStep: currentStep === step }"
            :key="step"
        ></div>
    </div>
    <formInfo v-if="currentStep === 1" @next-step="nextStep" />
    <formSelection
        v-if="currentStep === 2"
        @next-step="nextStep"
        @prev-step="prevStep"
    />
    <formResult v-if="currentStep === 3" @prev-step="prevStep" />
</template>

<script>
import formInfo from "@/components/treecon-form/form-info.vue";
import formSelection from "@/components/treecon-form/form-selection.vue";
import formResult from "@/components/treecon-form/form-result.vue";

export default {
    components: {
        formInfo,
        formSelection,
        formResult,
    },
    data() {
        return {
            currentStep: 1,
        };
    },
    methods: {
        nextStep() {
            this.currentStep++;
        },
        prevStep() {
            this.currentStep--;
        },
    },
};
</script>

<style lang="scss" scoped>
.progress-bar {
    display: flex;
    justify-content: center;
    position: relative;

    &::before {
        content: "";
        width: 80px;
        height: 5px;
        position: absolute;
        background-color: #f7e6cd;
        top: 50%;
        transform: translateY(-50%);
        z-index: -1;
    }
    .progress-bar-step {
        width: 20px;
        height: 20px;
        background-color: #f7e6cd;
        border-radius: 50px;
        margin: 5px;
    }
    .thisStep {
        background-color: #f3c580;
    }
}
</style>
