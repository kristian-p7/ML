import pandas as pd
import requests

# Ersetzen Sie 'DEIN_API_SCHLÜSSEL' durch Ihren Alpha Vantage API-Schlüssel
api_key = 'CEKRMJCF4Q07KVAC.'
symbol = 'AAPL'

# URL für den API-Aufruf zusammenstellen
url = f'https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol={symbol}&apikey={api_key}'

# API-Aufruf durchführen und JSON-Daten abrufen
response = requests.get(url)
data = response.json()

# Überprüfen Sie, ob die API-Anfrage erfolgreich war
if 'Time Series (Daily)' in data:
    # Die Daten in einen Pandas DataFrame umwandeln
    df = pd.DataFrame(data['Time Series (Daily)']).T

    # Spalten in geeignete Datentypen konvertieren
    df['date'] = pd.to_datetime(df.index)
    df[['1. open', '2. high', '3. low', '4. close', '5. volume']] = df[['1. open', '2. high', '3. low', '4. close', '5. volume']].astype(float)

    # DataFrame anzeigen
    print(df)
else:
    print("Fehler bei der API-Anfrage. Antwort von Alpha Vantage: {}".format(data))
