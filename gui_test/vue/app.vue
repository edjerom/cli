<template>
	<div>
		<h1>МФЦ демо панель</h1>
		<nodes></nodes>

		<div>
			<h3>Тест скорости выполнения контрактов</h3>
			<div class="btn btn-sm btn-primary" @click="run_test1()">Запустить</div>
			<span>{{tr_num}}</span>
			<div>{{enabled}}</div>
		</div>
	</div>
</template>

<script>
module.exports = {
	data() {
		return {
			v: "",
			data: "()",
			txid: "",
			enabled: false,
			tr_num: 0
		};
	},

	created() {},

	methods: {
		_test1() {
			axios
				.post(this.$conf.api_url + "call_contract", {
					params: {
						cid: "6036dd1afc8935780001582632371402000004",
						method: "reada",
						args: ""
					}
				})
				.then(res => {
					this.tr_num++;
					if (this.enabled) this._test1();
				});
		},

		run_test1() {
			this.tr_num = 0;

			this._test1();

			this.enabled = true;
			setTimeout(() => (this.enabled = false), 1000);
		},

		set() {
			axios
				.post(this.$conf.api_url + "call_contract", {
					params: {
						cid: "00155d17c1c099820001574316836970000004",
						method: "set",
						args: this.v
					}
				})
				.then(res => {
					this.txid = res.id;
					this.$refs.trs.load_transactions();
					setTimeout(() => this.$refs.trs.load_transactions(), 3000);
				});
		},
		get() {
			this.data = "Загрузка...";
			axios
				.post(
					this.$conf.api_url +
						"data/00155d17c1c099820001574316836970000004"
				)
				.then(res => {
					this.data = res.data;
				});
		}
	},

	components: {
		nodes: httpVueLoader("vue/nodes.vue"),
		transactions: httpVueLoader("vue/transactions.vue")
	}
};
</script>