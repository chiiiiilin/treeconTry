<template>
    <div class="page-btn">
        <button @click="prevStep" class="btn-lowest">上一頁</button>
    </div>
</template>

<script>
export default {
    data() {
        return;
    },
    methods: {
        prevStep() {
            this.$emit("prev-step");
        },
    },
};
</script>
