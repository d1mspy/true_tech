<script lang="ts">
	let inputValue = '';
	let messages: Array<{ text: string; isUser: boolean }> = [];
	let chatStarted = false;

	async function sendPost() {
		if (!inputValue.trim()) return;

		messages = [...messages, { text: inputValue, isUser: true }];
		const userMessage = inputValue;
		inputValue = '';

		if (!chatStarted) {
			chatStarted = true;
		}

		try {
			const res = await fetch('/api/test', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({ string: userMessage })
			});
			const json = await res.json();
			messages = [...messages, { text: JSON.stringify(json), isUser: false }];
		} catch (err) {
			messages = [...messages, { text: 'Ошибка при POST-запросе', isUser: false }];
		}
	}

	function handleKeyPress(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			sendPost();
		}
	}
</script>

<div class="container">
	{#if chatStarted}
		<div class="chat-header">
			<h2>Чат</h2>
		</div>
		<div class="messages-area">
			{#each messages as message}
				<div class="message {message.isUser ? 'user' : 'server'}">
					{message.text}
				</div>
			{/each}
		</div>

		<div class="input-container fixed">
			<input
				bind:value={inputValue}
				placeholder="Введите что-нибудь"
				on:keypress={handleKeyPress}
			/>
			<button on:click={sendPost} class="send-btn">
				<img src="./img/send.png" alt="Send" />
			</button>
		</div>
	{:else}
		<div class="welcome-center">
			<h2>Hi, send me a JSON file</h2>
			<div class="input-container centered">
				<input
					bind:value={inputValue}
					placeholder="Введите что-нибудь"
					on:keypress={handleKeyPress}
				/>
				<button on:click={sendPost} class="send-btn">
					<img src="./img/send.png" alt="Send" />
				</button>
			</div>
		</div>
	{/if}
</div>

<style>
	:global(body) {
		margin: 0;
		height: 100vh;
		display: flex;
		flex-direction: column;
		font-family: 'Roboto', sans-serif;
	}

	.container {
		width: 40rem;
		margin: 0 auto;
		padding: 20px;
		box-sizing: border-box;
		height: 100vh;
		display: flex;
		flex-direction: column;
		position: relative;
	}

	.welcome-center {
		flex-grow: 1;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		text-align: center;
		margin-top: -50px;
	}

	.welcome-center h2 {
		font-size: 1.5rem;
		margin: 0;
	}

	.chat-header {
		padding: 1rem 0;
		text-align: center;
	}

	.chat-header h2 {
		font-size: 1.5rem;
		color: black;
		margin: 0;
	}

	.messages-area {
		flex-grow: 1;
		overflow-y: auto;
		padding: 1rem 0;
		margin-bottom: 1rem;
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	.message {
		max-width: 80%;
		padding: 0.75rem 1rem;
		border-radius: 18px;
		word-wrap: break-word;
	}

	.user {
		align-self: flex-end;
		background: #2b91ff;
		color: white;
	}

	.server {
		align-self: flex-start;
		background: #e9e9e9;
		color: black;
	}

	.input-container {
		position: relative;
		width: 100%;
	}

	.input-container.centered {
		width: 100%;
		margin-top: 2rem;
	}

	.input-container.fixed {
		position: sticky;
		bottom: 0;
		background: white;
		padding: 1rem 0;
		margin-top: auto;
	}

	input {
		width: 100%;
		height: 50px;
		background-color: #f3f3f3;
		border-radius: 23px;
		padding-left: 1.25rem;
		border: none;
		outline: none;
		font-size: 1rem;
		box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
	}

	.send-btn {
		position: absolute;
		right: 15px;
		top: 50%;
		transform: translateY(-50%);
		background: none;
		border: none;
		cursor: pointer;
	}

	.send-btn img {
		width: 24px;
		height: 24px;
	}
</style>
