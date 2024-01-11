<template>
    <h1 class="text-center">A Classic URL shortener tool!</h1>
    <div class="card m-5">
        <div class="card-header bg-dark text-light">
            <h2 class="card-title">Shorten a long link</h2>
        </div>
        <form @submit.prevent="submitUrl" @keydown="form.onKeydown($event)">
            <div class="card-body">
                <div class="mb-3">
                    <label for="url" class="form-label"
                        >Paste a long URL
                        <span class="text-danger">*</span></label
                    >
                    <input
                        type="text"
                        id="url"
                        v-model="form.url"
                        class="form-control"
                        :class="{ 'is-invalid': form.errors.has('url') }"
                        name="url"
                        placeholder="Example: http://super-long-link.com/shorten-it"
                    />
                    <has-error :form="form" field="url" />
                    <p class="text-danger" v-if="short_url">{{ short_url }}</p>
                </div>
            </div>
            <div class="card-footer d-flex justify-content-between bg-dark">
                <v-button :loading="form.busy" class="btn btn-light"
                    >Save</v-button
                >
                <button
                    type="reset"
                    class="btn btn-secondary ms-auto"
                    @click="form.reset()"
                >
                    Reset
                </button>
            </div>
        </form>
    </div>
</template>

<script>
import toastr from "toastr";
import Form from "vform";
import VButton from "./Button.vue";
import { HasError } from "vform/src/components/bootstrap4";

export default {
    name: "Home",
    components: {
        HasError,
        VButton,
    },
    data() {
        return {
            short_url: "",
            form: new Form({
                url: "",
            }),
        };
    },
    methods: {
        async submitUrl() {
            this.short_url = "";
            await this.form
                .post(window.location.origin + "/api/short/url")
                .then((res) => {
                    this.short_url = res.data.url;
                    toastr.success("URL Created Successfully");
                })
                .catch(() => {
                    toastr.error("Please see error below");
                });

            this.form.reset();
        },
    },
};
</script>
