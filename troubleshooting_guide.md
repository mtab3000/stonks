# Troubleshooting Guide

## Common Errors and Solutions

### 1. KeyError: 'prices'

**Error Message:**
```
INFO:root:Error message: 'prices'
```

**Cause:** The CoinGecko API is not returning the expected data structure, usually due to:
- Invalid cryptocurrency coin ID in configuration
- Using fiat currency symbols instead of cryptocurrency coin IDs
- Network connectivity issues
- API rate limiting

**Solution:**

1. **Check your configuration** - The most common cause:
   ```yaml
   # ❌ WRONG - Don't use fiat currencies or ticker symbols
   ticker:
     currency: 'USD'     # Wrong: USD is fiat currency
     currency: 'BTC'     # Wrong: BTC is ticker symbol
   
   # ✅ CORRECT - Use CoinGecko coin IDs
   ticker:
     currency: 'bitcoin'  # Correct: CoinGecko coin ID
   ```

2. **Verify coin IDs** by checking the CoinGecko API:
   ```bash
   curl "https://api.coingecko.com/api/v3/coins/list" | grep -i bitcoin
   ```

3. **Test API connectivity**:
   ```bash
   curl "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=bitcoin"
   ```

### 2. Network/API Issues

**Error Messages:**
```
INFO:root:Issue with CoinGecko
INFO:root:Google says No
```

**Solutions:**
- Check internet connectivity: `ping google.com`
- Wait a few minutes and retry (API rate limiting)
- Check if CoinGecko is down: https://status.coingecko.com/

### 3. Display Issues

**Error Message:**
```
Initializing EPD...
[Display fails to initialize]
```

**Solutions:**
- Check SPI is enabled: `sudo raspi-config` → Interface Options → SPI → Enable
- Verify display connections
- Check power supply capacity
- Try virtual mode for testing: `python3 cryptotick.py -v`

### 4. Font Issues

**Error Message:**
```
OSError: cannot open resource
```

**Solutions:**
- Ensure font files are in the `fonts/` directory
- Install system fonts: `sudo apt-get install fonts-dejavu-core`
- Check file permissions on font directory

### 5. Image/Icon Issues

**Error Message:**
```
FileNotFoundError: [Errno 2] No such file or directory: 'images/...'
```

**Solutions:**
- Ensure `images/` directory exists in the project root
- Download missing image files
- Check file permissions

## Configuration Validation

### Valid Cryptocurrency Coin IDs

Test if your coin ID is valid:
```bash
# Replace 'bitcoin' with your coin ID
curl "https://api.coingecko.com/api/v3/coins/bitcoin" | jq '.id'
```

### Common Valid Coin IDs:
- Bitcoin: `bitcoin`
- Ethereum: `ethereum`
- Litecoin: `litecoin`
- Dogecoin: `dogecoin`
- Cardano: `cardano`
- Solana: `solana`
- Chainlink: `chainlink`
- Polkadot: `polkadot`
- Avalanche: `avalanche-2`
- Polygon: `matic-network`

### Find Coin IDs

Search for any cryptocurrency:
```bash
# Search for coin IDs containing "ada" (Cardano)
curl "https://api.coingecko.com/api/v3/coins/list" | jq '.[] | select(.id | contains("ada"))'

# Search for coin by symbol
curl "https://api.coingecko.com/api/v3/coins/list" | jq '.[] | select(.symbol == "ada")'
```

## Debug Mode

### Enable Verbose Logging

Add to your script:
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

### Test API Calls Manually

```python
import requests

# Test basic API call
url = "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=bitcoin"
response = requests.get(url)
print(f"Status: {response.status_code}")
print(f"Response: {response.json()}")

# Test historical data
url = "https://api.coingecko.com/api/v3/coins/bitcoin/market_chart/range?vs_currency=usd&from=1640995200&to=1672531200"
response = requests.get(url)
print(f"Has prices key: {'prices' in response.json()}")
```

### Virtual Display Mode

Test without physical display:
```bash
python3 cryptotick.py -v
```

## Performance Issues

### Slow Updates

**Causes:**
- Network latency
- API rate limiting (30-second delays between calls)
- Processing multiple cryptocurrencies

**Solutions:**
- Reduce number of cryptocurrencies
- Increase `updatefrequency` in config
- Use local caching (future enhancement)

### Memory Issues

**Solutions:**
- Reduce `sparklinedays` in configuration
- Limit number of cryptocurrencies per page
- Monitor with: `free -h`

## Getting Help

1. **Check logs**: Look for specific error messages
2. **Enable debug logging**: More detailed information
3. **Test configuration**: Validate your `config.yaml`
4. **Test API access**: Manually test CoinGecko API calls
5. **Virtual mode**: Test without hardware dependencies

### Useful Commands

```bash
# Check system resources
free -h
df -h

# Test network connectivity
ping -c 3 api.coingecko.com

# Check SPI interface
ls /dev/spi*

# Monitor system logs
sudo tail -f /var/log/syslog

# Test Python imports
python3 -c "import PIL, requests, matplotlib, yaml; print('All imports OK')"
```

## Configuration Examples

### Single Cryptocurrency
```yaml
ticker:
  currency: 'bitcoin'
  fiatcurrency: 'USD'
```

### Multiple Cryptocurrencies
```yaml
ticker:
  currency: 'bitcoin,ethereum,litecoin'
  fiatcurrency: 'USD,EUR,USD'
```

### Maximalist Mode (Bitcoin only with news)
```yaml
display:
  maximalist: true
ticker:
  currency: 'bitcoin'
```