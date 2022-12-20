# DRF
## APIView, Mixins, Generics API View, ViewSet  
> [참고링크](https://velog.io/@duo22088/DRF-APIView-Mixins-generics-APIView-ViewSet-%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C)  
>`APIView` < `Mixins` < `generics APIView` < `ViewSet`
### `APIView`
    * 하나의 CBV 이므로 하나의 URL 만 처리 가능
    * 각 요청 method(get, post, put, delete)에 맞게 멤버함수를 구현하면, 해당 method 요청이 들어올 때 호출됨
    * 호출이 되면 호출 이전 단계(initial)에서 다음을 처리함
        1. 직렬화
        2. 인증 체크
        3. 사용량 제한 체크
        4. 권한 클래스 지정 : 비인증 / 인증 유저에 대한 API 호출
        5. 요청한 버전 체크 : request.version에 저장

### `Mixins`
    * `APIView` 구현에서 각 request method 마다 직접 직렬화 처리를 반복함 => `Mixins`를 사용하여 간소화 가능
    *  `queryset`과 `serializer_class`를 지정해주고 상속받은 `Mixins`와 연결해주면 됨
    * request method 와 연결되는 부분은 Customizing 해야 함

### `generics APIView`
    * 하나 이상의 `Mixins`와 결합된 클래스 제공

    1. `UpdateAPIView`
        > PUT(리소스 전체 수정) vs PATCH(리소스 부분 수정)


### `ViewSet`
    * 하나의 핼퍼클래스로 두 개 이상의 URL 처리 가능


## ViewSet vs Generic Views
### Generic Views
1. data from db
2. serialize
3. response
>listAPIView => `many=True` instance를 여러개 serialize 함
>RetrieveAPIView => `many=False` instance를 한 개만 serialize 함

## Serializer
### Serialize
* 순서
    1. instance
    2. Serializer(instance= )
    3. dict
    4. json data
    5. response

* GET

* 다음 코드로 이해 가능
```python
from .models import Comment
from .serializers import CommentSerializer
from rest_framework.response import Response


instance = Comment.objects.all()[0] # instance
queryset = Comment.objects.all() # queryset : instance 집합

serializers = CommentSerializer(instance=queryset, many=True)
serializer = CommentSerializer(instance=instance, many=False) # default

print(type(serializer.data)) # <class 'rest_framework.utils.serializer_helpers.ReturnList'>
print(type(serializers.data)) # <class 'rest_framework.utils.serializer_helpers.ReturnList'>
# 
# Response : status_code = 200
Response(serializer.data)
Response(serializers.data)


```
### Deserialize
* 순서
    1. json data
    2. dict
    3. Serializer(data= )
    4. is_valid(), validated_data
    5. instance 생성
    6. save()

* POST, UPDATE, DELETE, PATCH


***

# Django Shell에서 자동완성 사용하기
### 방법1
```python
import os, django

os.environ.setdefault("DJANGO_SETTINGS_MODULE", 'backend.settings')
django.setup()
```

### 방법2
1. `pipenv install django-extensions --dev`
2. `settings.py` -> `django_extensions` app 추가
3. cmd -> `python manage.py shell_plus` 
>model 자동 import 됨