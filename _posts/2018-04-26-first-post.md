---
layout: post
title: "github page, jekyll로 블로그 시작하기"
date: 2018-04-26 12:00:00 
lastmod : 2016-04-26 16:30:00
sitemap :
  changefreq : weekly
  priority : 1.0
---
첫 포스팅을 시작하며
===========
블로그를 처음 시작하기 위해서 여러가지 플랫폼을 찾아봤다. `브런치`, `티스토리`, `github pages`, `네이버/다음` 들을 많이 사용하는 플랫폼으로 찾을 수 있었다.  
일단 사용목적은 앞으로 진행할 프로젝트에 대한 내용 기록이였고, 검색에 노출되는 것은 크게 중요하지 않았다. (`github pages`는 검색엔진에 노출 시키기 위해 따로 해야하는 작업이 있다.)  
그리고 평소에 업무에서 `SVN`을 사용하다보니 `git`을 사용할 기회가 적었고, 그럼 이번기회에 `git`이랑 친해져 봐야겠다는 생각으로 `github pages`로 선택했다.  
(막상 만들어보니 그렇게 친해지는 것 같진 않다. 첫 포스팅을 하기까지 너무 힘들었어..)  
아래는 각 사이트 링크.
    
[![github pages](/assets/images/github-pages-examples.png)](https://pages.github.com/){:target="_blank"}
   
[![jeykll](/assets/images/jekyll.png)](https://jekyllrb.com/){:target="_blank"}  

작업을 진행하는 과정은 왠만한 블로그에 이미 아주아주 잘 정리되어있다. 여기에는 내가 작업하면서 겪은 시행착오들을 정리해야겠다.  
(이것저것 해보려고 골랐다가 그냥 다른 플랫폼으로 넘어가야할지 정말 진지하게, 정말 여러번 고민했다.)  
  
###github pages repository 생성
- - -
먼저 github pages는 git repository에 리소스들을 저장한다. github pages는 이 저장된 리소스들로 블로그를 생성해준다. 그러기 위해서 제일 먼저 repository를 생성했다.  
여기서 문제는 내 github `username`과 github pages용 `url`을 동일하게 맞춰야 했는데 다르게 만들었다가 페이지 생성이 되지 않았다.
```
github username : whitekingfish
github pages url : https://whitekingfish.github.io
```
이런식으로 동일해야 하는데 다르게 해보려다가 삭제하고 다시 만들었다. `username`과 `url`은 동일하게 생성하자.  

###ruby devkit 설치
- - -
일단 github pages는 저장소에 `push`만 하면 페이지를 생성해준다. 하지만 글을 수정하고 작성할 때 마다 `commit`, `push` 하고 페이지에서 확인하기는 귀찮은 작업이다. 그리고 밑에 있지만 `_draft` 폴더에 있는 내용은 볼 수도 없다. 그래서 로컬에서 포스트를 작성하고 확인하려면 `ruby`를 설치해야한다.   
(처음에 `jekyll` 프로젝트를 생성하기 위해서도 설치해야 한다. 하지만 테마를 가져다 쓰는 경우에는 이미 구조화 된 프로젝트를 통채로 받을 수 있긴하다. 처음엔 이걸 몰랐다.)    
내가 앞으로 쓸일이 있을까 하는 프로그램은 설치하기가 꺼려진다. `ruby`도 그랬다. 최소한으로 필요한 것만 설치해보려다가 결국 지우고 다시 설치했다.(필요한 것을 모르고 설치했기 때문이다. 앞으로는 뭐가 필요한지부터 보자.) `ruby`를 설치하는 이유는 `jekyll`을 설치하고 실행하기 위해서이다. 또 `jekyll`을 설치하기 위해서는 `msys2`가 필요하고 리눅스 명령어도 필요하다.(os 환경이 맥이나 리눅스이면 아무 문제없다.) 필요한 모든 것이 `devkit`에 포함되어 있다. 용량이 커보인다고 패스하지말고, 그냥 디폴트로 쭉 설치하자.   
   
![ruby_devkit](/assets/images/ruby_devkit.jpg)   

###comment 추가하기 (DISQUS)
- - -
github pages는 댓글을 달려면 작업이 필요하다. `disqus`랑 연결하는 법은 정말 많이 나온다. 하지만 나처럼 jekyll 프로젝트를 정말 깡통으로 생성한 사람은 이 부분에서 많이 해맬 수도 있다.(나만 그런걸 수도 있다.)   
과정을 보면서 하다보면 `_layout/post.html`에 복사한 내용만 넣어주면 된다고 한다. 그러면 내가 열심히 쓴 포스팅내용은 없어지고 댓글창만 보인다.   
해결방법은 시키는 대로만 복사해서 `post.html`을 만들지 않으면 된다. 아래 소스를 참고하자.
```html
---
layout: default
---
<!-- 있어야 되는데 몰랐던 부분 start-->
<article class="post" itemscope itemtype="http://schema.org/BlogPosting">
    <header class="post-header">
        <div>
            <h1 class="post-title" itemprop="name headline">{{ page.title }}</h1>
            <p class="post-meta">
                <time datetime="{{ page.date | date_to_xmlschema }}" itemprop="datePublished">{{ page.date | date_to_long_string }}</time>
            </p>
        </div>
    </header>

    <div class="post-content " itemprop="articleBody">
        {{ content }}
    </div>

    <!-- 시키는대로 긁어온 부분 start-->
    <div id="disqus_thread"></div>
    <script>
        (function() {
        var d = document, s = d.createElement('script');
        s.src = 'https://whitekingfish.disqus.com/embed.js';
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
        })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
    <!-- 시키는대로 긁어온 부분 end-->
</article>
<!-- 있어야 되는데 몰랐던 부분 end-->
```
`post.html`에 내용을 추가하면 생성되는 모든 포스트에는 따로 작업하지 않아도 `post.html`에 있는 내용이 포함된다.(근데 나는 시키는대로 긁어온 부분만 `post.html`에 넣으니 모든 포스트에 댓글만 넣겠다고 설정했던 것.)

###google sitemap 등록
- - -
github pages는 기본적으로 검색엔진에서 검색이 되지 않는다. 나만보기 아쉬운 글들을 google에 공개하려면 추가로 작업이 또 필요하다.(초반에 작업이 너무 많이 필요하다.)   
필요한 작업은 `사이트맵`을 `google search console`에 등록해 주는 것이다. 사이트맵은 말그대로 사이트를 찾는 지도이고 기본양식은 금방 찾을 수 있다. 근데 등록을 하면 404 error가 나면서 sitemap을 등록할 수 없다고 한다. 정말 오래 걸렸다. Sitemap, sitemap, sitempa.xml, Sitemap.xml 다양한 이름으로 해보았지만 증상은 똑같았다.(등록은 앞에 언급한 것들 중 아무거나 해도 상관없었다.)  
문제는 `_config.yml` 이었다. 자꾸 url을 `https://whitekingfish.github.io`는 빼먹고 그 뒤에 포스팅 주소`2018/04/26/first-post.html`부터 찾는 것이 문제였다. 즉 `site.url` 을 찾지 못하는 문제를 자꾸 sitemap 파일명만 바꿔대고 있었다.   
`_config.yml`에서   
```
url: "https://whitekingfish.github.io"
```
을 등록해주자. 바로된다. (누군가 검색을 해서 들어오긴 할까?)    

###theme 적용
- - -
아직 적용하지 않았다. 맘에드는 테마가 보이면 바꿔봐야겠다.

###markdown, jekyll 프로젝트 파일들
- - -
중고나라에 글을 올리 듯이 간단하게 포스팅을 하겠다고 생각한 나는 여기까지 오는 과정에 큰 충격을 받았다. 해야할 작업이 너무많다. 또 `markdown`으로 글을 쓰는 것에 적응이 안된다. 부끄럽지만 마크다운이라는 용어를 처음 들었다. (redmine에서 그냥 썼던것이 마크다운이였다...) 내가 구상한 글이 머릿속에서 마크다운으로 그려질 때 까지 익숙해질 수 밖에 없는 것 같다.   
혹시나 github.io 라는 도메인이 마음에 들어서 github pages를 선택한 것이면 다른 플랫폼이 좋을 것 같다. 하지만 그만큼 시간을 써서 노력한 끝에 하나하나 추가되는 기능들을 보면서 뿌듯함을 느끼고 싶다면 github pages다. (처음에만 잘 만들어 놓으면, 마크다운만 적응이 된다면 완벽)   

###기타
- - -
1. `_draft`에는 블로그에 포함시키지 않을 포스팅이나 작성중인 포스팅을 넣어놓 수 있다.
2. 프로젝트구조를 잘 파악하자. (`_post`, `_layouts`, `_drafts`, `assets`) 어디에 무엇을 넣어야 할지 잘 알아야 마음껏 해보고 싶은 것들을 해볼 수 있다. 하지만 나도 아직 잘 모르므로.. 하나씩 알게되면 정리해야지.   

끝으로
========
이 글을 쓰면서, 그리고 첫 포스팅을 하기까지의 과정을 겪으면서  
무슨일 한다고 물어보면 개발자라고 답했던 내가 너무 멍청해보였다. 너무 알아보지 않고 접근을 해서 그런지 많이 헤매인 것 같다. 그래도 같은 문제때문에 이 글을 보고 해결하는 사람이 있다면 다행이다.(없으면 진짜 나만 멍청한거고..)