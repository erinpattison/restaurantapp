<template>
  <nav>
    <ul>
      <li><a href="#menu">Order Online</a></li>
      <li><a href="#reservations">Reservations</a></li>
      <li>
        <button @click="isMenu = !isMenu">
          <span v-if="isMenu">Innkeeper's Portal</span>
          <span v-else>Back to Home</span>
        </button>
      </li>
    </ul>
    <button id="checkout">Checkout {{ cartItemCount }}</button>
  </nav>
  <header class="mainheader">
    <div class="header-overlay">
      <h1>{{ sitename }}</h1>

      <p>
        Inn at the Crossroads has been serving fresh baked goods, wild game, and
        farm-to-table vegetables to the hungry patrons of Westeros since the
        Battle of the Six Kings.
      </p>

      <p>
        Whether you're looking for a meal and a bed, or you're just stopping in
        for a quick hen and ale, we've got you covered. And if you're on the go?
        Our to-go packages are perfect for those who are looking to take their
        meal with them. Our comprehensive online ordering system will make
        pickup and delivery seamless.
      </p>
    </div>
  </header>

  <main v-if="isMenu">
    <section id="menu">
      <header>
        <h2>Today's Menu</h2>
      </header>

      <div class="foodmenu">
        <article
          class="menu-item"
          v-for="(item, i) in menu"
          :id="menu.id"
          :key="item"
        >
          <img :src="item.photo" alt="" />
          <h3>
            <a :href="item.source" target="_blank">{{ item.name }}</a>
          </h3>
          <p>
            <em>{{ item.description }}</em>
          </p>
          <p>{{ item.price }}</p>
          <span
            ><button v-if="item.stock == 0" :disabled="isDisabled">
              Sold Out
            </button>
            <button v-else v-on:click="addToCart(i)">Add to Cart</button>
            <span v-if="item.stock >= 5">In stock</span>
            <span v-else-if="item.stock == 0">Sold Out</span>
            <span v-else-if="item.stock < 5">low stock</span>
          </span>
        </article>
      </div>
    </section>
    <!-- this reservations section is more to fill out the page to go with the theme -->
    <section id="reservations">
      <header class="reservationheader">
        <div>
          <h2>Reservations</h2>
        </div>
      </header>
      <div id="reservation">
        <article class="photo">
          <img src="../public/images/castle.png" />
        </article>
        <article class="form">
          <form @submit.prevent="addReservation">
            <h3>Book a meal and a bed</h3>
            <label for="res_type">Reservation Type</label><br />
            <select>
              <option>Meal</option>
              <option>Room</option>
              <option>Meal & Room</option>
            </select>
            <br />

            <label for="no_patrons">Number of Patrons:</label><br />
            <select id="no_patrons">
              <option>1</option>
              <option>2</option>
              <option>3</option>
              <option>4</option>
              <option>5</option>
            </select>
            <br />

            <label for="firstName">First name: </label> <br />
            <input
              type="text"
              id="firstName"
              v-model.trim.lazy="customer.firstName"
            />
            <br />

            <label for="lastName">Last name:</label> <br />
            <input
              type="text"
              id="lastName"
              v-model.trim.lazy="customer.lastName"
            />
            <br />

            <label for="raven">Raven Number</label> <br />
            <input type="tel" id="raven" v-model.trim.lazy="customer.raven" />
            <br />

            <label for="date">Date of reservation</label> <br />
            <input type="date" v-model.trim.lazy="customer.date" /><br />

            <label for="request">Special requests:</label> <br />
            <textarea
              id="request"
              rows="5"
              v-model.lazy="customer.requests"
            ></textarea>
            <br />

            <button>Confirm Reservation</button>
          </form>
        </article>
      </div>
    </section>
  </main>
  <main v-else>
    <section class="chefsportal">
      <header>
        <h2>Innkeeper's Portal</h2>
      </header>
      <article class="menutable">
        <h3>Current Menu</h3>
        <table v-for="item in menu" :key="item">
          <tr>
            <td class="small">{{ item.id }}</td>
            <td class="big">{{ item.name }}</td>
            <td class="big">{{ item.description }}</td>
            <td class="small">{{ item.price }}</td>
            <td class="medium">{{ item.category }}</td>
          </tr>
        </table>
      </article>
      <article class="form">
        <form @submit.prevent="addMenu">
          <h3>Add a menu item</h3>
          <label for="productid">ID</label><br />
          <input type="number" max="99" v-model="newMenu.id" /><br />

          <label for="dishname">Dish name</label><br />
          <input type="text" id="dishname" v-model="newMenu.name" /><br />

          <label for="description">Description</label><br />
          <textarea
            id="description"
            rows="5"
            v-model="newMenu.description"
          ></textarea
          ><br />

          <label for="quantity">Quantity</label><br />
          <input type="number" id="quantity" v-model="newMenu.quantity" /><br />

          <label for="price">Price</label><br />
          <input type="number" id="price" v-model="newMenu.price" /><br />

          <label for="category">Category</label><br />
          <select v-model="newMenu.category">
            <option>Breakfast</option>
            <option>Lunch</option>
            <option>Dinner</option>
            <option>Beverages</option>
            <option>Bakery</option></select
          ><br />
          <button>Submit</button>
          <button @click="isMenu = !isMenu">Back to Menu</button>
        </form>
      </article>
    </section>
  </main>

  <footer>
    <p>
      <a href="http://erinpattison.com/" target="_blank">Erin Pattison</a> |
      COSW 230 | Long Beach City College SP 2022
    </p>
  </footer>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      sitename: "Inn at the Crossroads",
      menu: [],
      reservations: [],
      cart: [],
      isMenu: true,
      customer: {
        firstName: "",
        lastName: "",
        raven: "",
        date: "",
        requests: ""
      },
      newMenu: {
        id: "",
        name: "",
        description: "",
        quantity: 10,
        inStock: true,
        price: "",
        category: ""
      }
    };
  },
  methods: {
    addReservation: function () {
      // in progress
    },
    addMenu: function () {
      const menuItem = {
        id: this.newMenu.id,
        name: this.newMenu.name,
        description: this.newMenu.description,
        stock: this.newMenu.quantity,
        price: this.newMenu.price,
        category: this.newMenu.category
      };
      this.menu.push(menuItem);
    },
    addToCart: function (index) {
      //this handles the stock (inplace of inStock method)
      this.menu[index].stock -= 1;
      this.cart.push(this.menu.id);
    }
  },
  computed: {
    cartItemCount() {
      return this.cart.length || "";
    }
  },
  created: function () {
    const appData = this;
    axios.get("data.json").then(function (response) {
      appData.menu = response.data.menu;
    });
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Alegreya:ital,wght@0,400;0,500;0,600;0,700;0,800;0,900;1,400;1,500;1,600;1,700;1,800;1,900&family=Inknut+Antiqua&display=swap");

/* 
font-family: 'Alegreya', serif;
font-family: 'Inknut Antiqua', serif;
*/

/* Generic styles- body styles in index.html head */
h1,
h2,
h3 {
  margin: 0;
  font-family: "Inknut Antiqua", serif;
  font-weight: lighter;
}
h1 {
  font-size: 4rem;
}
h2 {
  font-size: 3rem;
}
h3 {
  font-size: 2rem;
}

/* Hero */
.mainheader {
  height: 90vh;
  width: 100vw;
  background-image: url("https://assets.codepen.io/5737230/IMG_6089.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-attachment: fixed;
}
.header-overlay {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #dcdcdc;
  background-color: rgba(10, 10, 10, 0.7);
}
header h1 {
  font-size: 6vw;
}
header p {
  width: 60%;
}

/* NAVBAR */
nav {
  width: 100vw;
  height: auto;
  border-bottom: 2px solid #dcdcdc;
  position: sticky;
  top: 0;
  padding: 0.5rem;
  background-color: #0a0a0a;
  z-index: 100;
  display: flex;
  justify-content: space-between;
}
nav ul {
  list-style: none;
  display: flex;
}
nav li {
  color: #dcdcdc;
  margin: 0 1rem;
  font-size: 2rem;
}
nav a {
  color: #dcdcdc;
  text-decoration: none;
}
nav a:hover {
  text-decoration: underline;
}
#checkout {
  margin: 0 3rem;
  width: 150px;
  border: 2px solid #dcdcdc;
  color: #dcdcdc;
  background-color: #0a0a0a;
  cursor: pointer;
}

/* MENU SECTION */
#menu header,
.reservation header {
  padding: 2rem;
  text-align: center;
  color: #dcdcdc;
  margin-top: 10rem;
}

/* menu cards */
.foodmenu {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  width: 100vw;
  padding: 1.5rem;
}

.menu-item {
  width: 30vw;
  background-color: #000;
  max-width: 250px;
  margin: 2rem;
  color: #c8c8c8;
  padding: 1.5rem;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.menu-item img {
  max-width: 100%;
}
.menu-item h3 {
  text-align: center;
}
.menu-item a {
  text-decoration: none;
  color: #dcdcdc;
}
.menu-item button {
  background-color: #0a0a0a;
  color: #dcdcdc;
  border: 2px solid #dcdcdc;
}
.menu-item span {
  display: flex;
  justify-content: space-around;
}

/* reservation section- just for decoration/to full out the page right now */
.reservationheader {
  height: 30vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url("images/kingsroadspacer.png");
  background-size: cover;
  background-attachment: fixed;
  padding: 0;
}
.reservationheader h2 {
  font-size: 5vw;
  color: #dcdcdc;
}

#reservation {
  width: 100vw;
  height: auto;
  display: flex;
  padding: 2rem;
  color: #dcdcdc;
}
#reservation article {
  width: 50%;
}
.photo {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 3rem;
}
.photo img {
  width: 75%;
  filter: saturate(50%);
  z-index: 0;
}
.form {
  display: flex;
  justify-content: center;
  align-items: center;
}
form {
  color: #dcdcdc;
  padding: 2rem;
  width: 50%;
}
form h3 {
  text-align: center;
}
input[type="text"],
input[type="tel"],
textarea {
  width: 100%;
}
input,
select,
textarea {
  margin-bottom: 1rem;
}

/* ADD TO MENU PAGE */
.chefsportal header {
  text-align: center;
  color: #dcdcdc;
  padding: 2rem;
}
pre {
  color: #dcdcdc;
}
.menutable h3 {
  color: #dcdcdc;
  text-align: center;
  margin: 2rem;
}

table {
  color: #dcdcdc;
  width: 80%;
  margin: 0 auto;
  border-collapse: collapse;
  /* border: 2px solid #dcdcdc; */
}
td {
  border: 1px solid #dcdcdc;
  border-collapse: collapse;
  padding: 0.5rem;
}
table .small {
  width: 10%;
}
table .medium {
  width: 15%;
}
table .big {
  width: 20%;
}
.chefsportal button {
  margin-right: 1rem;
}
.chefsportal form {
  width: 65vw;
}

/* FOOTER STYLES */
footer {
  background-color: #000;
  color: #dcdcdc;
  border-top: 2px solid #dcdcdc;
  text-align: center;
  padding: 2rem;
}

@media only screen and (max-width: 600px) {
  menu-item {
    width: 45vw;
    margin: 1rem;
  }
  menu-item h3 {
    font-size: 1.8rem;
  }
}
</style>