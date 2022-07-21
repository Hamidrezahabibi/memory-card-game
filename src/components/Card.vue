<template>
    <div class="card" @click="selectCard" >
        <div v-if="visible" class="card-face is-front">
            <img :src="`../../public/image/${value}.jpg`" :alt="value" class="image"/>
            <img v-if="matched" src="../../public/image/checkmark.svg" class="checkmark-icon"/>
        </div>
        <div v-else class="card-face is-back">
        </div>
    </div>
</template>


<script>
export default {
    props: {
        matched: {
         type: Boolean,
         default: false
        },
        position: {
            type: Number,
            required: true
        },
        value: {
            type: String,
            required: true
        },
        visible: {
         type: Boolean,
         default: false
        }
    },
       

    setup(props, context){
        const selectCard = () => {
            context.emit('select-card', {
                position: props.position,
                faceValue: props.value
            })
        }
        return {
            selectCard
        }
    }   
}
</script>


<style>
.card {
    position: relative;
}
.card-face {
    width: 100%;
    height: 100%;
    position:absolute;
    border-radius: 10px;
}
.card-face.is-front {
    background-color: red;
    color:white;
}
.card-face.is-back {
    background-image: url('../../public/image/bg.jpg');
    color:white;
}
.checkmark-icon {
    position: absolute;
    right: 5px;
    bottom: 5px;
}
.image {
    width: 125px;
    height: 125px;
    justify-content: center;
    border-radius: 10px;
}
</style>