<template>
    <div class="treecon-form">
        <h2>環境資料</h2>
        <div v-for="question in questions" :key="question.id" class="question">
            <h3>{{ question.title }}</h3>
            <ul v-if="question.format === 'single'">
                <li v-for="option in question.options" :key="option.id">
                    <label
                        :class="{
                            checked: answers[question.id] === option.text,
                        }"
                    >
                        <input
                            type="radio"
                            :name="question.id"
                            :value="option.text"
                            v-model="answers[question.id]"
                        />
                        {{ option.text }}
                        <span class="radio-box"></span>
                    </label>
                </li>
            </ul>
            <ul v-if="question.format === 'multiple'">
                <li v-for="option in question.options" :key="option.id">
                    <label
                        :class="{ checked: answers[question.id][option.id] }"
                    >
                        <input
                            type="checkbox"
                            :name="question.id"
                            :value="option.text"
                            v-model="answers[question.id][option.id]"
                        />
                        {{ option.text }}
                        <span class="checkbox-box"></span>
                    </label>
                </li>
            </ul>
        </div>
        <div class="page-btn">
            <button @click="prevStep" class="btn-lowest">上一頁</button>
            <button @click="nextStep" class="btn-primary">下一頁</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            answers: {},
            questions: [
                {
                    id: 1,
                    title: "種植範圍土壤面積(有石塊、其他設施物覆蓋或根系已填滿均不計入面積)",
                    format: "single",
                    options: [
                        {
                            id: 1,
                            text: "種植範圍面積充足或開闊地單一、帶狀樹穴或花台",
                        },
                        {
                            id: 2,
                            text: "面積(大喬木>4m2；中喬木、棕櫚<2.5m2；小喬木<1.5m2)",
                        },
                        {
                            id: 3,
                            text: "面積(大喬木<4m2；中喬木、棕櫚類<2.5m2；小喬木<1.5m2)",
                        },
                    ],
                },
                {
                    id: 2,
                    title: "臨近樹木的公共設施或目標(樹高的 1.5 倍距離內)",
                    format: "multiple",
                    options: [
                        {
                            id: 1,
                            text: "學校",
                        },
                        {
                            id: 2,
                            text: "公園",
                        },
                        {
                            id: 3,
                            text: "人行道",
                        },
                    ],
                },
            ],
        };
    },
    // emits: ["next-step", "prev-step"],
    methods: {
        // submitAnswers() {
        //     console.log(this.answers);
        // },
        nextStep() {
            this.$emit("next-step");
            console.log(this.answers);
        },
        prevStep() {
            this.$emit("prev-step");
        },
    },
    created() {
        this.questions.forEach((question) => {
            if (question.format === "multiple") {
                this.answers[question.id] = [];
            } else if (question.format === "single") {
                this.answers[question.id] = "";
            }
        });
    },
};
</script>

<style lang="scss" scoped>
.treecon-form {
    text-align: left;

    .question {
        margin: 30px 0;
    }
    h3 {
        color: $primary;
    }
    label {
        margin: 15px;
        padding: 10px;
        padding-right: 25px;
        border: solid 1px $secondary;
        border-radius: 5px;
        display: block;
        position: relative;
        transition: 0.5s;

        &::before {
            content: " ";
            width: 0;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            z-index: -1;
            transition: 0.3s;
            background-color: $primary;
        }
    }
    .checked {
        font-weight: 700;
        color: white;
        &::before {
            width: 100%;
        }
    }
    input[type="radio"],
    input[type="checkbox"] {
        display: none;
    }
    .radio-box {
        width: 15px;
        height: 15px;
        border: 0.5px solid $secondary;
        position: absolute;
        right: 10px;
        top: 50%;
        transform: translateY(-50%);
        border-radius: 50px;
        &::before {
            content: "";
            width: 10px;
            height: 10px;
            background-color: white;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            border-radius: 50px;
        }
    }
    .checkbox-box {
        width: 15px;
        height: 15px;
        border: 0.5px solid $secondary;
        position: absolute;
        right: 10px;
        top: 50%;
        transform: translateY(-50%);
        border-radius: 2px;
        &::before {
            content: "✔";
            color: white;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            border-radius: 50px;
        }
    }
}
</style>
