<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crypto Shop</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--tg-theme-bg-color, #ffffff);
            color: var(--tg-theme-text-color, #000000);
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
        }
        .product-card {
            background: var(--tg-theme-secondary-bg-color, #f0f0f0);
            padding: 30px;
            border-radius: 20px;
            text-align: center;
            width: 80%;
            max-width: 350px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        .price {
            font-size: 28px;
            font-weight: bold;
            color: #248bcf;
            margin: 20px 0;
        }
        button {
            background-color: var(--tg-theme-button-color, #248bcf);
            color: var(--tg-theme-button-text-color, #ffffff);
            border: none;
            padding: 15px;
            border-radius: 12px;
            font-size: 18px;
            width: 100%;
            font-weight: bold;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <div class="product-card">
        <h1>Premium Доступ</h1>
        <p>Оплата производится в USDT</p>
        <div class="price">2.00 USDT</div>
        <button id="buy-btn">Купить сейчас</button>
    </div>

    <script>
        const tg = window.Telegram.WebApp;
        tg.ready();
        tg.expand();

        document.getElementById('buy-btn').addEventListener('click', async () => {
            // Визуальный отклик в Telegram
            tg.MainButton.setText("Создание счета...");
            tg.MainButton.show();

            try {
                // ВАЖНО: Мы используем вашу ссылку с Render
                const response = await fetch('https://cryptoshop.onrender.com/create-invoice', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' }
                });

                const data = await response.json();

                if (data.pay_url) {
                    // Открываем CryptoBot для оплаты
                    tg.openTelegramLink(data.pay_url);
                } else {
                    tg.showAlert("Ошибка: " + (data.error || "не удалось создать счет"));
                }
            } catch (e) {
                tg.showAlert("Сервер не отвечает. Попробуйте через 30 секунд (сервер просыпается).");
            } finally {
                tg.MainButton.hide();
            }
        });
    </script>
</body>
</html>