<template>
<div>
  <div class="layout">
        <Layout>
            <Header>
                <h2 class="header"> Bake-A-Holic</h2>
            </Header>
            <Layout :style="{padding: '0 50px'}">
                <Breadcrumb :style="{margin: '16px 0'}">
                    <BreadcrumbItem>Home</BreadcrumbItem>
                    <BreadcrumbItem>Items</BreadcrumbItem>
                </Breadcrumb>
                <Content :style="{padding: '24px 0', minHeight: '280px', background: '#fff'}">
                    <Layout>
                        <Sider hide-trigger :style="{background: '#fff'}">
                        </Sider>
                        <Content :style="{padding: '24px', minHeight: '280px', background: '#fff'}">
                          <Row style="background:#eee;padding:20px">
                            <Col v-for="key in items" :key="key.itemName" span="6" offset="2">
                                <Card :bordered="false">
                                    <p slot="title">{{ key.itemName }}</p>
                                    <img src="../assets/bakeaholic.jpeg">
                                    <p>Rs: {{ key.itemPrice }}</p>
                                    <Button type="primary" style="margin-top: 10px;">Add to cart</Button>
                                </Card>
                            </Col>
                        </Row>
                        </Content>
                    </Layout>
                </Content>
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
</style>

