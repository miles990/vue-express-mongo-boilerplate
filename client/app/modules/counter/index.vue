<template lang="pug">
	.container
		h2.title {{ "Demo" | i18n }}

		h3 {{ count }}
		button.button.success(@click="inc") 
			span.icon
				i.fa.fa-arrow-up 
			span {{ "Increment" | i18n }}
		br
		br
		button.button.warning(@click="dec") 
			span
				i.fa.fa-arrow-up 
			span {{ "Decrement" | i18n }}

</template>

<script>
	import * as actions from "./vuex/actions";
	import * as getters from "./vuex/getters";

	import Service from "../../core/service";

	export default {
		/**
		 * Set Vuex actions & getters
		 */
		vuex: {
			getters,
			actions
		},	

		/**
		 * Page methods
		 */
		methods: {

			/**
			 * Increment counter
			 */
			inc() {
				this.increment();
			},

			/**
			 * Decrement counter
			 */
			dec() {
				this.decrement();
			}
		},

		/**
		 * Socket handlers. Every property is an event handler
		 */
		socket: {

			prefix: "/counter/",

			//namespace: "/counter",

			events: {
				/**
				 * Counter value is changed
				 * @param  {Number} msg Value of counter
				 */
				changed(res) {
					console.log("Counter changed to " + res.data + (res.user ? " by " + res.user.fullName : ""));
					this.changedValue(res.data);
				}
			}
		},

		created() {
			this.$service = new Service("counter", this); 
			
			// Get the latest value of counter
			this.getValue(); 
		}
	};

</script>

<style lang="sass" scoped>
</style>