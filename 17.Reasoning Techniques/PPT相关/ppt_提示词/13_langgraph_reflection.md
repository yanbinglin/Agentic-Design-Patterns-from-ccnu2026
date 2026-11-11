【15:30–16:30】这一页是给工程师看的：同一套设计，写成代码长什么样。

一个小图：generate_query 生成检索式；web_research 调工具搜网页（绿色，外部工具）；reflection 判断「够不够、要不要再来一轮」；finalize_answer 定稿。

红色那条是条件边：reflection 说不够，就回到 web_research 再来一轮；说够了，就走下去定稿。所以这张图里有三件我们讲过的东西——自我纠正、ReAct 闭环、思考预算的终止条件——被压缩成了一个分支。

仓库是参考实现，Apache 2.0。重点看 graph.py 里那条条件边，别把它当生产后端。

[演示点 2] 打开 graph.py，指到 reflection 分支，三分钟以内收。