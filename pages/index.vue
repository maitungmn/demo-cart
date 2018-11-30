<template>
  <div id="app">
    <div id="product">
      <v-container grid-list-md text-xs-center>
        <v-layout justify-center row wrap>
          <v-flex xs4 v-for="(item, index) in items" :key="index">
            <v-card>
              <v-img
                :src="item.img"
                aspect-ratio="2.75"
                height="450px"
              ></v-img>
              <v-card-title>
                <div>
                  <h1>{{item.title}}</h1>
                </div>
              </v-card-title>
              <v-spacer></v-spacer>
              <div>
                <h3 class="headline mb-0" style="text-align: right">
                  $ {{item.price}}
                </h3>
              </div>
              <v-spacer></v-spacer>
              <v-btn fab dark color="indigo">
                <v-icon dark @click="addItem(index)">add</v-icon>
              </v-btn>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </div>

    <v-layout justify-center row wrap>
      <v-flex xs6>
        <v-data-table
          :headers="headers"
          :items="buyitems"
          class="elevation-1"
          hide-actions
        >
          <template slot="items" slot-scope="props" v-if="props.item.click">
            <td>{{ props.item.title }}</td>
            <td>$ {{ props.item.price }}</td>
            <td>
              {{ props.item.click }}
              <v-btn color="error" small @click="removeItem(props.index)">
                <v-icon>delete</v-icon>
              </v-btn>            </td>
            <td>$ {{ props.item.click * props.item.price }}</td>
          </template>
          <template slot="footer">
            <td :colspan="headers.length">
              <strong>Total: $ {{syncTotal}}</strong>
            </td>
          </template>
        </v-data-table>
      </v-flex>
    </v-layout>
  </div>

</template>

<script>

  export default {
    data() {
      return {
        items: [
          {
            img: "https://chenyiya.com/codepen/product-1.jpg",
            title: "Beer Bottle",
            price: "25",
            id: "beer"
          },
          {
            img: "https://chenyiya.com/codepen/product-2.jpg",
            title: "Eco Bag",
            price: "73",
            id: "eco-bag"
          },
          {
            img: "https://chenyiya.com/codepen/product-3.jpg",
            title: "Paper Bag",
            price: "35",
            id: "paper-bag"
          }
        ],
        buyitems: [
          {
            title: "Beer Bottle",
            price: "25",
            click: 0
          },
          {
            title: "Eco Bag",
            price: "73",
            click: 0
          },
          {
            title: "Paper Bag",
            price: "35",
            click: 0
          },
        ],
        headers: [
          {text: 'Shopping Cart', value: 'title'},
          {text: 'Price', value: 'price'},
          {text: 'Quantity', value: 'quantity'},
          {text: 'Total', value: 'total'},
        ],
        total: 0
      }
    },
    computed: {
      syncTotal() {
        this.total = 0;
        this.buyitems.forEach(i => {
          this.total += i.click * parseInt(i.price)
        });
        return this.total;
      }
    },
    methods: {
      addItem(index) {
        this.buyitems[index].click += 1
      },
      removeItem(index) {
        this.buyitems[index].click -= 1
      }
      // totalCount() {
      //   this.buyitems.forEach(i => {
      //     this.total += i.click * parseInt(i.price)
      //   });
      //   return this.total;
      // }
    },
    watch: {
      buyitems(newVal) {
        this.buyitems = newVal;
      }
    }
  }
</script>

<style lang="scss">
  html, body {
    background-color: #eee;
    font-family: calibri, sans-serif;
  }

  #app {
    /*width: 760px;*/
    /*max-width: 1024px;*/
    /*margin: 20px auto;*/
    #product {
      .box {
        width: 230px;
        background-color: #fff;
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
        display: inline-block;
        margin: 0 10px;
        position: relative;
        img {
          width: 230px;
        }
        i {
          width: 50px;
          height: 50px;
          background: #ED277F;
          color: #ffffff;
          border-radius: 25px;
          text-align: center;
          line-height: 50px;
          font-size: 1.4rem;
          position: absolute;
          right: 20px;
          top: 150px;
          box-shadow: 0 0 4px 2px rgba(80, 80, 80, 0.1);
          cursor: pointer;
          transition: all 0.3s;
          &:hover {
            transform: scale(1.05);
          }
        }
        h2 {
          margin-left: 20px;
        }
        p {
          margin-left: 20px;
        }
      }
    }
    #cart {
      margin-top: 50px;
      overflow: hidden;
      #head {
        width: 100%;
        border-bottom: 1px solid #BFBFBF;
        height: 40px;
        display: block;
        h3 {
          display: inline-block;
          line-height: 40px;
          margin: 0;
        }
        #price {
          display: inline-block;
          color: #777777;
          margin-left: 200px;
          line-height: 40px;
        }
        #quantity {
          display: inline-block;
          color: #777777;
          margin-left: 100px;
          line-height: 40px;
        }
        #total {
          display: inline-block;
          color: #777777;
          line-height: 40px;
          float: right;
        }
      }
      .row {
        width: 100%;
        border-bottom: 1px solid #BFBFBF;
        overflow: hidden;
        padding: 10px 0;
        display: block;
        flaot: left;
        img {
          height: 100px;
          float: left;
        }
        h4 {
          float: left;
          margin: 0;
          line-height: 100px;
          margin-left: 20px;
          width: 100px;
        }
        p {
          float: left;
          margin: 0;
          width: 80px;
          line-height: 100px;
          margin-left: 35px;
          text-align: center;
        }
        .qty-minus {
          float: left;
          width: 20px;
          line-height: 100px;
          margin-left: 60px;
          text-align: center;
          cursor: pointer;
        }
        .qty {
          float: left;
          width: 20px;
          line-height: 100px;
          margin-left: 20px;
          text-align: center;
        }
        .qty-plus {
          float: left;
          width: 20px;
          line-height: 100px;
          margin-left: 20px;
          text-align: center;
          cursor: pointer;
        }
        .del {
          float: left;
          width: 80px;
          line-height: 100px;
          margin-left: 60px;
          cursor: pointer;
          text-decoration: underline;
          color: #ED277F;
        }
        .totalprice {
          float: left;
          width: 80px;
          line-height: 100px;
          margin-left: 10px;
          text-align: right;
        }
        .row p::before, .box p::before, .totalprice::before {
          content: "$";
        }
      }
      h5 {
        font-size: 1.2rem;
        text-align: right;
      }
    }
  }

</style>
