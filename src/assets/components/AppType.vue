<script>
export default {
    props: {
        singleType: Object,
        isActive: Boolean,
    },
    data() {
        return {
            img: "/" + this.singleType.img,
            OfflineAudioCompletionEvent: false,
        };
    },
};
</script>
<template>
    <button>
        <!-- <template v-if="active === false"> -->
            <div class="card col-xxl-2 col-xl-3 col-lg-4 col-md-6 col-sm-12" :class="{ active: isActive }">
                <img :src="img" :alt="singleType.name" class="card-image col-xxl-2 col-xl-3 col-lg-4 col-md-6 col-sm-12">
                <div class="content">{{ singleType.name }}</div>
            </div>
        <!-- </template> -->
        <!-- <template v-if="active === true">
            <div class="card col-xxl-2 col-xl-3 col-lg-4 col-md-6 col-sm-12 card-active">
                <img :src="img" :alt="singleType.name" class="card-image  col-xxl-2 col-xl-3 col-lg-4 col-md-6 col-sm-12">
                <div class="content">{{ singleType.name }}</div>
            </div>
        </template> -->
    </button>
</template>
<style lang="scss" scoped>
.card {
    border-radius: 2rem;
    height: 200px;
    width: 150px;
    margin-bottom: 1rem;
    flex: 0 1 auto;

    img {
        background-size: cover;
        height: 70%;
        width: 100%;
        border-top-right-radius: 2rem;
        border-top-left-radius: 2rem;
    }

    .content {
        font-size: 1rem;
        font-weight: 500;
        text-align: center;
        padding-top: 1rem;
    }
}

.card:hover {
    background-color: rgb(55, 54, 60);
    color: white;
}

.active {
    background-color: rgb(55, 54, 60);
    color: white;
}

button {
    border: none;
    background-color: rgb(231, 165, 80);
}
</style>
