# amazon-web-scrapping
import requests
from bs4 import BeautifulSoup

# Define the headers to mimic a browser request
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36",
    "Accept-Language": "en-US,en;q=0.9",
}

# Amazon search URL
url = 'https://www.amazon.in/s?k=iphone+16pro+max&crid=3A240ODA26YCO&sprefix=ip%2Caps%2C235&ref=nb_sb_ss_ts-doa-p_2_2'

# Send a GET request
response = requests.get(url, headers=headers)

# Check the status code
if response.status_code == 200:
    soup = BeautifulSoup(response.content, 'html.parser')

    # Find all products on the page
    products = soup.find_all('div', {'data-component-type': 's-search-result'})

    for product in products:
        # Extract product title
        title = product.h2.a.text.strip()

        # Extract product link
        link = 'https://www.amazon.in' + product.h2.a['href']

        # Extract product price
        price = product.find('span', 'a-price-whole')
        price = price.text.strip() if price else 'N/A'


        # Extract ratings
        rating = product.find('span', 'a-icon-alt')
        rating = rating.text.strip() if rating else 'N/A'


        # Print or save extracted information
        print(f'Title: {title}')
        print(f'Link: {link}')
        print(f'Price: {price}')
        print(f'Rating: {rating}')
        print('-' * 80)

else:
    print(f"Failed to retrieve the page. Status code: {response.status_code}")
