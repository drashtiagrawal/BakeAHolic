<template>
<div>
  <div class="layout">
        <Layout>
            <Header>
                <h2 class="header"> Bake-A-Holic</h2>
            </Header>
            <Layout>
              <Sider hide-trigger :style="{background: '#fff'}">
                    <Menu active-name="1-2" theme="light" width="" :open-names="['1']">
                        <Submenu name="1">
                            <template slot="title">
                                <Icon type="ios-navigate"></Icon>
                                Item 1
                            </template>
                            <!-- <MenuItem name="1-1">Option 1</MenuItem>
                            <MenuItem name="1-2">Option 2</MenuItem>
                            <MenuItem name="1-3">Option 3</MenuItem> -->
                        </Submenu>
                        <Submenu name="2">
                            <template slot="title">
                                <Icon type="ios-keypad"></Icon>
                                Item 2
                            </template>
                            <!-- <MenuItem name="2-1">Option 1</MenuItem>
                            <MenuItem name="2-2">Option 2</MenuItem> -->
                        </Submenu>
                        <Submenu name="3">
                            <template slot="title">
                                <Icon type="ios-analytics"></Icon>
                                Item 3
                            </template>
                            <!-- <MenuItem name="3-1">Option 1</MenuItem>
                            <MenuItem name="3-2">Option 2</MenuItem> -->
                        </Submenu>
                    </Menu>
                </Sider>
            <Layout :style="{padding: '0 50px'}">  
                <Breadcrumb :style="{margin: '16px 0'}">
                    <BreadcrumbItem>Home</BreadcrumbItem>
                    <BreadcrumbItem @click="showItems">Items</BreadcrumbItem>
                     <Button  type="primary" v-show="cart === true" style="float: right; margin-right:8%" @click="clearCart">Clear Cart</Button>
                    <Button  type="primary" v-show="cart === true" style="float: right; margin-right:1%" @click="showItems">Back to Items</Button>
                    <Icon type="md-cart" size=24 style="float: right; margin-right:1%;" @click="showCart" />
                    
                </Breadcrumb>
                
                
                <Content :style="{padding: '24px 0', minHeight: '500px', background: '#fff'}">
                    <Layout v-show="cart === false">
                        <Content :style="{padding: '24px', minHeight: '500px', background: '#fff'}">
                          <Row style="background:#eee;padding:20px">
                            <Col v-for="key in items" :key="key.itemName" span="6" offset="2">
                                <Card :bordered="false">
                                    <p slot="title">{{ key.itemName }}</p>
                                    <img src="../assets/bakeaholic.jpeg">
                                    <p>Rs: {{ key.itemPrice }}</p>
                                    <Button  type="primary" style="margin-top: 10px;" @click="addToCart(key)">Add to cart</Button>
                                </Card>
                            </Col>
                          </Row>
                        </Content>
                    </Layout>
                    <Layout v-show="cart === true">
                      <Content  :style="{padding: '24px', minHeight: '500px', background: '#fff'}">
                          <List item-layout="vertical">
                  <ListItem v-for="item in carItems" :key="item.itemName">
                      <ListItemMeta :title="item.itemName" :description="item.Number" />
                      {{ item.content }}
                      <template slot="action">
                          <li>
                              <Icon type="ios-star-outline" /> 123
                          </li>
                          <li>
                              <Icon type="ios-thumbs-up-outline" /> 234
                          </li>
                          <li>
                              <Icon type="ios-chatbubbles-outline" /> 345
                          </li>
                      </template> 
                  </ListItem>
              </List>
                        </Content>
                    </Layout>
                </Content>
            </Layout>
            </Layout>
            <Footer class="layout-footer-center">2011-2016 &copy; TalkingData</Footer>
        </Layout>
    </div>
</div>
</template>

<script>
/* eslint-disable */
// @ is an alias to /src
import axios from 'axios'
export default {
  name: 'home',
  data(){
    return{
      items: [],
      selectedItem: '',
      cart: false,
      carItems: []
    }
  },
  methods: {
    clearCart(){
       axios({
        method:'delete',
        url: 'http://3.85.235.191/v1/cart'
        }).then((response) => {
          console.log(response)
          this.showCart()
        }).catch((err) => {
          console.log(err)
        })
    },
    showCart: async function(){
      console.log("Called")
      this.carItems = []
      this.cart = true
      axios({
      method:'get',
      url: 'http://3.85.235.191/v1/cart'
      
      }).then((response) => {
        console.log("Cart", response.data)
        for(let key in response.data){
          if(response.data.hasOwnProperty(key)){
            let jsonObject =  {
              "itemName": key,
              "Number": response.data[key]
            }
            this.carItems.push(jsonObject)
          }
        }
        axios({
        method:'get',
        url: 'http://3.85.235.191/v1/cart'
        
        }).then((res) => {
          console.log("Cart", res.data)
          for(let key in res.data){
            if(res.data.hasOwnProperty(key)){
              let jsonObject =  {
                "itemName": key,
                "Number": res.data[key]
              }
              this.carItems.push(jsonObject)
            }
          }
          axios({
          method:'get',
          url: 'http://3.85.235.191/v1/cart'
          
          }).then((result) => {
            console.log("Cart", result.data)
            for(let key in result.data){
            if(result.data.hasOwnProperty(key)){
              let jsonObject =  {
                "itemName": key,
                "Number": result.data[key]
              }
              this.carItems.push(jsonObject)
            }
          }
          console.log(this.carItems)
            
          }).catch((err) => {
            console.log(err)
          })
        }).catch((err) => {
          console.log(err)
        })
      }).catch((err) => {
        console.log(err)
      })

    },
    showItems()
    {
      this.cart = false
    },
    addToCart(key){
      console.log(key.itemName)
      let cartData = {
        "itemName": key.itemName,
        "number": "1"
      }
      axios({
        method: 'post',
        data: cartData,
        url: 'http://3.85.235.191/v1/addItem'
      }).then((response) => {
        console.log("Response:", response)
      }).catch((err) => {
        console.log("Error Calling Service", err)
      })
    }
  },
  mounted(){
    // axios.get('http://3.85.235.191/v1/items').then((response)=> {
    //   console.log("Axios response", response)
    // })
    axios({
      method:'get',
      url: 'http://3.85.235.191/v1/items'
      // headers: {
      //   "Access-Control-Allow-Origin": '*',
      //   "Content-Type": "application/json"
          
      // }
    }).then((response)=> {
      for(let key in response.data){
        if(response.data.hasOwnProperty(key)){
          let jsonObject =  {
            "itemName": key,
            "itemPrice": response.data[key]
          }
          this.items.push(jsonObject)
        }
      }
    }).catch((err) => {
      console.log(err)
    })
  }
}
</script>

<style scoped>
.header{
  color: aliceblue
}
.layout{
    border: 1px solid #d7dde4;
    background: #f5f7f9;
    position: relative;
    border-radius: 4px;
    overflow: hidden;
}
.layout-logo{
    width: 100px;
    height: 30px;
    background: #5b6270;
    border-radius: 3px;
    float: left;
    position: relative;
    top: 15px;
    left: 20px;
}
.layout-nav{
    width: 420px;
    margin: 0 auto;
    margin-right: 20px;
}
</style>

