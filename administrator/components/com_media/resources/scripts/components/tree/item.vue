<template>
    <li class="media-tree-item" :class="{active: isActive}">
        <a @click.stop.prevent="toggleItem()">
            <span class="item-icon"><span :class="iconClass"></span></span>
            <span class="item-name">{{ item.name }}</span>
        </a>
        <transition name="slide-fade">
            <media-tree v-if="hasChildren" v-show="isOpen" :root="item.path"></media-tree>
        </transition>
    </li>
</template>

<script>
    export default {
        name: 'media-tree-item',
        props: ['item'],
        computed: {
            /* Whether or not the item is active */
            isActive () {
                return (this.item.path === this.$store.state.selectedDirectory);
            },
            /* Whether or not the item is open */
            isOpen () {
                return this.$store.state.selectedDirectory.includes(this.item.path);
            },
            /* Whether or not the item has children */
            hasChildren() {
                return this.item.directories.length > 0;
            },
            iconClass() {
                return {
                    fa: true,
                    'fa-folder-o': !this.isOpen,
                    'fa-folder-open-o': this.isOpen,
                }
            }
        },
        methods: {
            /* Toggle an item open state */
            toggleItem () {
                this.$store.dispatch('getContents', this.item.path);
            }
        }
    }
</script>
