from telegram import Update, ReplyKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

users = {}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = [
        ["💰 Set Budget", "📉 Stop Loss"],
        ["📈 Take Profit", "📊 Status"]
    ]
    reply_markup = ReplyKeyboardMarkup(keyboard, resize_keyboard=True)

    users[update.effective_user.id] = {
        "budget": 0,
        "profit": 0,
        "loss": 0,
        "stoploss": 0,
        "takeprofit": 0
    }

    await update.message.reply_text("Bot Started ✅", reply_markup=reply_markup)

async def status(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user = users[update.effective_user.id]
    await update.message.reply_text(
        f"Budget: {user['budget']}\nProfit: {user['profit']}\nLoss: {user['loss']}"
    )

app = ApplicationBuilder().token("YOUR_BOT_TOKEN").build()

app.add_handler(CommandHandler("start", start))
app.add_handler(CommandHandler("status", status))

app.run_polling()
