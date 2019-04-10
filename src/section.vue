<!-- This is a Vue.js single file component. -->
<!-- Check the Vue.js doc here :  -->
<!-- https://vuejs.org/v2/guide/ -->

<!-- This is your HTML -->
<template>
    <div class="isometric_A">
        <!-- wwManager:start -->
        <wwSectionEditMenu :sectionCtrl="sectionCtrl"></wwSectionEditMenu>
        <!-- wwManager:end -->
        <wwObject class="background" :ww-object="section.data.background" ww-category="background"></wwObject>

        <div class="container" :style="containerHeight">
            <div class="half left">
                <wwObject class="title" :ww-object="section.data.title"></wwObject>
                <wwObject class="description" :ww-object="section.data.text"></wwObject>
            </div>

            <div class="half right">
                <div class="cards-container">
                    <div class="cards-column" :style="getColumnWidth" v-for="columnIndex in getColumnCount" :key="columnIndex">
                        <div class="card hover-transition" v-for="(card, index) in getCards(columnIndex - 1)" :key="card.uniqueId" :data-unique-id="card.uniqueId" @click="openPopup(card)" :class="{'active': activeCardIndex == index + (columnIndex-1)*3}">
                            <img class="card-shadow" :src="require('./assets/isometricweweb_cardshadow.png')">
                            <div class="p p4"/>
                            <div class="p p3"/>
                            <div class="p p2"/>
                            <div class="p p1"/>
                            <wwObject class="preview" :ww-object="card.image" wwNoTwicPics="true"></wwObject>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="flying-popup" v-if="openCard" :class="{'flying': startAnim}">
            <wwObject class="card-placeholder" :ww-object="openCard.image" wwNoTwicPics="true"></wwObject>

            <div class="content">
                <div class="close" v-on:click="closePopup()">
                    <i class="fa fa-times" aria-hidden="true"></i>
                </div>

                <div class="content-container">
                    <wwLayoutColumn tag="div" ww-default="ww-columns" :ww-list="openCard.wwObjects" class="wwobjects-wrapper" @ww-add="add(openCard, $event)" @ww-remove="remove(openCard, $event)">
                        <wwObject v-for="wwObject in openCard.wwObjects" :key="wwObject.uniqueId" :ww-object="wwObject"></wwObject>
                    </wwLayoutColumn>
                </div>
            </div>
        </div>

        <wwObject v-if="openCard" class="flying-card" :ww-object="openCard.image" :style="getCardPos" :class="{'flying': startAnim}" wwNoTwicPics="true"></wwObject>
        <div v-if=" openCard" class="transp-bg-popup" v-on:click="closePopup()"></div>
    </div>
</template>

