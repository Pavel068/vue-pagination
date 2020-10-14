<template>
	<div class="pagination">
		<div>Show: {{ ((page - 1) * per_page) + 1 }} - {{ page * per_page < total ? page * per_page : total }} items of
			{{ total }} total
		</div>
		<div class="pagination-buttons">
			<button class="btn" @click="pageUp" :disabled="max_vis_page === pages"> >> </button>
			<button class="btn"
					:class="{'btn-primary': page_ === page}"
					v-for="page_ in showing"
					@click="$emit('update', page_)">{{ page_ }}
			</button>
			<button class="btn" @click="pageDown" :disabled="min_vis_page <= 1"> << </button>
		</div>
	</div>
</template>

<script>
export default {
	name: "Pagination",
	props: ['meta'],
	data() {
		return {
			total_showing_pages: 20,
			min_vis_page: 1,
			max_vis_page: 20
		}
	},
	computed: {
		page() {
			return this.meta.page;
		},
		pages() {
			return this.meta.pages;
		},
		per_page() {
			return this.meta.per_page;
		},
		total() {
			return this.meta.total;
		},
		showing() {
			if (this.pages_array.length < this.total_showing_pages) {
				return this.pages_array;
			} else {
				return this.pages_array.slice(this.min_vis_page - 1, this.max_vis_page);
			}
		},
		pages_array() {
			let pages = [];
			for (let i = 1; i <= this.pages; i++) {
				pages.push(i);
			}
			return pages;
		}
	},
	methods: {
		pageUp() {
			this.min_vis_page += this.total_showing_pages;
			this.max_vis_page += this.total_showing_pages;
			if (this.max_vis_page > this.pages) {
				this.max_vis_page = this.pages;
			}
		},
		pageDown() {
			this.min_vis_page -= this.total_showing_pages;
			this.max_vis_page -= this.total_showing_pages;
		}
	}
}
</script>

<style scoped>
.pagination {
	display: flex;
	flex-direction: column;
	margin-top: 10px;
	padding: 10px;
	border: 1px solid rgba(0, 0, 0, .125);
}

.pagination-buttons > .btn {
	border: 1px solid rgba(0, 0, 0, .125);
}
</style>