<template lang="html">
    <input type="tel"
           :value="formattedValue"
           @change="change"
           ref="input"
           :disabled="disabled"
           v-money="{precision, decimal, thousands, prefix, suffix}"
           @keyup.enter="$emit('enter')"
           class="v-money"/>
</template>

<script>
    import money from "./directive";
    import defaults from "./options";
    import {format, unformat} from "./utils";

    export default {
        name : "Money",
        props: {
            value    : {
                required: true,
                type    : [Number, String],
                default : 0
            },
            masked   : {
                type   : Boolean,
                default: false
            },
            precision: {
                type   : Number,
                default: () => defaults.precision
            },
            decimal  : {
                type   : String,
                default: () => defaults.decimal
            },
            thousands: {
                type   : String,
                default: () => defaults.thousands
            },
            prefix   : {
                type   : String,
                default: () => defaults.prefix
            },
            suffix   : {
                type   : String,
                default: () => defaults.suffix
            },
            disabled : {
                type   : Boolean,
                default: false
            }
        },

        directives: {money},

        data() {
            return {
                formattedValue: ""
            };
        },

        watch: {
            value: {
                immediate: true,
                handler(newValue, oldValue) {
                    const formatted = format(newValue, this.$props);

                    if (formatted !== this.formattedValue) {
                        this.formattedValue = formatted;
                    }
                }
            }
        },

        methods: {
            change(evt) {
                this.$emit("input", this.masked ? evt.target.value : unformat(evt.target.value, this.precision));
            },

            focus() {
                this.$refs.input.focus();
            }
        }
    };
</script>
