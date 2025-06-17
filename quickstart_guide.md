# Quick Start Guide

Get your ePaper crypto ticker running in 5 minutes!

## 🚀 Quick Setup

### 1. Clone and Install
```bash
git clone https://github.com/mtab3000/stonks.git
cd stonks
pip3 install -r requirements.txt
```

### 2. Configure Your Crypto
```bash
cp config.example.yaml config.yaml
nano config.yaml
```

**⚠️ IMPORTANT:** Use CoinGecko coin IDs, not ticker symbols!

```yaml
ticker:
  currency: 'bitcoin'     # ✅ Use 'bitcoin', not 'BTC' or 'USD'
  fiatcurrency: 'USD'     # ✅ Your preferred fiat currency
```

### 3. Test Without Hardware
```bash
python3 cryptotick.py -v
```
This opens a virtual display window - perfect for testing!

### 4. Run on Real Hardware
```bash
python3 cryptotick.py
```

## 🔧 Common Configurations

### Bitcoin Only
```yaml
ticker:
  currency: 'bitcoin'
  fiatcurrency: 'USD'
function:
  mode: 'crypto'
  weight: '1'
```

### Top 3 Cryptos
```yaml
ticker:
  currency: 'bitcoin,ethereum,solana'
  fiatcurrency: 'USD,USD,USD'
```

### Portfolio Tracking
```yaml
ticker:
  currency: 'bitcoin,ethereum,cardano,solana,chainlink'
  fiatcurrency: 'USD,USD,USD,USD,USD'
  coinsperpage: 3
```

## 🆘 Quick Troubleshooting

### Error: `'prices'` KeyError
**Your cryptocurrency is misconfigured!**

❌ **Don't use:**
- Fiat currencies: `'USD'`, `'EUR'`, `'GBP'`
- Ticker symbols: `'BTC'`, `'ETH'`, `'ADA'`

✅ **Use CoinGecko coin IDs:**
- `'bitcoin'` (not BTC)
- `'ethereum'` (not ETH)  
- `'cardano'` (not ADA)

### Find Your Coin ID
```bash
# Search for any cryptocurrency
curl "https://api.coingecko.com/api/v3/coins/list" | grep -i "your_crypto_name"
```

### Test Your Configuration
```bash
# Replace 'bitcoin' with your coin ID
curl "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=bitcoin"
```

If this returns data, your coin ID is correct!

## 📊 Display Modes

Configure what content to show:

```yaml
function:
  mode: 'crypto,redditquotes,wordaday,guardianheadlines'
  weight: '10,1,1,1'  # Crypto 10x more likely than others
```

**Available modes:**
- `crypto` - Cryptocurrency prices and charts
- `redditquotes` - Inspirational quotes
- `wordaday` - Daily vocabulary
- `newyorkercartoon` - Daily cartoon
- `guardianheadlines` - News headlines
- `textfilequotes` - Local quote files
- `stoic` - Daily stoic philosophy
- `readwise` - Your reading highlights

## 🎯 Hardware Setup

### Raspberry Pi + ePaper Display
1. Enable SPI: `sudo raspi-config` → Interface Options → SPI → Enable
2. Connect display to SPI pins
3. Optional: Button on GPIO 17 for shutdown

### Display Settings
```yaml
display:
  vcom: -2.61      # Adjust for your specific display
  inverted: false  # Try true if colors look wrong
  xshift: 0        # Adjust horizontal position
  yshift: 30       # Adjust vertical position
```

## 🔄 Updates and Timing

```yaml
ticker:
  updatefrequency: 300    # Update every 5 minutes
  sparklinedays: 14       # 2 weeks of price history
```

**Note:** Minimum update frequency is 60 seconds due to API rate limiting.

## 🆘 Need Help?

1. **Check the logs** for specific error messages
2. **Test in virtual mode** first: `python3 cryptotick.py -v`
3. **Read the troubleshooting guide**: `TROUBLESHOOTING.md`
4. **Validate your config** with the API test commands above

## 📚 Next Steps

- Read the full `README.md` for advanced configuration
- Check `TROUBLESHOOTING.md` if you encounter issues
- Explore different display modes and weightings
- Set up automatic startup with systemd

Happy crypto tracking! 🚀