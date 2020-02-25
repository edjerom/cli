<template>
	<div class="form-div card-body">
		<h3>Контракты</h3>
		<div>
			<div v-for="tr in contracts" :key="tr">{{tr}}</div>
		</div>

		<div>
			<h3>Тест скорости выполнения контрактов</h3>
			<div class="btn btn-sm btn-primary" @click="run_test1()">Запустить</div>
			<span>{{tr_num}}</span>
		</div>
	</div>
</template>

<script>
module.exports = {
	created() {
		
	},

	data() {
		return {
			enabled: false,
			tr_num: 0
			// v: 100,
			// transactions: [],
			// contracts: [],
			// contract_code: "",
			// created_contract: "",
			// created_transaction: "",
			// cid: "",
			// method: "",
			// params: ""
		};
	},

	methods: {
		run_test1(){
			this.tr_num = 0
			axios
				.post(this.$conf.api_url + "call_contract", {params: {cid: "6036dd1afc8935780001582632371402000004", method: "reada", args: ""}})
				.then(res => {
					this.tr_num++;
				});
				
				this.enabled = true
				setTimeout(() => this.enabled = false, 3000)
		},

		load_contracts() {
			console.log("LOADING contracts");
			axios
				.post(this.$conf.api_url + "list_contracts")
				.then(res => (this.contracts = res.data));
		},

		load_transactions() {
			axios.post(this.$conf.api_url + "list_transactions").then(res => {
				console.log(res);
				this.transactions = res.data;
			});
		},

		create_contract() {
			axios
				.post(this.$conf.api_url + "create_contract", {
					params: { code: this.contract_code }
				})
				.then(res => {
					this.created_contract = res.id;
					this.load_contracts();
				});
			// alert(this.contract_code);
		},

		execute_contract() {
			axios
				.post(this.$conf.api_url + "call_contract", {
					params: { cid: this.cid, method: this.method, args: this.params }
				})
				.then(res => {
					console.log(res);
					this.load_transactions();
				});
		}
	}
};

// alert();
</script>
