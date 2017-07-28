#  二叉树&&并查集：
----
重新看了二叉树和并查集，理解了怎么层次建树
# Python
----
看了一些网站的爬虫，大致的了解了简单网站爬虫的实现原理<br>
百思不得姐视频源码
```.py
import urllib.request, re, requests
url_name = []
def get(url,pan):
    hd = {'User-Agent':'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'}
    #url = 'http://www.budejie.com/video/'
    html = requests.get(url, headers=hd).text
    # print(html)
    url_content = re.compile(r'(<div class="j-r-list-c">.*?</div>.*?</div>)',re.S)
    url_contents = re.findall(url_content,html)
    # print(url_contents)
    for i in url_contents: # 大盒子里面的html
        url_reg = r'data-mp4="(.*?)"'
        url_item = re.findall(url_reg,i)
        # print(type(url_items)) # <class 'list'>
        # print(url_item)
        if url_item:
            name_reg = re.compile(r'<a href="/detail-.{8}?.html">(.*?)</a>',re.S) # .{8}?匹配8位数字
            name_item = re.findall(name_reg,i) # findall返回的是一个列表
            # print(type(name_items)) # <class 'list'>
            # print(name_items)
            for i,k in zip(name_item,url_item):
                url_name.append([i,k]) # 将列表添加到列表中，其实，也可以将元组存入列表，url_name.append((i,k))
                # print(url_name)
                # print(i,k)
    for i in url_name:
        print('正在下载>>>>>  '+i[0]+':'+i[1])
        # 每个元素的i[0]是名称，i[1]是视频url
        urllib.request.urlretrieve(i[1],pan+'\\%s.mp4'%(i[0])) # video\\%s
if __name__ == '__main__':
    url = input('请输入网址：')
    pan = input('请输入盘符：')
    get(url,pan)
 ```
