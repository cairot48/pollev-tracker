import os
import hashlib
import requests
from twilio.rest import Client

URL = "https://pollev.com/asnowden"

TWILIO_SID = os.environ["TWILIO_SID"]
TWILIO_TOKEN = os.environ["TWILIO_TOKEN"]
TWILIO_FROM = os.environ["TWILIO_FROM"]   # your Twilio number e.g. +15551234567
YOUR_NUMBER = os.environ["YOUR_NUMBER"]   # your real number e.g. +17048675309

HASH_FILE = "last_hash.txt"

def get_page_hash():
    headers = {"User-Agent": "Mozilla/5.0"}
    r = requests.get(URL, headers=headers, timeout=15)
    r.raise_for_status()
    return hashlib.md5(r.text.encode()).hexdigest(), r.text

def send_text(message):
    client = Client(TWILIO_SID, TWILIO_TOKEN)
    client.messages.create(body=message, from_=TWILIO_FROM, to=YOUR_NUMBER)

def main():
    current_hash, _ = get_page_hash()

    if os.path.exists(HASH_FILE):
        with open(HASH_FILE, "r") as f:
            last_hash = f.read().strip()

        if current_hash != last_hash:
            send_text(f"🚨 PollEv Update Detected!\npollev.com/asnowden just changed. Check it now!")
            print("Change detected — text sent!")
        else:
            print("No change detected.")
    else:
        print("First run — saving baseline hash.")

    with open(HASH_FILE, "w") as f:
        f.write(current_hash)

if __name__ == "__main__":
    main()
