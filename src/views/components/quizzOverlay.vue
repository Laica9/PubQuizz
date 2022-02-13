<template>
   <div class="w-screen h-screen absolute z-30 bg-black bg-opacity-60 flex justify-center items-center">
        <div class="bg-green-700 p-4 text-center text-white">
            <p class="font-bold text-white text-2xl">Finished</p>
            <p class="my-4 font-bold text 3xl"> {{percent}} out of 3</p>
        </div>   
    </div>
</template>

<script>
export default {
    props:['percent'],
    setup() {
    },
}
</script>