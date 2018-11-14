<!-- This is a Vue.js single file component. -->
<!-- Check the Vue.js doc here :  -->
<!-- https://vuejs.org/v2/guide/ -->

<!-- This is your HTML -->
<template>
    <div class="isometric_A">

        <!-- wwManager:start -->
        <wwSectionEditMenu v-bind:sectionCtrl="sectionCtrl"></wwSectionEditMenu>
        <!-- wwManager:end -->

        <wwObject class="background" v-bind:ww-object="section.data.background" ww-category="background"></wwObject>

        <div class="container" v-bind:style="containerHeight">
            <div class="half left">
                <wwObject class="title" v-bind:ww-object="section.data.title"></wwObject>
                <wwObject class="description" v-bind:ww-object="section.data.text"></wwObject>
            </div>

            <div class="half right">
                <div class="cards-container">
                    <div class="cards-column" v-bind:style="getColumnWidth" v-for="columnIndex in getColumnCount" v-bind:key="columnIndex">
                        <div class="card hover-transition" v-for="(card, index) in getCards(columnIndex - 1)" v-bind:key="card.uniqueId" :data-unique-id="card.uniqueId" @click="openPopup(card)" v-bind:class="{'active': activeCardIndex == index + (columnIndex-1)*3}">
                            <img class="card-shadow" :src="require('./assets/isometricweweb_cardshadow.png')" />
                            <div class="p p4" />
                            <div class="p p3" />
                            <div class="p p2" />
                            <div class="p p1" />
                            <wwObject class="preview" v-bind:ww-object="card.image"></wwObject>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="flying-popup" v-if="openCard" v-bind:class="{'flying': startAnim}">

            <wwObject class="card-placeholder" v-bind:ww-object="openCard.image"></wwObject>

            <div class="content">

                <div class="close" v-on:click="closePopup()">
                    <i class="fa fa-times" aria-hidden="true"></i>
                </div>

                <div class="logo-container">
                    <wwObject class="logo" v-bind:ww-object="openCard.logo" ww-category="icon"></wwObject>
                </div>

                <div class="row">
                    <div class="col-xs-12 ">
                        <wwObject class="title" v-bind:ww-object="openCard.title" ww-no-anim="true"></wwObject>
                        <wwObject class="text" v-bind:ww-object="openCard.text" ww-no-anim="true"></wwObject>
                    </div>
                </div>

                <div class="buttons-container">
                    <div class="button">
                        <wwObject class="button-wrapper" v-bind:ww-object="openCard.button1"></wwObject>
                    </div>
                    <div class="button">
                        <wwObject class="button-wrapper" v-bind:ww-object="openCard.button2"></wwObject>
                    </div>
                </div>
            </div>
        </div>

        <wwObject v-if="openCard" class="flying-card" v-bind:ww-object="openCard.image" v-bind:style="getCardPos" v-bind:class="{'flying': startAnim}"></wwObject>
        <div v-if=" openCard" class="transp-bg-popup" v-on:click="closePopup()">
        </div>

    </div>
</template>