<!-- This is your Javascript -->
<!-- ✨ Here comes the magic ✨ -->
<script>
export default {
    name: "__COMPONENT_NAME__",
    props: {
        sectionCtrl: Object
    },
    data() {
        return {
            activeCardIndex: 0,
            openCard: null,
            startAnim: false,
            cardPerColumn: 3,
            cardCount: 6,
            containerHeight: "auto"
        };
    },
    computed: {
        section() {
            return this.sectionCtrl.get();
        },
        getColumnCount() {
            return Math.ceil(this.section.data.cards.length / this.cardPerColumn);
        },
        getColumnWidth() {
            return {
                width: Math.floor(100 / this.getColumnCount) + "%"
            };
        },
        getCardPos() {
            let originalImage = this.$el.querySelector('[data-unique-id="' + this.openCard.uniqueId + '"] .preview');

            let pos = originalImage.getBoundingClientRect();
            let elPos = this.$el.getBoundingClientRect();

            let center = {
                x: pos.x + pos.width / 2,
                y: pos.y + pos.height / 2 - elPos.y
            };

            let ratio = this.openCard.image.ratio > 0 ? this.openCard.image.ratio : 16 / 9;

            let width = Math.sqrt(Math.pow(pos.width, 2) / (1 + 1 / Math.pow(ratio, 2)));

            return {
                top: center.y + "px",
                left: center.x + "px",
                width: width + "px"
            };
        },
    },
    created() {
        //Initialize section data
        this.initData();
    },
    mounted() {
        this.init();
    },
    methods: {
        initData() {
            this.section.data.cards = this.section.data.cards || [];


            //Init objects
            let needUpdate = this.migrateCards();

            if (!this.section.data.background) {
                this.section.data.background = wwLib.wwObject.getDefault({
                    type: "ww-color",
                    data: { color: "white" }
                });
                needUpdate = true;
            }
            if (!this.section.data.title) {
                this.section.data.title = wwLib.wwObject.getDefault({
                    type: "ww-text",
                    data: { color: "#58585a" }
                });
                needUpdate = true;
            }
            if (!this.section.data.text) {
                this.section.data.text = wwLib.wwObject.getDefault({
                    type: "ww-text",
                    data: { color: "#58585a" }
                });
                needUpdate = true;
            }

            if (_.isEmpty(this.section.data.cards)) {
                for (let i = 0; i < this.cardCount; i++) {
                    let image = wwLib.wwObject.getDefault({ type: "ww-image" });

                    let background = wwLib.wwObject.getDefault({
                        type: "ww-color",
                        data: { backgroundColor: "#FFFFFF" }
                    });

                    this.section.data.cards.push({
                        uniqueId: wwLib.wwUtils.getUniqueId(),
                        image: image,
                        background: background,
                        wwObjects: []
                    });
                }
                needUpdate = true;
            }
            if (needUpdate) {
                this.sectionCtrl.update(this.section);
            }
        },

        migrateCards() {
            if (this.section.data["image1"]) {
                for (let i = 0; i < this.cardCount; i++) {
                    let image = this.section.data["image" + (i + 1)] || wwLib.wwObject.getDefault({ type: "ww-image" });

                    let logo = this.section.data["logo" + (i + 1)] || wwLib.wwObject.getDefault({ type: "ww-image" });

                    let title = this.section.data["title" + (i + 1)] || wwLib.wwObject.getDefault({ type: "ww-text" });

                    let text = this.section.data["text" + (i + 1)] || wwLib.wwObject.getDefault({ type: "ww-text" });

                    let background = wwLib.wwObject.getDefault({ type: "ww-color", data: { backgroundColor: "#FFFFFF" } });

                    let button1 = this.section.data["button1" + (i + 1)] || wwLib.wwObject.getDefault({ type: "ww-button", data: { color: "#58585a" } });

                    let button2 = this.section.data["button2" + (i + 1)] || wwLib.wwObject.getDefault({ type: "ww-button", data: { color: "#58585a" } });

                    this.section.data.cards.push({
                        uniqueId: wwLib.wwUtils.getUniqueId(),
                        image: image,
                        logo: logo,
                        background: background,
                        title: title,
                        text: text,
                        button1: button1,
                        button2: button2
                    });
                }

                for (let i = 0; i < 6; i++) {
                    delete this.section.data["image" + (i + 1)];
                    delete this.section.data["logo" + (i + 1)];
                    delete this.section.data["title" + (i + 1)];
                    delete this.section.data["text" + (i + 1)];
                    delete this.section.data["button1" + (i + 1)];
                    delete this.section.data["button2" + (i + 1)];
                }
                return true;
            }
            return false;
        },

        setContainerHeight() {
            if (!this.$el) {
                return;
            }

            let pos = this.$el.querySelector(".cards-container").getBoundingClientRect();

            this.containerHeight = {
                height: pos.height + 120 + "px"
            };
        },

        getCards(columnIndex) {
            let cards = [];
            for (let i = columnIndex * this.cardPerColumn; i < columnIndex * this.cardPerColumn + this.cardPerColumn; i++) {
                if (this.section.data && this.section.data.cards && this.section.data.cards[i]) {
                    cards.push(this.section.data.cards[i]);
                }
            }
            return cards;
        },

        init() {
            this.changeActiveCard();
            this.$nextTick(() => {
                this.setContainerHeight();
            });

            window.addEventListener("resize", this.onResize);
        },

        onResize() {
            this.setContainerHeight();
        },

        changeActiveCard() {
            let self = this;

            setTimeout(function () {
                self.activeCardIndex++;
                if (self.activeCardIndex >= self.section.data.cards.length) {
                    self.activeCardIndex = 0;
                }

                self.changeActiveCard();
            }, 3000);
        },

        closePopup() {
            this.startAnim = false;
            this.openCard = null;
        },

        openPopup(card) {
            this.openCard = card;

            this.$nextTick(() => {
                this.startAnim = true;
            });
        },

        /* wwManager:start */
        add(openCard, options) {
            if (_.isEmpty(openCard.wwObjects)) {
                openCard.wwObjects = [];
            }

            openCard.wwObjects.splice(options.index, 0, options.wwObject);

            this.sectionCtrl.update(this.section);
        },
        remove(openCard, options) {
            if (_.isEmpty(openCard.wwObjects)) {
                openCard.wwObjects = [];
            }

            openCard.wwObjects.splice(options.index, 1);

            this.sectionCtrl.update(this.section);
        }
        /* wwManager:end */


    },

    beforeDestroy() {
        window.removeEventListener("resize", this.onResize);
    }
};
</script>

<!-- This is your CSS -->
<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- Add lang="scss" or others to use computed CSS -->
<style scoped lang='scss'>
.background {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
}

