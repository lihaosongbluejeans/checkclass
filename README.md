# mini-program booking classroom
这是我的综合实践设计模块，预约教室模块
使用 微信开发工具+微信云开发
主要是数据库比较难设计，本人以前没有接触过小程序，从零开始探索，加上云开发又是没接触过的json数据库
所以对于数据库的设计比较花时间，大致学习了一遍mongodb 并查阅小程序官方文档，综合设计出来的可以查和改的数据库结构


![image](https://user-images.githubusercontent.com/44400254/143726870-f399c627-20b8-4898-80ec-a89e1d5ba447.png)



{
	"_id": "1",
	"name": "4B101",
	"floor": "1",
	"alaborary": "第四教学楼B区",
	"kebiao": [
		{
			"week": 1,
			"day": 1,
			"sw1": 1,
			"sw2": 0,
			"xw1": 0,
			"xw2": 1,
			"ws": 1
		},
		{
			"week": 1,
			"day": 2,
			"sw1": 1,
			"sw2": 0,
			"xw1": 0,
			"xw2": 1,
			"ws": 1
		}
   ]
}



就是这个结构，查比较好查，主要是修改有问题，因为云数据库的修改虽然是支持内置文档的对象修改，但是需要指定数组下标，也就是kebiao字段的数组下标
但是一个学期有17周，一周有5天，一共就是85个对象，如果用下标的话，可以，但是下标不支持变量，也就是说基本要用85个if才可以，这不现实
所以想到了当天的课表用完就可以删除，这样每次修改对象的时候下标都是 0 就ok了，然后就开始查云函数node.js ，很好，确实有定时器触发，我把定时设成每天的23点59分删除当天的课表
大功告成。数据库ok了项目已经成功了90%
其实还有一个想法是另外加一个表作为预约成功的判断，查询出来原本的课表再查预约成功的课程有没有当前的那节课，觉得也可行，就是每次都多一次判断和数据库查询，还是不如一个表来的简单
数据库设计也是弯弯曲曲的，从一开始没有考虑到课表，只管查询的时候设计的课表就是上午 下午 晚上 ，到后来考虑到每周和每天的课不一样，改成设计出一天一个数据库，
通过前缀后缀名拼串来实现查哪天的数据库，例如Frist_Mon第一周的周一，当时正在调试没有发现这玩意竟然要设计85个表，所以当即就放弃了，开始考虑内置文档存储
内置文档设计也不成熟，主要还是没有了解过这种数据库，当时的想法是把课表数据部分设计成5个数组Mon Tue Wed 每个数组里是17个对象{week：first 上午下午晚上}{week：second 上午下午晚上}
这种需要判断五次查哪个数组，用了几天这个发现这也不行，查询语句写太多了。最后突然就想到了现在的这个数据库。也是比较幸运，
预约的页面比较好写，虽然预约的界面代码比展示教室的界面多点，可能是因为是有写教室展示界面的经验了，顺理成章就出来了
