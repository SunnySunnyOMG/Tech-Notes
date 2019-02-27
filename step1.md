# Desk Nibbles test

## answer

![output](https://ucarecdn.com/e006566b-a94e-4934-96e7-b16aef43864d/WX201902262040542x.png)

## solution code

You can easily test the solution code in your own Chrome dev console.

#### 1. Just copy the code below

```js
async function search() {
  // fetch data 
  const [snackers, { products }] = await Promise.all([
    getData('https://s3.amazonaws.com/misc-file-snack/MOCK_SNACKER_DATA.json'),
    getData('https://ca.desknibbles.com/products.json?limit=250')
  ]).catch(() => console.error(
    `=== NOTE: CORS issue ===
     The code should be run under the dev console of 
     https://s3.amazonaws.com/misc-file-snack/MOCK_SNACKER_DATA.json`
  ))

  // pre-process the product data 
  let products_preprocessed = {}
  products.forEach(product => {
    products_preprocessed[product.title] = product
  })

  // save stocked snacks
  let snack_list = new Set()
  // save the target snackers
  let snacker_list = []
  // save the total price
  let total_price = 0

  // search in loop
  snackers.forEach(snaker => {
    const snack_name = snaker.fave_snack
    if (snack_name && products_preprocessed[snack_name]) {
      snack_list.add(snack_name)
      snacker_list.push(snaker)
      total_price += Number(products_preprocessed[snack_name].variants[0].price)
    }
  })

  // output results
  coloredLog("a) List the real stocked snacks you found under the snacker's 'fave_snack'")
  console.table([...snack_list])

  coloredLog("b) The emails of the snackers who listed those as a 'fave_snack'")
  console.table(snacker_list, ['name', 'email', 'fave_snack'])

  coloredLog("c) The total price")
  console.log(total_price)

  // utils
  function getData(uri) {
    return fetch(uri).then(res => res.json())
  }
  
  function coloredLog(string){
    console.log(`%c${string}`, 'background-color:#FF5B62;color:white;font-weight:bold')
  }
}

// run the search
search()

```

#### 2. open the Chrome dev console in the [the fake data webpage](https://s3.amazonaws.com/misc-file-snack/MOCK_SNACKER_DATA.json) and paste the code to the console

![paste code](https://ucarecdn.com/fc854459-7c0f-4294-8962-9dbfc79d8a36/WX201902262058492x.png)

#### 3. then press enter, you will see,

![output](https://ucarecdn.com/e006566b-a94e-4934-96e7-b16aef43864d/WX201902262040542x.png)


## Code Doesn't Work?

- make sure you are running the code under [the fake data webpage](https://s3.amazonaws.com/misc-file-snack/MOCK_SNACKER_DATA.json) to avoid the CORS problem

