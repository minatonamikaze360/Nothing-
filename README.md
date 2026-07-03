# Nothing-
Time sms 
Qk9YQTRSQoN6TnV4WIdReUWXbVJkkWV6YpV1a0ZrjGZDjZd0QVBr

Username: zrtimsms9900
Password: 0zR#9s9M_stm


import requests
import time
import re
from datetime import datetime, timedelta

# ------------------ CONFIG ------------------
API_URL = "http://147.135.212.197/crapi/time/viewstats"
TOKEN = "Qk9YQTRSQoN6TnV4WIdReUWXbVJkkWV6YpV1a0ZrjGZDjZd0QVBr"

TELEGRAM_BOT_TOKEN = "8611026935:AAEGIdy86zQQwcqSny8nPBpH8is3DThpTrU"
TELEGRAM_GROUP_ID = "-1003822404208"

# ------------------ MEMORY ------------------
processed_ids = set()

# ------------------ COUNTRY DATA ------------------
country_data = {
    "1": ("🇺🇸", "US"), "7": ("🇷🇺", "RU"),
    "20": ("🇪🇬", "EG"), "27": ("🇿🇦", "ZA"),
    "30": ("🇬🇷", "GR"), "31": ("🇳🇱", "NL"),
    "32": ("🇧🇪", "BE"), "33": ("🇫🇷", "FR"),
    "34": ("🇪🇸", "ES"), "36": ("🇭🇺", "HU"),
    "39": ("🇮🇹", "IT"), "40": ("🇷🇴", "RO"),
    "41": ("🇨🇭", "CH"), "43": ("🇦🇹", "AT"),
    "44": ("🇬🇧", "GB"), "45": ("🇩🇰", "DK"),
    "46": ("🇸🇪", "SE"), "47": ("🇳🇴", "NO"),
    "48": ("🇵🇱", "PL"), "49": ("🇩🇪", "DE"),
    "51": ("🇵🇪", "PE"), "52": ("🇲🇽", "MX"),
    "53": ("🇨🇺", "CU"), "54": ("🇦🇷", "AR"),
    "55": ("🇧🇷", "BR"), "56": ("🇨🇱", "CL"),
    "57": ("🇨🇴", "CO"), "58": ("🇻🇪", "VE"),
    "60": ("🇲🇾", "MY"), "61": ("🇦🇺", "AU"),
    "62": ("🇮🇩", "ID"), "63": ("🇵🇭", "PH"),
    "64": ("🇳🇿", "NZ"), "65": ("🇸🇬", "SG"),
    "66": ("🇹🇭", "TH"), "81": ("🇯🇵", "JP"),
    "82": ("🇰🇷", "KR"), "84": ("🇻🇳", "VN"),
    "86": ("🇨🇳", "CN"), "90": ("🇹🇷", "TR"),
    "91": ("🇮🇳", "IN"), "92": ("🇵🇰", "PK"),
    "93": ("🇦🇫", "AF"), "94": ("🇱🇰", "LK"),
    "95": ("🇲🇲", "MM"), "98": ("🇮🇷", "IR"),

    # AFRICA FULL
    "211": ("🇸🇸", "SS"), "212": ("🇲🇦", "MA"),
    "213": ("🇩🇿", "DZ"), "216": ("🇹🇳", "TN"),
    "218": ("🇱🇾", "LY"), "220": ("🇬🇲", "GM"),
    "221": ("🇸🇳", "SN"), "222": ("🇲🇷", "MR"),
    "223": ("🇲🇱", "ML"), "224": ("🇬🇳", "GN"),
    "225": ("🇨🇮", "CI"), "226": ("🇧🇫", "BF"),
    "227": ("🇳🇪", "NE"), "228": ("🇹🇬", "TG"),
    "229": ("🇧🇯", "BJ"), "230": ("🇲🇺", "MU"),
    "231": ("🇱🇷", "LR"), "232": ("🇸🇱", "SL"),
    "233": ("🇬🇭", "GH"), "234": ("🇳🇬", "NG"),
    "235": ("🇹🇩", "TD"), "236": ("🇨🇫", "CF"),
    "237": ("🇨🇲", "CM"), "238": ("🇨🇻", "CV"),
    "239": ("🇸🇹", "ST"), "240": ("🇬🇶", "GQ"),
    "241": ("🇬🇦", "GA"), "242": ("🇨🇬", "CG"),
    "243": ("🇨🇩", "CD"), "244": ("🇦🇴", "AO"),
    "245": ("🇬🇼", "GW"), "248": ("🇸🇨", "SC"),
    "249": ("🇸🇩", "SD"), "250": ("🇷🇼", "RW"),
    "251": ("🇪🇹", "ET"), "252": ("🇸🇴", "SO"),
    "253": ("🇩🇯", "DJ"), "254": ("🇰🇪", "KE"),
    "255": ("🇹🇿", "TZ"), "256": ("🇺🇬", "UG"),
    "257": ("🇧🇮", "BI"), "258": ("🇲🇿", "MZ"),
    "260": ("🇿🇲", "ZM"), "261": ("🇲🇬", "MG"),
    "262": ("🇷🇪", "RE"), "263": ("🇿🇼", "ZW"),
    "264": ("🇳🇦", "NA"), "265": ("🇲🇼", "MW"),
    "266": ("🇱🇸", "LS"), "267": ("🇧🇼", "BW"),
    "268": ("🇸🇿", "SZ"), "269": ("🇰🇲", "KM"),

    # MIDDLE EAST
    "970": ("🇵🇸", "PS"), "971": ("🇦🇪", "AE"),
    "972": ("🇮🇱", "IL"), "973": ("🇧🇭", "BH"),
    "974": ("🇶🇦", "QA"), "975": ("🇧🇹", "BT"),
    "976": ("🇲🇳", "MN"), "977": ("🇳🇵", "NP"),
    "978": ("🇮🇷", "IR"), "979": ("🇮🇷", "IR"),

    # EUROPE EXTRA
    "380": ("🇺🇦", "UA"), "381": ("🇷🇸", "RS"),
    "382": ("🇲🇪", "ME"), "383": ("🇽🇰", "XK"),
    "385": ("🇭🇷", "HR"), "386": ("🇸🇮", "SI"),
    "387": ("🇧🇦", "BA"), "389": ("🇲🇰", "MK"),
    "420": ("🇨🇿", "CZ"), "421": ("🇸🇰", "SK"),
    "423": ("🇱🇮", "LI"),

    # AMERICA EXTRA
    "590": ("🇬🇵", "GP"), "591": ("🇧🇴", "BO"),
    "592": ("🇬🇾", "GY"), "593": ("🇪🇨", "EC"),
    "594": ("🇬🇫", "GF"), "595": ("🇵🇾", "PY"),
    "596": ("🇲🇶", "MQ"), "597": ("🇸🇷", "SR"),
    "598": ("🇺🇾", "UY"), "599": ("🇧🇶", "BQ"),
}
def get_country_info(number):
    for code in sorted(country_data, key=len, reverse=True):
        if number.startswith(code):
            return country_data[code]
    return ("🌍", "UN")

