<template>
  <div id="app">
    <v-container grid-list-lg>
      <v-layout row wrap>
        <v-flex md8 xs12 class="m-col-l pr-3">
          <div class="top-info">
            <h4 class="headline font-weight-light">List items</h4>
          </div>
          <div id="product" class="mt-3">
            <v-layout justify-center row wrap>
              <v-flex md4 xs12 v-for="(item, index) in items" :key="index">
                <v-card class="item-card mate-card1">
                  <v-img
                    :src="item.img"
                  ></v-img>
                  <div class="box-ctn-detail">
                    <h1 class="text-xs-left">{{item.title}}</h1>
                    <h3 class="text-xs-left">
                      $ {{item.price}}
                    </h3>
                    <v-btn fab dark class="btn-add">
                      <v-icon dark @click="addItem(index)">add</v-icon>
                    </v-btn>
                  </div>
                </v-card>
              </v-flex>
            </v-layout>
          </div>
        </v-flex>
        <v-flex md4 xs12 class="m-col-r pl-3">
          <div class="top-info">
            <h4 class="headline font-weight-light">Shopping Cart</h4>
          </div>
          <div class="table-price mt-3">
            <v-layout justify-center row wrap>
              <v-flex xs12>
                <div class="mate-card1">
                  <v-toolbar class="toolbar-cart pb-4" flat color="white">
                    <v-icon medium white>shopping_cart</v-icon>
                    <v-chip small style="background-color:#6394F8" text-color="white" v-if="totalClick">{{totalClick}}</v-chip>
                    <v-spacer></v-spacer>
                    <div class="text-xs-right"><span style="color:#ABB0BE;">Total:</span> <span style="color:#6394F8;">$ {{syncTotal}}</span>
                    </div>
                  </v-toolbar>
                  <v-data-table
                    :items="buyitems"
                    hide-actions
                    hide-headers
                    class="box-cart-checkout"
                  >
                    <template slot="items" slot-scope="props" v-if="props.item.click">
                      <td class="text-xs-left">
                        <div class="item-short-view">
                          <v-img class="img-thumb" :src="props.item.img"></v-img>
                          <v-card class="info-item-ic">
                            <v-card-title class="item-name">
                              {{ props.item.title }}
                              <v-spacer></v-spacer>
                              <v-tooltip bottom>
                                <v-icon
                                  slot="activator"
                                  small
                                  @click="hideItem(props.index)">delete
                                </v-icon>
                                <span>Remove</span>
                              </v-tooltip>
                            </v-card-title>
                            <v-card-title class="item-price">$ {{ props.item.price }}</v-card-title>
                            <v-card-actions>
                              <div class="item-quantity">
                                <span style="color:#ABB0BE;">Quantity : </span>
                                <v-tooltip bottom>
                                  <v-icon
                                    slot="activator"
                                    small
                                    outline
                                    class="mr-1"
                                    @click="addItem(props.index)">add
                                  </v-icon>
                                  <span>Increase</span>
                                </v-tooltip>
                                {{ props.item.click }}
                                <v-tooltip bottom>
                                  <v-icon
                                    slot="activator"
                                    small
                                    outline
                                    class="ml-1"
                                    @click="removeItem(props.index)">remove
                                  </v-icon>
                                  <span>Decrease</span>
                                </v-tooltip>
                              </div>
                              <div class="item-price-total">$ {{ props.item.click * props.item.price }}</div>
                            </v-card-actions>
                          </v-card>
                        </div>
                      </td>
                    </template>
                    <template slot="footer">
                      <td class="text-xs-center" :colspan="headers.length">
                        <div class="box-foo">
                          <v-btn block depressed class="checkout-btn">Checkout</v-btn>
                        </div>
                      </td>
                    </template>
                  </v-data-table>
                </div>
              </v-flex>
              <v-snackbar
                v-model="closeSnack"
                :timeout="0"
                bottom
              >
                {{ text }}
                <v-btn
                  color="pink"
                  flat
                  @click="applyDiscount"
                >
                  Apply
                </v-btn>
                <v-btn
                  color="pink"
                  flat
                  @click="closeSnack = false"
                >
                  Close
                </v-btn>
              </v-snackbar>
            </v-layout>
          </div>
        </v-flex>
      </v-layout>
    </v-container>
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
          },
          {
            img: "https://chenyiya.com/codepen/product-1.jpg",
            title: "Beer Bottle 2",
            price: "55",
            id: "beer2"
          },
          {
            img: "https://chenyiya.com/codepen/product-2.jpg",
            title: "Eco Bag 2",
            price: "71",
            id: "eco-bag2"
          },
          {
            img: "https://chenyiya.com/codepen/product-3.jpg",
            title: "Paper Bag 2",
            price: "32",
            id: "paper-bag2"
          }
        ],
        buyitems: [
          {click: 0},
          {click: 0},
          {click: 0},
          {click: 0},
          {click: 0},
          {click: 0}
        ],
        headers: [
          {text: 'Item name', value: 'title'},
          {text: 'Price', value: 'price'},
          {text: 'Quantity', value: 'quantity', align: 'center'},
          {text: 'Total', value: 'total', align: 'right'},
          {text: '', value: 'remove', align: 'right', sortable: false,},
        ],
        total: 0,
        totalClick: 0,
        text: 'You have a 20% discount. Use now ?',
        discount: false,
        closeSnack: false
      }
    },
    computed: {
      syncTotal() {
        this.total = 0;
        this.totalClick = 0;
        this.buyitems.forEach(i => {
          this.total += i.click * parseInt(i.price ? i.price : 0);
          this.totalClick += i.click
        });
        if (this.discount) {
          return this.total = parseInt(this.total * 0.8)
        }
        if (this.total >= 1000) {
          this.closeSnack = true
        }
        return this.total;
      }
    },
    methods: {
      addItem(index) {
        this.buyitems[index].title = this.items[index].title;
        this.buyitems[index].price = this.items[index].price;
        this.buyitems[index].img = this.items[index].img;
        this.buyitems[index].click += 1
        this.buyitems[index].showItem = true
      },
      removeItem(index) {
        if (this.buyitems[index].click > 0) {
          this.buyitems[index].click -= 1
        } else if (this.buyitems[index].click = 0) {
          this.buyitems[index].showItem = false
        }
      },
      hideItem(index) {
        this.buyitems[index].click = 0;
        this.buyitems[index].showItem = false;
      },
      applyDiscount() {
        this.discount = true;
        this.closeSnack = false;
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
      .toolbar-cart {
        .v-toolbar__content {
          border-bottom: 0px solid rgba(0, 0, 0, 0.12);
          background-color: #fff;
          position: relative;
          &:after {
            position: absolute;
            height: 1px;
            width: calc(100% - 40px);
            background-color: rgba(0, 0, 0, 0.12);
            bottom: 0;
            left: 0;
            right: 0;
            content: '';
            margin: 0 auto;
          }
        }
      }
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
        &.mate-card1 {
          border: 1px solid #dfe1e5;
        }
        .box-ctn-detail {
          position: relative;
          .btn-add {
            position: absolute;
            top: -31px;
            right: 3px;
            background-color: #6394F8;
            width: 48px;
            height: 48px;
          }
          h1 {
            margin-left: 20px;
            padding-top: 20px;
            font-size: 18px;
            font-weight: 500;
          }
          h3 {
            margin-left: 20px;
            padding-bottom: 20px;
            padding-top: 5px;
            font-size: 18px;
            font-weight: 400;
          }
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

  .box-cart-checkout {
    .v-table {
      tbody {
        tr {
          border-bottom: none !important;
          td {
            padding: 0 12px !important;
            background-color: #FFF !important;
          }
        }
      }
      tfoot {
        tr {
          border-top: none;
          td {
            padding: 0 12px;
            .box-foo {
              padding-left: 10px;
              padding-right: 10px;
            }
          }
        }
      }
    }
    .checkout-btn {
      &.v-btn {
        background-color: #6394F8 !important;
        color: #FFF;
        margin: 25px 0 25px 0;
        height: 46px;
        border-radius: 6px;
      }
    }
  }

  .mate-card1 {
    background-color: #fff;
    border: 1px solid #dfe1e5;
    border-radius: 6px;
    box-shadow: none;
    transition: box-shadow .3s linear;
    overflow: hidden;
    &:hover {
      box-shadow: 0 2px 10px 0px rgba(60, 64, 67, .15);
    }
  }

  .item-short-view {
    padding-bottom: 5px;
    margin-bottom: 0;
    background-color: #FFF;
    padding-top: 5px;
    padding-left: 10px;
    padding-right: 10px;
    .img-thumb {
      float: left;
      width: 70px;
      height: 70px;
      position: relative;
      z-index: 9;
    }
    .info-item-ic {
      padding-top: 0;
      padding-left: 80px;
      min-height: 70px;
      box-shadow: none;
      .item-name {
        padding: 0;
        font-weight: bold;
      }
      .item-price {
        padding: 5px 0 0 0;
      }
      .v-card__actions {
        padding: 5px 10px 10px 0;
      }
      .item-quantity {
        i {
          border-radius: 30px;
          border: 1px solid rgba(0, 0, 0, .12);
        }
      }
      .item-price-total {
        margin-left: 20px;
        color: #6394F8;
      }
      .item-remove {

      }
    }
  }
</style>
