<script lang="ts">
	let inputValue = '';
	let responseText = '';

	async function sendPost() {
		try {
			const res = await fetch('/api/test', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({ string: inputValue })
			});
			const json = await res.json();
			responseText = JSON.stringify(json);
		} catch (err) {
			responseText = 'Ошибка при POST-запросе';
		}
	}
</script>

<div class="main">
	<h2>Hi, send me a JSON file</h2>
	<div class="input-container">
		<input bind:value={inputValue} placeholder="Введите что-нибудь" />
		<button on:click={sendPost} class="send-button"><img src="./img/send.png" alt="" /></button>
	</div>
</div>


<style>
	.main{
		text-align: center;
		position: relative;
        padding: 3rem;
        text-align: center;
        margin-top: 15rem;
        width: 450px;
        margin-left: auto;
        margin-right: auto;
		font-family: "Roboto", sans-serif;
	}

	h2{
		font-size: 1,2rem;
		color: black;
	}

	.input-container {
		position: relative;
		width: 100%;
		margin-top: 1rem;
	}

	input{
		width: 100%;
		height: 50px;
		background-color: #f3f3f3;
		border-radius: 23px;
		padding: 0 50px 0 20px;
		border: none;
		outline: none;
		font-size: 1rem;
		box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
	}

	.send-button {
		position: absolute;
		right: 1rem;
		top: 50%;
		transform: translateY(-50%);
		background: none;
		border: none;
		cursor: pointer;
		padding: 5px;
	}

	.send-button img {
		width: 24px;
		height: 24px;
	}

</style>