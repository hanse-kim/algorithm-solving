## 2020 카카오 블라인드 채용 - 괄호 변환 [🔗](https://programmers.co.kr/learn/courses/30/lessons/72410)

 ### 풀이

문제에 주어진 그대로 단계별로 처리했다.

```python
import re

def solution(new_id):
    new_id = new_id.lower()
    new_id = re.sub('[^0-9a-z\._-]', '', new_id)
    new_id = re.sub('\.\.+', '.', new_id)
    
    while new_id[0] == '.' and len(new_id) > 1:
        new_id = new_id[1:]
    while new_id[-1] == '.' and len(new_id) > 1:
        new_id = new_id[:-1]
    if new_id == '.':
        new_id = ''
    
    if not new_id:
        new_id = 'a'
    
    new_id = new_id[:15]
    while new_id[-1] == '.':
        new_id = new_id[:-1]
        
    while len(new_id) <= 2:
        new_id += new_id[-1]
    
    return new_id
```

### 풀이 - JavaScript

```javascript
function solution(new_id) {
    new_id = new_id.toLowerCase(new_id);
    new_id = new_id.replace(/[^0-9a-z\._-]/g, '');
    new_id = new_id.replace(/\.\.+/g, '.');
    
    while (new_id.length > 1 && new_id.startsWith('.'))
        new_id = new_id.slice(1);
    while (new_id.length > 1 && new_id.endsWith('.'))
        new_id = new_id.slice(0, -1);
    if (new_id === '.')
        new_id = '';
    
    if (!new_id.length)
        new_id = 'a';
    
    new_id = new_id.slice(0, 15);
    while (new_id.length > 1 && new_id.endsWith('.'))
        new_id = new_id.slice(0, -1);
    
    while (new_id.length <= 2)
        new_id += new_id[new_id.length-1];

    return new_id;
}
```

