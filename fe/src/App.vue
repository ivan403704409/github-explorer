<template>
    <div>
        <menu-openstatehandler></menu-openstatehandler>
        <menu-fullstatehandler></menu-fullstatehandler>
        <div id="menu-overlay"
            @click="closeNavMenu"
        ></div>
        <nav-menu></nav-menu>
        <main-content></main-content>
    </div>
</template>

<script>
import store from './vuex/store';
import { closeNavMenu } from './vuex/actions';

import MenuOpenstatehandler from './components/MenuOpenstatehandler';
import MenuFullstatehandler from './components/MenuFullstatehandler';
import NavMenu from './components/NavMenu';
import MainContent from './components/MainContent';

export default {
    store,
    vuex: {
        actions: {
            closeNavMenu
        }
    },
    components: {
        MenuOpenstatehandler,
        MenuFullstatehandler,
        NavMenu,
        MainContent
    },
}
</script>

<style lang="less">
html {
    overflow: hidden;
}

body {
    font-family: GothamPro;
    -webkit-font-smoothing: antialiased;
    overflow: hidden;
    background-color: #3d536d;
    margin: 0;
}

h1, h2, h3, h4 {
    font-weight: 500;
}

a {
    text-decoration: none;
}

.round-btn {
    width: 120px;
    height: 32px;
    border-radius: 50px;
    border: solid 0.5px #7ed321;
    font-family: GothamPro-Bold;
    font-size: 14px;
    text-align: center;
    color: #ffffff;
    text-transform: uppercase;
    line-height: 32px;
}

.green-btn {
    display: inline-block;
    width: 245px;
    height: 48px;
    line-height: 48px;
    border-radius: 8px;
    background-image: linear-gradient(to bottom, #a5de41, #55b130);
    box-shadow: inset 0 0.5px 0.5px 0 #e2ffce;
    font-family: GothamPro-Bold;
    font-size: 14px;
    letter-spacing: 0.5px;
    text-align: center;
    color: #ffffff !important;
    text-decoration: none !important;
}

#menu-overlay {
    position: fixed;
    top: 0;
    right: 0;
    left: 84%;
    bottom: 0;
    z-index: 3;
    transform: translate3d(1000px, 0, 0);
}

.list-appear, .list-enter {
    opacity: 0.01;
    transition: opacity 0.8s, transform 0.5s;
    &:nth-child(0) { transform: translate3d(0, 100px, 0) }
    &:nth-child(1) { transform: translate3d(0, 200px, 0) }
    &:nth-child(2) { transform: translate3d(0, 300px, 0) }
    &:nth-child(3) { transform: translate3d(0, 400px, 0) }
    &:nth-child(4) { transform: translate3d(0, 500px, 0) }
    &:nth-child(5) { transform: translate3d(0, 600px, 0) }
    &:nth-child(6) { transform: translate3d(0, 700px, 0) }
    &:nth-child(7) { transform: translate3d(0, 700px, 0) }
    &:nth-child(8) { transform: translate3d(0, 800px, 0) }
    &:nth-child(9) { transform: translate3d(0, 900px, 0) }
    &:nth-child(10) { transform: translate3d(0, 1000px, 0) }
}

.list-appear.list-appear-active, .list-enter.list-enter-active {
    opacity: 1;
    transform: translate3d(0, 0, 0) !important;
}
.list-leave {
    opacity: 0;
}

.trasition-wrapper {
    position: relative;
    min-height: 100%;
    z-index: 1;
    .transition-item {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        z-index: 99;
    }
}
</style>
