<template>
    <div>
        <NavHeader />
    </div>
    <div>
        <MainPage />
    </div>
    <div>
        <InformationFooter />
    </div>
</template>

<script>
import NavHeader from './components/NavHeader.vue'
import MainPage from './components/MainPage.vue'
import  InformationFooter from './components/InformationFooter.vue'
export default {
    components: {
        NavHeader,
        MainPage,
        InformationFooter
    }
}
</script>
