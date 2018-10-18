# ajax 微博爬取

## url: https://m.weibo.cn/u/2830678474

## 如何查看是否AJAX 
* 定义：Asynchronous Javascript and XML,异步的Javascript和XML,利用js在页面不刷新、链接不改变的情况下，
与服务器交换数据并更新部分网页。
* 原理：实例化xmlhttp对象，使用此对象与后台通信 发送请求、解析内容、渲染网页
* 查看请求： 请求类型为xhr，Request Headers中有个信息为X-Requested-With:XMLHttpRequest,标记请求
    Preview看到响应内容，经过Chrome自动解析，Response为真实数据
    
## python模拟ajax请求
1. 分析请求  
    GET请求，请求链接https://m.weibo.cn/api/container/getIndex?type=uid&value=2830678474&containerid=1076032830678474&page=2
    参数有4个：type,value,containerid,page type始终为uid，value为页面链接的数字，用户ID，containerid是107603加上用户id,改变的值为
    page,控制分页
2. 分析响应  
    响应内容为JSON，关键信息cardlistinfo & cards。前者包含total，可估算分页数，后者为列表，10个元素，每个里面mblog较为重要，
    包含attitudes_count(赞数目),comments_count(评论数),reposts_count(转发数),created_at(发布时间),text(微博正文)，都是格式化的
3. 步骤：请求、解析、保存

## 重点部分
* 可以对比test.py和ajax_weibo.py，查看写作过程，注意点
* test是自己之前写的，漏洞很多，比如  
    `File "C:/Users/Lenovo/PycharmProjects/spider2/ajax_weibo/test.py", line 38, in parse_page
    for item in items:
TypeError: 'NoneType' object is not iterable`
* 参考代码优点：
    + 获取网页时return response.json() 以及 page
    + parse_page函数将page也作为参数，enumerate，主函数results=parse_page(*json)
