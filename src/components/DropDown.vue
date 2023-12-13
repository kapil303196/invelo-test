<template>
    <div class="relative">
        <button @click="toggleDropdown"
            class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-300 transition duration-300 ease-in-out">
            <span v-if="selectedItems.length === 0">{{ allowMultiple ? 'Select Mutliple' : 'Select'}}</span>
            <span v-else>{{ selectedItems.join(', ') }}</span>
            <span class="ml-2 inline-block transform transition-transform duration-300"
                :class="{ 'rotate-180': isOpen }">▼</span>
        </button>
        <transition name="dropdown" @enter="startTransition" @leave="endTransition">
            <div v-if="isOpen"
                class="absolute mt-1 bg-white rounded shadow-lg z-10 w-52 transition duration-300 ease-in-out">
                <ul class="max-h-60 overflow-auto">
                    <li v-for="item in items" :key="item.value" @click="() => selectItem(item)"
                        class="flex items-center px-4 py-2 hover:bg-gray-100 cursor-pointer transition-colors duration-300"
                        :class="{ 'bg-gray-200': isSelected(item) }">
                        <input type="checkbox" class="mr-2" :checked="isSelected(item)" @click.stop>
                        {{ item.title }}
                    </li>
                </ul>
            </div>
        </transition>
    </div>
</template>
  
<script>
import { ref, onMounted, onBeforeUnmount, defineComponent } from 'vue';

export default defineComponent({
    props: {
        items: {
            type: Array,
            required: true
        },
        allowMultiple: Boolean
    },
    setup(props, { emit }) {
        const isOpen = ref(false);
        const selectedItems = ref([]);

        const toggleDropdown = () => {
            isOpen.value = !isOpen.value;
        };

        const selectItem = (item) => {
            if (props.allowMultiple) {
                const index = selectedItems.value.indexOf(item.title);
                if (index === -1) {
                    selectedItems.value.push(item.title);
                } else {
                    selectedItems.value.splice(index, 1);
                }
            } else {
                selectedItems.value = [item.title];
                isOpen.value = false;
            }
            emit('update:selected', selectedItems.value);
        };

        const isSelected = (item) => {
            return selectedItems.value.includes(item.title);
        };

        const handleClickOutside = (event) => {
            if (!event.target.closest('.relative')) {
                isOpen.value = false;
            }
        };

        const startTransition = (el) => {
            el.style.height = '0';
            setTimeout(() => {
                el.style.height = el.scrollHeight + 'px';
            });
        };

        const endTransition = (el) => {
            el.style.height = el.scrollHeight + 'px';
            setTimeout(() => {
                el.style.height = '0';
            });
        };

        onMounted(() => {
            document.addEventListener('click', handleClickOutside);
        });

        onBeforeUnmount(() => {
            document.removeEventListener('click', handleClickOutside);
        });

        return { isOpen, selectedItems, toggleDropdown, selectItem, isSelected, startTransition, endTransition };
    }
});
</script>
  
<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.2s;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

/* Dropdown open/close animation */
.dropdown-enter-active,
.dropdown-leave-active {
    transition: height 0.2s ease;
}

.dropdown-enter-from,
.dropdown-leave-to {
    height: 0;
    overflow: hidden;
}
</style>
  