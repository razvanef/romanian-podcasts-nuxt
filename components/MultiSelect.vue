<template>
    <div class="multi-select flex-grow relative items-center border-b border-b-2 border-teal-500 py-1">
        <div class="relative z-10 flex multi-select__input text-left w-full mr-3 py-1 px-2 bg-transparent border-none text-gray-700 leading-tight"
            @click="openSelect = true">
            <div class="flex flex-grow flex-wrap items-center">
                <span v-for="(selection, index) in multipleSelections.slice(0, displaySelections)"
                    :key="index">
                    {{selection.text}}
                </span>
                <small v-if="multipleSelections.length > displaySelections">
                    &nbsp;(+{{multipleSelections.length-1}} others)
                </small>
                <input class="flex-1 min-w-0 focus:outline-none" ref="searchInput"
                    v-model="searchOption"
                    type="text" :placeholder="multipleSelections.length === 0 ? 'Categorii' : ''">
            </div>
            <div class="multi-select__input-icon flex cursor-pointer"
                :class="{'multi-select--active': openSelect}"
                @click.stop="openSelect=!openSelect">
                <div class="text-xl mr-2"
                    v-if="multipleSelections.length > 0"
                    @click.stop="clearSelection()">
                    <fa :icon="['fas', 'times-circle']" />
                </div>
                <div class="text-xl">
                    <fa :icon="['fas', 'caret-down']" />
                </div>
            </div>
        </div>
        <button v-if="openSelect" @click="openSelect=false" class="fixed top-0 right-0 bottom-0 left-0 w-full h-full cursor-default"></button>
        <div class="select-options absolute overflow-auto select-option block w-full shadow-strong mt-1 py-2 border-t-2 border-teal-500 text-left bg-white"
            v-if="options && openSelect">
            <div class="flex py-2 px-4 items-center hover:bg-gray-100 cursor-pointer"
                :key="option.value" v-for="option in filterOptions"
                @click="selectElement(option)">
                <div class="text-xl mr-6">
                    <fa :icon="['fas', 'check-square']" v-if="option.checked" />
                    <fa :icon="['far', 'square']" v-if="!option.checked" />
                </div>
                <div>{{ option.text }}</div>
            </div>
            <div class="px-4" v-if="filterOptions.length === 0">No options to show</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'MultiSelect',
    props: ["options"],
    data: () => ({
        openSelect: false,
        multipleSelections: [],
        displaySelections: 1,
        searchOption: ""
    }),
    computed: {
        filterOptions() {
            const search = this.searchOption
              .toLowerCase()
              .normalize("NFD")
              .replace(/[\u0300-\u036f]/g, "")

            return this.options.filter(option => (
                option.text
                .toLowerCase()
                .normalize("NFD")
                .replace(/[\u0300-\u036f]/g, "")
                .includes(search)
            ))
        }
    },
    methods: {
        selectElement(element) {
            if(typeof element.checked === 'undefined') this.$set(element, 'checked', false);
            element.checked = !element.checked;
            const index = this.multipleSelections.findIndex(v => v.value == element.value)
            if (index >= 0) this.multipleSelections.splice(index, 1)
            else this.multipleSelections.push(element);
            this.$refs.searchInput.focus()
            this.$emit('elementSelected', this.multipleSelections)
        },
        clearSelection() {
            this.multipleSelections = [];
            this.options.map(option => option.checked = false)
            this.$emit('elementSelected', this.multipleSelections)
        }
    }
}
</script>

<style>
    .multi-select__input-icon svg {
        transition: .3s cubic-bezier(.25,.8,.5,1),visibility 0s;
    }
    .multi-select--active svg {
        transform: rotate(180deg)
    }
    .select-options {
        max-height: 300px;
    }
</style>
