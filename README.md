<!DOCTYPE html>  <html>  
<head>  
<title> Shopping App </title>  
<style>  
body{background: grey;}  
li{margin-bottom:10px}  
img{vertical-align:middle;margin-right:10px}  
button{margin-left:10px}  
h1{color:green; font-size:50px;}  
h2{color: blue; font-size:30px;}  
h3{color: lightgreen; font-size: 50px;}  
</style>  
</head>  
<body>  
<h1> Ocheli Jethro: <br> Shopping App </h1>  
<input id="search" placeholder="Search products">  <h2> Products</h2>  
<ul id="products"></ul> 

 <h2> Cart </h2>  
 <button onclick="clearCart()">Clear Cart</button>
<ul id="cart"></ul>  <h3> Total: $<span id="total">0</span></h3>    
<h2>Checkout</h2>

<input id="name" placeholder="Full Name"><br><br>
<input id="address" placeholder="Address"><br><br>
<input id="phone" placeholder="Phone Number"><br><br>

<button id="checkoutBtn">Checkout</button>
<p id="status"></p>  <script>  
const productsList = document.getElementById("products")  
const cartList = document.getElementById("cart")  
const total = document.getElementById("total")  
const search = document.getElementById("search")  
  
let products = [  
  {name: "Phone", price: 300, img:  
  "https://i.ibb.co/DgmpHdpn/phone.png", desc: "Apple iPhone 17 pro Max 5G  6.9-12GB RAM-512 ROM Nano SIM-Cosmic Orange"},  
    
  {name: "Laptop", price: 1000, img:"https://i.ibb.co/KjrSfDwH/laptop.png", desc: "Laptoptouchscreen"},  
 
  {name: "Headset", price: 70, img: "https://i.ibb.co/9PWdJyZ/headset.png", desc: "Headset"},  
 {name: "House", price:18, img:  "https://i.ibb.co/ymB6pQc8/house.png", desc: "Home forsale"},{name: "Car", price: 12, img: "https://i.ibb.co/zhgndDdj/car.png", desc: "Lexus Jeep for sale"},  
 {name: "Phone", price: 700, img: "https://i.ibb.co/TDVgKY6m/phone.png", desc: "Smartphone"},  
 {name: "Phone", price: 1200, img:  
 "https://i.ibb.co/RpjYK08d/Phone.png", desc: "Galaxys20+"}  
 ]  
  
let cart = []
    
    function clearCart(){
  cart = []
  render()
  }
function render(){  
  const keyword = search.value.toLowerCase()  
  
  const filtered = products.map((p,i)=>({...p, index:i}))  
    .filter(p=>p.name.toLowerCase().includes(keyword))  


  
  // Render products  
 productsList.style="display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:10px"  
  
productsList.innerHTML = filtered.map(p=>`
<li data-view="${p.index}" 
    title="${p.name} - ${p.desc.slice(0,40)}... ($${p.price})"
    style="list-style:none;text-align:center;cursor:pointer">
    <img src="${p.img}" width="300"
onerror="this.src='https://via.placeholder.com/300'">
 <br>
${p.name}<br>
<small>${p.desc || ""}</small><br>
$${p.price}<br>
<button data-add=${p.index}>Add</button>
</li>
`).join("")
  
  // Render cart  
  cartList.innerHTML = cart.map((c,i)=>`  
  <li>  
    <img src="${c.img}" width="200"
    onerror="this.src='https://via.placeholder.com/200'">
    ${c.name} x${c.qty} - $${c.price * c.qty}  
    <button data-plus="${i}">+</button>
    <button data-minus="${i}">-</button>
    <button data-remove="${i}">Remove</button>  
  </li>  
`).join("")
  
  // Update total  
  total.textContent = cart.reduce((sum,c)=>sum + (c.price * c.qty),0)  
}  
  
// Add to cart  
productsList.onclick = e => {
  const add = e.target.dataset.add
  
  const view = e.target.closest("li")?.dataset.view

  // ADD TO CART
  if(!isNaN(add)){
    const existing = cart.find(c => c.index === add)

    if(existing){
      existing.qty++
    } else {
      cart.push({...products[add], qty:1, index:add})
    }

    render() // ✅ VERY IMPORTANT
    return
  }

  // VIEW PRODUCT
  if(view !== undefined){
    const p = products[view]
    alert(
      p.name + "\n\n" +
      p.desc + "\n\n" +
      "Price: $" + p.price
    )
  }
}
  
// Remove from cart  
cartList.onclick = e => {
  const i = e.target.dataset

  if(i.plus){
    cart[i.plus].qty++
  }

  if(i.minus){
    if(cart[i.minus].qty > 1) cart[i.minus].qty--
  }

  if(i.remove){
    cart.splice(i.remove,1)
  }

  render()
}
  
// Search products  
search.oninput = render  
  
// Initial render  
render()  
   
const status = document.getElementById("status")  
  
const checkoutBtn = document.getElementById("checkoutBtn")

checkoutBtn.onclick = () => {
  if(cart.length === 0){
    status.textContent = "Cart is empty"
    return
  }

  const name = document.getElementById("name").value
  const address = document.getElementById("address").value
  const phone = document.getElementById("phone").value

  if(!name || !address || !phone){
    status.textContent = "Please fill all checkout details"
    return
  }

  const totalAmount = cart.reduce((s,c)=>s + c.price*c.qty,0)

  const confirmOrder = confirm(
    "Name: " + name + "\n" +
    "Address: " + address + "\n" +
    "Phone: " + phone + "\n\n" +
    "Total: $" + totalAmount + "\n\nConfirm order?"
  )

  if(confirmOrder){
    status.textContent = "Order placed successfully ✅"
    cart = []
    render()

    // clear inputs
    document.getElementById("name").value = ""
    document.getElementById("address").value = ""
    document.getElementById("phone").value = ""
  } else {
    status.textContent = "Checkout cancelled ❌"
  }
}
</script>  </body>  
</html>
