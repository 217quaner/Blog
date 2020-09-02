---
layout: post
---

# Django Redirect 与 HttpResponseRedirect区别
1. HttpResponseRedirect仅可以接收url作为参数传入。
2. Redirect可以接收model、view或者url作为参数传入，并返回HttpResponseRedirect。
