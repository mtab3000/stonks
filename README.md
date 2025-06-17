# Stonks - ePaper Crypto Ticker

An ePaper multi-purpose information station that displays cryptocurrency prices, quotes, news, and more on an e-ink display.

## 🚨 Common Configuration Issue - FIXED

**If you're getting a `'prices'` KeyError, your configuration is likely incorrect.**

### ❌ Wrong Configuration:
```yaml
ticker:
  currency: 'USD'  # This is WRONG - USD is a fiat currency, not a crypto
  fiatcurrency: 'USD'
```

### ✅ Correct Configuration:
```yaml
ticker:
  currency: 'bitcoin'  # Use CoinGecko coin ID
  fiatcurrency: 'USD'   # This is the fiat currency for pricing
```

## Installation

1. Clone this repository:
```bash
git clone https://github.com/mtab3000/stonks.git
cd stonks
```

2. Install dependencies:
```bash
pip3 install -r requirements.txt
```

3. Configure your settings in `config.yaml` (see Configuration section below)

4. Run the application:
```bash
python3 cryptotick.py
```

## Configuration

### Basic Crypto Configuration

Edit `config.yaml` and set the correct cryptocurrency coin IDs:

```yaml
ticker:
  currency: 'bitcoin'          # CoinGecko coin ID (NOT fiat currency)
  fiatcurrency: 'USD'          # Fiat currency for pricing
  exchange: 'default'          # Use 'default' for CoinGecko prices
  updatefrequency: 300         # Update frequency in seconds
  sparklinedays: 14            # Days of historical data for sparklines
  coinsperpage: 3              # Number of coins per display page
```

### Multiple Cryptocurrencies

For multiple coins, use comma-separated values:

```yaml
ticker:
  currency: 'bitcoin,ethereum,litecoin'
  fiatcurrency: 'USD,USD,USD'
```

### Common CoinGecko Coin IDs

| Cryptocurrency | CoinGecko ID |
|----------------|--------------|
| Bitcoin        | `bitcoin`    |
| Ethereum       | `ethereum`   |
| Litecoin       | `litecoin`   |
| Dogecoin       | `dogecoin`   |
| Cardano        | `cardano`    |
| Solana         | `solana`     |
| Chainlink      | `chainlink`  |
| Polkadot       | `polkadot`   |

**To find more coin IDs:** Visit https://api.coingecko.com/api/v3/coins/list

### Display Modes

Configure what content to display:

```yaml
function:
  mode: 'crypto,redditquotes,wordaday,newyorkercartoon,guardianheadlines,textfilequotes,stoic,readwise'
  weight: '10,1,1,1,1,1,1,0'  # Higher weight = more likely to display
```

Available modes:
- `crypto` - Cryptocurrency prices and charts
- `redditquotes` - Inspirational quotes from Reddit
- `wordaday` - Word of the day
- `newyorkercartoon` - Daily New Yorker cartoon
- `guardianheadlines` - News headlines
- `textfilequotes` - Local quote files
- `stoic` - Daily stoic quotes
- `readwise` - Readwise highlights (requires API token)

## Troubleshooting

### Error: `'prices'` KeyError

This error occurs when:
1. **Wrong coin ID**: You're using a fiat currency (like 'USD') instead of a cryptocurrency coin ID
2. **Invalid coin ID**: The coin ID doesn't exist on CoinGecko
3. **API rate limiting**: Too many requests to CoinGecko

**Solution:**
1. Check your `config.yaml` file
2. Ensure `currency` uses valid CoinGecko coin IDs (like 'bitcoin', not 'BTC' or 'USD')
3. Verify coin IDs at: https://api.coingecko.com/api/v3/coins/list

### API Rate Limiting

The script includes built-in delays to respect CoinGecko's rate limits:
- 30-second delays between API calls
- Exponential backoff on failures
- Maximum 5 retry attempts

### Hardware Requirements

- Raspberry Pi (tested on Pi 4)
- Compatible e-ink display (IT8951 controller)
- Internet connection
- GPIO pin 17 for shutdown button (optional)

### Dependencies

Key Python packages required:
- `PIL` (Pillow) - Image processing
- `requests` - API calls
- `matplotlib` - Chart generation
- `feedparser` - RSS feeds
- `qrcode` - QR code generation
- `yaml` - Configuration files
- `gpiozero` - GPIO control
- `IT8951` - E-ink display driver

## Hardware Setup

1. Connect your e-ink display to the Raspberry Pi SPI interface
2. Optional: Connect a button to GPIO pin 17 for shutdown functionality
3. Ensure proper power supply for your display

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## Credits

Original project by Veeb Projects (https://veeb.ch)
Fixes and improvements by the community.

## License

GNU General Public License v3.0 - see LICENSE file for details.