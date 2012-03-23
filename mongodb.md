\thispagestyle{empty}
\changepage{}{}{}{-1.5cm}{}{2cm}{}{}{}
![The Little MongoDB Book, By Karl Seguin](title.png)\

\clearpage
\changepage{}{}{}{1.5cm}{}{-2cm}{}{}{}

## 关于本书 ##

### 许可证 ###
这本书，The Little MongoDB Book，基于Attribution-NonCommercial 3.0 Unported license发布。**您不需要为本书付钱。**

您有权复制、分发、修改或展示本书。但请认可本书的作者是Karl Seguin，也请勿将其用于任何商业用途。

您可以在以下链接查看该许可证的全文：

<http://creativecommons.org/licenses/by-nc/3.0/legalcode>

### 关于作者 ###
Karl Seguin是一位在多个技术领域有着丰富经验的研发人员，精通.NET以及Ruby。作为技术文档撰写人，他有时还会参与OSS项目的工作或做演讲。在MongoDB方面，他曾是C# MongoDB library NoRM的核心开发人员，开发了互动教程[mongly](http://mongly.com)以及[Mongo Web Admin](https://github.com/karlseguin/Mongo-Web-Admin)。他还用MongoDB开发了[mogade.com](http://mogade.com/)为业余游戏开发者提供免费服务。

