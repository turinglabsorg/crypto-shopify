{% if {{checkout.transactions[0].gateway}} == "Crypto Payment" %}
<br><br>
<div id="root" style="text-align:center; padding:15px;"></div>
<script>
window.CryptoAddresses = {
    bitcoin: 'YOUR_BTC_ADDRESS',
    kalkulus: 'YOUR_KLKS_ADDRESS',
    ethereum: 'YOUR_XLM_ADDRESS',
    binancecoin: 'YOUR_BNB_ADDRESS'
};
for (var prop in window.CryptoAddresses) {
    window.coinselected = prop;
    window.paytoaddress = window.CryptoAddresses[prop]
    break;
}
const app = document.getElementById('root');
const container = document.createElement('div');
app.setAttribute('class', 'content-box');
app.appendChild(container);
const container2 = document.createElement('div');
container2.setAttribute('class', 'content-box__row');
var head = document.createElement('H2');     
head.textContent = "Select Cryptocurrency"; 
app.appendChild(container2);
container2.appendChild(head);

const containerselect = document.createElement('div');
var selectList = document.createElement("select");
selectList.id = "CryptoSelect";
containerselect.appendChild(selectList);

for (var prop in window.CryptoAddresses) {
    var option = document.createElement("option");
    option.value = prop;
    option.text = prop.toUpperCase();
    selectList.appendChild(option);
}
app.appendChild(containerselect);
document.getElementById("CryptoSelect").style["width"] = "100%";
document.getElementById("CryptoSelect").style["padding"] = "5px";
document.getElementById("CryptoSelect").style["font-size"] = "20px";
document.getElementById("CryptoSelect").style["-webkit-appearance"] = "menulist-button";
document.getElementById("CryptoSelect").style["-moz-appearance"] = "menulist-button";
document.getElementById("CryptoSelect").style["appearance"] = "menulist-button";
document.getElementById("CryptoSelect").style["text-align"] = "center";

var request = new XMLHttpRequest();
var amountstring = "{{checkout.total_price | money}}";
var amountstring = amountstring.replace(',','.');
var currencysymbol = amountstring.substring(0,1);
var amount = parseFloat(amountstring.substring(1));
var currency = "usd";
switch(currencysymbol){
   case "€":
      currency = 'eur';
   break;
   case "£":
      currency = 'gbp';
   break;
}
function calculateCryptoAmount(){
    if(document.getElementById('cryptoqrcode')){
        document.getElementById('cryptoqrcode').remove();
    }
    const container3 = document.createElement('div');
    container3.setAttribute('class', 'content-box__row');
    container3.setAttribute('id', 'cryptoqrcode');
    app.appendChild(container3);
    var pricerequest = new XMLHttpRequest();
    pricerequest.open('GET', 'https://api.coingecko.com/api/v3/coins/' + window.coinselected, true);
    pricerequest.onload = function () {
        var data = JSON.parse(this.response);
        var price = data.market_data.current_price[currency];
        if (pricerequest.status >= 200 && pricerequest.status < 400) {
            var amountneeded = parseFloat(parseFloat(amount) / price).toFixed(8);
            amountneeded = amountneeded.toString();
            const p = document.createElement('p');
            p.textContent = `Send ${amountneeded} ${data.symbol.toUpperCase()} to`;
            container3.appendChild(p);
            const paddr = document.createElement('p');
            paddr.textContent = window.paytoaddress;
            container3.appendChild(paddr);
            const br = document.createElement('br');
            container3.appendChild(br);
            const p2 = document.createElement('p');
            p2.textContent = "Equals to "+ amount + currencysymbol;
            container3.appendChild(p2);
            container3.appendChild(br);
            container3.appendChild(br);
            const qr= document.createElement('img');
            qr.src = `https://chart.googleapis.com/chart?cht=qr&chl=${data.symbol}:` + window.paytoaddress + `%26amount=${amountneeded}&chs=300x300&chld=L|0`;
            container3.appendChild(qr);
            const p3 = document.createElement('p');
            p3.textContent = 'Please complete payment and proceed.';
            container3.appendChild(p3);
        } else {
            const errorMessage = document.createElement('p');
            errorMessage.textContent = `Sorry it's not working! Try refreshing the page.`;
            app.appendChild(errorMessage);
        }
    }
    pricerequest.send();
}

containerselect.onchange = function (){
    var elem = (typeof this.selectedIndex === "undefined" ? window.event.srcElement : this);
    var value = elem.value || elem.options[elem.selectedIndex].value;
    window.coinselected = value
    window.paytoaddress = window.CryptoAddresses[value]
    calculateCryptoAmount();
};

calculateCryptoAmount();
</script>

{% endif %}
