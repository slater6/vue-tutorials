<template>
<div class="col-sm-6 col-md-4">
	<div class="panel panel-info">
		<div class="panel-heading">
			<h3 class="panel-title">
          {{ stock.name }}
          <small>({{ stock.price | currency }} | Quantity: {{ stock.quantity}})</small>
        </h3>
		</div>
		<div class="panel-body">
			<div class="pull-left">
				<input type="number" class="form-control" placeholder="Quatity" v-model="quantity">
			</div>
			<div class="pull-right">
				<button @click="sellStock" :disabled="isNotValidQuantity()" class="btn btn-primary">Sell</button>
			</div>
		</div>
	</div>
</div>
</template>

<script>
import {
	mapActions
} from 'vuex';

export default {
	props: ['stock'],
	data() {
		return {
			quantity: 0
		}
	},
	computed: {
		insufficientStocks() {
			return this.quantity > this.stock.quantity;
		}
	},
	methods: {
		...mapActions({
			sellStocks: 'sellStock'
		}),
		sellStock() {
			const order = {
				stockId: this.stock.id,
				stockPrice: this.stock.price,
				quantity: this.quantity
			};

			this.sellStocks(order);

			this.quantity = 0;
		},
		isNotValidQuantity() {
			return this.insufficientStocks || (this.quantity < 1) || (this.quantity % 1 !== 0);
		}
	}
}
</script>
