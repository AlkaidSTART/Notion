<template>
    <nav class="header">
        <div class="logo">
            <img src="../assets/img/notion.png" alt="">
        </div>
        <div class="items">
            <div class="item" @mouseenter="toggleRotateWithDelay('notion', true)"
                @mouseleave="toggleRotateWithDelay('notion', false)" style="cursor:pointer;">
                Notion
                <img ref="notionImgRef" src="../assets/img/向下箭头.png" alt="">
            </div>
            <div class="item">Mail</div>
            <div class="item">Calendar</div>
            <div class="item">AI</div>
            <div class="item">Enterprise</div>
            <div class="item">Pricing</div>
            <div class="item" @mouseenter="toggleRotateWithDelay('explore', true)"
                @mouseleave="toggleRotateWithDelay('explore', false)" style="cursor:pointer;">
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
    <div class="child" v-show="bol" ref="isref">
        <slot name="Notion" v-if="notionRotated" slot-content="notion"></slot>
        <slot name="Explore" v-if="exploreRotated" slot-content="explore"></slot>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import notion from './notion.vue'
const notionData = ref(null)
const bol = ref(true)
const notionImgRef = ref(null)
const exploreImgRef = ref(null)
const notionRotated = ref(false)
const exploreRotated = ref(false)
function leavingbox(type, isEntering) {
    const leavebox = document.querySelector('.child');
    if (!leavebox) return;

    const sevenVw = window.innerWidth * 0.07;
    // 使用插槽内容容器作为检测目标（需在插槽内容根元素添加slot-content类）
    const slotContentSelector = `.slot-content[slot-content="${type}"]`;

    const handleMouseMove = (e) => {
        // 检查鼠标是否在插槽内容区域内
        const isInSlot = e.target.closest(slotContentSelector) !== null;

        if (e.clientX <= sevenVw && !isInSlot) {
            isLeave(type);
            bol.value = false;
            checkvalue(type, isEntering);
        }
    };

    // 添加持久化mousemove监听（不立即移除）
    leavebox.addEventListener('mousemove', handleMouseMove);

    // 修复mouseleave事件冒泡问题
    leavebox.addEventListener('mouseleave', (e) => {
        // 检查鼠标是否真的离开整个.child区域
        const rect = leavebox.getBoundingClientRect();
        if (!(e.clientX >= rect.left && e.clientX <= rect.right &&
              e.clientY >= rect.top && e.clientY <= rect.bottom)) {
            isLeave(type);
            bol.value = false;
            checkvalue(type, isEntering);
        }
    });

    // 添加组件卸载时的事件清理
    onUnmounted(() => {
        leavebox.removeEventListener('mousemove', handleMouseMove);
    });
}

function toggleRotate(type, isEntering) {
    if (type === 'notion') {
        notionRotated.value = isEntering;
        setRotationStyle(notionImgRef.value, notionRotated.value);
        bol.value = isEntering;
    }
    // 处理Explore类型的旋转和显示控制
    else if (type === 'explore') {
        exploreRotated.value = isEntering;
        setRotationStyle(exploreImgRef.value, exploreRotated.value);
        bol.value = isEntering;
    }
    // 添加默认情况处理
    else {
        console.warn(`未知类型: ${type}`);
    }
}
// 1. 将 perfect 函数移到 toggleRotate 外部
function perfect(toggleRotate, delay) {
    let timer = null;
    return (type, isEntering) => {
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => {
            toggleRotate(type, isEntering);
        }, delay);
    };
}
const isleave = ref(false)
function isLeave(type) {
    if (type === 'notion') {
        notionRotated.value = false;
        setRotationStyle(notionImgRef.value, notionRotated.value);
    }
    else if (type === 'explore') {
        exploreRotated.value = false;
        setRotationStyle(exploreImgRef.value, exploreRotated.value);
    }
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

function setRotationStyle(element, isRotated) {
    if (!element) return;
    element.style.transform = isRotated ? 'rotate(180deg)' : 'rotate(0deg)';
    element.style.transition = 'transform 0.4s ease';
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
    /* display: flex; */
    position: fixed;
    z-index: 9999;
    margin-bottom: 10vw;
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
    height: auto;
    background-color: #fff;
    margin-left: 7vw;
    margin-right: 15vw;
    position: fixed;
    top: 5rem;
}
</style>
<style lang="scss"></style>