<template>
<form>
    <div v-if="alert?.success" class="alert alert-success">
        {{ alert.success }}
    </div>
    <div v-if="alert?.error" class="alert alert-danger">
        {{ alert.error }}
    </div>
    <div class="mb-3">
        <label htmlFor="firstName">Title</label>
        <input v-model="uResource.title" type="text" class="form-control" id="title" placeholder="How to survice in mountains" />
    </div>
    <div class="mb-3">
        <label htmlFor="firstName">Description</label>
        <input type="text" v-model="uResource.description" class="form-control" id="description" placeholder="Just some description" />
    </div>

    <div class="mb-3">
        <label htmlFor="link">Resource Link</label>
        <div class="input-group">
            <input type="text" class="form-control" id="link" v-model="uResource.link" placeholder="Link" />
        </div>
    </div>

    <div class="mb-3">
        <label htmlFor="type">Type</label>
        <div class="input-group">
            <select class="form-control" id="type" v-model="uResource.type">
                <option v-for="type in types" v-bind:key="type" v-bind:value="type">
                    {{ type }}
                </option>
            </select>
        </div>
    </div>

    <hr class="mb-4" />
    <button class="btn btn-primary btn-lg btn-block" type="submit" @click="submitForm">
        Submit
    </button>
</form>
</template>

<script>
import {
    updateResource
} from "@/actions";
export default {
    props: {
        resource: Object,
    },
    data() {
        return {
            uResource: {
                ...this.resource,
            },
            types: ["blog", "video", "book"],
            alert: this.initAlert(),
        };
    },
    watch: {
        resource(newResource) {
            this.uResource = {
                ...newResource,
            };
        },
    },
    emits: ["on-resource-update"],
    methods: {
        initAlert() {
            return {
                success: null,
                error: null,
            };
        },
        setAlert(type, message) {
            this.alert = this.initAlert();
            this.alert[type] = message;

            setTimeout(() => {
                this.initAlert();
            }, 5000);
        },
        async submitForm() {
            try {
                const updatedResource = await updateResource(
                    this.uResource._id,
                    this.uResource
                );
                this.$emit("on-resource-update", updatedResource);
                this.alert.success = "Resouces updated";
            } catch (error) {
                this.setAlert("error", error);
            }
        },
    },
};
</script>
