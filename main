import setuptools
import telebot
from telebot import types
import requests
import json
import time

# Import the aliexpress package
import python_aliexpress_api as aliexpress_api

from main import APP_SECRET 

TOKEN = '6822634998:AAGmY_7F372gOUzadzqc_VE8tRDnmnMU-Sc'
APP_KEY = '502588'
APP_SECRET = 'v9vOxRrzy9BKJ5lS80s9Hgfneaz1uW7S'
ali_obj = aliexpress_api.Aliexpress(APP_KEY, APP_SECRET)  # initializing API

bot = telebot.TeleBot(TOKEN)

CURRENCY = 'USD' # default currency

user_agent = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36"

def format_response(response):
    if response.status_code == 200:
        content = json.loads(response.content)
        return content
    else:
        return None

def get_product_info(product_link):
    product_id = product_link.split('.html?')[0].split('/')[-1]  # Considering the product link format has id before .html
    product_info = ali_obj.get_product_details(product_id)
    return product_info

@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
    bot.send_message(message.chat.id, "مرحبا بك في البوت، يمكنك إرسال رابط AliExpress وسأقدم لك الخصم والعمولة!")

@bot.message_handler(content_types=["text"])
def echo_handler(message):
    product_info = get_product_info(message.text)

    # save and check the cache
    product_info = get_product_info_from_cache(message.text)
    if product_info is None:
        product_info = get_product_info(message.text)
        save_product_info_to_cache(product_info)

    # use json to store product data
    product_info = json.dumps(product_info)

    bot.send_message(message.chat.id, """
**معلومات عن المنتج**

* اسم المنتج: {product_info.get("title", "")}
* عدد مبيعات المنتج: {product_info.get("sells_number", "")}
* معدل تقييم الإيجابي: {product_info.get("positiveRate", "")}%
* نسبة تخفيض المنتج بالعملات: {product_info.get("coins_discount_percentage", "")}%

**معلومات عن روابط المنتج**

* السعر الحالي (الأصلي): {product_info.get("current_price", "")}
* سعر التخفيض بالعملات: {product_info.get("coins_price", "")}
* رابط العملات: {product_info.get("coins_link", "")}
* سعر السوبر ديلز: {product_info.get("super_deals_price", "")}
* رابط السوبر ديلز: {product_info.get("super_deals_link", "")}
* سعر العرض المحدود: {product_info.get("limited_offer_price", "")}
* رابط العرض المحدود: {product_info.get("limited_offer_link", "")}
* سعر تخفيض محتمل: {product_info.get("other_discount_price", "")}
* رابط تخفيض محتمل: {product_info.get("other_discount_link", "")}

* اسم شركة الشحن: {product_info.get("shipping_provider_name", "")}
* عمولة الشحن: {product_info.get("shipping_fees", "")}
    """)

# Add missing function definitions here
def get_product_info_from_cache(product_link):
    # Check cache for product info
    cache = {}
    return cache.get(product_link)

def generate_affiliate_link(product_info, currency):
    # Placeholder function for generating an affiliate link. You need to replace this with your own function.
    return "placeholder_affiliate_link"

def calculate_discount(product_info):
    # Placeholder function for calculating a discount. You need to replace this with your own function.
    return 0

def save_product_info_to_cache(product_info):
    # Placeholder function for saving product info to cache. You need to replace this with your own function.
    pass

bot.polling()
