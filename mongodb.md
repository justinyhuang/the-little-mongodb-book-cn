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
本书的中文版由[justinyhuang](http://justinyhuang.com)完成，基于与原著相同的许可证。最新版本在[GitHub](https://github.com/justinyhuang/the-little-mongodb-book-cn)。译文的纰漏欢迎告知<yang.huang@ymail.com>或直接提交github。

\clearpage

## 简介 ##
 > 本章很短，但不是我的错，MongoDB就是那么简单易学。

人们总是说科技的发展风驰电掣。确实，一直以来都不断有新的技术涌现出来。但是我却一直坚持认为程序员所用的基本技术的发展相对而言就缓慢很多。您可以很多年不学习什么但还是可以混过去。让人瞩目的是业已成熟的技术被替代的速度。仿佛一夜间，那些长期以来业已成熟的技术忽然就失去了开发者的关注，昔日地位岌岌可危。

NoSQL逐步攻陷了传统关系数据库的领地，就是这种急剧转变最好的例子。仿佛就在昨天所有的网页还是由一些RDBMS驱动的，而一早起来就已经有大约5种NoSQL的方案证明了他们都是有价值的解决方案。

虽然这些转变看起来是一夜间就发生的，事实却是这些新生的技术历经多年才被接受并应用于实践。一开始先是由一小部分开发者或者企业推动，然后逐步吸取教训，改善方案并见证新技术地位的确立。其他的跟随者之后也慢慢地开始了尝试。对于NoSQL来说也是一样。很多新方案的出现都不是为了去代替更加传统的存储方案，而是为了填补后者所能满足需求之外的一些空白。

说了那么多，在这里我们首先要做的是弄清楚什么是NoSQL。这是一个很宽泛的概念，不同的人有不同的解读。我个人用NoSQL来泛指参与数据存储的系统。换句话说，NoSQL（还是我个人的意见），是一种观念，这种观念认为维护数据的持久性不必是单个系统的责任。相比之下，关系数据库的缔造者一开始就力图把他们的软件当作通用解决方案。NoSQL则更倾向于负责系统中的一小部分功能：限定了部分功能，便可以使用最适合的工具。因此，您以后的NoSQL架构中依旧有可能利用到关系数据库，比如说MySQL，于此同时还可能在特定的部件使用Redis，还会用Hadoop进行大量的数据处理。简而言之，NoSQL就是保持开放和警醒，利用已有的可用的工具和方法去管理您的数据。

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

您也许已经觉得奇怪，为什么要用新的术语（表换成集合，行换成文档，列换成域），这不是越弄越复杂了么？这是因为虽然这些概念和那些关系数据库中的相应概念很相似，但是还是存在差异的。关键的差异在于关系数据库是在‘表’这一层次定义‘列’的，而一个面向文档的数据库则是在‘文档’这一层次定义‘域’的。也就是说，集合中的每个文档都可以有独立的域。因此，虽说集合相对于表来说是一个简化了的容器，而文档则包含了比行要多得多的信息。

虽然这些异同很重要，但是如果您现在还没搞清楚也不必担心。以后试着插入几次（数据）就知道我们这里说的是什么了。最后，集合对其中储存的内容并没有严格的要求（它是无模式的（schema-less）），域是与其所在的文档绑定的。当中的优缺点我们会在后续的章节中继续探讨。

开始动手实践吧。如果您还没有运行Mongo，现在就可以启动`mongod`服务器以及Mongo的shell。Mongo的shell运行在JavaScript之上，您可以执行一些全局的指令，如`help`或者`exit`。操作对象`db`来执行针对当前数据库的操作，例如`db.help()`或是`db.stats()`。操作对象`db.COLLECTION_NAME`来执行针对某一给集合的操作，比如说`db.unicorns.help()`或是`db.unicorns.count()`。我们以后将会有许多针对集合的操作。

试试输入`db.help()`，您会看到一串命令列表，这些命令都可以用来操作`db`对象。

顺带说一句，因为我们用的是JavaScript的shell，如果您执行一个命令而忘了加上`()`，您看到的将是这个命令的实现而并没有执行它。知道这个，您在这么做并看到以`function(...){`开头的信息的时候就不会觉得惊讶了。比如说如果您输入`db.help`（后面没有括弧），你就将看到`help`命令的具体实现。

首先我们用全局命令`use`来切换数据库。输入`use learn`。这个数据库是否存在并没有关系，我们创建第一个集合后这个`learn`数据库就会生成的。现在您应该已经在一个数据库里面了，可以执行一些诸如`db.getCollectionNames()`的数据库命令了。如果您现在就这么做，将会看到一个空的数组（`[]`）。因为集合是无模式的，我们不需要专门去创建它。我们要做的只是把一个文档插入一个新的集合，这个集合就生成了。您可以像下面一样调用`insert`命令去插入一个文档：

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
在第一章中我们介绍了CRUD（Create、Read、Update、Delete）中的三个操作。本章专门用来介绍前面跳过的第四个操作：`update`。`update`有一些出人意料的行为，这就是为什么我们专门在这章当中讨论它。

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

`update`的一个比较讨喜的出人意料之处就是它完全支持插新(`upsert`)。当目标文档存在的时候，插新操作会更新该文档，否则就插入该新文档。插新在某些情况下是很方便的，当您碰到这种情况的时候就会知道了。为了打开插新的功能，我们在使用`update`时把第三个参数设为`true`。

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
本章完成了对基础的集合操作，CRUD的介绍。我们细致的了解了`update`以及它的三个有意思的行为：第一，和SQL的update不同，MongoDB的`update`会替换实际的文档。因此`$set`修改符就显得很有用了。第二，`update`支持直观的`插新`，这在和`$inc`修改符结合起来的时候特别有用。最后，`update`的默认行为是只更新第一个找到的文档。

一定要记住的是，我们是在MongoDB的shell中介绍它的。实际应用时您所采用的驱动或是库有可能会修改这些默认的行为，或是提供一个不同的编程接口（API）。例如：Ruby的驱动把最后的两个参数合并为一个哈希表：`{:upsert => false, :multi => false}`。类似地，PHP的驱动把最后的两个参数合并到了一个数组中：`array('upsert' => false, 'multiple' => false)`。

\clearpage

## 第三章 - 掌握查找 ##
第一章对`find`命令作了一些简单的介绍，除了选择器以外，`find`还有很多其他的特性。之前有提到过`find`返回的结果是一个游标。现在就将对这点做深入的讨论。

### 域的选择 ###
在开始游标的话题之前，您需要知道`find`还有第二个可选参数。该参数是一个列表，用户在这个表中指明要求`find`读取的域。例如，可以用下面的命令获取所有独角兽的名字：

	db.unicorns.find(null, {name: 1});

`_id`域在默认情况下总是会被`find`返回的。`{name:1, _id: 0}`可以显式地从返回结果中排除它。

除了上面排除`_id`域的情况外，不可以将选择与排除的表达式混在一起使用（译者：比如说`{_id:1, name:0}`或者`{name:1, gender:0}`都是不合法的）。想一想其实也合乎逻辑:不是显式地选择，就是说明哪些域需要排除。

### 排序 ###
我已经好几次提到，`find`返回的是一个游标，对游标的操作直到必要的时候才会执行。然而在shell中的感觉却是`find`马上就执行了。这仅仅是`find`在shell中的行为。我们可以通过把`find`和一个命令连接起来的方法观察`find`真正的行为。我们先用`sort`来做这个实验。`sort`的运作方式有点像上一节提到的域的选择：标明哪些域需要排序，用1表示升序，用-1表示降序。例如：

	//最重的独角兽排在第一
	db.unicorns.find().sort({weight: -1})

	//优先按名字排序再按吸血技能排序
	db.unicorns.find().sort({name: 1, vampires: -1})

如同关系数据库，MongoDB也可以利用索引进行排序。我们会在后面再详细讨论索引。只是您要知道，当没有建立索引时，MongoDB是限制排序对象大小的（译者：目前是16MB）。也就是说，如果您尝试对一个大规模的结果的集进行排序，而又没有为这个结果建立索引，那么就会看到错误的提示。对一些人来说，这是MongoDB的局限性。但是说实话，我真希望更多的数据库能够像MongoDB那样拒绝执行那些未经优化的查询。（我倒不是要把每个MongoDB的缺点都硬掰成优点，只是我见过太多缺乏优化的数据库了，所以我真的很希望它们能有一个`严格模式`以限制未经优化的查询。）

### 分页（paging） ###
对结果的分页可以通过`limit`以及`skip`这两个游标操作来实现。比如可以用一下的命令来得到第二，第三重的独角兽：

	db.unicorns.find().sort({weight: -1}).limit(2).skip(1)

对非索引域进行排序是很麻烦的，联合`limit`一起使用`sort`是避免此类麻烦的好方法。

### 计数 ###
在shell中我们可以对一个集合直接地执行`count`命令，例如：

	db.unicorns.count({vampires: {$gt: 50}})

而现实中`count`却是一个游标的操作，shell只是提供了一条捷径而已。在使用没有提供这些捷径的驱动时，就要用到下面的命令（在shell中也有用）：

	db.unicorns.find({vampires: {$gt: 50}}).count()

### 本章小结 ###
`find`和`cursors`的使用是比较直接明了的。还有一些额外的命令，有一些会在后面的章节继续介绍，其他的几乎都只是用在很少见的情况了。到现在为止您应该可以比较自如的在mongo的shell工作，也了解了MongoDB的基础知识了。

\clearpage

## 第四章 - 数据建模 ##
这一章我们切换频道，谈谈关于MongoDB的一个比较抽象的话题吧。解释新的名字或者是新的句法都不是什么难事，而用新的范式探讨建模的问题就没那么简单了。事实上当涉及用新技术建模的问题时，我们中的大多数人还仍然在探索这些技术究竟是否合适。这是一个现在就开始的话题，但最终您还是需要自己实践并从真正的代码中去学习。

与大多数NoSQL方案相比,在建模方面,面向文档的数据库算是和关系数据库相差最小的。这些差别是很小，但是并不是说不重要。

### 没有连接 ###
您要接受的第一个也是最基本的一个差别，就是MongoDB没有连接（join）。我不知道MongoDB不支持某些类型连接句法的具体原因，但是我知道一般而言人们认为连接是不可扩展的。也就是说，一旦开始横向分割数据，最终不可避免的就是在客户端（应用程序服务器）使用连接。且不论MongoDB为什么不支持连接，事实是数据*是有关系的*，可是MongoDB不支持连接。（译者：这里的关系指的是不同的数据之间是有关联的，对于没有关系的数据，就完全不需要连接。）

为了在没有连接的MongoDB中生存下去，在没有其他帮助的情况下，我们必须在自己的应用程序中实现连接。基本上我们需要用第二次查询去`找到`相关的数据。找到并组织这些数据相当于在关系数据库中声明一个外来的键。现在先别管什么`独角兽`了，我们来看看我们的`员工`。首先我们创建一个员工的数据（这次我告诉您具体的`_id`值，这样我们的例子就是一样的了）：

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d730"), name: 'Leto'})

然后我们再加入几个员工并把`Leto`设成他们的老板：

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d731"), name: 'Duncan', manager: ObjectId("4d85c7039ab0fd70a117d730")});
	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d732"), name: 'Moneo', manager: ObjectId("4d85c7039ab0fd70a117d730")});

（有必要再强调一下，`_id`可以是任何的唯一的值。在实际工作中你很可能会用到`ObjectId`， 所以我们在这里也使用它）

显然，要找到Leto的所有员工，只要执行：

	db.employees.find({manager: ObjectId("4d85c7039ab0fd70a117d730")})

没什么了不起的。在最糟糕的情况下，为弥补连接的缺失需要做的只是再多查询一次而已，该查询很可能是经过索引了的。

#### 数组和嵌入文档（Embedded Documents） ####
MongoDB没有连接并不意味着它没有其他的优势。还记得我们曾说过MongoDB支持数组并把它当成文档中的一级对象吗？当处理多对一或是多对多关系的时候，这一特性就显得非常好用了。用一个简单的例子来说明，如果一个员工有两个经理，我们可以把这个关系储存在一个数组当中：

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d733"), name: 'Siona', manager: [ObjectId("4d85c7039ab0fd70a117d730"), ObjectId("4d85c7039ab0fd70a117d732")] })

需要注意的是，在这种情况下，有些文档中的`manager`可能是一个向量，而其他的却是数组。在两种情况下，前面的`find`还是一样可以工作：

	db.employees.find({manager: ObjectId("4d85c7039ab0fd70a117d730")})

很快您就会发现数组中的值比起多对多的连接表（join-table）来说要更容易处理。

除了数组，MongoDB还支持嵌入文档。尝试插入含有内嵌文档的文档，像这样：

	db.employees.insert({_id: ObjectId("4d85c7039ab0fd70a117d734"), name: 'Ghanima', family: {mother: 'Chani', father: 'Paul', brother: ObjectId("4d85c7039ab0fd70a117d730")}})

也许您会这样想，确实也可以这样做：嵌入文档可以用‘.’符号来查询：

	db.employees.find({'family.mother': 'Chani'})

就这样，我们简要地介绍了嵌入文档适用的场合以及您应该怎样使用它。

#### DBRef ####
MongoDB支持一个叫做DBRef的功能，许多MongoDB的驱动都提供对这一功能的支持。当驱动遇到一个`DBRef`时它会把当中引用的文档读取出来。`DBRef`包含了所引用的文档的ID和所在的集合。它通常专门用于这样的场合：相同集合中的文档需要引用另外一个集合中的不同文档。例如，文档1的`DBRef`可能指向`managers`中的文档，而文档2中的`DBRef`可能指向`employees`中的文档。

#### 反规范化（Denormalization） ####
代替连接的另一种方法就是反规范化数据。在过去，反规范化是为性能敏感代码所设，或者是需要数据快照（例如审计日志）的时候才应用的。然而，随着NoSQL的日渐普及，有许多这样的数据库并不提供连接操作，于是作为规范建模的一部分，反规范化就越来越常见了。这样说并不是说您就需要为每个文档中的每一条信息创建副本。与此相反，与其在设计的时候被复制数据的担忧牵着走，还不如按照不同的信息应该归属于相应的文档这一思路来对数据建模。

比如说，假设您在编写一个论坛的应用程序。把一个`user`和一篇`post`关联起来的传统方法是在`posts`中加入一个`userid`的列。这样的模型中，如果要显示`posts`就不得不读取（连接）`users`。一种简单可行的替代方案就是直接把`name`和`userid`存储在`post`中。您甚至可以用嵌入文档来实现，比如说`user: {id: ObjectId('Something'), name: 'Leto'}`。当然，如果允许用户更改他们的用户名，那么每当有用户名修改的时候，您就需要去更新所有的文档了（这需要一个额外的查询）。

对一些人来说改用这种方法并非易事。甚至在一些情况下根本行不通。不过别不敢去尝试这种方法：有时候它不仅可行，而且就是正确的方法。

#### 您应该选择哪一种？ ####
当处理一对多或是多对多问题的时候，采用id数组往往都是正确的策略。可以这么说，`DBRef`并不是那么常用，虽然您完全可以试着采用这项技术。这使得新手们在面临选择嵌入文档还是手工引用（manual reference）时犹豫不决。

首先，要知道目前一个单独的文档的大小限制是4MB，虽然已经比较大了。了解了这个限制可以为如何使用文档提供一些思路。目前看来多数的开发者还是大量地依赖手工引用来维护数据的关系。嵌入文档经常被使用，but mostly for small pieces of data which we want to always pull with the parent document。一个真实的例子，我把`accounts`文档嵌入存储在用户的文档中，就像这样：

	db.users.insert({name: 'leto', email: 'leto@dune.gov', account: {allowed_gholas: 5, spice_ration: 10}})

这不是说您就应该低估嵌入文档的作用，也不是说应该把它当成是鲜少用到的工具并直接忽略。将数据模型直接映射到目标对象上可以使问题变得更加简单，也往往因此而不再需要连接操作。当您知道MongoDB允许对嵌入文档的域进行查询并做索引后，这个说法就尤其显得正确了。

### 集合：少一些还是多一些？ ###
既然集合不强制使用模式，那么就完全有可能用一个单一的集合以及一个不匹配的文档构建一个系统。以我所见过的情况，大部分的MongoDB系统都像您在关系数据库中所见到的那样布局。换句话说，如果在关系数据库中会用表，那么很有可能在MongoDB中就要用集合（多对多连接表在这里是一个不可忽视的例外）

当把嵌入文档引进来的时候，讨论就会变得更加有意思了。最常见的例子就是博客系统。是应该分别维护`posts`和`comments`两个集合，还是在每个`post`中嵌入一个`comments`数组？暂且不考虑那个4MB的限制（哈姆雷特所有的评论也不超过200KB，谁的博客会比他更受欢迎？），大多数的开发者还是倾向于把数据划分开。因为这样既简洁又明确。

没有什么硬性的规定（呃，除了4MB的限制）。做了不同的尝试之后您就可以凭感觉知道怎样做是对的了。

### 本章小结 ###
本章的目的在于为您用MongoDB给数据建模提供一些有用的指引。用面向文档的系统来建模与用关系数据库不一样，但也不会相差很大。用MongoDB会有一个限制还有多出一些灵活性，不过对于新系统来说，一切都会很好的运行起来的。唯一有可能出错的情况就是不去尝试。

\clearpage

## 第五章 - 何时使用MongoDB ##
至此您应该对MongoDB有了足够的了解并且知道在现有系统中何处以及怎样应用它了。然而，新的存储技术不止一个，让人很容易就被这么多的可选方案搞得不知所措。

对我来说，虽然与MongoDB无关，但最重要的一点是再也不会依赖于某一单一的方案来处理数据了。当然，对很多项目，很可能是大多数项目而言，采用单一的解决方案有很明显的好处，甚至是明智的选择。然而这里要强调的是您并非必须使用不同的技术，而是您可以这样做。只有您知道引入新技术带来的好处会不会大于采用新技术所需的代价。

说了这些，我希望基于目前对MongoDB的介绍，您会把它当成一个通用的方案。前面多次提到，面向文档的数据库和关系数据库有很多的共同点。因此，与其对之避而不谈，不如干脆把MongoDB视为关系数据库的平行方案（alternative。译者：这里的意思是MongoDB与关系数据库的功能类似但又有差异，如果说关系数据库是台式机，可以把MongoDB理解为笔记本电脑，两者有相似处，但是又各有优势，且没有哪一个能替代另一个）。只要是Lucene可以加强关系数据库全文索引能力的场合，或者是Redis可以当作持久性键-值存储的地方，MongoDB都可以用来集中存储管理这些数据。

注意，我没有说MongoDB是关系数据库的替代方案， 而是平行方案。作为一个工具，MongoDB能做的很多其他的工具也可以做到。有些方面MongoDB做得好些，有些方面差些。那么我们现在就来做进一步的分析。

### 无模式 ###
面向文档的数据库常见的一个卖点就是它是无模式的。这使得它比传统数据库的表更加灵活。我同意无模式是很不错的一个特性，但不是因为大多数人说的那些原因。

当人们谈到无模式时，神奇得就好像忽然间我们就可以把一堆毫不匹配的数据通通塞进去一样。确实有这么一些领域或是数据格式，如果用关系数据库来建模是很痛苦的，但我认为这些只是一些不常见的边缘情况。无模式是很酷，不过系统的大部分数据都将是有着良好结构的。偶尔使用不匹配是会很方便，尤其是在引入新功能的时候。可是事实上也可以考虑一下添加一个可以为空的列，这样问题就解决了。

我认为无模式设计的真正益处在于不需要过多的设置，以及与面向对象编程语言结合使用的时候更少的阻力。这一点在使用静态语言的时候更是如此。我曾在C#和Ruby上使用过MongoDB，其间的差别不是一般的大。Ruby的动态特性还有广受欢迎的ActiveRecord实现使得这门语言本身就已经降低了面向关系和面向对象编程之间差异带来的难度。并不是说MongoDB和Ruby不是一个好的组合，相反的它们还真的很搭。我要说的是Ruby程序员眼中MongoDB带来的应该只是一些改进，而对于C#或者Java开发者来说，MongoDB带来的是处理数据方式的翻天覆地的转变。

以一个驱动开发者的角度来看，如果想要保存一个对象，可以把数据串行化为JSON（严格来说应该是BSON，不过JSON也足够接近了）再发送给MongoDB。不需要做属性或类型映射。作为终端开发者的您自然也直接得到了好处。

### 写操作 ###
MongoDB擅长的一个特别角色是日志的记录。MongoDB有两点使得它的写操作非常快。第一，发出一条写命令后它会马上返回而不等待真正的写动作执行。第二，随着1.8中引入的日记功能（journaling），以及2.0中所做的优化加强，现在已经可以根据数据持久性来控制写操作的行为。这些设定的值，加上指定多少个服务器得到一份数据后才算是一次成功的写操作，在每次写的时候都是可以设置的。这些使得对写操作的性能以及数据的持久性的控制都上了一个档次。

除了上述性能上的因素，日志数据还是这样的一种数据格式：它们用无模式集合往往更有优势。最后，MongoDB还有一项技术叫做[定量集合（capped collection）](http://www.mongodb.org/display/DOCS/Capped+Collections)。目前为止，我们所创建的集合都是隐式创建的普通集合。我们可以用`db.createCollection`命令创建并标明它是给标量集合：

	//限制标量集合的大小为1MB
	db.createCollection('logs', {capped: true, size: 1048576})

当上面的定量集合增长到1MB的限制时后，旧的文档就会被自动删除。可以用`max`来限制文档的个数而不是整个集合的尺寸。定量集合有一些有意思的特性。比如说，你可以不断的更新文档，但是文档不会变大。同时，它会保存插入的顺序，因此没有需要添加额外的索引来实现基于时间的排序。

是时候说明这个了：如果需要知道写操作有没有出错（这和默认的射后不理的写行为相反）（译者：fire-and-forget，[射后不理](http://zh.wikipedia.org/wiki/%E5%B0%84%E5%BE%8C%E4%B8%8D%E7%90%86)）只需要再发一个命令：`db.getLastError()`。多数的驱动都会把这种行为封装成一个*安全的写操作*，用`{:safe => true}`作为`insert`的第二个参数来声明。

### 持久性（Durability） ###
在1.8版之前，MongoDB是不支持单服务器的持久性的。也就是说，一个服务器当机就会导致数据丢失。当时的解决方法就是在多台服务器上运行MongoDB（MongoDB支持复制）。新加入到1.8的一个重要特性就是日记（journaling）。打开这个功能需要在我们最早设置MongoDB时创建的`mongodb.config`文件中加入一行`journal=true`（如果想要立即生效，还需要重启服务器）。您应该是会想要打开这项功能的。（在以后的版本中将会默认打开）。不过，在有些情况下，您可能会要关闭日记以增加吞吐量，哪怕这样做存在风险。（需要指出的是有些应用对损失一些数据还是可以接受的）

关于持久性的话题只会在这里提及，因为为了克服MongoDB缺乏单服务器持久性的弊病人们已经做了大量的工作。这段话还可能会出现在以后的Google搜索结果中。您看到的那些说MongoDB这个缺点的信息都是过时了的。

### 全文搜索 ###
真正的全文搜索功能希望能够在将来的MongoDB版本中实现。有了它对数组的支持，基本的全文搜索应该是很容易实现的。至于更高级一点的功能，就需要依仗像Lucene/Solr之类的方案了。当然，这一点上其他很多关系数据库也是一样的。

### 事务（transaction） ###
MongoDB是不支持事务的。不过它有两个替代的方案，其中一个很不错但是不怎么有人用，另外一个很麻烦同时又很灵活。

第一个就是MongoDB的众多原子操作。只要它们能够解决你的问题，都很好用。之前已经介绍过一些简单的诸如`$inc`和`$set`。还有像`findAndModify`这样的原子操作，可以更新或是删除一个文档并返回修改过后的文档，且所有动作在一个原子操作内完成。

当原子操作不能满足要求时，可以退而尝试第二种方案：两阶段提交。两阶段提交之于事务就好比手工解引用（dereference，译者：关于这个词的翻译有很多种，用引、提领、解引用等，大家知道是什么意思就好。）之于连接。这是一个代码中实现的独立于存储系统的方案。两阶段提交其实在关系数据库中有普遍的应用，用以在多个数据库之间实现事务。MongoDB的网站有[一个例子](http://www.mongodb.org/display/DOCS/two-phase+commit)演示了最常见的场合（资金转账）。其主要思想就是把事务的状态储存在需要更新的文档中，并手工一步一步完成初始化-等待-提交或是回滚的每一个步骤。

MongoDB对嵌套文档以及无模式的支持使得两阶段提交稍微不那么痛苦了，不过这依旧不是一个很好的流程，尤其是当您刚刚开始学/用它的时候。

### 数据处理 ###
MongoDB依靠MapReduce来完成大部分的数据处理工作。它有一些[基本的聚合能力](http://www.mongodb.org/display/DOCS/Aggregation)，尽管如此，无论在何种情况下您还是应该使用MapReduce。下一章我们将详细讨论MapReduce。现在把它当成是一个非常强大的工具，另外一种实现`group by`的方法。（这样说事实上低估了MapReduce）MapReduce的一个长处是它可以并行地处理大量的数据。可是MongoDB的实现却依赖于单线程的JavaScript。这又意味着什么呢？意味着如果是处理大量的数据，很可能还是需要用其他的工具，比如说Hadoop。幸好，这两个系统很好的实现了互补，且看这个[MongoDB的Hadoop适配器](https://github.com/mongodb/mongo-hadoop)。

当然，并行处理数据也不是关系数据库所擅长的。现在已经有计划在将来的MongoDB版本中加强处理非常大的数据的能力。

### 地理空间 ###
MongoDB的另外一个很强大的功能就是它对地理信息索引功能的支持。这个功能允许把X和Y坐标储存在文档中，然后可以用`$near`查找文档中靠近某个坐标的点，或是用`$within`找出位于某个矩形或是一个圆形中的点。这个功能可以用一些可视的例子来演示，如果您想了解多一些，我邀请您来试试这个[5分钟交互式地理空间教程](http://tutorial.mongly.com/geo/index)

### 成熟度与可用工具 ###
你应该很可能早已经知道了答案，不过MongoDB明显比大多数的关系数据库要年轻。这个当然是您需要考虑的。究竟这个因素有多重要取决于您在做的是什么系统以及将怎样实现它。无论怎样，一个客观的评价是不会忽略这一事实：MongoDB很年轻而且周边可用的工具也不是很好用（虽然很多非常成熟的关系数据库可用的工具也很糟糕！）例如，不能支持十进制浮点数对货币数据系统来说就是一个很明显的问题（虽然不一定是致命的缺陷）

正面点来看，MongoDB为很多语言提供了驱动，它的协议很现代简约，开发速度非常快。有很多对不成熟的工具心存疑虑的公司都将MongoDB用在了自己已经发布的产品中。这些疑虑当然是有根据的，可是短时间后都已经成为了历史。


### 本章小结 ###
本章要传递的信息是大多数情况下MongoDB都可以替代关系数据库。它更简单更直接，更快而且对应用程序开发者的约束更少。MongoDB不支持事务确实是一个缺点也值得慎重考虑。但是当人们问道*在新数据存储阵营中MongoDB是怎样的一种技术？*时，答案很简单：**它很中庸**。（译者：这里的意思是，作为NoSQL方案的一种，MongoDB显得更通用一些，不像Lucene那样专为搜索而生，也不像结构严格的Redis。）

\clearpage

## 第六章 - MapReduce ##
MapReduce是一种数据处理的方法，有相比较为传统的方案它有两个显著的优势。第一个优势是它卓越的性能，也是最初开发MapReduce的主要目的。理论上MapReduce可以并行工作，可以利用多核/多CPU/多机器同时处理非常大量的数据。我们也说过，这点优势MongoDB无法利用上。第二个优势就是用户可以为数据处理编写真正的程序。与SQL相比，用MapReduce可以实现无线多种功能，在逼不得已寻求更专业的方案之前，MapReduce提供了更多的可能。

MapReduce这种模式越来越普及，几乎任何语言上都有它的实现：C#，Ruby，Java，Python等等。我要说的是一开始它看起来和其他方案很不一样而且很复杂，不过不要泄气，花些时间来实践。无论您用不用MongoDB，它都很值得您去了解。

### 理论与实践 ###
MapReduce的流程分两步。首先做映射（map）然后做缩减（reduce）。在映射时转换输入的文档并输出（emit）键-值组合（键或值可以很复杂）。在缩减时将一个键以及为该键输出的值的数组生成最终的结果。我们来看看这当中的每一步以及相应的输出。

下面的例子假设为某个数据源（比如说一个网页）生成每天的点击数。这相当于MapReduce的*hello world*。为了实现这个应用，我们需要有一个`hits`集合，其中有两个域：`resource`和`date`。我们设计的输出分为：`resource`，`year`，`month`，`day`以及`count`。

又假设`hits`的数据如下：

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

我们希望最终有下面的输出：

	resource  year   month   day   count
	index     2010   1       20    3
	about     2010   1       20    1
	about     2010   1       21    3
	index     2010   1       21    2
	index     2010   1       22    1

当前分析的这个方法有一个好处，那就是通过存储输出的数据，报告很快就可以生成，且数据的增长是可控的。（对于上面的数据源，每天只需要增加最多一个文档）

我们先专注于概念的理解，到了本章快结束时，会有数据和代码的示例供您亲自实验。

首先来看看映射函数。映射的目的在于输出（emit）值以便后续缩减。一个映射有可能不输出或者输出多次值。在我们的例子中，映射总是会输出一次（很正常的做法）。可以把这里的映射想象成遍历hits中的每一个文档。对于每个文档我们要输出一个包含了resource，year，month和day的键，还有一个简单的值，1：

	function() {
		var key = {
		    resource: this.resource,
		    year: this.date.getFullYear(),
		    month: this.date.getMonth(),
		    day: this.date.getDate()
		};
		emit(key, {count: 1});
	}

`this`指的是当前正在分析的文档。希望看到下面映射输出可以让这个过程清楚一些。基于前面的数据，完整的映射输出应该是：

	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}, {count:1}]
	{resource: 'about', year: 2010, month: 0, day: 20} => [{count: 1}]
	{resource: 'about', year: 2010, month: 0, day: 21} => [{count: 1}, {count: 1}, {count:1}]
	{resource: 'index', year: 2010, month: 0, day: 21} => [{count: 1}, {count: 1}]
	{resource: 'index', year: 2010, month: 0, day: 22} => [{count: 1}]

了解这一中间步骤是了解MapReduce的关键。输出的值根据键的不同被组织成相应的数组。.NET和Java的程序员可以把这视为类型`IDictionary<object, IList<object>>`（.NET）或者是`HashMap<Object, ArrayList>`（Java）。


接下来我们人为的修改一下映射函数的行为：

	function() {
		var key = {resource: this.resource, year: this.date.getFullYear(), month: this.date.getMonth(), day: this.date.getDate()};
		if (this.resource == 'index' && this.date.getHours() == 4) {
			emit(key, {count: 5});
		} else {
			emit(key, {count: 1});
		}
	}

第一个中间输出因此变成：

	{resource: 'index', year: 2010, month: 0, day: 20} => [{count: 5}, {count: 1}, {count:1}]

值得注意的是每一次输出是如何按照键的不同来分组生成新的值的。

缩减函数接受中间结果后产生了最后的结果。例子中的缩减函数见下：

	function(key, values) {
		var sum = 0;
		values.forEach(function(value) {
			sum += value['count'];
		});
		return {count: sum};
	};

得到的结果是：

	{resource: 'index', year: 2010, month: 0, day: 20} => {count: 3}
	{resource: 'about', year: 2010, month: 0, day: 20} => {count: 1}
	{resource: 'about', year: 2010, month: 0, day: 21} => {count: 3}
	{resource: 'index', year: 2010, month: 0, day: 21} => {count: 2}
	{resource: 'index', year: 2010, month: 0, day: 22} => {count: 1}

MongoDB中的输出是：

	_id: {resource: 'home', year: 2010, month: 0, day: 20}, value: {count: 3}

希望您注意到这个就是我们想要的结果了。

如果您有注意到，可能会问*为什么不直接用`sum = values.length`？*如果在计算值都是1的数组，这个方法确实是很有效的。可是实际上缩减函数不见得总是会得到完整的中间数据，比如说，不是：

	{resource: 'home', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}, {count:1}]

而是像下面这样调用Reduce：

	{resource: 'home', year: 2010, month: 0, day: 20} => [{count: 1}, {count: 1}]
	{resource: 'home', year: 2010, month: 0, day: 20} => [{count: 2}, {count: 1}]

结果应该还是3，不过计算的路径就不一样了。因此，缩减函数必须具有幂等性。也就是说，多次调用该函数和只调用一次的效果应该是一样的。

一个比较常见的做法是将多个缩减函数链接起来实现更加复杂的分析功能，不过我们在这里就不再深入了。

### Pure Practical ###
MongoDB中是对集合使用`mapReduce`的。`mapReduce`需要一个映射函数，一个缩减函数以及一个输出指令。在shell中我们可以创建并传递一个JavaScript函数的调用。大多数的库都支持这种将函数当作字符串值的方式（虽然有点难看）。首先我们还是来创建一些数据：

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

然后创建我们自己的映射和缩减函数（MongoDB的shell允许多行声明，回车之后您会看到*...*说明shell在等待后续的输入）：

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
有了上面两个函数，就可以对`hits`集合使用`mapReduce`命令了：

	db.hits.mapReduce(map, reduce, {out: {inline:1}})
执行上面的命令后，应该就可以看到期望的输出了。把`out`设成`inline`是为了把`mapReduce`的输出流直接返回到shell中显示。这个功能目前只能用于最多16MB的结果。另外的一个方法就是使用`{out: 'hit_stats'}`以把结果存储在`hit_stats`集合里：

	db.hits.mapReduce(map, reduce, {out: 'hit_stats'});
	db.hit_stats.find();

上面的命令执行之后，`hit_stats`中既有的数据就会丢失。如果用的是`{out: {merge: 'hit_stats'}}`已有键的值就会被新的值覆盖且新的键-值组合就会被作为新的文档插入到集合中。最后，我们可以用`reduce`函数中的`out`选项处理更复杂的情况（比如说插新）。

第三个参数是一个可选项，比如可以用来过滤、排序或是限制需要分析的文档。也可以将一个`finalize`方法应用到`reduce`之后的结果上。

### 本章小结 ###
这是介绍了MongoDB真正与众不同指出的第一个章节。如果您觉得很不自在，要知道您总是可以使用MongoDB的其他[聚合能力](http://www.mongodb.org/display/DOCS/Aggregation)事情变得简单一些。不过归根结底，MapReduce是MongoDB最吸引人的功能之一。学会编写映射函数和缩减函数的关键在于把`映射`输出的数据以及`缩减`所需要的数据可视化，并真正了解这些数据。

\clearpage

## 第七章 - 性能与工具 ##
最后一章，我们将介绍一些性能相关的话题，以及MongoDB开发者可以使用的工具。我们不会过深地涉及这些话题，但当中重要的部分都会有所介绍。

### 索引 ###
最开始的时候我们介绍了特殊的`system.indexes`集合，它含有数据库中所有索引的信息。MongoDB中的索引和关系数据库很像：都有助于改进查询和排序的性能。索引是通过`ensureIndex`创建的：

	// "name"是域的名字
	db.unicorns.ensureIndex({name: 1});

并由`dropIndex`丢弃：

	db.unicorns.dropIndex({name: 1});

要创建唯一的索引可以将第二个参数`unique`设为`true`：

	db.unicorns.ensureIndex({name: 1}, {unique: true});

索引可以在嵌入域（使用`.`符号）和数组域上。也可以创建复合索引：

	db.unicorns.ensureIndex({name: 1, vampires: -1});

索引的顺序（1为升序，-1为降序）对于单键索引没有关系，不过对于复合索引来说在排序或是使用范围条件时就有影响了。

[索引页](http://www.mongodb.org/display/DOCS/Indexes)有更多关于索引的信息。

### Explain ###
要知道索引有没有使用索引，可以对游标使用`explain`方法：

	db.unicorns.find().explain()

命令的结果告诉我们查询使用的是`BasicCursor`（也就是说没有用索引），扫描了12个对象，用了多长时间， 是否用了什么索引，如果有用索引的话还有一些额外有用的信息。

如果我们改用索引来查询，我们会看到的查询使用的`BtreeCursor`，以及用以查询的索引：

	db.unicorns.find({name: 'Pilot'}).explain()

### 射后不理的写操作 ###
之前我们有提到过，MongoDB中的写操作默认为[射后不理](http://zh.wikipedia.org/wiki/%E5%B0%84%E5%BE%8C%E4%B8%8D%E7%90%86)。这样做可以获得一定的性能提高，同时也带来了系统奔溃时丢失数据的风险。有意思的是这种类型的写操作在插入/更新破坏了某唯一的约束时，是不返回错误的。若需要得到写失败的通知，就要在插入后调用`db.getLastError()`。很多驱动都把这一细节封装起来了，取而代之的是*安全的写*操作——往往会多提供一个参数用来设置。

可惜的是shell并不提供安全的插入，因此我们就无法实验这一特性了。

### 分片（sharding） ###
MongoDB支持自动分片。分片技术是将数据水平切分存储在多台服务器上以实现可扩展性的一种方法。比较简单的实现可以将所有以A至M字母开头的用户信息存在1号服务器上然后剩下的都存在2号服务器上。幸运的是，MongoDB的分片功能远远超过了这种简单的方法。不过分片已经超出了本书要讨论的范畴，但您应该知道它的存在并且在系统需要用到多台服务器时会考虑这种技术。

### 复制 ###
MongoDB的复制于关系数据库的复制类似。写入的数据发送到主服务器，主服务器再与其他从服务器进行同步。读操作可以选择在从服务器上做或者是在主服务器上做。当主服务器当机的时候，可以将一台从服务器升级为新的主服务器。这样做可以分散系统的负荷，不过有读到陈旧数据的可能。MongoDB的复制也超出了本书要讨论的范围。

虽然复制可以提高性能（通过分散读操作），它的主要作用还是增加可靠性。将分片和复制结合是一种很普遍的做法。例如，每一个分片都可以由一个主服务器和一个从服务器维护。（从技术角度上还需要一个仲裁机以解决两个从服务器试图升级为主服务器的问题。不过仲裁机耗费的资源非常少，因此可以用在多个分片上）

### 统计 ###
可以通过`db.stats()`获得数据库的数据统计信息。当中的大多数都和数据库的大小有关。也可以获取某个集合的统计信息。比如说可以用`db.unicorns.stats()`获得`unicorns`的相关信息。这些信息大部分还是和集合的大小相关。

### 网络接口 ###
在MongoDB启动时的信息中有一个基于网络的管理工具的链接（如果您在shell中向上翻页到启动`mongod`时的部分应该还可以看到）。可以在浏览器中输入<http://localhost:28017/>以访问该工具。为了更好的使用这个工具，还需要在配置文件中加入`rest=true`并重启`mongod`进程。网络接口提供了很多关于服务器当前状态的信息。

### 分析器（Profiler） ###
下面的命令将启动MongoDB的分析器：

	db.setProfilingLevel(2);

启动之后，可以运行下面的命令：

	db.unicorns.find({weight: {$gt: 600}});

再读取分析器中的值：

	db.system.profile.find()

最后的输出提供了这些信息：什么时候运行了什么命令，有多少文档被扫描过以及返回了多少数据。

可以将参数设成`0`，用`setProfileLevel`再次关闭分析器。当把参数设为`1`时，只有对耗时多于100毫秒的查询才会进行分析。或者也可以在第二个参数中指明最少的时间，以毫秒为单位：

	//分析所有耗费时间多于一秒的操作
	db.setProfilingLevel(1, 1000);

### 备份和恢复 ###
在MongoDB的`bin`目录中有一个`mongodump`可执行文件。执行这一文件将连接到localhost并且将所有的数据库备份到一个叫做`dump`的子目录中。更多的选项可以通过`mongodump --help`获得。常见的选项有`--db DBNAME`，用以对某一特定的数据库进行备份，还有`--collection COLLECTIONAME`可以用来备份一个集合。利用`bin`下的`mongorestore`可以恢复上一次的备份。同样的，用`--db`和`--collection`参数可以恢复某一指定的数据库或者集合。

例如，以下命令将把我们创建的`learn`数据库备份到`backup`目录（该命令是在终端窗口下输入的，而非mongo的shell中）：

	mongodump --db learn --out backup

如果只需要恢复`unicorns`集合，可以这样做：

	mongorestore --collection unicorns backup/learn/unicorns.bson

需要指出的是`mongoexport`和`mongoimport`这两个可执行文件可以用以输出和导入JSON或是CSV中的数据。例如可以输出为JSON格式的数据：

	mongoexport --db learn -collection unicorns

或者是CSV格式的输出：

	mongoexport --db learn -collection unicorns --csv -fields name,weight,vampires

请注意，`mongoexport`和`mongoimport`并不总是能反应真实的数据。只有`mongodump`和`mongorestore`可以用以真正的备份。

### 本章小结 ###
在这章中我们看到了MongoDB的不同命令、工具以及性能方面的细节。并不是所有的东西都有介绍，我们只选了最常见的那些。MongoDB的索引和关系数据库中的索引很相像，其他的很多工具也是这样。但是，很多工具的使用在MongoDb中更简洁扼要。

\clearpage

## 总结 ##
至此您对MongoDB的了解已经足以开始在实际项目中使用它了。关于MongoDB的远不止我们所介绍的这些，不过您需要做的下一件事应该是把在这里所学到的只是汇总起来，并熟悉您即将用到的驱动。MongoDb的[网站](http://www.mongodb.com/)有很多有用的信息。其[官方讨论组](http://groups.google.com/group/mongodb-user)则是一个问问题的好地方。

NoSQL的诞生不仅仅因为有必要，同时也是为了实践新的方法。应该承认的是这个领域一直在向前发展，尽管有时候会失败，但是如果我们不去尝试，就无法拥抱成功。我想，这应该是我们推进职业生涯的正确方法。
