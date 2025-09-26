<template>
    <nav class="header">
        <div class="logo">
            <img src="../assets/img/notion.png" alt="">
        </div>
        <div class="items">
            <div class="item" @mouseenter="toggleRotateWithDelay('notion', true)" @mouseleave="toggleRotateWithDelay('notion', false)"
                style="cursor:pointer;">
                Notion
                <img ref="notionImgRef" src="../assets/img/向下箭头.png" alt="">
            </div>
            <div class="item">Mail</div>
            <div class="item">Calendar</div>
            <div class="item">AI</div>
            <div class="item">Enterprise</div>
            <div class="item">Pricing</div>
            <div class="item" @mouseenter="toggleRotateWithDelay('explore', true)" @mouseleave="toggleRotateWithDelay('explore', false)"
                style="cursor:pointer;">
                Explore
                <img ref="exploreImgRef" src="../assets/img/向下箭头.png" alt="">
            </div>
            <div class="item">Request a demo</div>
        </div>
        <div class="login">Log in</div>
        <div class="free">
            <button>Get Notion free</button>
        </div>
    </nav>
    <div class="child" v-show="bol">
        <slot name="Notion"></slot>
        <slot name="Explore"></slot>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const bol = ref(false)
const notionImgRef = ref(null)
const exploreImgRef = ref(null)
const notionRotated = ref(false)
const exploreRotated = ref(false)

function toggleRotate(type, isEntering) {
    if (type === 'notion') {
        notionRotated.value = isEntering
        notionImgRef.value.style.transform = notionRotated.value ? 'rotate(180deg)' : 'rotate(0deg)'
        notionImgRef.value.style.transition = 'transform 0.4s ease'
        bol.value = isEntering 
    }
    if (type === 'explore') {
        exploreRotated.value = isEntering
        exploreImgRef.value.style.transform = exploreRotated.value ? 'rotate(180deg)' : 'rotate(0deg)'
        exploreImgRef.value.style.transition = 'transform 0.4s ease'
        bol.value = isEntering 
    }
}
function perfect(toggleRotate, delay) {
    let timer = null;
    return (type, isEntering) => {
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => {
            toggleRotate(type, isEntering);
        }, delay);
    };
}
function checkvalue(type, isenter) {
    if (type === 'notion') {
        if (isenter) {
            document.querySelector('.Notion').style.display = 'flex'
            console.log(1);
            
        } else {
            document.querySelector('.Notion').style.display = 'flex'
            console.log(2);
        }
    }
    if (type === 'explore') {
        if (isenter) {
            document.querySelector('.Explore').style.display = 'flex'
        } else {
            document.querySelector('.Explore').style.display = 'none'
        }
    }
}

const toggleRotateWithDelay = perfect(toggleRotate, 1000);
</script>
<style scoped>
* {
    overflow-y: hidden;
}

img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.header {
    width: 100%;
    height: 5rem;
    display: flex;
    padding: 1rem 1rem 0.5rem 2rem;
    align-items: center;
    display: fixed;
}

.logo {
    width: 2rem;
    height: 2rem;
    margin-right: 12rem;
}

.items {
    width: 40rem;
    height: 3rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.item>img {
    width: 1rem;
    height: 1rem;
    margin-left: 0.2rem;
    padding-top: 0.2rem;
}

.login {
    width: 4rem;
    height: 2rem;
    font-size: 1.2rem;
    margin-left: 5rem;
    font-weight: 700;
    color: #000;
}

.free>button {
    width: 11rem;
    height: 2.5rem;
    font-size: 1.2rem;
    font-weight: 700;
    color: #ffffff;
    border-radius: 1rem;
    border: 1px solid #000;
    margin-left: 1rem;
    background-color: #000;
}

.child {
    height:auto;
    background-color: #fff;
    margin-left: 7vw;
    margin-right: 15vw;

}
</style>
<style lang="scss"></style>
