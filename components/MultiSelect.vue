<template>
    <div class="relative items-center border-b border-b-2 border-teal-500 py-2">
        <input class="bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
            type="text" placeholder="Categorii"
            @focus="openSelect = true" @blur="openSelect = false">
        <div class="select-options absolute overflow-auto select-option block w-full shadow-strong mt-2 py-2 border-t-2 border-teal-500 text-left bg-white"
            v-if="options && openSelect">
            <div class="flex py-2 px-4 items-center hover:bg-gray-100 cursor-pointer" :key="option.value" v-for="option in options">
                <input class="leading-tight mr-4" type="checkbox" :value="option.value" />
                <div>{{ option.text }}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'MultiSelect',
    props: ["options"],
    data: () => ({
        openSelect: false
    })
}
</script>

<style>
    .select-options {
        max-height: 300px;
    }
</style>
