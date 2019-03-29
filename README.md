<h1 align="center">Accept almost any cyrptocurrency in your Shopify Shop</h1>

What's there? A simple script that fetches for you the prices from CoinGecko and creates a QR code.

You've to create a manual payment in your shopify store called "Crypto Payments" and add the script:

https://github.com/turinglabsorg/crypto-shopify/blob/master/src/ShopifyCode.md in your settings (custom scripts)

Add your own address just editing the first row:
```javascript
window.CryptoAddresses = { 
    bitcoin: 'YOUR_BTC_ADDRESS', 
    kalkulus: 'YOUR_KLKS_ADDRESS', 
    ethereum: 'YOUR_XLM_ADDRESS', 
    binancecoin: 'YOUR_BNB_ADDRESS' 
};
```

The only important things is that the first part (for ex. bitcoin) matches the coingecko one, you can try just putting your coin in this link:

https://api.coingecko.com/api/v3/coins/yourcoin

Here's a guide created by PirateChain that explains all the steps to activate the script (the guide is with the old version, but the steps are the same): https://medium.com/piratechain/piratechain-raises-anchor-on-the-shopify-plugin-6c5819726f07
