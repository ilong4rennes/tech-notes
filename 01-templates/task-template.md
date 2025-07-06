---
created: <% tp.date.now("YYYY-MM-DD") %>
tags: 
org:
  - <% tp.system.prompt("谁 assign 给你这个 task？ 公司/学校老师/自己") %>
goal: <% tp.system.prompt("一句话概括要解决的问题？") %>
---

<%*
tR += `我需要完成如下：

1. 

`;
tR += `目前我收集到的信息有：

- 

`;
tR += `请从头到尾帮我理清思路，step by step`;
%>
