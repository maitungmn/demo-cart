<template>
  <div id="app">
    <div id="product">
      <v-container grid-list-md text-xs-center>
        <v-layout justify-center row wrap>
          <v-flex xs4 v-for="(item, index) in items" :key="index">
            <v-card class="item-card">
              <v-img
                :src="item.img"
              ></v-img>
              <h1 class="text-xs-left">{{item.title}}</h1>
              <h3 class="text-xs-left">
                $ {{item.price}}
              </h3>
              <v-btn fab dark class="btn-add">
                <v-icon dark @click="addItem(index)">add</v-icon>
              </v-btn>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </div>
    <div class="table-price">
      <v-container grid-list-md text-xs-center>
        <v-layout justify-center row wrap>
          <v-flex xs12>
            <v-data-table
              :headers="headers"
              :items="buyitems"
              class="elevation-1"
              hide-actions
            >
              <template slot="items" slot-scope="props" v-if="props.item.title">
                <td class="text-xs-left">{{ props.item.title }}</td>
                <td class="text-xs-left">$ {{ props.item.price }}</td>
                <td class="text-xs-center">
                  <v-tooltip bottom>
                    <v-icon
                      slot="activator"
                      class="mr-2"
                      small
                      @click="removeItem(props.index)">remove
                    </v-icon>
                    <span>Decrease</span>
                  </v-tooltip>
                  {{ props.item.click }}
                  <v-tooltip bottom>
                    <v-icon
                      slot="activator"
                      class="ml-2"
                      small
                      @click="addItem(props.index)">add
                    </v-icon>
                    <span>Increase</span>
                  </v-tooltip>
                </td>
                <td class="text-xs-right">
                  $ {{ props.item.click * props.item.price }}
                </td>
                <td class="text-xs-right del-box">
                  <v-tooltip bottom>
                    <v-icon
                      slot="activator"
                      small
                      @click="hideItem(props.index)">delete
                    </v-icon>
                    <span>Remove</span>
                  </v-tooltip>
                </td>
              </template>
              <template slot="footer">
                <td class="text-xs-right total-txt" :colspan="headers.length">
                  <strong>Total: $ {{syncTotal}}</strong>
                </td>
              </template>
            </v-data-table>
          </v-flex>
        </v-layout>
      </v-container>
    </div>
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
          {click: 0},
          {click: 0},
          {click: 0}
        ],
        headers: [
          {text: 'Shopping Cart', value: 'title'},
          {text: 'Price', value: 'price'},
          {text: 'Quantity', value: 'quantity', align: 'center'},
          {text: 'Total', value: 'total', align: 'right'},
          {text: '', value: 'remove', align: 'right', sortable: false,},
        ],
        total: 0
      }
    },
    computed: {
      syncTotal() {
        this.total = 0;
        this.buyitems.forEach(i => {
          this.total += i.click * parseInt(i.price ? i.price : 0)
        });
        return this.total;
      }
    },
    methods: {
      addItem(index) {
        this.buyitems[index].title = this.items[index].title;
        this.buyitems[index].price = this.items[index].price;
        this.buyitems[index].click += 1
      },
      removeItem(index) {
        if (this.buyitems[index].click > 0) {
          this.buyitems[index].click -= 1
        }
      },
      hideItem(index) {
        this.buyitems[index].click = 0;
        this.buyitems[index].title = ''
      }
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
    .table-price {
      max-width: 960px;
      margin: 0 auto;
      > .container {
        padding-top: 12px;
        .del-box {
          // border-left: 1px solid rgba(0,0,0,0.12);
          width: 50px;
          position: relative;
          &:before {
            position: absolute;
            content: '';
            width: 1px;
            height: 30px;
            background-color: rgba(0, 0, 0, 0.12);
            left: 0;
            top: 10px;
          }
        }
        .total-txt {
          padding: 24px;
          font-size: 18px;
        }
      }
    }
    #product {
      max-width: 960px;
      margin: 0 auto;
      > .container {
        padding-bottom: 12px;
      }
      .item-card {
        position: relative;
        .btn-add {
          position: absolute;
          position: absolute;
          top: 189px;
          right: 9px;
          background-color: #27ae60;
        }
        h1 {
          margin-left: 20px;
          padding-top: 30px;
          font-size: 24px;
        }
        h3 {
          margin-left: 20px;
          padding-bottom: 20px;
          padding-top: 15px;
          font-size: 20px;
          font-weight: 400;
        }
      }
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
