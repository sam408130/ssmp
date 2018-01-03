唯品会商品数据爬取主要分为4个部分：
* 品牌
* 品牌商品列表数据
* 商品详情数据
* 库存检测方法



## 品牌

唯品会app和小程序首页的限时活动每天都很多，多达几百个，而且进行活动的品类不完全符合我们的测试内容。所以选择首页女装品牌分类为入口，先爬取所有的女装品牌（279个），爬取入口在唯品会分类页，如下图。

![](/assets/屏幕快照 2018-01-03 上午11.52.32.png)

数据结构如下图：

![](/assets/屏幕快照 2018-01-03 上午11.56.29.png)


* pic 品牌活动图
* sliderCode 目前不知道干嘛用的
* url 详情页链接
* top_pc_img 品牌活动页宣传图
* shopIntroduce 品牌介绍
* title 品牌名称
* brand_id 品牌id（会出现没有品牌id的数据，应该是唯品会组织的专场）
* warehouse 仓库（VIP_NH-广州仓，VIP_CD-成都仓，VIP_SH-上海仓，VIP_BJ-北京仓）


## 品牌商品列表数据

![](/assets/屏幕快照 2018-01-03 下午12.04.12.png)

品牌列表的数据存放在vip_brand_item_list中（测试环境叫vip_category_list），表结构如下：


        command = (
            "CREATE TABLE IF NOT EXISTS {} ("
            "`id` INT(8) NOT NULL AUTO_INCREMENT,"
            "`product_id` BIGINT NOT NULL,"
            "`product_name` TEXT NOT NULL,"
            "`vipshop_price` INT(10),"
            "`market_price` INT(10),"
            "`vip_discount` CHAR(10),"
            "`sku_id` BIGINT,"
            "`small_image` TEXT,"
            "`state` INT(2) NOT NULL,"
            "`v_spu_id` BIGINT NOT NULL,"
            "`brand_id` BIGINT NOT NULL,"
            "`brand_name` TEXT,"
            "`brand_store_logo` TEXT,"
            "`brand_store_sn` BIGINT,"
            "`sell_time_to` DATETIME,"
            "`sell_time_from` DATETIME,"
            "`mid` BIGINT NOT NULL,"
            "`create_time` DATETIME NOT NULL,"
            "`content` TEXT,"
            "`finished` INT(2),"
            "PRIMARY KEY(id),"
            "UNIQUE KEY `v_spu_id` (`v_spu_id`)"
            ") ENGINE=InnoDB".format(config.vip_category_list_table)
        )
        

其中content存放的原始json数据，目前只爬取了第一页的120个数据。商品的唯一id是v_spu_id, 同款不同颜色会有不同的mid和product_id，这一点要注意。

## 商品详情

因为网页上的不同颜色被当做了不同商品，因此商品数据是从app爬取的，app返回数据结构比较杂乱，保存形式如下：

唯品会app商品详情页的数据是同个body中传入不同的方法来获取的：

```
    def start_requests(self):
        command = "SELECT * FROM {0} WHERE `finished` = \'{1}\' ".format(config.vip_category_list_table, 0)
        data = self.sql.query(command)
        for i, item in enumerate(data):
            utils.log('parse item:%s' % (item[2]))
            timestamp = int(time.time()*1000)
            formData = [{
                "method": "getProductSlide",
                "params": {
                    "page": "product-%s-%s.html" % (item[10], item[1]),
                    "query": ""
                },
                "id": timestamp,
                "jsonrpc": "2.0"
            },{
                "method": "getProductMultiColor",
                "params": {
                    "page": "product-%s-%s.html" % (item[10], item[1]),
                    "query": ""
                },
                "id": timestamp+1,
                "jsonrpc": "2.0"
            },{
                "method": "getProductSize",
                "params": {
                    "page": "product-%s-%s.html" % (item[10], item[1]),
                    "query": ""
                },
                "id": timestamp+2,
                "jsonrpc": "2.0"
            }]
            formdata = json.dumps(formData)
            yield FormRequest(
                url = 'https://m.vip.com/server.html?rpc',
                body = formdata,
                headers = self.headers,
                callback = self.get_item,
                meta = {
                    'v_spu_id': item[9],
                    'brand_id': item[10]
                }
            )
```
如上面代码formData是post请求中body的内容，接口返回数据也是一个数组，分别是请求方法对应的数据。所以我将上面三个方法获取原始数据分别存在了相应字段中：

![](/assets/屏幕快照 2018-01-03 下午3.08.18.png)

商品详情数据和品牌数据在product_size中重复返回了，所以就没有专门请求product_detail接口。

## 库存检测

查询库存情况的接口是：

https://stock.vip.com/detail/?callback=stock_detail&merchandiseId=310876851

其中merchandiseId是对应vip_brand_item_list中的mid