# ------------------ HELPERS ------------------
def mask_number(number):
    return number[:4] + "***" + number[-4:]

def extract_otp(msg):
    match = re.search(r'\d{3}-\d{3}|\d{4,8}', msg)
    return match.group(0) if match else None

def is_recent(dt):
    try:
        t = datetime.strptime(dt, "%Y-%m-%d %H:%M:%S")
        return datetime.now() - t < timedelta(minutes=2)
    except:
        return False

# ------------------ FORMAT ------------------
def format_message(entry, otp):
    number = entry.get("num", "")
    flag, iso = get_country_info(number)
    masked = mask_number(number)

    return f"""{flag} {iso} +{masked}

📋 `{otp}`"""

# ------------------ TELEGRAM SEND ------------------
def send_telegram(msg):
    url = f"https://api.telegram.org/bot{TELEGRAM_BOT_TOKEN}/sendMessage"

    keyboard = {
        "inline_keyboard": [
            [
                {"text": "Main Channel", "url": "https://t.me/panthernumbers"},
                {"text": "Number Channel", "url": "https://t.me/panthernumbers"}
            ]
        ]
    }

    data = {
        "chat_id": TELEGRAM_GROUP_ID,
        "text": msg,
        "parse_mode": "Markdown",
        "reply_markup": keyboard
    }

    try:
        res = requests.post(url, json=data, timeout=10)
        print("✅ SENT:", res.text)
    except Exception as e:
        print("❌ TELEGRAM ERROR:", e)

# ------------------ FETCH ------------------
def fetch_sms():
    try:
        res = requests.get(
            API_URL,
            params={"token": TOKEN, "records": 100},
            timeout=15
        )
        data = res.json()
        return data.get("data", []) if isinstance(data, dict) else []
    except Exception as e:
        print("API ERROR:", e)
        return []

# ------------------ START ------------------
print("🚀 BOT STARTED")

# test message
send_telegram("✅ BOT ACTIVE")

# ------------------ LOOP ------------------
while True:
    entries = fetch_sms()

    for entry in entries:
        msg = entry.get("message", "")
        phone = entry.get("num", "")
        date = entry.get("dt", "")

        if not msg or not phone:
            continue

        uid = f"{phone}-{msg}-{date}"

        # skip duplicate
        if uid in processed_ids:
            continue

        # skip old
        if not is_recent(date):
            processed_ids.add(uid)
            continue

        otp = extract_otp(msg)

        if not otp:
            processed_ids.add(uid)
            continue

        final_msg = format_message(entry, otp)

        send_telegram(final_msg)

        processed_ids.add(uid)

    time.sleep(6)
