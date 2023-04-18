<template>
    <div class="treecon-form">
        <h2>樹籍資料</h2>
        <ul class="infos" v-for="question in questions" :key="question.id">
            <li v-if="question.format === 'text'">
                <h3>{{ question.title }}</h3>
                <input
                    type="text"
                    v-model="answers[question.id]"
                    :name="question.id"
                />
            </li>
            <li v-if="question.format === 'select'">
                <h3>{{ question.title }}</h3>
                <n-select
                    v-model="answers[question.id]"
                    :options="question.options"
                    filterable
                />
            </li>
            <li v-if="question.format === 'number'">
                <h3>{{ question.title }}</h3>
                <input
                    type="number"
                    v-model="answers[question.id]"
                    :name="question.id"
                />
            </li>
        </ul>
        <h2>重大危害狀況</h2>
        <h3>
            確認樹的目標區有標的物(如行人、物品等)且發生下述重大危害狀況，可免填後續評估項目
        </h3>
        <ul>
            <li v-for="risk in risks" :key="risk.id">
                <label :class="{ checked: isRisk[risk.id] }">
                    <input
                        type="checkbox"
                        :value="risk.text"
                        v-model="isRisk[risk.id]"
                    />{{ risk.text }}
                    <span class="checkbox-box"></span>
                </label>
            </li>
        </ul>
    </div>
    <div class="page-btn">
        <button @click="nextStep" class="btn-primary">下一頁</button>
    </div>
</template>

<script setup>
import { NSelect } from "naive-ui";
</script>
<script>
export default {
    data() {
        return {
            answers: {},
            questions: [
                {
                    id: 1,
                    title: "樹籍編號",
                    format: "text",
                },
                {
                    id: 2,
                    title: "樹種",
                    format: "select",
                    options: [
                        {
                            label: "榕樹",
                            value: "榕樹",
                        },
                        {
                            label: "丁香",
                            value: "丁香",
                        },
                        {
                            label: "南洋杉",
                            value: "南洋杉",
                        },
                    ],
                },
                {
                    id: 3,
                    title: "地點或地址",
                    format: "text",
                },
                {
                    id: 4,
                    title: "胸高直徑",
                    format: "number",
                },
            ],
            risks: [
                {
                    id: "1",
                    text: "確認枯萎範圍超過 80%以上之樹木。",
                },
                {
                    id: "2",
                    text: "樹洞深度超過斷面直徑 2/3 且外殼開放度 1/3 以上。",
                },
            ],
            isRisk: {
                1: false,
                2: false,
            },
        };
    },
    emits: ["next-step"],
    methods: {
        nextStep() {
            this.$emit("next-step");
            console.log(this.answers);
            console.log(this.isRisk);
        },
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
    .infos {
        h3 {
            margin-bottom: 0;
        }
        input[type="text"],
        input[type="number"] {
            caret-color: $secondary;
            outline: none;
            padding: 5px;
            font-size: 14px;
            border-radius: 3px;
            border: solid 1px $secondary;
            width: 90%;
            margin: 5px 15px;

            &:focus {
                border: solid 1.5px $primary;
            }
        }
        .n-select {
            width: 93%;
            margin: 5px 15px;
            .n-base-selection {
                --n-border-active: $primary;
            }
        }
    }
}
</style>
