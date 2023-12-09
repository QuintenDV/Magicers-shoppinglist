# Magicers-shoppinglist
Exporteren en importeren van shopplings lists voor `magicers.nl`.

Wil je de inhoud van je winkelmandje laten bestellen door iemand anders? [Volg deze stappen.](#exporteren-van-shopping-list).  
Ben jij de uitverkorene om de bestelling te plaasen en wil je de bestelling van je vrienden importeren? [Volg deze stappen.](#importeren-van-shopping-list).

## Exporteren van shopping list

### Stap 1
Vul je shopping cart met alles dat je wil bestellen.

### Stap 2
Ga naar de **Homepage** van [magicers.nl](https://www.magicers.nl/home).

### Stap 3
In je browser open de console (F12 of ctrl+shift+i) en klik op `console`.
![image](https://github.com/QuintenDV/Magicers-shoppinglist/assets/9536846/264d7234-7cb8-4107-bcfe-16c26e9e86f0)

### Stap 4
Copy paste deze code (klik op het knopje rechtsboven) in de console en druk op enter.

```js
var productData = {
    foils: [],
    nonFoils: []
};

// Loop for non-foils
document.querySelectorAll('[id^="cart_amount_"]').forEach((cartInput) => {
    const productId = cartInput.id.replace('cart_amount_', '');
    const productName = document.getElementById(`product_name_${productId}`).value;
    const cartAmount = cartInput.value;

    const productTuple = {
        id: productId,
        name: productName,
        amount: cartAmount
    };

    productData.nonFoils.push(productTuple);
});

// Loop for foils
document.querySelectorAll('[id^="foilcart_amount_"]').forEach((cartInput) => {
    const productId = cartInput.id.replace('foilcart_amount_', '');
    const productName = document.getElementById(`product_name_${productId}`).value;
    const cartAmount = cartInput.value;

    const productTuple = {
        id: productId,
        name: productName,
        amount: cartAmount
    };

    productData.foils.push(productTuple);
});

// Convert the productData to a string for easy copying
const productDataString = JSON.stringify(productData, null, 2);

// Copy the productDataString to the clipboard
const textarea = document.createElement('textarea');
textarea.value = "var itemsToAdd = " + productDataString + ";";
document.body.appendChild(textarea);
textarea.select();
document.execCommand('copy');
document.body.removeChild(textarea);

window.alert('Shoppinglist gekopieerd naar je clipboard!');

```

### Stap 5

Je shopping list zou nu gekopieerd moeten zijn naar de clipboard. Je kan het nu plakken in chat/mail en doorsturen naar de persoon die gaat bestellen.
___

## Importeren van shopping list

# Stap 1

Volg stap 1 tot 3 van hierboven.

# Stap 2

Neem de volledige tekst die je van je vriend gekregen hebt. Deze zou er als volgt moeten uitzien:
```js
var itemsToAdd = {
  "foils": [
    {
      "id": "073912",
      "name": "Wilhelt, the Rotcleaver",
      "amount": "1"
    },
    ...
  ],
  "nonFoils": [
    {
      "id": "087335",
      "name": "Rebel Token",
      "amount": "3"
    },
    ...
};
```

Het moet beginnen met `var itemsToAdd` en eindigen met een `;`.

# Stap 3

Plak de tekst in de console zoals aangegeven op de afbeelding bij stap 3 hierboven en druk op enter.

# Stap 4
Plak nu ook deze code in je console en druk op enter. Raak je computer niet meer aan tot je in de console de melding krijgt dat alle kaarten gekopieerd zijn.


```js
function waitAWhile(ms){
    return new Promise(resolve => setTimeout(resolve, ms));
}

// Function to add items to the shopping list
async function addToShoppingList(id, amount, isFoil, name) {
        open_product(id);
        await waitAWhile(300);
        for (let i = 0; i < amount; i++) {
            if (isFoil){
                cart_foiladdto(id);
            } else {
                cart_addto(id);
            }
            await waitAWhile(400);
        }
            // await new Promise(resolve => setTimeout(resolve, 1000));
    console.log(`Added ${amount} x ${name} (${isFoil ? 'foil' : 'non-foil'}) ${id} to the shopping list.`);
};

for (const item of itemsToAdd.nonFoils) {
    await addToShoppingList(item.id, parseInt(item.amount), false, item.name);
}

// Add foil items
for (const item of itemsToAdd.foils) {
    await addToShoppingList(item.id, parseInt(item.amount), true, item.name);
}

window.alert('All kaarten zijn geimporteerd! Refresh even de pagina zodat er zeker niks kapot gaat.');

```
