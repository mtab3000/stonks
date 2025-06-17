# Changelog

## [Latest] - 2025-06-17

### Fixed
- **CRITICAL:** Fixed `KeyError: 'prices'` error that prevented crypto ticker from working
- Added comprehensive error handling for CoinGecko API responses
- Added validation for cryptocurrency configuration
- Improved logging with detailed error messages and API response debugging
- Added timeout handling for API requests to prevent hanging
- Enhanced response validation to catch malformed API responses

### Added
- Input validation for cryptocurrency coin IDs
- Better error messages that explain configuration issues
- Comprehensive documentation with troubleshooting guide
- Configuration examples with correct and incorrect formats
- API response validation and error recovery
- Debug logging for API calls and responses

### Changed
- Enhanced `getgecko()` function with proper error handling
- Improved `getData()` function with input validation
- Updated error messages to be more descriptive and actionable
- Added response status code checking for API calls

### Documentation
- Added comprehensive README with configuration examples
- Created TROUBLESHOOTING.md with common issues and solutions
- Added config.example.yaml with proper configuration format
- Documented common CoinGecko coin IDs and how to find them
- Added installation and setup instructions

## Background

### The Problem
Many users were experiencing a `KeyError: 'prices'` error when running the crypto ticker. This occurred because:

1. **Configuration Error**: Users were setting `currency: 'USD'` in their config, but the `currency` field should contain cryptocurrency coin IDs (like 'bitcoin'), not fiat currencies.

2. **Poor Error Handling**: The original code didn't validate API responses or provide helpful error messages when the CoinGecko API returned unexpected data.

3. **Lack of Documentation**: Users didn't have clear guidance on how to properly configure the cryptocurrency settings.

### The Solution
- **Fixed Configuration**: Clear documentation showing that `currency` should use CoinGecko coin IDs like 'bitcoin', 'ethereum', etc.
- **Enhanced Error Handling**: Added checks for API response structure and meaningful error messages
- **Comprehensive Documentation**: Created guides showing correct configuration and troubleshooting steps

### Example Fix
**Before (causing errors):**
```yaml
ticker:
  currency: 'USD'        # ❌ Wrong: USD is fiat currency
  fiatcurrency: 'USD'
```

**After (working correctly):**
```yaml
ticker:
  currency: 'bitcoin'    # ✅ Correct: CoinGecko coin ID
  fiatcurrency: 'USD'    # ✅ Correct: Fiat currency for pricing
```

## Migration Guide

If you're upgrading from the previous version:

1. **Update your `config.yaml`:**
   - Change `currency` from fiat currencies or ticker symbols to CoinGecko coin IDs
   - Examples: 'USD' → 'bitcoin', 'BTC' → 'bitcoin', 'ETH' → 'ethereum'

2. **Test your configuration:**
   ```bash
   # Verify your coin ID is valid
   curl "https://api.coingecko.com/api/v3/coins/YOUR_COIN_ID"
   ```

3. **Use the new error handling:**
   - Check logs for detailed error messages if issues persist
   - Refer to TROUBLESHOOTING.md for common solutions

## Common CoinGecko Coin IDs

| Cryptocurrency | Ticker | CoinGecko ID |
|----------------|--------|--------------|
| Bitcoin        | BTC    | `bitcoin`    |
| Ethereum       | ETH    | `ethereum`   |
| Litecoin       | LTC    | `litecoin`   |
| Dogecoin       | DOGE   | `dogecoin`   |
| Cardano        | ADA    | `cardano`    |
| Solana         | SOL    | `solana`     |
| Chainlink      | LINK   | `chainlink`  |
| Polkadot       | DOT    | `polkadot`   |

Find more at: https://api.coingecko.com/api/v3/coins/list