.container {
    width: 100%;
    height: 100%;
    position: relative;
    margin: 50px 0 40px 0;
    display: flex;
    flex-direction: column;
    overflow-x: hidden;

    .half {
        padding: 30px;

        &.left {
            .description {
                position: relative;
                margin-top: 10px;
            }
            & > * {
                color: white;
            }
        }

        &.right {
            display: flex;
            align-items: center;
            flex-grow: 1;
        }
    }
}

.cards-container {
    z-index: 2;
    transform: rotateX(45deg) rotateY(0deg) rotateZ(45deg);
    width: 100%;
    transition: transform 0.5s ease;

    .cards-column {
        width: 49%;
        display: inline-block;
        vertical-align: top;

        .card {
            position: relative;
            display: inline-block;
            width: calc(100% - 10px);
            margin: 5px;
            cursor: pointer;

            .card-shadow {
                z-index: -1;
                width: 100%;
                height: 100%;
                position: absolute;
            }

            .preview {
                width: 100%;
            }

            .p {
                position: absolute;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                border-bottom: 2px solid #848484;
                border-right: 2px solid white;
                padding: 0;
                opacity: 0;
            }

            &.hover-transition {
                .preview,
                .p {
                    transition: transform 0.4s ease, opacity 0.4s ease;
                }
            }

            &.active,
            &:hover {
                .preview {
                    transform: translate(-30px, -30px);
                }
                .p {
                    opacity: 1;
                }
                @for $i from 1 through 4 {
                    .p#{$i} {
                        transform: translate(-30px + $i, -30px + $i);
                    }
                }
            }
        }
    }
}

.flying-card {
    z-index: 5;
    position: absolute;
    transition: all 0.7s ease;
    transform: translate(-50%, -50%) rotateX(45deg) rotateY(0deg) rotateZ(45deg);

    &.flying {
        transform: translate(-50%, -100%);
        top: calc(50% - 20px) !important;
        left: 50% !important;
        width: 360px !important;
    }
}

.transp-bg-popup {
    position: absolute;
    transform: translateZ(-999px);
    z-index: 2;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(255, 255, 255, 0.5);
    transition: opacity 0.7s ease;
    opacity: 0;
    pointer-events: all;

    &.flying {
        opacity: 1;
    }
}

.flying-popup {
    z-index: 6;
    position: absolute;
    transform: translate(-50%, -30%);
    top: 50%;
    left: 50%;
    transition: all 0.7s ease;
    opacity: 0;

    &.flying {
        transform: translate(-50%, -50%);
        opacity: 1;
    }

    .content {
        position: relative;
        width: 100%;
        width: 360px;
        height: 280px;

        .content-container {
            position: relative;
            width: 100%;
            height: 100%;
            border-bottom-left-radius: 5px;
            border-bottom-right-radius: 5px;
            overflow: hidden;
            background-color: white;
            box-shadow: 0px 5px 19px -8px rgba(0, 0, 0, 0.75);
        }
    }

    .card-placeholder {
        opacity: 0;
        width: 100%;
    }

    .logo-container {
        z-index: 1;
        position: absolute;
        left: 50%;
        top: 0;
        transform: translate(-50%, -50%);
        width: 70px;
        height: 70px;
    }

    .logo {
        width: 100%;
        /*padding-bottom: 100%;*/
    }

    .title {
        margin-bottom: 10px;
    }

    .buttons-container {
        position: absolute;
        width: calc(100% - 40px);
        left: 20px;
        bottom: 0;

        .button {
            width: 49%;
            padding: 20px 10px;
            text-align: center;
            display: inline-block;
            position: relative;
            cursor: pointer;

            a {
                text-decoration: none;
                color: inherit;
            }
        }
    }

    .close {
        z-index: 1;
        position: absolute;
        right: 0;
        top: 0;
        width: 70px;
        height: 40px;
        cursor: pointer;
        background-color: white;
        transform: translateY(-50%);
        box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.5);
        border-top-left-radius: 20px;
        border-bottom-left-radius: 20px;
        text-align: center;
        padding-top: 7px;
        pointer-events: all;

        i {
            font-size: 25px;
            color: #848484;
        }
    }
}

@media (min-width: 992px) {
    .isometric_A .cards-container {
        transform: translateX(-25%) rotateX(45deg) rotateY(0deg) rotateZ(45deg);
    }

    .container {
        flex-direction: row;

        .half {
            flex-basis: 50%;
        }
    }
}

/* wwManager:start */
.ww-editing {
    .cards-container {
        transform: none !important;
    }

    .card {
        &.active,
        &:hover {
            .preview {
                transform: none !important;
            }
            .p {
                opacity: 0 !important;
            }
            @for $i from 1 through 4 {
                .p#{$i} {
                    transform: none !important;
                }
            }
        }
    }
}
/* wwManager:end */
</style>