Karl还著有[The Little Redis Book](http://openmymind.net/2012/1/23/The-Little-Redis-Book/)。

他的博客<http://openmymind.net>，推特账号是[@karlseguin](http://twitter.com/karlseguin)。

### 致谢 ###
Perry Neal，谢谢你给我无法衡量的帮助，谢谢你锐利的眼光、独到的思路以及无比的热情。非常感谢。

### 最新版本 ###
本书的最新版本可以在下面的链接找到：

<http://github.com/karlseguin/the-little-mongodb-book>.

### 关于本书的中文版本 ###
本书的中文版由[justinyhuang](http://justinyhuang.com)完成，基于与原著相同的许可证。最新版本在xxxxxxxx。译文的纰漏欢迎告知<justin.y.huang@live.com>或直接提交github。

\clearpage

## 简介 ##
 > 本章很短，但不是我的错，MongoDB就是那么简单易学。

人们总是说科技的发展风驰电掣。确实，一直以来都不断有新的技术涌现出来。但是我却一直坚持认为程序员所用的基本技术的发展相对而言就缓慢很多。您可以很多年不学习什么但还是可以混过去。让人瞩目的是业已成熟的技术被替代的速度。仿佛一夜间，那些长期以来业已成熟的技术忽然就失去了开发者的关注，昔日地位岌岌可危。

NoSQL逐步攻陷了传统关系数据库的领地，就是这种急剧转变最好的例子。仿佛就在昨天所有的网页还是由一些RDBMS驱动的，而一早起来就已经有大约5种NoSQL的方案证明了他们都是有价值的解决方案。

虽然这些转变看起来是一夜间就发生的，事实却是这些新生的技术历经多年才被接受并应用于实践。一开始先是由一小部分开发者或者企业推动，然后逐步吸取教训，改善方案并见证新技术地位的确立。其他的跟随者之后也慢慢地开始了尝试。对于NoSQL来说也是一样。很多新方案的出现都不是为了去代替更加传统的存储方案，而是为了填补后者所能满足需求之外的一些空白。

说了那么多，在这里我们首先要做的是弄清楚什么是NoSQL。这是一个很宽泛的概念，不同的人有不同的解读。我个人用NoSQL来泛指参与数据存储的系统。换句话说，NoSQL（还是我个人的意见），是一种观念，这种观念认为your persistence layer isn't necessarily the responsibility of a single system。相比之下，关系数据库的缔造者一开始就力图把他们的软件当作通用解决方案。NoSQL则更倾向于负责系统中的一小部分功能：限定了部分功能，便可以使用最适合的工具。因此，您以后的NoSQL架构中依旧有可能利用到关系数据库，比如说MySQL，而与此同时还会用Redis完成系统特定数据的持久化查询（？？），还会用Hadoop进行大量的数据处理。简而言之，NoSQL就是保持开放和警醒，利用已有的可用的工具和方法去管理您的数据。

您也许会想，MongoDB怎么能搞定那么多？作为一个面向文档的数据库，Mongo是一个更加通用的NoSQL方案。可以认为它是关系数据库的一个替代方案。和关系数据库一样，Mongo也可以和其它更细化的NoSQL方案协作而更加强大。MongoDB既有优点也有缺点，我们在后续的章节中都会提及。

您也许也注意到了，在书中我们是混用Mongo和MongoDB这两个词的。

## 准备 ##
本书大部分篇幅会用来关注MongoDB的核心功能。所以我们基本上使用的是MongoDB的外壳（shell）。shell在学习MongoDB还有管理数据库的时候很有用，不过您的实际代码还是会用相应的语言来驱动mongoDB的。

这也引出了关于MongoDB您首先需要了解的东西：它的驱动。MongoDB有许多针对不同语言的[官方驱动](http://www.mongodb.org/display/DOCS/Drivers)。可以认为这些驱动和您所熟知的各种数据库驱动是一样的。基于这些驱动，MongoDB的开发社区又搭建了更多语言/框架相关的库。比如说[NoRM](https://github.com/atheken/NoRM)就是一个实现了LINQ的C#库，还有[MongoMapper](https://github.com/jnunemaker/mongomapper)，一个很好地支持ActiveRecord的Ruby库。您可以自行决定直接针对MongoDB的核心驱动编程，或者采用一些高层的库。在这里指出这点，是因为不少MongoDB的新手都会为既有官方驱动又有社区提供的库而困惑不已——前者着重与MongoDB的核心通讯/连接，而后者则提供了更多语言/框架相关的具体实现。

我建议您在阅读本书的同时，也在MongoDB中尝试我给出的例子。如果在这个过程中您自己发现了什么问题，也可以在MongoDB环境中探索需求答案。安装并运行MongoDB其实很简单，只需要几分钟的时间。那么现在就开始吧。

1. 从[官方下载页面](http://www.mongodb.org/downloads)的第一行（这是推荐的稳定版本）下载与您操作系统相应的安装包。根据不同的开发需要，选择32位或是64位的包。

2. 解压下载的包（到任意路径）并进入`bin`子目录，暂且不要执行任何命令。让我先介绍一下，`mongod`将启动服务器进程而`mongo`会打开客户端的shell——大部分时间我们将和这两个可执行文件打交道。

3. 在`bin`子目录中创建一个新的文本文件，取名为`mongodb.config`。

4. 在mongodb.config中加一行：`dbpath=PATH_TO_WHERE_YOU_WANT_TO_STORE_YOUR_DATABASE_FILES`。例如，在Windows中您需要添加的可能是`dbpath=c:\mongodb\data`而在Linux下可能就是`dbpath=/etc/mongodb/data`。

5. 确认您指定的`dbpath`是存在的。

6. 执行mongod，带上参数`--config /path/to/your/mongodb.config`。

以Windows用户为例，如果您把下载的的文件解压到`c:\mongodb\`，创建了`c:\mongodb\data\`，然后在`c:\mongodb\bin\mongodb.config`中添加了`dbpath=c:\mongodb\data\`。那么您就可以在命令行中输入以下指令来启动`mongod`：

`c:\mongodb\bin\mongod --config c:\mongodb\bin\mongodb.config`

您可以把这个`bin`加入到您的默认路径中省得每次都要输入完整的路径。对于MacOSX和Linux的用户，方法也是几乎一样的。唯一的区别在于路径不同。

我希望您现在已经安装并可以运行MongoDB了。如果您遇到什么错误，注意看输出的错误信息——服务器（server）很善于解释究竟是哪里出了问题。

此时您可以运行`mongo`了（没有*d*），它会启动一个shell并连接到运行中的服务器。输入'db.version()`以确认所有的东西都正常工作：您应该可以看到您所安装的软件版本。

\clearpage

## 第一章 - 基础 ##
从了解基本的构成开始，我们开始踏上MongoDB探索之路。显然，这是认识MongoDB的关键，同时也有助于搞清楚MongoDB适用范围的高层次问题。

作为开始，我们需要了解6个简单的概念：

1. MongoDB有着与您熟知的‘数据库’（database，对于Oracle就是‘schema’）一样的概念。在一个MongoDB的实例中您有若干个数据库或者一个也没有，不过这里的每一个数据库都是高层次的容器，用来储存其他的所有数据。

2. 一个数据库可以有若干‘集合’（collection），或者一个也没有。集合和传统概念中的‘表’有着足够多的共同点，所以您大可认为这两者是一样的东西。

3. 集合由若干‘文档’（document）组成，也可以为空。类似的，可以认为这里的文档就是‘行’。

4. 文档又由一个或者更多个‘域’（field）组成，您猜的没错，域就像是‘列’。

5. ‘索引’（index）在MongoDB中的意义就如同索引在RDBMS中一样。

6. ‘游标’（cursor）和以上5个概念不同，它很重要但是却常常被忽略，有鉴于此我认为应该进行专门讨论。关于游标有一点很重要，就是每当向MongoDB索要数据时，它总是返回一个游标。基于游标我们可以作诸如计数或是直接跳过之类的操作，而不需要真正去读数据。

小结一下，MongoDB由‘数据库’组成，数据库由‘集合’组成，集合由‘文档’组成。‘域’组成了文档，集合可以被‘索引’，从而提高了查找和排序的性能。最后，我们从MongoDB读取数据的时候是通过‘游标’进行的，除非需要，游标不会真正去作读的操作。

您也许已经觉得奇怪，为什么要用新的术语（表换成集合，行换成文档，列换成域），这不是越弄越复杂了么？这是因为虽然这些概念和那些关系数据库中的相应概念很相似，但是还是存在差异的。
关键的差异在于关系数据库是在‘表’这一层次定义‘列’的，而一个面向文档的数据库则是在‘文档’这一层次定义‘域’的。也就是说，集合中的每个文档都可以有独立的域。因此，集合相对于表来说是一个简化了的容器，而文档则包含了比行要多得多的信息。（？？）

虽然这些异同很重要，但是如果您现在还没搞清楚也不必担心。以后试着插入几次（数据）就知道我们这里说的是什么了。最后，集合对其中储存的内容并没有严格的要求（它是无模式的（schema-less）），Fields are tracked with each individual document（？？）。当中的优缺点我们会在后续的章节中继续探讨。

开始动手实践吧。如果您还没有运行Mongo，现在就可以启动`mongod`服务器以及Mongo的shell。Mongo的shell运行在JavaScript之上，您可以执行一些全局的指令，如`help`或者`exit`。操作对象`db`来执行针对当前数据库的操作，例如`db.help()`或是`db.stats()`。操作对象`db.COLLECTION_NAME`来执行针对某一给集合的操作，比如说`db.unicorns.help()`或是`db.unicorns.count()`。我们以后将会有许多针对集合的操作。

试试输入`db.help()`，您会看到一串命令列表，这些命令都可以用来操作`db`对象。

顺带说一句，因为我们用的是JavaScript的shell，如果您执行一个命令而忘了加上`()`，您看到的将是这个命令的实现而并没有执行它。知道这个，您在这么做并看到以`function(...){`开头的信息的时候就不会觉得惊讶了。比如说如果您输入`db.help`（后面没有括弧），你就将看到`help`命令的具体实现。

首先我们用全局命令`use`来切换数据库。输入`use learn`。这个数据库是否存在并没有关系，我们创建第一个集合后这个`learn`数据库就会生成的。现在您应该已经在一个数据库里面了，可以执行一些诸如`db.getCllectionNames()`的数据库命令了。如果您现在就这么做，将会看到一个空的数组（`[]`）。因为集合是无模式的，我们不需要专门去创建它。我们要做的只是把一个文档插入一个新的集合，这个集合就生成了。您可以像下面一样调用`insert`命令去插入一个文档：

	db.unicorns.insert({name: 'Aurora', gender: 'f', weight: 450})

以上命令对`unicorns`对象执行`insert`操作，并传入一个参数。在MongoDB内部，数据是以二进制的串行JSON格式存储的。这对外部的用户而言，意味着JSON的大量应用，就如同上面的参数一样。如果我们现在执行`db.getCollectionNames()`，将看到两个集合：`unicorns`以及`system.indexes`。`system.indexes`在每个数据库中都会创建，它包含了数据库中的索引信息。

现在您可以对`unicorns`对象执行`find`命令，得到一个文档列表:

	db.unicorns.find()

请注意，除了您在文档中输入的各个域，还有一个一个叫做`_id`的域。每一个文档都必须有一个独立的`_id`域。您可以自己创建，也可以让MongoDB为您生成这个ObjectId。大部分情况下您还是会让MongoDB为您生成ID的。默认情况下，`_id`域是被索引了的——这就是为什么会有`system.indexes`创建出来的原因。看看`system.indexes`有什么：

	db.system.indexes.find()

在结果中您会看到该索引的名字，它所绑定的数据库和集合的名字，还有包含这个索引的域。

回到我们前面关于无模式集合的讨论。现在往`unicorns`插入一个完全不同的文档，比如说这样：

	db.unicorns.insert({name: 'Leto', gender: 'm', home: 'Arrakeen', worm: false})

再次用`find`可以列出所有的文档。学习到后面，我们将继续讨论MongoDB无模式的这一有趣的行为，不过我希望您已经开始慢慢了解为什么传统的那些术语不适合用在这里了。

### 掌握选择器（selector） ###
除了刚才介绍过的6个概念，MongoDB还有一个很实用的概念：查询选择器（query selector），在进入更高阶的内容之前，您也需要很好的了解它是什么。
MongoDB的查询选择器就像SQL代码中的`where`语句。因此您可以用它在集合中查找，统计，更新或是删除文档。选择器就是一个JSON对象，最简单的形式就是`{}`，用来匹配所有的文档(`null`也可以）。如果我们需要找到所有雌性的独角兽(unicorn)，我们可以用选择器`{gender:'f'}`来匹配。

在深入选择器之前，我们先输入一些数据以备后用。首先用`db.unicorns.remove()`删除之前我们在`unicorns`集合中输入的所有数据。（由于在这条命令中我们没有指定选择器，于是所有的文档都将被清除）。然后用下面的插入命令准备一些数据（建议拷贝粘帖这些命令）：

	db.unicorns.insert({name: 'Horny', dob: new Date(1992,2,13,7,47), loves: ['carrot','papaya'], weight: 600, gender: 'm', vampires: 63});
	db.unicorns.insert({name: 'Aurora', dob: new Date(1991, 0, 24, 13, 0), loves: ['carrot', 'grape'], weight: 450, gender: 'f', vampires: 43});
	db.unicorns.insert({name: 'Unicrom', dob: new Date(1973, 1, 9, 22, 10), loves: ['energon', 'redbull'], weight: 984, gender: 'm', vampires: 182});
	db.unicorns.insert({name: 'Roooooodles', dob: new Date(1979, 7, 18, 18, 44), loves: ['apple'], weight: 575, gender: 'm', vampires: 99});
	db.unicorns.insert({name: 'Solnara', dob: new Date(1985, 6, 4, 2, 1), loves:['apple', 'carrot', 'chocolate'], weight:550, gender:'f', vampires:80});
	db.unicorns.insert({name:'Ayna', dob: new Date(1998, 2, 7, 8, 30), loves: ['strawberry', 'lemon'], weight: 733, gender: 'f', vampires: 40});
	db.unicorns.insert({name:'Kenny', dob: new Date(1997, 6, 1, 10, 42), loves: ['grape', 'lemon'], weight: 690,  gender: 'm', vampires: 39});
	db.unicorns.insert({name: 'Raleigh', dob: new Date(2005, 4, 3, 0, 57), loves: ['apple', 'sugar'], weight: 421, gender: 'm', vampires: 2});
	db.unicorns.insert({name: 'Leia', dob: new Date(2001, 9, 8, 14, 53), loves: ['apple', 'watermelon'], weight: 601, gender: 'f', vampires: 33});
	db.unicorns.insert({name: 'Pilot', dob: new Date(1997, 2, 1, 5, 3), loves: ['apple', 'watermelon'], weight: 650, gender: 'm', vampires: 54});
	db.unicorns.insert({name: 'Nimue', dob: new Date(1999, 11, 20, 16, 15), loves: ['grape', 'carrot'], weight: 540, gender: 'f'});
	db.unicorns.insert({name: 'Dunx', dob: new Date(1976, 6, 18, 18, 18), loves: ['grape', 'watermelon'], weight: 704, gender: 'm', vampires: 165});

现在我们有足够的数据，我们可以来掌握选择器了。`{field: value}`用来查找所有`field`等于`value`的文档。通过`{field1: value1, field2: value2}`的形式可以实现`与`操作。`$lt`、`$lte`、`$gt`、`$gte`以及`$ne`分别表示小于、小于或等于、大于、大于或等于以及不等于。举个例子，查找所有体重超过700磅的雄性独角兽的命令是：

	db.unicorns.find({gender: 'm', weight: {$gt: 700}})
	//或者 (效果并不完全一样，仅用来为了演示不同的方法)
	db.unicorns.find({gender: {$ne: 'f'}, weight: {$gte: 701}})

`$exists`操作符用于匹配一个域是否存在，比如下面的命令：

	db.unicorns.find({vampires: {$exists: false}})
会返回单个文档（译者：只有这个文档没有vampires域）。如果需要*或*而不是*与*，可以用`$or`操作符并作用于需要进行或操作的数组：

	db.unicorns.find({gender: 'f', $or: [{loves: 'apple'}, {loves: 'orange'}, {weight: {$lt: 500}}]})

以上命令返回所有或者喜欢苹果，或者喜欢橙子，或者体重小于500磅的雌性独角兽。

您可能已经注意到了，在最后的一个例子中有一个非常棒的特性：`loves`域是一个数组。在MongoDB中数组是一级对象(first class object)。这是非常非常有用的功能。一旦用过，没有了它你可能都不知道怎么活下去。更有意思的是基于数组的选择是非常简单的：`{loves: 'watermelon'}`就会找到`loves`中有`watermelon`这个值的所有文档。
除了我们目前所介绍过的，还有更多的操作符
可以使用。最灵活的是`$where`，允许输入JavaScript并在服务器端运行。这些都在MongoDB网站的[Advanced Queries](http://www.mongodb.org/display/DOCS/Advanced+Queries#AdvancedQueries)部分有详细介绍。不过这里介绍的都是基本的命令，了解了这些您就可以开始使用Mongo了。而这些命令也往往是您大多数时间会用到的所有命令。

我们已经介绍过怎样在`find`命令中使用选择器了。此外选择器还可以用在`remove`命令中，我们已经大致提过；还有`count`命令中，我们并没有介绍不过您自己可以去试试看；还有`update`命令，我们在后面还会提到。

MongoDB为`_id`域生成的`ObjectId`也是可以被选择的，就像这样：

	db.unicorns.find({_id: ObjectId("TheObjectId")})

### 本章小结 ###
我们还没有介绍`update`命令以及`find`的更华丽的功能。不过我们已经让MongoDB运行起来，并且执行了`insert`和`remove`命令（这些命令看过本章的介绍也差不多了）。我们还介绍了`find`命令并见识了什么是MongoDB的选择器。一个好的开头为后面的深入奠定了坚实的基础。不管你信不信，您事实上已经了解了关于MongoDB所需要知道的知识——它本来就是易学易用的。我强烈建议您在继续读下去之前多多在MongoDB练习尝试一下。试着插入不同的文档，可以插入到新的集合中，并且熟悉不同的选择器表达式。多用`find`，`count`和`remove`。经过您自己的实践，那些初看很别扭的东西最后都会变得好用起来的。

\clearpage

## 第二章 - 更新 ##
在第一章中我们介绍了CRUD（Copy、Read、Update、Delete）中的三个操作。本章专门用来介绍前面跳过的第四个操作：`update`。`update`有一些出人意料的行为，这就是为什么我们专门在这章当中讨论它。

### update: replace 与 $set ###
`update`最简单的执行方式有两个参数：一个是选择器(选择更新的范围)，一个是需要更新的域。如果Roooooodles长胖了，我们就需要：

	db.unicorns.update({name: 'Roooooodles'}, {weight: 590})

（如果您用的是自己创建的`unicorns`集合，原来的数据都丢失了，那么就用`remove`删除掉所有文档，重新插入第一章中的数据）

在实际的代码中，您也许会基于`_id`来更新记录，不过既然我不知道MongoDB给您分配的`_id`是什么，我们就用`name`好了。如果我们看看更新过的记录：

	db.unicorns.find({name: 'Roooooodles'})

您就会发现`update`第一个出人意料的地方：上面的命令找不到任何文档。这是因为命令中输入的第二个参数是用来**替换（replace）**原来的文档的。换句话说，`update`先是根据`name`找到一个文档，然后用新的文档（也就是第二个参数）去覆盖找到的整个文档。这和SQL中的`update`的行为是不一样的。在某些情况下，这一行为非常理想，可以用于实现完全动态的更新。然而当您需要的仅是改变某个文档的某个值或者几个域，最好还是用MongoDB的`$set`修改符（modifier）：

	db.unicorns.update({weight: 590}, {$set: {name: 'Roooooodles', dob: new Date(1979, 7, 18, 18, 44), loves: ['apple'], gender: 'm', vampires: 99}})

这样做就会重设那些丢失的域。新的`weight`值不会被覆盖，因为我们没有在命令中指定它。如果现在执行：

	db.unicorns.find({name: 'Roooooodles'})

得到的就是预想的结果。所以在一开始时正确的更新体重的方法应该是：

	db.unicorns.update({name: 'Roooooodles'}, {$set: {weight: 590}})

### 更新修改符 ###
除了`$set`，还有其他的修改符可以用来非常漂亮地完成一些任务。所有的更新修改符都作用于域上——这样您的文档就不会被整个改写。比如说，`$inc`可以用来将一个域的值增加一个正的或负的数值。举个例子，如果由于失误，Pilot多获得了一些吸血的技能(vampire skill。译者：这里的独角兽可以理解为游戏中的某个角色，而这个角色也许可以通过升级打怪的方式提升某些技能，比如说吸血技能。)，我们可以用下面的命令来纠正这个错误：

	db.unicorns.update({name: 'Pilot'}, {$inc: {vampires: -2}})

如果Aurora忽然长出了一颗可爱的牙（译者：可以吃糖了），可以用`$push`修改符为她的`loves`域添加一个新的值：

	db.unicorns.update({name: 'Aurora'}, {$push: {loves: 'sugar'}})

MongoDB网站上的[Updating](http://www.mongodb.org/display/DOCS/Updating)部分可以找到其他更新修改符的更多信息。

### 插新（Upsert） ###
 > 译者：[Upsert](http://en.wikipedia.org/wiki/Upsert)的意思是update if present; insert if not。是update和insert合体的产物。没有找到一个合适的词作为翻译，于是我斗胆发明了“插新”这个词，取或插入或更新之意。如有更好的办法，还请指点。

`update`的一个比较讨喜的出人意料之处就是它完全支持插新(`upsert')。当目标文档存在的时候，插新操作会更新该文档，否则就插入该新文档。插新在某些情况下是很方便的，当您碰到这种情况的时候就会知道了。为了打开插新的功能，我们在使用`update`时把第三个参数设为`true`。

一个很常见的例子就是网站的点击计数器。如果需要得到实时的点击累计数值，我们需要知道这个页面的点击记录是否存在，然后决定是要更新点击数还是插入。如果忽略第三个参数（或者是设置为false），下面的命令什么也不做：

	db.hits.update({page: 'unicorns'}, {$inc: {hits: 1}});
	db.hits.find();

如果打开了插新，结果就不一样了：

	db.hits.update({page: 'unicorns'}, {$inc: {hits: 1}}, true);
	db.hits.find();

这一次，因为没有文档有域`page`的值为`unicorns`，就插入一个新的文档。再执行一次上面的命令，创建好的文档就会被更新，而`hits`的值就会增加为2。

	db.hits.update({page: 'unicorns'}, {$inc: {hits: 1}}, true);
	db.hits.find();

### 多重更新 ###
`update`最后的一个惊喜是，它会默认地只更新一个文档。到目前为止就我们所见到的例子来看，这样做是合理的。不过如果执行下面的命令：

	db.unicorns.update({}, {$set: {vaccinated: true }});
	db.unicorns.find({vaccinated: true});

您想要做的应该是找出所有已经注射过疫苗（vaccinated）的独角兽，但为了达到这样的目的，需要把第四个参数设为true：

	db.unicorns.update({}, {$set: {vaccinated: true }}, false, true);
	db.unicorns.find({vaccinated: true});

### 本章小结 ###
本章完成了对基础的集合操作，CRUD的介绍。我们细致的了解了`update`以及它的三个有意思的行为：第一，和SQL的update不同，MongoDB的`update会替换实际的文档。因此`$set`修改符就显得很有用了。第二，`update`支持直观的`插新`，这在和`$inc`修改符结合起来的时候特别有用。最后，`update`的默认行为是只更新第一个找到的文档。

一定要记住的是，我们是在MongoDB的shell中介绍它的。实际应用时您所采用的驱动或是库有可能会修改这些默认的行为，或是提供一个不同的编程接口（API）。例如：Ruby的驱动把最后的两个参数合并为一个哈希表：`{:upsert => false, :multi => false}`。类似地，PHP的驱动把最后的两个参数合并到了一个数组中：`array('upsert' => false, 'multiple' => false)`。

\clearpage

## 第三章 - 掌握查找 ##
Chapter 1 provided a superficial look at the `find` command. There's more to `find` than understanding `selectors` though. We already mentioned that the result from `find` is a `cursor`. We'll now look at exactly what this means in more detail.

### Field Selection ###
Before we jump into `cursors`, you should know that `find` takes a second optional parameter. This parameter is the list of fields we want to retrieve. For example, we can get all of the unicorns names by executing:

	db.unicorns.find(null, {name: 1});

By default, the `_id` field is always returned. We can explicitly exclude it by specifying `{name:1, _id: 0}`.

Aside from the `_id` field, you cannot mix and match inclusion and exclusion. If you think about it, that actually makes sense. You either want to select or exclude one or more fields explicitly.

### Ordering ###
A few times now I've mentioned that `find` returns a cursor whose execution is delayed until needed. However, what you've no doubt observed from the shell is that `find` executes immediately. This is a behavior of the shell only. We can observe the true behavior of `cursors` by looking at one of the methods we can chain to `find`. The first that we'll look at is `sort`. `sort` works a lot like the field selection from the previous section. We specify the fields we want to sort on, using 1 for ascending and -1 for descending. For example:

	//heaviest unicorns first
	db.unicorns.find().sort({weight: -1})

	//by vampire name then vampire kills:
	db.unicorns.find().sort({name: 1, vampires: -1})

Like with a relational database, MongoDB can use an index for sorting. We'll look at indexes in more detail later on. However, you should know that MongoDB limits the size of your sort without an index. That is, if you try to sort a large result set which can't use an index, you'll get an error. Some people see this as a limitation. In truth, I wish more databases had the capability to refuse to run unoptimized queries. (I won't turn every MongoDB drawback into a positive, but I've seen enough poorly optimized databases that I sincerely wish they had a strict-mode.)

### Paging ###
Paging results can be accomplished via the `limit` and `skip` cursor methods. To get the second and third heaviest unicorn, we could do:

	db.unicorns.find().sort({weight: -1}).limit(2).skip(1)

Using `limit` in conjunction with `sort`, is a good way to avoid running into problems when sorting on non-indexed fields.

### Count ###
The shell makes it possible to execute a `count` directly on a collection, such as:

	db.unicorns.count({vampires: {$gt: 50}})

In reality, `count` is actually a `cursor` method, the shell simply provides a shortcut. Drivers which don't provide such a shortcut need to be executed like this (which will also work in the shell):

	db.unicorns.find({vampires: {$gt: 50}}).count()

### In This Chapter ###
Using `find` and `cursors` is a straightforward proposition. There are a few additional commands that we'll either cover in later chapters or which only serve edge cases, but, by now, you should be getting pretty comfortable working in the mongo shell and understanding the fundamentals of MongoDB.

\clearpage

## Chapter 4 - Data Modeling ##
Let's shift gears and have a more abstract conversation about MongoDB. Explaining a few new terms and some new syntax is a trivial task. Having a conversation about modeling with a new paradigm isn't as easy. The truth is that most of us are still finding out what works and what doesn't when it comes to modeling with these new technologies. It's a conversation we can start having, but ultimately you'll have to practice and learn on real code.

Compared to most NoSQL solutions, document-oriented databases are probably the least different, compared to relational databases, when it comes to modeling. The differences which exist are subtle but that doesn't mean they aren't important.

### No Joins ###
The first and most fundamental difference that you'll need to get comfortable with is MongoDB's lack of joins. I don't know the specific reason why some type of join syntax isn't supported in MongoDB, but I do know that joins are generally seen as non-scalable. That is, once you start to horizontally split your data, you end up performing your joins on the client (the application server) anyways. Regardless of the reasons, the fact remains that data *is* relational, and MongoDB doesn't support joins.

Without knowing anything else, to live in a join-less world, we have to do joins ourselves within our application's code. Essentially we need to issue a second query to `find` the relevant data. Setting our data up isn't any different than declaring a foreign key in a relational database. Let's give a little less focus to our beautiful `unicorns` and a bit more time to our `employees`. The first thing we'll do is create an employee (I'm providing an explicit `_id` so that we can build coherent examples)

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d730"), name: 'Leto'})

Now let's add a couple employees and set their manager as `Leto`:

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d731"), name: 'Duncan', manager: ObjectId("4d85c7039ab0fd70a117d730")});
	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d732"), name: 'Moneo', manager: ObjectId("4d85c7039ab0fd70a117d730")});


(It's worth repeating that the `_id` can be any unique value. Since you'd likely use an `ObjectId` in real life, we'll use them here as well.)

Of course, to find all of Leto's employees, one simply executes:

	db.employees.find({manager: ObjectId("4d85c7039ab0fd70a117d730")})

There's nothing magical here. In the worst cases, most of the time, the lack of join will merely require an extra query (likely indexed).

#### Arrays and Embedded Documents ####
Just because MongoDB doesn't have joins doesn't mean it doesn't have a few tricks up its sleeve. Remember when we quickly saw that MongoDB supports arrays as first class objects of a document? It turns out that this is incredibly handy when dealing with many-to-one or many-to-many relationships. As a simple example, if an employee could have two managers, we could simply store these in an array:

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d733"), name: 'Siona', manager: [ObjectId("4d85c7039ab0fd70a117d730"), ObjectId("4d85c7039ab0fd70a117d732")] })

Of particular interest is that, for some documents, `manager` can be a scalar value, while for others it can be an array. Our original `find` query will work for both:

	db.employees.find({manager: ObjectId("4d85c7039ab0fd70a117d730")})

You'll quickly find that arrays of values are much more convenient to deal with than many-to-many join-tables.

Besides arrays, MongoDB also supports embedded documents. Go ahead and try inserting a document with a nested document, such as:

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d734"), name: 'Ghanima', family: {mother: 'Chani', father: 'Paul', brother: ObjectId("4d85c7039ab0fd70a117d730")}})

In case you are wondering, embedded documents can be queried using a dot-notation:

	db.employees.find({'family.mother': 'Chani'})

We'll briefly talk about where embedded documents fit and how you should use them.

#### DBRef ####
MongoDB supports something known as `DBRef` which is a convention many drivers support. When a driver encounters a `DBRef` it can automatically pull the referenced document. A `DBRef` includes the collection and id of the referenced document. It generally serves a pretty specific purpose: when documents from the same collection might reference documents from a different collection from each other. That is, the `DBRef` for document1 might point to a document in `managers` whereas the `DBRef` for document2 might point to a document in `employees`.


#### Denormalization ####
Yet another alternative to using joins is to denormalize your data. Historically, denormalization was reserved for performance-sensitive code, or when data should be snapshotted (like in an audit log). However, with the ever-growing popularity of NoSQL, many of which don't have joins, denormalization as part of normal modeling is becoming increasingly common. This doesn't mean you should duplicate every piece of information in every document. However, rather than letting fear of duplicate data drive your design decisions, consider modeling your data based on what information belongs to what document.

For example, say you are writing a forum application. The traditional way to associate a specific `user` with a `post` is via a `userid` column within `posts`. With such a model, you can't display `posts` without retrieving (joining to) `users`. A possible alternative is simply to store the `name` as well as the `userid` with each `post`. You could even do so with an embedded document, like `user: {id: ObjectId('Something'), name: 'Leto'}`. Yes, if you let users change their name, you'll have to update each document (which is 1 extra query).

Adjusting to this kind of approach won't come easy to some. In a lot of cases it won't even make sense to do this. Don't be afraid to experiment with this approach though. It's not only suitable in some circumstances, but it can also be the right way to do it.

#### Which Should You Choose? ####
Arrays of ids are always a useful strategy when dealing with one-to-many or many-to-many scenarios. It's probably safe to say that `DBRef` aren't used very often, though you can certainly experiment and play with them. That generally leaves new developers unsure about using embedded documents versus doing manual referencing.

First, you should know that an individual document is currently limited to 4 megabytes in size. Knowing that documents have a size limit, though quite generous, gives you some idea of how they are intended to be used. At this point, it seems like most developers lean heavily on manual references for most of their relationships. Embedded documents are frequently leveraged, but mostly for small pieces of data which we want to always pull with the parent document. A real world example I've used is to store an `accounts` document with each user, something like:

	db.users.insert({name: 'leto', email: 'leto@dune.gov', account: {allowed_gholas: 5, spice_ration: 10}})

That doesn't mean you should underestimate the power of embedded documents or write them off as something of minor utility. Having your data model map directly to your objects makes things a lot simpler and often does remove the need to join. This is especially true when you consider that MongoDB lets you query and index fields of an embedded document.

### Few or Many Collections ###
Given that collections don't enforce any schema, it's entirely possible to build a system using a single collection with a mismatch of documents.  From what I've seen, most MongoDB systems are laid out similarly to what you'd find in a relational system. In other words, if it would be a table in a relational database, it'll likely be a collection in MongoDB (many-to-many join tables being an important exception).

The conversation gets even more interesting when you consider embedded documents. The example that frequently comes up is a blog. Should you have a `posts` collection and a `comments` collection, or should each `post` have an array of `comments` embedded within it. Setting aside the 4MB limit for the time being (all of Hamlet is less than 200KB, just how popular is your blog?), most developers still prefer to separate things out. It's simply cleaner and more explicit.

There's no hard rule (well, aside from 4MB). Play with different approaches and you'll get a sense of what does and does not feel right.

### In This Chapter ###
Our goal in this chapter was to provide some helpful guidelines for modeling your data in MongoDB. A starting point if you will. Modeling in a document-oriented system is different, but not too different than a relational world. You have a bit more flexibility and one constraint, but for a new system, things tend to fit quite nicely. The only way you can go wrong is by not trying.

\clearpage

## Chapter 5 - When To Use MongoDB ##
By now you should have a good enough understanding of MongoDB to have a feel for where and how it might fit into your existing system. There are enough new and competing storage technologies that it's easy to get overwhelmed by all of the choices.

For me, the most important lesson, which has nothing to do with MongoDB, is that you no longer have to rely on a single solution for dealing with your data. No doubt, a single solution has obvious advantages and for a lot projects, possibly even most, a single solution is the sensible approach. The idea isn't that you must use different technologies, but rather that you can. Only you know whether the benefits of introducing a new solution outweigh the costs.

With that said, I'm hopeful that what you've seen so far has made you see MongoDB as a general solution. It's been mentioned a couple times that document-oriented databases share a lot in common with relational databases. Therefore, rather than tiptoeing around it, let's simply state that MongoDB should be seen as a direct alternative to relational databases. Where one might see Lucene as enhancing a relational database with full text indexing, or Redis as a persistent key-value store, MongoDB is a central repository for your data.

Notice that I didn't call MongoDB a *replacement* for relational databases, but rather an *alternative*. It's a tool that can do what a lot of other tools can do. Some of it MongoDB does better, some of it MongoDB does worse. Let's dissect things a little further.

### Schema-less ###
An oft-touted benefit of document-oriented database is that they are schema-less. This makes them much more flexible than traditional database tables. I agree that schema-less is a nice feature, but not for the main reason most people mention.

People talk about schema-less as though you'll suddenly start storing a crazy mismatch of data. There are domains and data sets which can really be a pain to model using relational databases, but I see those as edge cases. Schema-less is cool, but most of your data is going to be highly structured. It's true that having an occasional mismatch can be handy, especially when you introduce new features, but in reality it's nothing a nullable column probably wouldn't solve just as well.

For me, the real benefit of schema-less design is the lack of setup and the reduced friction with OOP. This is particularly true when you're working with a static language. I've worked with MongoDB in both C# and Ruby, and the difference is striking. Ruby's dynamism and its popular ActiveRecord implementations already reduce much of the object-relational impedance mismatch. That isn't to say MongoDB isn't a good match for Ruby, it really is. Rather, I think most Ruby developers would see MongoDB as an incremental improvement, whereas C# or Java developers would see a fundamental shift in how they interact with their data.

Think about it from the perspective of a driver developer. You want to save an object? Serialize it to JSON (technically BSON, but close enough) and send it to MongoDB. There is no property mapping or type mapping. This straightforwardness definitely flows to you, the end developer.

### Writes ###
One area where MongoDB can fit a specialized role is in logging. There are two aspects of MongoDB which make writes quite fast. First, you can send a write command and have it return immediately without waiting for it to actually write. Secondly, with the introduction of journaling in 1.8, and enhancements made in 2.0, you can control the write behavior with respect to data durability. These settings, in addition to specifying how many servers should get your data before being considered successful, are configurable per-write, giving you a great level of control over write performance and data durability.

In addition to these performance factors, log data is one of those data sets which can often take advantage of schema-less collections. Finally, MongoDB has something called a [capped collection](http://www.mongodb.org/display/DOCS/Capped+Collections). So far, all of the implicitly created collections we've created are just normal collections. We can create a capped collection by using the `db.createCollection` command and flagging it as capped:

	//limit our capped collection to 1 megabyte
	db.createCollection('logs', {capped: true, size: 1048576})

When our capped collection reaches its 1MB limit, old documents are automatically purged. A limit on the number of documents, rather than the size, can be set using `max`. Capped collections have some interesting properties. For example, you can update a document but it can't grow in size. Also, the insertion order is preserved, so you don't need to add an extra index to get proper time-based sorting.

This is a good place to point out that if you want to know whether your write encountered any errors (as opposed to the default fire-and-forget), you simply issue a follow-up command: `db.getLastError()`. Most drivers encapsulate this as a *safe write*, say by specifying `{:safe => true}` as a second parameter to `insert`.

### Durability ###
Prior to version 1.8, MongoDB didn't have single-server durability. That is, a server crash would likely result in lost data. The solution had always been to run MongoDB in a multi-server setup (MongoDB supports replication). One of the major features added to 1.8 was journaling. To enable it add a new line with `journal=true` to the `mongodb.config` file we created when we first setup MongoDB (and restart your server if you want it enabled right away). You probably want journaling enabled (it'll be a default in a future release). Although, in some circumstances the extra throughput you get from disabling journaling might be a risk you are willing to take. (It's worth pointing out that some types of applications can easily afford to lose data).

Durability is only mentioned here because a lot has been made around MongoDB's lack of single-server durability. This'll likely show up in Google searches for some time to come. Information you find about this missing feature is simply out of date.

### Full Text Search ###
True full text search capability is something that'll hopefully come to MongoDB in a future release. With its support for arrays, basic full text search is pretty easy to implement. For something more powerful, you'll need to rely on a solution such as Lucene/Solr. Of course, this is also true of many relational databases.

### Transactions ###
MongoDB doesn't have transactions. It has two alternatives, one which is great but with limited use, and the other that is a cumbersome but flexible.

The first is its many atomic operations. These are great, so long as they actually address your problem. We already saw some of the simpler ones, like `$inc` and `$set`. There are also commands like `findAndModify` which can update or delete a document and return it atomically.

The second, when atomic operations aren't enough, is to fall back to a two-phase commit. A two-phase commit is to transactions what manual dereferencing is to joins. It's a storage-agnostic solution that you do in code.  Two-phase commits are actually quite popular in the relational world as a way to implement transactions across multiple databases. The MongoDB website [has an example](http://www.mongodb.org/display/DOCS/two-phase+commit) illustrating the most common scenario (a transfer of funds). The general idea is that you store the state of the transaction within the actual document being updated and go through the init-pending-commit/rollback steps manually.

MongoDB's support for nested documents and schema-less design makes two-phase commits slightly less painful, but it still isn't a great process, especially when you are just getting started with it.

### Data Processing ###
MongoDB relies on MapReduce for most data processing jobs. It has some [basic aggregation](http://www.mongodb.org/display/DOCS/Aggregation) capabilities, but for anything serious, you'll want to use MapReduce. In the next chapter we'll look at MapReduce in detail. For now you can think of it as a very powerful and different way to `group by` (which is an understatement). One of MapReduce's strengths is that it can be parallelized for working with large sets of data. However, MongoDB's implementation relies on JavaScript which is single-threaded. The point? For processing of large data, you'll likely need to rely on something else, such as Hadoop. Thankfully, since the two systems really do complement each other, there's a [MongoDB adapter for Hadoop](https://github.com/mongodb/mongo-hadoop).

Of course, parallelizing data processing isn't something relational databases excel at either. There are plans for future versions of MongoDB to be better at handling very large sets of data.

### Geospatial ###
A particularly powerful feature of MongoDB is its support for geospatial indexes. This allows you to store x and y coordinates within documents and then find documents that are `$near` a set of coordinates or `$within` a box or circle. This is a feature best explained via some visual aids, so I invite you to try the [5 minute geospatial interactive tutorial](http://tutorial.mongly.com/geo/index), if you want to learn more.

### Tools and Maturity ###
You probably already know the answer to this, but MongoDB is obviously younger than most relational database systems. This is absolutely something you should consider. How much a factor it plays depends on what you are doing and how you are doing it. Nevertheless, an honest assessment simply can't ignore the fact that MongoDB is younger and the available tooling around isn't great (although the tooling around a lot of very mature relational databases is pretty horrible too!). As an example, the lack of support for base-10 floating point numbers will obviously be a concern (though not necessarily a show-stopper) for systems dealing with money.

On the positive side, drivers exist for a great many languages, the protocol is modern and simple, and development is happening at blinding speeds. MongoDB is in production at enough companies that concerns about maturity, while valid, are quickly becoming a thing of the past.

### In This Chapter ###
The message from this chapter is that MongoDB, in most cases, can replace a relational database. It's much simpler and straightforward; it's faster and generally imposes fewer restrictions on application developers. The lack of transactions can be a legitimate and serious concern. However, when people ask *where does MongoDB sit with respect to the new data storage landscape?* the answer is simple: **right in the middle**.

\clearpage

## Chapter 6 - MapReduce ##
MapReduce is an approach to data processing which has two significant benefits over more traditional solutions. The first, and main, reason it was developed is performance. In theory, MapReduce can be parallelized, allowing very large sets of data to be processed across many cores/CPUs/machines. As we just mentioned, this isn't something MongoDB is currently able to take advantage of. The second benefit of MapReduce is that you get to write real code to do your processing. Compared to what you'd be able to do with SQL, MapReduce code is infinitely richer and lets you push the envelope further before you need to use a more specialized solution.

MapReduce is a pattern that has grown in popularity, and you can make use of it almost anywhere; C#, Ruby, Java, Python and so on all have implementations. I want to warn you that at first this'll seem very different and complicated. Don't get frustrated, take your time and play with it yourself. This is worth understanding whether you are using MongoDB or not.

### A Mix of Theory and Practice ###
MapReduce is a two-step process. First you map and then you reduce. The mapping step transforms the inputted documents and emits a key=>value pair (the key and/or value can be complex). The reduce gets a key and the array of values emitted for that key and produces the final result. We'll look at each step, and the output of each step.

The example that we'll be using is to generate a report of the number of hits, per day, we get on a resource (say a webpage). This is the *hello world* of MapReduce. For our purposes, we'll rely on a `hits` collection with two fields: `resource` and `date`. Our desired output is a breakdown by `resource`, `year`, `month`, `day` and `count`.

Given the following data in `hits`:

	resource     date
	index        Jan 20 2010 4:30
	index        Jan 20 2010 5:30
	about        Jan 20 2010 6:00
	index        Jan 20 2010 7:00
	about        Jan 21 2010 8:00
	about        Jan 21 2010 8:30
	index        Jan 21 2010 8:30
	about        Jan 21 2010 9:00
	index        Jan 21 2010 9:30
	index        Jan 22 2010 5:00

We'd expect the following output:

	resource  year   month   day   count
	index     2010   1       20    3
	about     2010   1       20    1
	about     2010   1       21    3
	index     2010   1       21    2
	index     2010   1       22    1

(The nice thing about this type of approach to analytics is that by storing the output, reports are fast to generate and data growth is controlled (per resource that we track, we'll add at most 1 document per day.)

For the time being, focus on understanding the concept. At the end of this chapter, sample data and code will be given for you to try on your own.

The first thing to do is look at the map function. The goal of map is to make it emit a value which can be reduced. It's possible for map to emit 0 or more times. In our case, it'll always emit once (which is common). Imagine map as looping through each document in hits. For each document we want to emit a key with resource, year, month and day, and a simple value of 1:

	function() {
		var key = {
		    resource: this.resource,
		    year: this.date.getFullYear(),
		    month: this.date.getMonth(),
		    day: this.date.getDate()
		};
		emit(key, {count: 1});
	}

`this` refers to the current document being inspected. Hopefully what'll help make this clear for you is to see what the output of the mapping step is. Using our above data, the complete output would be:

	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}, {count:1}]
	{resource: 'about', year: 2010, month: 0, day: 20} => [{count: 1}]
	{resource: 'about', year: 2010, month: 0, day: 21} => [{count: 1}, {count: 1}, {count:1}]
	{resource: 'index', year: 2010, month: 0, day: 21} => [{count: 1}, {count: 1}]
	{resource: 'index', year: 2010, month: 0, day: 22} => [{count: 1}]

Understanding this intermediary step is the key to understanding MapReduce. The values from emit are grouped together, as arrays, by key. .NET and Java developers can think of it as being of type `IDictionary<object, IList<object>>` (.NET) or `HashMap<Object, ArrayList>` (Java).

Let's change our map function in some contrived way:

	function() {
		var key = {resource: this.resource, year: this.date.getFullYear(), month: this.date.getMonth(), day: this.date.getDate()};
		if (this.resource == 'index' && this.date.getHours() == 4) {
			emit(key, {count: 5});
		} else {
			emit(key, {count: 1});
		}
	}

The first intermediary output would change to:

	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 5}, {count: 1}, {count:1}]

Notice how each emit generates a new value which is grouped by our key.

The reduce function takes each of these intermediary results and outputs a final result. Here's what ours looks like:

	function(key, values) {
		var sum = 0;
		values.forEach(function(value) {
			sum += value['count'];
		});
		return {count: sum};
	};

Which would output:

	{resource: 'index', year: 2010, month: 0, day: 20} => {count: 3}
	{resource: 'about', year: 2010, month: 0, day: 20} => {count: 1}
	{resource: 'about', year: 2010, month: 0, day: 21} => {count: 3}
	{resource: 'index', year: 2010, month: 0, day: 21} => {count: 2}
	{resource: 'index', year: 2010, month: 0, day: 22} => {count: 1}

Technically, the output in MongoDB is:

	_id: {resource: 'home', year: 2010, month: 0, day: 20}, value: {count: 3}

Hopefully you've noticed that this is the final result we were after.

If you've really been paying attention, you might be asking yourself *why didn't we simply use `sum = values.length`?* This would seem like an efficient approach when you are essentially summing an array of 1s. The fact is that reduce isn't always called with a full and perfect set of intermediate data. For example, instead of being called with:

	{resource: 'home', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}, {count:1}]

Reduce could be called with:

	{resource: 'home', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}]
	{resource: 'home', year: 2010, month: 0, day: 20} => [{count: 2}, {count: 1}]

The final output is the same (3), the path taken is simply different. As such, reduce must always be idempotent. That is, calling reduce multiple times should generate the same result as calling it once.

We aren't going to cover it here but it's common to chain reduce methods when performing more complex analysis.

### Pure Practical ###
With MongoDB we use the `mapReduce` command on a collection. `mapReduce` takes a map function, a reduce function and an output directive. In our shell we can create and pass a JavaScript function. From most libraries you supply a string of your functions (which is a bit ugly). First though, let's create our simple data set:

	db.hits.insert({resource: 'index', date: new Date(2010, 0, 20, 4, 30)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 20, 5, 30)});
	db.hits.insert({resource: 'about', date: new Date(2010, 0, 20, 6, 0)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 20, 7, 0)});
	db.hits.insert({resource: 'about', date: new Date(2010, 0, 21, 8, 0)});
	db.hits.insert({resource: 'about', date: new Date(2010, 0, 21, 8, 30)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 21, 8, 30)});
	db.hits.insert({resource: 'about', date: new Date(2010, 0, 21, 9, 0)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 21, 9, 30)});
	db.hits.insert({resource: 'index', date: new Date(2010, 0, 22, 5, 0)});

Now we can create our map and reduce functions (the MongoDB shell accepts multi-line statements, you'll see *...* after hitting enter to indicate more text is expected):

	var map = function() {
		var key = {resource: this.resource, year: this.date.getFullYear(), month: this.date.getMonth(), day: this.date.getDate()};
		emit(key, {count: 1});
	};

	var reduce = function(key, values) {
		var sum = 0;
		values.forEach(function(value) {
			sum += value['count'];
		});
		return {count: sum};
	};

Which we can use the `mapReduce` command against our `hits` collection by doing:

	db.hits.mapReduce(map, reduce, {out: {inline:1}})

If you run the above, you should see the desired output. Setting `out` to `inline` means that the output from `mapReduce` is immediately streamed back to us. This is currently limited for results that are 16 megabytes or less. We could instead specify `{out: 'hit_stats'}` and have the results stored in the `hit_stats` collections:

	db.hits.mapReduce(map, reduce, {out: 'hit_stats'});
	db.hit_stats.find();

When you do this, any existing data in `hit_stats` is lost. If we did `{out: {merge: 'hit_stats'}}` existing keys would be replaced with the new values and new keys would be inserted as new documents. Finally, we can `out` using a `reduce` function to handle more advanced cases (such an doing an upsert).

The third parameter takes additional options, for example we could filter, sort and limit the documents that we want analyzed. We can also supply a `finalize` method to be applied to the results after the `reduce` step.

### In This Chapter ###
This is the first chapter where we covered something truly different. If it made you uncomfortable, remember that you can always use MongoDB's other [aggregation capabilities](http://www.mongodb.org/display/DOCS/Aggregation) for simpler scenarios. Ultimately though, MapReduce is one of MongoDB's most compelling features. The key to really understanding how to write your map and reduce functions is to visualize and understand the way your intermediary data will look coming out of `map` and heading into `reduce`.

\clearpage

## Chapter 7 - Performance and Tools ##
In this last chapter, we look at a few performance topics as well as some of the tools available to MongoDB developers. We won't dive deeply into either topic, but we will examine the most import aspects of each.

### Indexes ###
At the very beginning we saw the special `system.indexes` collection which contains information on all the indexes in our database. Indexes in MongoDB work a lot like indexes in a relational database: they help improve query and sorting performance. Indexes are created via `ensureIndex`:

	// where "name" is the fieldname
	db.unicorns.ensureIndex({name: 1});

And dropped via `dropIndex`:

	db.unicorns.dropIndex({name: 1});

A unique index can be created by supplying a second parameter and setting `unique` to `true`:

	db.unicorns.ensureIndex({name: 1}, {unique: true});

Indexes can be created on embedded fields (again, using the dot-notation) and on array fields. We can also create compound indexes:

	db.unicorns.ensureIndex({name: 1, vampires: -1});

The order of your index (1 for ascending, -1 for descending) doesn't matter for a single key index, but it can have an impact for compound indexes when you are sorting or using a range condition.

The [indexes page](http://www.mongodb.org/display/DOCS/Indexes) has additional information on indexes.

### Explain ###
To see whether or not your queries are using an index, you can use the `explain` method on a cursor:

	db.unicorns.find().explain()

The output tells us that a `BasicCursor` was used (which means non-indexed), 12 objects were scanned, how long it took, what index, if any was used as well as a few other pieces of useful information.

If we change our query to use an index, we'll see that a `BtreeCursor` was used, as well as the index used to fulfill the request:

	db.unicorns.find({name: 'Pilot'}).explain()

### Fire And Forget Writes ###
We previously mentioned that, by default, writes in MongoDB are fire-and-forget. This can result in some nice performance gains at the risk of losing data during a crash. An interesting side effect of this type of write is that an error is not returned when an insert/update violates a unique constraint. In order to be notified about a failed write, one must call `db.getLastError()` after an insert. Many drivers abstract this detail away and provide a way to do a *safe* write - often via an extra parameter.

Unfortunately, the shell automatically does safe inserts, so we can't easily see this behavior in action.

### Sharding ###
MongoDB supports auto-sharding. Sharding is an approach to scalability which separates your data across multiple servers. A naive implementation might put all of the data for users with a name that starts with A-M on server 1 and the rest on server 2. Thankfully, MongoDB's sharding capabilities far exceed such a simple algorithm. Sharding is a topic well beyond the scope of this book, but you should know that it exists and that you should consider it should your needs grow beyond a single server.

### Replication ###
MongoDB replication works similarly to how relational database replication works. Writes are sent to a single server, the master, which then synchronizes itself to one or more other servers, the slaves. You can control whether reads can happen on slaves or not, which can help distribute your load at the risk of reading slightly stale data. If the master goes down, a slave can be promoted to act as the new master. Again, MongoDB replication is outside the scope of this book.

 While replication can improve performance (by distributing reads), its main purpose is to increase reliability. Combining replication with sharding is a common approach. For example, each shard could be made up of a master and a slave. (Technically you'll also need an arbiter to help break a tie should two slaves try to become masters. But an arbiter requires very few resources and can be used for multiple shards.)

### Stats ###
You can obtain statistics on a database by typing `db.stats()`. Most of the information deals with the size of your database. You can also get statistics on a collection, say `unicorns`, by typing `db.unicorns.stats()`. Again, most of this information relates to the size of your collection.

### Web Interface ###
Included in the information displayed on MongoDB's startup was a link to a web-based administrative tool (you might still be able to see if if you scroll your command/terminal window up to the point where you started `mongod`). You can access this by pointing your browser to <http://localhost:28017/>. To get the most out of it, you'll want to add `rest=true` to your config and restart the `mongod` process. The web interface gives you a lot of insight into the current state of your server.

### Profiler ###
You can enable the MongoDB profiler by executing:

	db.setProfilingLevel(2);

With it enabled, we can run a command:

	db.unicorns.find({weight: {$gt: 600}});

And then examine the profiler:

	db.system.profile.find()

The output tells us what was run and when, how many documents were scanned, and how much data was returned.

You can disable the profiler by calling `setProfileLevel` again but changing the argument to `0`. Another option is to specify `1` which will only profile queries that take more than 100 milliseconds. Or, you can specify the minimum time, in milliseconds, with a second parameter:

	//profile anything that takes more than 1 second
	db.setProfilingLevel(1, 1000);

### Backups and Restore ###
Within the MongoDB `bin` folder is a `mongodump` executable. Simply executing `mongodump` will connect to localhost and backup all of your databases to a `dump` subfolder. You can type `mongodump --help` to see additional options. Common options are `--db DBNAME` to back up a specific database and `--collection COLLECTIONAME` to back up a specific collection. You can then use the `mongorestore` executable, located in the same `bin` folder, to restore a previously made backup. Again, the `--db` and `--collection` can be specified to restore a specific database and/or collection.

For example, to back up our `learn` database to a `backup` folder, we'd execute (this is its own executable which you run in a command/terminal window, not within the mongo shell itself):

	mongodump --db learn --out backup

To restore only the `unicorns` collection, we could then do:

	mongorestore --collection unicorns backup/learn/unicorns.bson

It's worth pointing out that `mongoexport` and `mongoimport` are two other executables which can be used to export and import data from JSON or CSV. For example, we can get a JSON output by doing:

	mongoexport --db learn -collection unicorns

And a CSV output by doing:

	mongoexport --db learn -collection unicorns --csv -fields name,weight,vampires

Note that `mongoexport` and `mongoimport` cannot always represent your data. Only `mongodump` and `mongorestore` should ever be used for actual backups.

### In This Chapter ###
In this chapter we looked a various commands, tools and performance details of using MongoDB. We haven't touched on everything, but we've looked at the most common ones. Indexing in MongoDB is similar to indexing with relational databases, as are many of the tools. However, with MongoDB, many of these are to the point and simple to use.

\clearpage

## Conclusion ##
You should have enough information to start using MongoDB in a real project. There's more to MongoDB than what we've covered, but your next priority should be putting together what we've learned, and getting familiar with the driver you'll be using. The [MongoDB website](http://www.mongodb.com/) has a lot of useful information. The official [MongoDB user group](http://groups.google.com/group/mongodb-user) is a great place to ask questions.

NoSQL was born not only out of necessity, but also out of an interest to try new approaches. It is an acknowledgement that our field is ever advancing and that if we don't try, and sometimes fail, we can never succeed. This, I think, is a good way to lead our professional lives.
