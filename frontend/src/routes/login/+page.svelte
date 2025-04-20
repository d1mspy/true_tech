<script lang="ts">
	// ID вашего OAuth-приложения в Яндексе
	const YANDEX_CLIENT_ID = 'ваш_client_id';
	const REDIRECT_URI = 'http://localhost:3000/auth'; // URI перенаправления

	let isAuthenticating = false;

	// Функция для авторизации через Яндекс
	function authWithYandex() {
		isAuthenticating = true;
		const authUrl = `https://oauth.yandex.ru/authorize?response_type=token&client_id=${YANDEX_CLIENT_ID}&redirect_uri=${REDIRECT_URI}`;
		window.location.href = authUrl;
	}
</script>

<div class="login-page">
	<div class="login-box">
		<h1>Вход через Яндекс</h1>
		<button on:click={authWithYandex} disabled={isAuthenticating} class="yandex-auth-btn">
			{isAuthenticating ? 'Авторизация...' : 'Войти с Яндекс ID'}
		</button>
	</div>
</div>

<style>
	:global(body) {
		margin: 0;
		font-family: 'Roboto', sans-serif;
	}

	.login-page {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		background-color: #f5f5f5;
	}

	.login-box {
		background: white;
		padding: 2rem;
		border-radius: 8px;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
		text-align: center;
		width: 350px;
	}

	h1 {
		color: #333;
		margin-bottom: 2rem;
	}

	.yandex-auth-btn {
		background-color: #ffcc00;
		color: #000;
		border: none;
		padding: 12px 24px;
		border-radius: 4px;
		font-size: 1rem;
		cursor: pointer;
		transition: background-color 0.3s;
		width: 100%;
	}

	.yandex-auth-btn:hover {
		background-color: #ffdb4d;
	}

	.yandex-auth-btn:disabled {
		background-color: #ccc;
		cursor: not-allowed;
	}
</style>