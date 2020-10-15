<template>
<ul class="list-group mb-3 resource-list">
    <li v-for="resource in resources" v-bind:key="resource._id" @click="onItemClick(resource)" :class="`list-group-item d-flex justify-content-between lh-condensed resource-list-item ${activeItemClass(
        resource
      )}`">
        <div>
            <h6 class="my-0">{{ resource.title }}</h6>
            <small class="text-muted">{{ resource.description }}</small>
        </div>
        <span class="text-muted">{{ resource.type }}</span>
    </li>
</ul>
</template>

<script>
export default {
    props: {
        resources: {
            type: Array,
            default: () => [],
        },
        activeId: String,
    },
    computed: {
        activeItemClass() {
            return (resource) => (resource._id === this.activeId ? "is-active" : "");
        },
    },
    emits: ["on-item-click"],
    methods: {
        onItemClick(resource) {
            this.$emit("on-item-click", resource);
        },
        // activeItemClass(resource) {
        //     return resource._id === this.activeId ? "is-active" : "";
        // },
    },
};
</script>

<style lang="scss" scoped>
.resource-list {
    max-height: 350px;
    overflow: auto;

    &-item {
        cursor: pointer;

        &:hover {
            background-color: #f3e6e6;
        }
    }
}

.is-active {
    background-color: #f3e6e6;
}
</style>
