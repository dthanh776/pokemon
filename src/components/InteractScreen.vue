<template>
    <div class="screen">
        <div class="screen__inner"
            :style="{ width: `${((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) * Math.sqrt(cardsContext.length)}px` }">
            <card-flip v-for="(card, index) in cardsContext" :key="index" :ref="`card-${index}`"
                :imgBackFaceUrl="`images/${card}.png`" :card="{ index: index, value: card }" @onFlip="checkRule($event)"
                :cardsContext="cardsContext"></card-flip>
        </div>
    </div>
    <!-- value có thể 2 giá trị giống nhau, index là duy nhất  -->
</template>

<script>
import CardFlip from "./CardFlip.vue";

export default {
    props: {
        cardsContext: {
            //Lấy từ App.vue
            type: Array,
            default: function () {
                return [];
            },
        },
    },
    data() {
        return {
            rules: [], // Mảng chỉ có 2 phần tử (2 thẻ bài)
        };
    },
    methods: {
        checkRule(card) {
            console.log(card);
            if (this.rules.length === 2) return false; // Mảng chỉ có 2 phần tử (2 thẻ bài)

            this.rules.push(card); // đưa thẻ vừa click vào mảng

            if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) { // 2 thẻ giống nhau
                // Thêm class 'disabled' vào component Card để card ko bị đóng lại khi giống nhau
                this.$refs[`card-${this.rules[0].index}`][0].onOpenCard();
                this.$refs[`card-${this.rules[1].index}`][0].onOpenCard();
                // reset rules
                this.rules = []

                const disabledElements = document.querySelectorAll('.screen .card.disabled')

                if (disabledElements && disabledElements.length === this.cardsContext.length - 2) {
                    setTimeout(() => {
                        this.$emit('onFinish') // Kết thúc trò chơi
                    }, 920);

                }

            } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) { // 2 thẻ khác nhau

                // đóng 2 thẻ
                setTimeout(() => {
                    this.$refs[`card-${this.rules[0].index}`][0].onCloseCard();
                    this.$refs[`card-${this.rules[1].index}`][0].onCloseCard();

                    // reset rules
                    this.rules = []
                }, 800);
            } else {
                return false;
            }
        },
    },
    components: { CardFlip },
};
</script>

<style lang="css" scoped>
.screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: var(--dark);
    color: var(--light);
}

.screen__inner {
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
}
</style>