唯品会商品数据爬取主要分为3个部分：

## 1.品牌

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
