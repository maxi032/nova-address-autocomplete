<template>
    <default-field :field="field">
        <template slot="field">
            <div class="form-group">
                <vue-google-autocomplete
                        ref="address"
                        :id="field.name"
                        class="w-full form-control form-input form-input-bordered"
                        :class="errorClasses"
                        :placeholder="field.name"
                        :country="field.countries"
                        v-on:placechanged="getAddressData">
                </vue-google-autocomplete>
            </div>

            <p v-if="hasError" class="my-2 text-danger">
                {{ firstError }}
            </p>
        </template>
    </default-field>
</template>

<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova'
import VueGoogleAutocomplete from 'vue-google-autocomplete'

export default {
    components: { VueGoogleAutocomplete },

    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],
    data: function () {
        return {
            address: ''
        }
    },
    methods: {
        getAddressData: function (addressData, placeResultData, id) {
            this.address = addressData;
        },
        /*
         * Set the initial, internal value for the field.
         */
        setInitialValue() {
          this.value = this.field.value || ''
        },

        /**
         * Fill the given FormData object with the field's internal value.
         */
        fill(formData) {
          formData.append(this.field.attribute, this.value || '')
        },

        /**
         * Update the field's internal value.
         */
        handleChange(value) {
          this.value = value
        }
    }
}
</script>
