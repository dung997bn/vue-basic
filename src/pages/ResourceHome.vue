<template>
<div class="container">
    <Header></Header>
    <div class="row">
        <div class="col-md-4 order-md-2 mb-4">
            <h4 class="d-flex justify-content-between align-items-center mb-3">
                <span class="text-muted">Your Resources</span>
                <span class="badge badge-secondary badge-pill">{{
            resourcesLength
          }}</span>
            </h4>
            <!-- {/* Seach Inputs Starts */} -->
            <resource-search></resource-search>
            <!-- {/* Seach Inputs Ends */} -->

            <!-- {/* Resource List Starts */} -->
            <resource-list :resources="resources" @on-item-click="selectResource" :activeId="activeResource?._id"></resource-list>
            <button @click="addResource" class="btn btn-sm btn-primary">
                Add Resource
            </button>
            <!-- {/* Resource List Ends */} -->
        </div>
        <!-- {/* Update Form Starts */} -->
        <!--   {/* Update Form Ends */} -->
        <!-- {/* Detail View Starts */} -->
        <div class="col-md-8 order-md-1">
            <h4 class="mb-3">
                Update Resource {{ selectedResource?._id }}
                <button v-on:click="toggleView" v-bind:class="`btn btn-sm ${toggleClass}`">
                    {{ isDetailView ? "Update" : "Detail" }}
                </button>
            </h4>
            <resource-detail v-if="isDetailView" :resource="activeResource"></resource-detail>
            <resource-update v-else :resource="activeResource" @on-resource-update="hydrateResources"></resource-update>
        </div>
        <!-- {/* Detail View Ends */} -->
    </div>
</div>
</template>

<script>
import Header from "@/components/Header";
import ResourceSearch from "@/components/ResourceSearch";
import ResourceDetail from "@/components/ResourceDetail";
import ResourceList from "@/components/ResourceList";
import ResourceUpdate from "../components/ResourceUpdate";
import {
    fetchResources
} from "@/actions";

export default {
    components: {
        Header,
        ResourceUpdate,
        ResourceList,
        ResourceDetail,
        ResourceSearch,
    },
    data() {
        return {
            isDetailView: true,
            selectedResource: null,
            resources: [],
        };
    },
    async created() {
        const resources = await fetchResources();
        this.resources = resources;
    },
    computed: {
        //re-render only when dependencies change
        resourcesLength() {
            return this.resources.length;
        },
        toggleClass() {
            return this.isDetailView ? "btn-warning" : "btn-primary";
        },
        hasResource() {
            return this.resourcesLength > 0;
        },
        activeResource() {
            return this.selectedResource || (this.hasResource && this.resources[0]);
        },
    },
    methods: {
        //re-render everty call
        getResourcesLength() {
            return this.resources.length;
        },
        toggleView() {
            this.isDetailView = !this.isDetailView;
        },
        addResource() {
            const id = "_" + Math.random().toString(36).slice(2);
            const type = ["book", "blog", "video"][Math.floor(Math.random() * 3)];
            const newResource = {
                _id: id,
                title: `Resource ${id} Title`,
                description: `Resource ${id} Description`,
                link: "",
                type,
            };

            this.resources.push(newResource);
        },
        selectResource(rc) {
            this.selectedResource = rc;
        },
        hydrateResources(newResource) {

            const index = this.resources.findIndex((r) => r._id === newResource._id);

            this.resources[index] = newResource;
            this.selectedResource = newResource;
        },
    },
};
</script>
