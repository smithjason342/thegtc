杏运主管娱乐【Q-——333307——】杏运主管娱乐【 辋芷《888yx●vip》 】
杏运主管娱乐【Q-——333307——】杏运主管娱乐【 辋芷《888yx●vip》 】

 Python 爬虫实战：从零构建一个 GitHub 热门项目监控机器人

> 每天花两分钟手动查看 GitHub Trending？作为一个开发者，我受够了这种重复劳动。今天手把手教你用 Python + GitHub API 构建一个自动监控机器人，代码不到 100 行，却能帮你每天自动抓取热门仓库并推送通知。

 为什么你需要一个监控机器人？

GitHub 每天有成千上万个新仓库诞生，而 Trending 页面只展示 25 个项目。手动刷新页面不仅浪费时间，还会错过你感兴趣的技术方向。通过自动化监控，你可以：

- 第一时间获取特定语言（如 Python、TypeScript）的爆款项目
- 跟踪竞品或开源库的 Star 增长趋势
- 自动筛选出符合关键词的新仓库

 核心实现：GitHub API + 定时任务 + 推送通知

 第一步：获取 Trending 数据

GitHub 官方 API 不直接提供 Trending 接口，但我们有两个替代方案：

1. 使用第三方 API（如 `github-trending-api`）
2. 直接解析 HTML（推荐，无需额外依赖）

这里我们用 requests + BeautifulSoup 实现 HTML 解析：

```python
import requests
from bs4 import BeautifulSoup

def get_trending_repos(language='python'):
    url = f'https://github.com/trending/{language}'
    soup = BeautifulSoup(requests.get(url).text, 'html.parser')
    repos = []
    for article in soup.select('article.Box-row'):
        repo = {
            'name': article.h2.a['href'].strip('/'),
            'stars': article.select_one('a.Link--muted').text.strip(),
            'description': article.select_one('p').text.strip() if article.p else ''
        }
        repos.append(repo)
    return repos[:10]
```

 第二步：设置定时检查

使用 `schedule` 库，每天定时执行一次：

```python
import schedule

def job():
    repos = get_trending_repos('python')
     对比上次存储的数据，更新变化
    check_and_notify(repos)

schedule.every().day.at("09:00").do(job)
```

 第三步：差异检测与通知推送

将上次的仓库名称存入 JSON 文件，每次对比差异，再通过 Server酱（或钉钉 Webhook）推送：

```python
import json
import os

def check_and_notify(new_repos):
    old_file = 'repos.json'
    old_repos = json.load(open(old_file)) if os.path.exists(old_file) else []
    new_names = {r['name'] for r in new_repos}
     找出新增仓库
    for repo in new_repos:
        if repo['name'] not in old_names:
            send_message(f"新增热门项目: {repo['name']} ⭐{repo['stars']}")
    json.dump(new_repos, open(old_file, 'w'))
```

 进阶优化建议

- 多语言支持：循环调用 `get_trending_repos()` 并传不同语言参数
- Star 增长追踪：每天记录仓库的 Star 数量，计算增长率
- 部署到服务器：用 `cron` 代替 `schedule`，或者部署到 GitHub Actions

 完整代码获取

关注公众号【程序员围城】后台回复“爬虫”，获取完整可运行代码 + 详细的部署文档。

---

今日互动：你目前最关注哪个技术方向的 GitHub 项目？评论区说说，我帮你监控起来！如果这篇文章对你有帮助，欢迎点赞、在看、转发三连支持。

相关推荐：

https://github.com/barajasamanda48/keavzi/blob/main/Tr%E1%BB%B1c%20tuy%E1%BA%BFn%20%F0%9F%90%93%EF%BC%9A%E6%9D%8F%E7%A6%8F%E5%AE%98%E7%BD%91%E5%BC%80%E6%88%B7_%E8%BF%9C%E9%A6%96%E7%BC%B4%E9%83%A8%E6%B7%A4yylsr.md

<img src="https://i.postimg.cc/k4Xgf0Qp/xingyun-00004.png" />

相关推荐：

https://github.com/barajasamanda48/keavzi/commit/d40150ae79ff6a93c337d8982755b86e49798060

<img src="https://i.postimg.cc/ZnXYY0G9/xingyun-00011.png" />
相关推荐：

https://github.com/melendezeric38/enrusi/blob/main/Tr%E1%BB%B1c%20tuy%E1%BA%BFn%20%F0%9F%90%93%EF%BC%9A%E6%9D%8F%E7%A6%8F%E5%AE%98%E7%BD%91%E6%B5%8B%E9%80%9F_%E5%BD%B0%E4%BA%A4%E5%81%8E%E6%96%AF%E5%86%A0ogzmy.md

<img src="https://i.postimg.cc/Jn4zPf3R/xingyun-00005.png" />
相关推荐：

https://github.com/melendezeric38/enrusi/commit/addc47b73a858e1603f51a249fb73a0139ade53a

<img src="https://i.postimg.cc/Jn4zPf3R/xingyun-00005.png" />

资讯来源：新华网、人民网、央视新闻、中新网、凤凰网、澎湃新闻、界面新闻、新浪新闻、搜狐网、财新网、观察者网、第一财经等主流平台，以独树一帜的观察视角与扎实的深度报道能力，在资讯领域收获广泛关注。
