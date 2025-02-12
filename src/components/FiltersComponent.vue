<template>
	<div class="filters-component">
		<transition name="fade">
			<div v-if="!filters.hideFilters" class="filters">
				<FilterComponent
					v-for="filter in options"
					:key="filter.key"
					:filter="filter"
					v-model="filters[filter.key]"
					@change="updateFilters(filters)" />

				<div class="info" v-if="showInfo">
					<IconComponent
						name="question-circle"
						fill="white"
						v-tippy="{ placement: 'bottom' }"
						:content="'You only need to complete the number of base guns there are for each category to earn the Platinum camouflage challenge. For example, the Assault Rifles requires a total of 8 Gold camouflages to unlock the Platinum camouflage challenge for all weapons in that category. Press this icon to read more.'"
						@click="$router.push('/requirements')" />
					<div class="mobile">
						<IconComponent name="question-circle" fill="white"></IconComponent>
						<p>
							You only need to complete the number of base guns there are for each category to earn
							the Platinum camouflage challenge. For example, the Assault Rifles requires a total of
							8 Gold camouflages to unlock the Platinum camouflage challenge for all weapons in that
							category. Read more
							<router-link to="/requirements">here</router-link>.
						</p>
					</div>
				</div>
			</div>
		</transition>

		<button
			@click="toggleFilters()"
			id="toggle-filter-button"
			v-tippy="{ content: `${filters.hideFilters ? 'Show' : 'Hide'} filters` }">
			<IconComponent v-if="filters.hideFilters" name="filter" />
			<IconComponent v-else name="filter-slash" />
			<span>{{ filters.hideFilters ? 'Show' : 'Hide' }} filters</span>
		</button>
	</div>
</template>

<script>
import { mapState, mapActions } from 'pinia'
import { useStore } from '@/stores/store'

import FilterComponent from '@/components/FilterComponent.vue'

export default {
	components: {
		FilterComponent,
	},

	props: {
		options: {
			type: Array,
			required: true,
		},

		showInfo: {
			type: Boolean,
			default: false,
		},
	},

	computed: {
		...mapState(useStore, ['filters']),
	},

	methods: {
		...mapActions(useStore, ['setFilters', 'storeProgress']),

		toggleFilters() {
			const filters = { ...this.filters }
			filters.hideFilters = !filters.hideFilters
			this.updateFilters(filters)
		},

		updateFilters(filters) {
			this.setFilters(filters)
			this.storeProgress()
		},
	},
}
</script>

<style lang="scss" scoped>
.filters-component {
	align-items: center;
	display: flex;
	margin: 50px 0;

	@media (max-width: $tablet) {
		flex-direction: column-reverse;
		margin-top: 0;
	}

	.filters {
		align-items: center;
		display: flex;
		flex-grow: 1;

		@media (max-width: $tablet) {
			flex-direction: column;
			padding-top: 50px;
			width: 100%;
		}

		.info {
			cursor: pointer;
			margin-left: auto;
			margin-right: 25px;
			transition: $transition;

			@media (max-width: $tablet) {
				margin-right: 0;
				margin-top: 25px;
			}

			.icon-component {
				opacity: 0.5;

				@media (max-width: $tablet) {
					display: none;
				}
			}

			.mobile {
				align-items: center;
				background: darken($elevation-7-color, 10%);
				border-radius: $border-radius;
				color: darken(white, 10%);
				cursor: default;
				display: none;
				margin-top: 15px;
				padding: 25px;
				position: relative;
				text-align: left;

				@media (max-width: $tablet) {
					display: flex;
				}

				.icon-component {
					align-items: center;
					background: $background-color;
					border-radius: 100%;
					display: flex;
					height: 28px;
					justify-content: center;
					left: -10px;
					position: absolute;
					top: -10px;
					transform: scale(1.5);
					width: 28px;

					::v-deep svg {
						position: relative;
						top: -1px;
					}
				}
			}
		}
	}
}

button#toggle-filter-button {
	align-items: center;
	background: $elevation-2-color;
	border: 2px solid $elevation-4-color;
	border-radius: $border-radius;
	color: rgba($text-color, 0.75);
	display: inline-flex;
	font-size: 16px;
	justify-content: center;
	padding: 15px;
	width: 100%;

	@media (min-width: $tablet) {
		margin-left: auto;
		padding: 3px 5px;
		width: auto;
	}

	.icon-component {
		margin-right: 5px;
		opacity: 0.75;
		width: 18px;

		@media (min-width: $tablet) {
			margin-right: 0;
		}
	}

	span {
		display: block;

		@media (min-width: $tablet) {
			display: none;
		}
	}
}
</style>
