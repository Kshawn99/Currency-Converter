# Currency-Converter
import requests

API_KEY = "706582ed7d43783f9ca875af"  
BASE_URL = f"https://v6.exchangerate-api.com/v6/{API_KEY}/latest/USD"

def get_exchange_rates():
    """Fetch real-time exchange rates from the API."""
    try:
        response = requests.get(BASE_URL)
        data = response.json()
        
        if response.status_code == 200:
            return data["conversion_rates"]
        else:
            print("Error:", data.get("error-type", "Failed to fetch rates"))
            return None
    except requests.exceptions.RequestException as e:
        print("Error fetching data:", e)
        return None

def convert_currency(amount, from_currency, to_currency, rates):
    """Convert amount from one currency to another."""
    if from_currency not in rates or to_currency not in rates:
        return "Invalid currency code."

    # Convert amount using real-time rates
    amount_in_usd = amount / rates[from_currency]
    converted_amount = amount_in_usd * rates[to_currency]
    return round(converted_amount, 2)

def main():
    """Main function to handle user input and conversion."""
    rates = get_exchange_rates()
    
    if not rates:
        print("Could not fetch exchange rates. Try again later.")
        return

    print("Available currencies:", ", ".join(rates.keys()))

    from_currency = input("Enter the currency you have (e.g., USD): ").upper()
    to_currency = input("Enter the currency to convert to (e.g., EUR): ").upper()
    amount = float(input("Enter the amount: "))

    result = convert_currency(amount, from_currency, to_currency, rates)

    print(f"{amount} {from_currency} is equal to {result} {to_currency}")

if __name__ == "__main__":
    main()
