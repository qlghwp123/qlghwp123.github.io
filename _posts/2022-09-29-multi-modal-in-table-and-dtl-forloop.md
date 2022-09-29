---
layout : single
tags : [django, html]
title : 테이블 내 여러 행에 대한 modal 설정하는 방법
---

## 왜?

* 현재 국비교육을 받는 과정 속에서 간단한 CRUD 페이지를 만들어 보고 있다.
* 기존 데이터를 수정하는 부분에 대해 페이지 추가 생성이 아닌 modal 을 활용하는게 더 낫다고 판단했다.
* 그러나 부트스트랩 복붙을 해서 망각한 부분이 바로 html 태그 속성 중 id 의 특성이다.



## 방법

* html 에서 각 태그에 대한 id 값은 유일해야 한다.

* DTL(Django Template Language) 를 통해서 db 에 저장되어 있는 id 값을 각 태그 id 값에 추가해줬다.

* **model.py**
  ```python
  from django.db import models
  
  # Create your models here.
  class Todo(models.Model):
      content = models.CharField(max_length=80)
      completed = models.BooleanField(default=False)
      priority = models.IntegerField(default=3)
      created_at = models.DateField(auto_now_add=True)
      deadline = models.DateField(null=True)
  ```

* **views.py**
  ```python
  from django.shortcuts import render, redirect
  from .models import Todo
  
  """
  ...
  """
  
  def todo_read(req):
      db_data = Todo.objects.all().order_by('id')
      
      d = {'db_data': db_data}
  
      return render(req, 'todo/index.html', d)
  ```

* **index.html**
  ```html
  ...
              <td><button ...data-bs-target="#Modal{{ i.id }}" data-bs-whatever="{{ i.id }}">변경</button></td>
  ...
  
          <div class="modal fade" id="Modal{{ i.id }}" ...>
              <div class="modal-dialog">
                  <div class="modal-content">
                      <div class="modal-header">
                          <h5 class="modal-title" id="exampleModalLabel{{ i.id }}">수정</h5>
  
  ```

* views.py 에서 id 순으로 정렬된 레코드들을 가져온다.
* 해당 레코드들을 하나씩 꺼내서 id 필드에 해당하는 값을 DTL 을 사용하여 id 속성 값에 추가를 해줬다.

* models.py 는 todo 스키마를 보여준다.



## References

[1] [테이블에서 여러 행에 대한 모달 설정](https://stackoverflow.com/questions/35777410/multi-modals-bootstrap-in-for-loop-django)
