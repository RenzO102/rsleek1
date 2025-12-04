import os
from flask import Flask, request
import telebot

# === НАСТРОЙКИ ===
BOT_TOKEN = "8519682015:AAEPlJTEoCiEjxnAh2BKDws1vH9hbv5EZr0"     # вставьте токен от BotFather
CHAT_ID = 371536576            # вставьте ЧИСЛО (без кавычек!)
ESP32_SECRET = "esp32_secret"    # просто пароль, чтобы команда шла только вам

bot = telebot.TeleBot(BOT_TOKEN)
app = Flask(__name__)

# Временная команда, которую ESP32 забирает
command = "none"


@app.route("/", methods=["GET"])
def home():
    return "Server working!", 200


# === сюда ЮKassa будет слать webhook ===
@app.route("/pay_callback", methods=["POST"])
def pay_callback():
    global command
    data = request.json

    # Проверяем что оплата успешная
    if data and data.get("event") == "payment.succeeded":
        command = "start"
        bot.send_message(CHAT_ID, "💰 Оплата прошла! Запускаю массажёр на 15 минут.")
        return "OK", 200

    return "Ignored", 200


# === ESP32 спрашивает команду ===
@app.route("/get_command", methods=["GET"])
def get_command():
    global command
    c = command
    command = "none"       # сбрасываем после выдачи
    return c, 200


if __name__ == "__main__":
    # Render даёт нам переменную среды PORT
    port = int(os.environ.get("PORT", 5000))
    app.run(host="0.0.0.0", port=port)