<!-- This is your Javascript -->
<!-- ✨ Here comes the magic ✨ -->
<script>
export default {
    name: "isometric_A",
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
        }
    },
    computed: {
        section() {
            return this.sectionCtrl.get();
        },
        getColumnCount() {
            return Math.ceil(this.section.data.cards.length / this.cardPerColumn)
        },
        getColumnWidth() {
            return {
                width: Math.floor(100 / this.getColumnCount) + '%'
            }
        },
        getCardPos() {

            let originalImage = this.$el.querySelector('[data-unique-id="' + this.openCard.uniqueId + '"] .preview')

            let pos = originalImage.getBoundingClientRect();
            let elPos = this.$el.getBoundingClientRect();

            let center = {
                x: pos.x + pos.width / 2,
                y: pos.y + pos.height / 2 - elPos.y
            }

            let ratio = this.openCard.image.ratio > 0 ? this.openCard.image.ratio : 16 / 9;

            let width = Math.sqrt(Math.pow(pos.width, 2) / (1 + 1 / Math.pow(ratio, 2)));

            return {
                top: center.y + 'px',
                left: center.x + 'px',
                width: width + 'px'
            }

        }
    },
    created() {

        //Initialize section data
        this.section.data = !_.isEmpty(this.section.data) ? this.section.data : { "text": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "sans-serif", "size": 1.5, "text": { "fr_FR": "Use it as a gateway to present your story, products or services." }, "align": "", "color": "#FFFFFF", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.9692732985649573, "wwVersion": 3, "altTags": [] }, "isNew": false, "logo1": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": 100, "hidden": true, "content": { "data": { "alt": { "en_GB": "webeo", "fr_FR": "webeo" }, "url": "/img/gyUl8CvgISga5AXN4ZgDiA5jwG27nJSi.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": ["ww-class-img-format-round"], "imgSize": { "h": 812, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.08315566483005599, "wwVersion": 3, "altTags": [] }, "logo2": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": 100, "hidden": true, "content": { "data": { "alt": { "en_GB": "webeo", "fr_FR": "webeo" }, "url": "/img/gyUl8CvgISga5AXN4ZgDiA5jwG27nJSi.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": ["ww-class-img-format-round"], "imgSize": { "h": 812, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.3040312371227889, "wwVersion": 3, "altTags": [] }, "logo3": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": 100, "hidden": true, "content": { "data": { "alt": { "en_GB": "webeo", "fr_FR": "webeo" }, "url": "/img/gyUl8CvgISga5AXN4ZgDiA5jwG27nJSi.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": ["ww-class-img-format-round"], "imgSize": { "h": 812, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.6122771058898919, "wwVersion": 3, "altTags": [] }, "logo4": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": 100, "hidden": true, "content": { "data": { "alt": { "en_GB": "webeo", "fr_FR": "webeo" }, "url": "/img/gyUl8CvgISga5AXN4ZgDiA5jwG27nJSi.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": ["ww-class-img-format-round"], "imgSize": { "h": 812, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.9558762772556122, "wwVersion": 3, "altTags": [] }, "logo5": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": 100, "hidden": true, "content": { "data": { "alt": { "en_GB": "webeo", "fr_FR": "webeo" }, "url": "/img/gyUl8CvgISga5AXN4ZgDiA5jwG27nJSi.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": ["ww-class-img-format-round"], "imgSize": { "h": 812, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.6269368815797671, "wwVersion": 3, "altTags": [] }, "logo6": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": 100, "hidden": true, "content": { "data": { "alt": { "en_GB": "webeo", "fr_FR": "webeo" }, "url": "/img/gyUl8CvgISga5AXN4ZgDiA5jwG27nJSi.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": ["ww-class-img-format-round"], "imgSize": { "h": 812, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.9820230245670014, "wwVersion": 3, "altTags": [] }, "text1": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.9574001980142528, "wwVersion": 3, "altTags": [] }, "text2": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.10067039402342393, "wwVersion": 3, "altTags": [] }, "text3": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.6146005726865036, "wwVersion": 3, "altTags": [] }, "text4": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.6596628889701837, "wwVersion": 3, "altTags": [] }, "text5": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.8998989425671486, "wwVersion": 3, "altTags": [] }, "text6": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.6318415949082363, "wwVersion": 3, "altTags": [] }, "title": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "Oswald Regular", "size": 2.5, "text": { "fr_FR": "This is the isometric section." }, "align": "", "color": "#FFFFFF", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.22125007625782467, "wwVersion": 3, "altTags": [] }, "image1": { "data": { "popup": null }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "alt": { "fr_FR": "" }, "url": "/img/Manhattan.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": [], "imgSize": { "h": 682, "w": 1023 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null, "backgroundColor": "" }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.8617616379442543, "wwVersion": 3, "altTags": [] }, "image2": { "data": { "popup": null }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "alt": { "fr_FR": "" }, "url": "/img/hhee5zfcfJdvjvCSRqnA3A61yj8zeQzC.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": [], "imgSize": { "h": 1000, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null, "backgroundColor": "" }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.7418149627260899, "wwVersion": 3, "altTags": [] }, "image3": { "data": { "popup": null }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "alt": { "fr_FR": "" }, "url": "/img/EmpireStateView.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": [], "imgSize": { "h": 682, "w": 1446 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null, "backgroundColor": "" }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.6586063851114277, "wwVersion": 3, "altTags": [] }, "image4": { "data": { "popup": null }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "alt": { "fr_FR": "" }, "url": "/img/u8fdyaW32v15uu8hJl1QQzENz5dq8HJR.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": [], "imgSize": { "h": 1000, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null, "backgroundColor": "" }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.5919457281651861, "wwVersion": 3, "altTags": [] }, "image5": { "data": { "popup": null }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "alt": { "fr_FR": "" }, "url": "/img/NYCTimesSquare.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": [], "imgSize": { "h": 682, "w": 1035 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null, "backgroundColor": "" }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.8912067321611914, "wwVersion": 3, "altTags": [] }, "image6": { "data": { "popup": null }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "alt": { "fr_FR": "" }, "url": "/img/FeE0M4tmGgWctnzHTaIrYFr8Ms4zC2XW.jpg", "zoom": -1, "hover": { "name": "", "options": {} }, "ratio": -1, "classes": [], "imgSize": { "h": 996, "w": 1500 }, "position": { "x": 0, "y": 0 }, "borderColor": "", "filterGradient": null, "backgroundColor": "" }, "type": "ww-image" }, "children": {}, "paddings": {}, "uniqueId": 0.6685988149836293, "wwVersion": 3, "altTags": [] }, "title1": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "Oswald Regular", "size": 1, "text": { "fr_FR": "De Finibus Bonorum et Malorum" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.12399490082146136, "wwVersion": 3, "altTags": [] }, "title2": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "Oswald Regular", "size": 1, "text": { "fr_FR": "De Finibus Bonorum et Malorum" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.296817677258302, "wwVersion": 3, "altTags": [] }, "title3": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "Oswald Regular", "size": 1, "text": { "fr_FR": "De Finibus Bonorum et Malorum" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.7285783519860707, "wwVersion": 3, "altTags": [] }, "title4": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "Oswald Regular", "size": 1, "text": { "fr_FR": "De Finibus Bonorum et Malorum" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.7366191493957477, "wwVersion": 3, "altTags": [] }, "title5": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "Oswald Regular", "size": 1, "text": { "fr_FR": "De Finibus Bonorum et Malorum" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.8266161063387252, "wwVersion": 3, "altTags": [] }, "title6": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "Oswald Regular", "size": 1, "text": { "fr_FR": "De Finibus Bonorum et Malorum" }, "align": "", "color": "", "classes": [], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.2925712314779394, "wwVersion": 3, "altTags": [] }, "button11": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Learn more" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.914139617092719, "wwVersion": 3, "altTags": [] }, "button12": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Discover" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.5933001497455688, "wwVersion": 3, "altTags": [] }, "button21": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Learn more" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.707749466577253, "wwVersion": 3, "altTags": [] }, "button22": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Discover" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.3821487173548297, "wwVersion": 3, "altTags": [] }, "button31": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Learn more" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.6857558170493421, "wwVersion": 3, "altTags": [] }, "button32": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Discover" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.2774132692680453, "wwVersion": 3, "altTags": [] }, "button41": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Learn more" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.876626437269747, "wwVersion": 3, "altTags": [] }, "button42": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Discover" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.3421607757539924, "wwVersion": 3, "altTags": [] }, "button51": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Learn more" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.17517204722892776, "wwVersion": 3, "altTags": [] }, "button52": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Discover" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.08893594707821695, "wwVersion": 3, "altTags": [] }, "button61": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Learn more" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.032118769879558196, "wwVersion": 3, "altTags": [] }, "button62": { "data": { "popup": null, "children": {} }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "tag": "div", "font": "", "size": "", "text": { "fr_FR": "Discover" }, "align": "", "color": "#676767", "classes": ["ww-class-btn-border-small", "ww-class-btn-padding-small", "ww-class-btn-format-round-medium", "ww-class-btn-shadow-bottom-small", ""], "children": [] }, "type": "ww-text" }, "children": {}, "paddings": {}, "uniqueId": 0.15374293907188696, "wwVersion": 3, "altTags": [] }, "background": { "data": { "popup": null }, "link": { "data": {}, "type": "none" }, "tags": [], "ratio": -1, "hidden": false, "content": { "data": { "backgroundColor": "#000000" }, "type": "ww-color" }, "children": {}, "paddings": {}, "uniqueId": 0.0707738102928468, "wwVersion": 3, "altTags": [] }, "sectionTitle": "Isometrique" };

        if (this.section.data) {
            if (this.section.data.logo1) {
                this.migrateCards();
            }
        }

        this.section.data.cards = this.section.data.cards || [];


        for (let i = this.section.data.cards.length; i < this.cardCount; i++) {
            this.section.data.cards.push({
                uniqueId: wwLib.wwUtils.getUniqueId(),
                image: wwLib.wwObject.getDefault({
                    type: 'ww-image',
                    data: {
                        url: 'http://cdn.wewebapp.io/public/images/weweb-wp.png'
                    }
                }),
                logo: wwLib.wwObject.getDefault({
                    type: 'ww-image',
                    data: {
                        url: 'http://cdn.wewebapp.io/public/images/weweb-wp.png'
                    }
                }),
                title: wwLib.wwObject.getDefault({
                    type: 'ww-text',
                    data: {
                        text: {
                            fr_FR: 'title ! ' + i,
                            en_GB: 'title ! ' + i
                        }
                    }
                }),
                text: wwLib.wwObject.getDefault({
                    type: 'ww-text',
                    data: {
                        text: {
                            fr_FR: 'desc ! ' + i,
                            en_GB: 'desc ! ' + i
                        }
                    }
                }),
                button1: wwLib.wwObject.getDefault({
                    type: 'ww-button'
                }),
                button2: wwLib.wwObject.getDefault({
                    type: 'ww-button'
                })
            });
        }

        this.section.data.background = this.section.data.background || wwLib.wwObject.getDefault({
            type: 'ww-image',
            data: {
                url: 'http://cdn.wewebapp.io/public/images/weweb-wp.png'
            }
        });

        this.section.data.title = this.section.data.title || wwLib.wwObject.getDefault({
            type: 'ww-text',
            data: {
                text: {
                    fr_FR: 'main title ! ',
                    en_GB: 'main title ! '
                }
            }
        });

        this.section.data.text = this.section.data.text || wwLib.wwObject.getDefault({
            type: 'ww-text',
            data: {
                text: {
                    fr_FR: 'main text ! ',
                    en_GB: 'main text ! '
                }
            }
        });


    },
    mounted() {
        this.init();
    },
    methods: {

        migrateCards() {

            this.section.data.cards = [];

            for (let i = 0; i < this.cardCount; i++) {
                let image = this.section.data['image' + (i + 1)] || wwLib.wwObject.getDefault({
                    type: 'ww-image',
                    data: {
                        url: 'http://cdn.wewebapp.io/public/images/weweb-wp.png'
                    }
                });

                let logo = this.section.data['logo' + (i + 1)] || wwLib.wwObject.getDefault({
                    type: 'ww-image',
                    data: {
                        url: 'http://cdn.wewebapp.io/public/images/weweb-wp.png'
                    }
                });

                let title = this.section.data['title' + (i + 1)] || wwLib.wwObject.getDefault({
                    type: 'ww-text',
                    data: {
                        text: {
                            fr_FR: '<b>Titre de la carte</b>',
                            en_GB: '<b>Card Title</b>'
                        }
                    }
                });

                let text = this.section.data['text' + (i + 1)] || wwLib.wwObject.getDefault({
                    type: 'ww-text',
                    data: {
                        text: {
                            fr_FR: 'Texte de la carte',
                            en_GB: 'Card text'
                        }
                    }
                });

                let button1 = this.section.data['button1' + (i + 1)] || wwLib.wwObject.getDefault({
                    type: 'ww-button'
                });

                let button2 = this.section.data['button2' + (i + 1)] || wwLib.wwObject.getDefault({
                    type: 'ww-button'
                });

                this.section.data.cards.push({
                    uniqueId: wwLib.wwUtils.getUniqueId(),
                    image: image,
                    logo: logo,
                    title: title,
                    text: text,
                    button1: button1,
                    button2: button2
                })
            }

            for (let i = 0; i < 6; i++) {

                delete this.section.data['image' + (i + 1)];
                delete this.section.data['logo' + (i + 1)];
                delete this.section.data['title' + (i + 1)];
                delete this.section.data['text' + (i + 1)];
                delete this.section.data['button1' + (i + 1)];
                delete this.section.data['button2' + (i + 1)];

            }
        },

        setContainerHeight() {

            if (!this.$el) {
                return;
            }

            let pos = this.$el.querySelector('.cards-container').getBoundingClientRect();

            this.containerHeight = {
                height: (pos.height + 120) + 'px'
            }

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
            })


            window.addEventListener('resize', this.onResize);
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

        }

    },
    beforeDestroy() {
        window.removeEventListener('resize', this.onResize);
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
        margin-top: 10px;
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
    background-color: white;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
    padding: 45px 10px 10px 10px;
  }

  .card-placeholder {
    opacity: 0;
    width: 100%;
  }

  .logo-container {
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
