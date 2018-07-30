# Python3网络爬虫快速入门篇

- 转载请注明作者和出处： [https://zhuanlan.zhihu.com/p/40886441](https://zhuanlan.zhihu.com/p/40886441)
- Github代码获取：[https://github.com/jiguang123/Web-Spider-and-Data-Analysis](https://github.com/jiguang123/Web-Spider-and-Data-Analysis)
- Python版本： Python3.6
- 运行环境： Win10 + Anaconda + jupyter Notebook + Sublime text3

## 1、网络爬虫简介

网络爬虫，也叫网络蜘蛛(Web Spider)。它根据网页地址(URL)爬取网页内容，而网页地址(URL)就是我们在浏览器中输入的网站链接。比如：https://www.baidu.com/，它就是一个URL。

在讲解爬虫内容之前，我们需要先学习一项写爬虫的必备技能：审查元素

### 1.1、审查元素

在浏览器的地址栏输入URL地址，在网页处右键单击，找到检查。(不同浏览器的叫法不同，Chrome浏览器叫做检查，Firefox浏览器叫做查看元素，但是功能都是相同的)

![1](https://pic1.zhimg.com/80/v2-d7b1fe29eb683621b850b044c990583a_hd.jpg)

我们可以看到，右侧出现了一大推代码，这些代码就叫做HTML。什么是HTML？举个容易理解的例子：我们的基因决定了我们的原始容貌，服务器返回的HTML决定了网站的原始容貌。

![2](https://pic3.zhimg.com/80/v2-5182bc1f89c4075e88c21ed2a9752294_hd.jpg)

为啥说是原始容貌呢？因为人可以整容啊！扎心了，有木有？那网站也可以"整容"吗？可以！请看下图：

![3](https://pic2.zhimg.com/80/v2-02fff2a6cf6291a80bbac6a780866bb4_hd.jpg)

我能有这么多钱吗？显然不可能。我是怎么给网站"整容"的呢？就是通过修改服务器返回的HTML信息。我们每个人都是"整容大师"，可以修改页面信息。我们在页面的哪个位置点击审查元素，浏览器就会为我们定位到相应的HTML位置，进而就可以在本地更改HTML信息。

再举个小例子：我们都知道，使用浏览器"记住密码"的功能，密码会变成一堆小黑点，是不可见的。可以让密码显示出来吗？可以，只需给页面"动个小手术"！以淘宝为例，在输入密码框处右键，点击检查。

![4](https://pic3.zhimg.com/80/v2-162be41dd5db664d52e78cb3e17829b5_hd.jpg)

可以看到，浏览器为我们自动定位到了相应的HTML位置。将下图中的password属性值改为text属性值(直接在右侧代码处修改)：

![5](https://pic3.zhimg.com/80/v2-ea01c7778e34f6092e1394c8f4a695fc_hd.jpg)

我们让浏览器记住的密码就这样显现出来了：

![6](https://pic4.zhimg.com/80/v2-ef53f8393f9a5b90203f6cbc6bc6b1b2_hd.jpg)


说这么多，什么意思呢？浏览器就是作为客户端从服务器端获取信息，然后将信息解析，并展示给我们的。我们可以在本地修改HTML信息，为网页"整容"，但是我们修改的信息不会回传到服务器，服务器存储的HTML信息不会改变。刷新一下界面，页面还会回到原本的样子。这就跟人整容一样，我们能改变一些表面的东西，但是不能改变我们的基因。


## 2、简单实例

网络爬虫的第一步就是根据URL，获取网页的HTML信息。在Python3中，可以使用urllib.request和requests进行网页爬取。

- urllib库是python内置的，无需我们额外安装，只要安装了Python就可以使用这个库。
- requests库是第三方库，需要我们自己安装。

requests库强大好用，所以本文使用requests库获取网页的HTML信息。requests库的github地址：[https://github.com/requests/requests](https://github.com/requests/requests)


**requests安装**

在cmd中，使用如下指令安装requests：

	pip install requests

**简单实例**

requests库的基础方法如下：

![7](https://pic4.zhimg.com/80/v2-4c47e8dc34bea8de9a387a079357e48a_hd.jpg)


官方中文教程地址：[http://docs.python-requests.org/zh_CN/latest/user/quickstart.html](http://docs.python-requests.org/zh_CN/latest/user/quickstart.html)

requests库的开发者为我们提供了详细的中文教程，查询起来很方便。本文不会对其所有内容进行讲解，摘取其部分使用到的内容，进行实战说明。

首先，让我们看下requests.get()方法，它用于向服务器发起GET请求，不了解GET请求没有关系。我们可以这样理解：get的中文意思是得到、抓住，那这个requests.get()方法就是从服务器得到、抓住数据，也就是获取数据。让我们看一个例子(以 http://www.gitbook.cn为例)来加深理解：


	# -*- coding:UTF-8 -*-
	import requests
	
	if __name__ == '__main__':
	    target = 'http://gitbook.cn/'
	    req = requests.get(url=target)
	    print(req.text)


requests.get()方法必须设置的一个参数就是url，因为我们得告诉GET请求，我们的目标是谁，我们要获取谁的信息。运行程序看下结果：

![8](https://pic4.zhimg.com/80/v2-eee091e712427f9460a30ccd8f4466c6_hd.jpg)

左侧是我们程序获得的结果，右侧是我们在http://www.gitbook.cn网站审查元素获得的信息。我们可以看到，我们已经顺利获得了该网页的HTML信息。这就是一个最简单的爬虫实例，可能你会问，我只是爬取了这个网页的HTML信息，有什么用呢？客官稍安勿躁，接下来进入我们的实战正文。

## 3、爬虫实战

### 3.1、实战背景

网站笔趣阁：[www.biqukan.com/](https://link.zhihu.com/?target=http%3A//www.biqukan.com/)是一个盗版小说网站，这里有很多起点中文网的小说，该网站小说的更新速度稍滞后于起点中文网正版小说的更新速度。并且该网站只支持在线浏览，不支持小说打包下载。因此，本次实战就是从该网站爬取并保存一本名为《一念永恒》的小说，该小说是耳根正在连载中的一部玄幻小说。

### 3.2、小试牛刀

我们先看下《一念永恒》小说的第一章内容。

URL：[http://www.biqukan.com/1_1094/5403177.html](http://www.biqukan.com/1_1094/5403177.html)

![9](https://pic3.zhimg.com/80/v2-8a066c804cdf0c5bae3e8c1dda6bb3da_hd.jpg)

我们先用已经学到的知识获取HTML信息试一试，编写代码如下：

	# -*- coding:UTF-8 -*-
	import requests
	
	if __name__ == '__main__':
	    target = 'http://www.biqukan.com/1_1094/5403177.html'
	    req = requests.get(url=target)
	    print(req.text)


运行代码，可以看到如下结果：

![10](https://pic3.zhimg.com/80/v2-a14a4766a2a45265eb3c9066d5aa3916_hd.jpg)

可以看到，我们很轻松地获取了HTML信息。但是，很显然，很多信息是我们不想看到的，我们只想获得如右侧所示的正文内容，我们不关心div、br这些html标签。如何把正文内容从这些众多的html标签中提取出来呢？这就是本次实战的主要内容。

### 3.3、Beautiful Soup

爬虫的第一步，获取整个网页的HTML信息，我们已经完成。接下来就是爬虫的第二步，解析HTML信息，提取我们感兴趣的内容。对于本小节的实战，我们感兴趣的内容就是文章的正文。提取的方法有很多，例如使用正则表达式、Xpath、Beautiful Soup等。对于初学者而言，最容易理解，并且使用简单的方法就是使用Beautiful Soup提取感兴趣内容。

Beautiful Soup的安装方法和requests一样，使用如下指令安装(也是二选一)：

- pip install beautifulsoup4
- easy_install beautifulsoup4

一个强大的第三方库，都会有一个详细的官方文档。我们很幸运，Beautiful Soup也是有中文的官方文档。URL：[http://beautifulsoup.readthedocs.io/zh_CN/latest/](http://beautifulsoup.readthedocs.io/zh_CN/latest/)



同理，我会根据实战需求，讲解Beautiful Soup库的部分使用方法，更详细的内容，请查看官方文档。

现在，我们使用已经掌握的审查元素方法，查看一下我们的目标页面，你会看到如下内容：

![11](https://pic2.zhimg.com/80/v2-72f39b1368b4aa8fdc1488cda3c1c574_hd.jpg)


不难发现，文章的所有内容都放在了一个名为div的“东西下面”，这个"东西"就是html标签。HTML标签是HTML语言中最基本的单位，HTML标签是HTML最重要的组成部分。不理解，没关系，我们再举个简单的例子：
    
    一个女人的包包里，会有很多东西，她们会根据自己的习惯将自己的东西进行分类放好。镜子和口红这些会经常用到的东西，会归放到容易拿到的外侧口袋里。那些不经常用到，需要注意安全存放的证件会放到不容易拿到的里侧口袋里。

html标签就像一个个“口袋”，每个“口袋”都有自己的特定功能，负责存放不同的内容。显然，上述例子中的div标签下存放了我们关心的正文内容。这个div标签是这样的：

	<div id="content", class="showtxt">

细心的朋友可能已经发现，除了div字样外，还有id和class。id和class就是div标签的属性，content和showtxt是属性值，一个属性对应一个属性值。这东西有什么用？它是用来区分不同的div标签的，因为div标签可以有很多，我们怎么加以区分不同的div标签呢？就是通过不同的属性值。

仔细观察目标网站一番，我们会发现这样一个事实：class属性为showtxt的div标签，独一份！这个标签里面存放的内容，是我们关心的正文部分。

知道这个信息，我们就可以使用Beautiful Soup提取我们想要的内容了，编写代码如下：

	# -*- coding:UTF-8 -*-
	from bs4 import BeautifulSoup
	import requests
	if __name__ == "__main__":
	     target = 'http://www.biqukan.com/1_1094/5403177.html'
	     req = requests.get(url = target)
	     html = req.text
	     bf = BeautifulSoup(html)
	     texts = bf.find_all('div', class_ = 'showtxt') 
		 print(texts)

在解析html之前，我们需要创建一个Beautiful Soup对象。BeautifulSoup函数里的参数就是我们已经获得的html信息。然后我们使用find_all方法，获得html信息中所有class属性为showtxt的div标签。find_all方法的第一个参数是获取的标签名，第二个参数class_是标签的属性，为什么不是class，而带了一个下划线呢？因为python中class是关键字，为了防止冲突，这里使用class_表示标签的class属性，class_后面跟着的showtxt就是属性值了。看下我们要匹配的标签格式：

	<div id="content", class="showtxt">

这样对应的看一下，是不是就懂了？可能有人会问了，为什么不是find_all('div', id = 'content', class_ = 'showtxt')?这样其实也是可以的，属性是作为查询时候的约束条件，添加一个class_='showtxt'条件，我们就已经能够准确匹配到我们想要的标签了，所以我们就不必再添加id这个属性了。运行代码查看我们匹配的结果：

![12](https://pic3.zhimg.com/80/v2-65dbfaf9d63ba8edc73f55a236abed22_hd.jpg)


我们可以看到，我们已经顺利匹配到我们关心的正文内容，但是还有一些我们不想要的东西。比如div标签名，br标签，以及各种空格。怎么去除这些东西呢？我们继续编写代码：


	# -*- coding:UTF-8 -*-
	from bs4 import BeautifulSoup
	import requests
	if __name__ == "__main__":
	     target = 'http://www.biqukan.com/1_1094/5403177.html'
	     req = requests.get(url = target) html = req.text
	     bf = BeautifulSoup(html)
	     texts = bf.find_all('div', class_ = 'showtxt')
	     print(texts[0].text.replace('\xa0'*8,'\n\n'))

find_all匹配的返回的结果是一个列表。提取匹配结果后，使用text属性，提取文本内容，滤除br标签。随后使用replace方法，剔除空格，替换为回车进行分段。&nbsp;在html中是用来表示空格的。replace('\xa0'*8,'\n\n')就是去掉下图的八个空格符号，并用回车代替：

![13](https://pic1.zhimg.com/80/v2-71ebd90a4c1eafadf8651ba150eeca49_hd.jpg)

程序运行结果如下：

![14](https://pic1.zhimg.com/80/v2-216c55250a76047ced25aeff75fb7fd6_hd.jpg)

可以看到，我们很自然的匹配到了所有正文内容，并进行了分段。我们已经顺利获得了一个章节的内容，要想下载正本小说，我们就要获取每个章节的链接。我们先分析下小说目录：

URL：[http://www.biqukan.com/1_1094/](http://www.biqukan.com/1_1094/)

![15](https://pic3.zhimg.com/80/v2-a0a08015de5d7613804991d679ce6692_hd.jpg)

通过审查元素，我们发现可以发现，这些章节都存放在了class属性为listmain的div标签下，选取部分html代码如下：

	<div class="listmain">
	<dl>
	<dt>《一念永恒》最新章节列表</dt>
	<dd><a href="/1_1094/15932394.html">第1027章 第十道门</a></dd>
	<dd><a href="/1_1094/15923072.html">第1026章 绝伦道法！</a></dd>
	<dd><a href="/1_1094/15921862.html">第1025章 长生灯！</a></dd>
	<dd><a href="/1_1094/15918591.html">第1024章 一目晶渊</a></dd>
	<dd><a href="/1_1094/15906236.html">第1023章 通天道门</a></dd>
	<dd><a href="/1_1094/15903775.html">第1022章 四大凶兽！</a></dd>
	<dd><a href="/1_1094/15890427.html">第1021章 鳄首！</a></dd>
	<dd><a href="/1_1094/15886627.html">第1020章 一触即发！</a></dd>
	<dd><a href="/1_1094/15875306.html">第1019章 魁祖的气息！</a></dd>
	<dd><a href="/1_1094/15871572.html">第1018章 绝望的魁皇城</a></dd>
	<dd><a href="/1_1094/15859514.html">第1017章 我还是恨你！</a></dd>
	<dd><a href="/1_1094/15856137.html">第1016章 从来没有世界之门！</a></dd>
	<dt>《一念永恒》正文卷</dt> <dd><a href="/1_1094/5386269.html">外传1 柯父。</a></dd>
	<dd><a href="/1_1094/5386270.html">外传2 楚玉嫣。</a></dd>
    <dd><a href="/1_1094/5386271.html">外传3 鹦鹉与皮冻。</a></dd>
	<dd><a href="/1_1094/5403177.html">第一章 他叫白小纯</a></dd>
    <dd><a href="/1_1094/5428081.html">第二章 火灶房</a></dd>
	<dd><a href="/1_1094/5433843.html">第三章 六句真言</a></dd> 
    <dd><a href="/1_1094/5447905.html">第四章 炼灵</a></dd>
	</dl>
	</div>

在分析之前，让我们先介绍一个概念：父节点、子节点、孙节点。div和/div限定了div标签的开始和结束的位置，他们是成对出现的，有开始位置，就有结束位置。我们可以看到，在div标签包含dl标签，那这个dl标签就是div标签的子节点，dl标签又包含dt标签和dd标签，那么dt标签和dd标签就是div标签的孙节点。有点绕？那你记住这句话：谁包含谁，谁就是谁儿子！

他们之间的关系都是相对的。比如对于dd标签，它的子节点是a标签，它的父节点是dl标签。这跟我们人是一样的，上有老下有小。

看到这里可能有人会问，这有好多dd标签和a标签啊！不同的dd标签，它们是什么关系啊？显然，兄弟姐妹喽！我们称它们为兄弟结点。

好了，概念明确清楚，接下来，让我们分析一下问题。我们看到每个章节的名字存放在了a标签里面。a标签还有一个href属性。这里就不得不提一下a标签的定义了，a标签定义了一个超链接，用于从一张页面链接到另一张页面。a标签最重要的属性是href属性，它指示链接的目标。



我们将之前获得的第一章节的URL和a标签对比看一下：

	http://www.biqukan.com/1_1094/5403177.html
	<a href="/1_1094/5403177.html">第一章 他叫白小纯</a>


不难发现，a标签中href属性存放的属性值/1_1094/5403177.html是章节URL:[http://www.biqukan.com/1_1094/5403177.html](http://www.biqukan.com/1_1094/5403177.html)的后半部分。其他章节也是如此！那这样，我们就可以根据a标签的href属性值获得每个章节的链接和名称了。

总结一下：小说每章的链接放在了class属性为listmain的div标签下的a标签中。链接具体位置放在html->body->div->dl->dd->a的href属性中。先匹配class属性为listmain的div标签，再匹配a标签。编写代码如下：

	# -*- coding:UTF-8 -*-
	from bs4 import BeautifulSoup
	import requests
	if __name__ == "__main__":
	     target = 'http://www.biqukan.com/1_1094/'
	     req = requests.get(url = target)
	     html = req.text
	     div_bf = BeautifulSoup(html)
	     div = div_bf.find_all('div', class_ = 'listmain')
	     print(div[0])

还是使用find_all方法，运行结果如下：

![16](https://pic1.zhimg.com/80/v2-faa34bd51009ec1dbcb2e0749320e3e4_hd.jpg)


很顺利，接下来再匹配每一个a标签，并提取章节名和章节文章。如果我们使用Beautiful Soup匹配到了下面这个a标签，如何提取它的href属性和a标签里存放的章节名呢？

	<a href="/1_1094/5403177.html">第一章 他叫白小纯</a>

方法很简单，对Beautiful Soup返回的匹配结果a，使用a.get('href')方法就能获取href的属性值，使用a.string就能获取章节名，编写代码如下：


	# -*- coding:UTF-8 -*-
	from bs4 import BeautifulSoup
	import requests
	if __name__ == "__main__":
	     server = 'http://www.biqukan.com/'
	     target = 'http://www.biqukan.com/1_1094/'
	     req = requests.get(url = target) html = req.text
	     div_bf = BeautifulSoup(html)
	     div = div_bf.find_all('div', class_ = 'listmain')
	     a_bf = BeautifulSoup(str(div[0]))
	     a = a_bf.find_all('a')
	     for each in a:
	          print(each.string, server + each.get('href'))

因为find_all返回的是一个列表，里边存放了很多的a标签，所以使用for循环遍历每个a标签并打印出来，运行结果如下。

![17](https://pic3.zhimg.com/80/v2-e4b4eb2d8dc052ce3ed87e7eba9bdbc4_hd.jpg)

最上面匹配的一千多章的内容是最新更新的12章节的链接。这12章内容会和下面的重复，所以我们要滤除，除此之外，还有那3个外传，我们也不想要。这些都简单地剔除就好。


### 3.4、整合代码

每个章节的链接、章节名、章节内容都有了。接下来就是整合代码，将获得内容写入文本文件存储就好了。编写代码如下：

	# -*- coding:UTF-8 -*-
	from bs4 import BeautifulSoup
	import requests, sys
	
	"""
	类说明:下载《笔趣看》网小说《一念永恒》
	Parameters:
	    无
	Returns:
	    无
	"""
	class downloader(object):
	
	    def __init__(self):
	        self.server = 'http://www.biqukan.com/'
	        self.target = 'http://www.biqukan.com/1_1094/'
	        self.names = []            #存放章节名
	        self.urls = []            #存放章节链接
	        self.nums = 0            #章节数
	
	    """
	    函数说明:获取下载链接
	    Parameters:
	        无
	    Returns:
	        无
	    """
	    def get_download_url(self):
	        req = requests.get(url = self.target)
	        html = req.text
	        div_bf = BeautifulSoup(html)
	        div = div_bf.find_all('div', class_ = 'listmain')
	        a_bf = BeautifulSoup(str(div[0]))
	        a = a_bf.find_all('a')
	        self.nums = len(a[15:])                                #剔除不必要的章节，并统计章节数
	        for each in a[15:]:
	            self.names.append(each.string)
	            self.urls.append(self.server + each.get('href'))
	
	    """
	    函数说明:获取章节内容
	    Parameters:
	        target - 下载连接(string)
	    Returns:
	        texts - 章节内容(string)
	    """
	    def get_contents(self, target):
	        req = requests.get(url = target)
	        html = req.text
	        bf = BeautifulSoup(html)
	        texts = bf.find_all('div', class_ = 'showtxt')
	        texts = texts[0].text.replace('\xa0'*8,'\n\n')
	        return texts
	
	    """
	    函数说明:将爬取的文章内容写入文件
	    Parameters:
	        name - 章节名称(string)
	        path - 当前路径下,小说保存名称(string)
	        text - 章节内容(string)
	    Returns:
	        无
	    """
	    def writer(self, name, path, text):
	        write_flag = True
	        with open(path, 'a', encoding='utf-8') as f:
	            f.write(name + '\n')
	            f.writelines(text)
	            f.write('\n\n')
	
	if __name__ == "__main__":
	    dl = downloader()
	    dl.get_download_url()
	    print('《一年永恒》开始下载：')
	    for i in range(dl.nums):
	        dl.writer(dl.names[i], '一念永恒.txt', dl.get_contents(dl.urls[i]))
	        sys.stdout.write("  已下载:%.3f%%" %  float(i/dl.nums) + '\r')
	        sys.stdout.flush()
	    print('《一年永恒》下载完成')
	
		

很简单的程序，单进程跑，没有开进程池。下载速度略慢，喝杯茶休息休息吧。代码运行效果如下图所示：

![18](https://pic3.zhimg.com/v2-c08627a0745ef25fb9c69616af752e2e_b.gif)





























