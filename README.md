# dcinside-python3-api
Deadly simple non official dcinside api for python3

```python
api = dc_api.API()
# 프로그래밍 갤러리 글 무한 크롤링
async for metadoc in api.board(board_id="programming"):
    print(metadoc.title)          # => 땔감 벗어나는법.tip
    doc = await metadoc.document
    print(doc.contents)           # => 자바를 한다
    async for comm in metadoc.comments:
        print(com.contents)       # => ㅇㅇ(1.224) 지랄 ㄴ
```

```python
# 댓글 작성
await dc_api.write_comment(board_id="programming", doc_id=149123, name="ㅇㅇ", password="1234", contents="ㅇㅈ")
# 글 작성
doc_id = await dc_api.write_document(board_id="programming", title="java vs python", contents="닥치고 자바", name="ㅇㅇ", password="1234")
# 글 삭제
await dc_api.remove_document(board_id="programming", doc_id=doc_id, password="1234")
```

# Dependency
python3 aiohttp, lxml

# Features
- [x] Board crawling
- [x] Fetch document body
- [x] Fetch comments 
- [x] Fetch document images
- [x] Write/Modify/Delete document
- [x] Write comment
- [ ] Delete comment
- [ ] Login/Logout
- [ ] Upvote/Downvote

# Usage
Place dc_api.py in your working directory

or install via pip

```
pip3 install --user dc_api
```

```python
import dc_api

api = dc_api.API()

async for metadoc in api.board(board_id="programming", num=-1, start_page=1, doc_id_upper_limit=None, doc_id_lower_limit=None):
    metadoc.id         # => 835027
    metadoc.board_id   # => programming
    metadoc.title      # => "땔감 벗어나는법.tip"
    metadoc.author     # => "ㅇㅇ(10.20)"
    metadoc.author_id  # => None (고닉일 경우 고닉 아이디 반환)
    metadoc.time       # => datetime("2020-01-01 01:41:00.000000")
    metadoc.comment_count # => 3
    metadoc.voteup_count  # => 0
    metadoc.view_count    # => 14

    doc = await metadoc.document
    doc.id         # => 835027
    doc.board_id   # => "programming"
    doc.title      # => "땔감 벗어나는법.tip"
    doc.author     # => "ㅇㅇ(10.20)"
    doc.author_id  # => None (고닉일 경우 고닉 아이디 반환)
    doc.time       # => datetime("2020-01-01 01:41:00.000000")
    doc.comment_count   # => 3
    doc.voteup_count    # => 0
    doc.logined_voteup_count  # => 0
    doc.votedown_count  # => 0
    doc.view_count # => 14.id
    doc.contents   # => "자바를 한다"
    doc.html       # => "<p> 자바를 한다 </p>" 

    for image in doc.images:
        image.src         # => "https://..."
        image.document_id # => 835027
        image.board_id    # => "programming"
        await image.load()# => raw image binary

    async for com in metadoc.comments:
        com.id            # => 123123
        com.parent_id     # => 123122
        com.time          # => "1:55"
        com.author        # => "ㅇㅇ(192.23)"
        com.author_id     # => None (고닉일 경우 아이디 반환)
        com.contents      # => "개솔 ㄴㄴ"
        com.dccon         # => None (디시콘일경우 디시콘 주소 반환)
        com.voice         # => None (보이스리플일경우 보이스리플 주소 반환)

        
doc = await api.document(board_id="programming", document_id=835027)

async for comm in api.comments(board_id="programming", document_id=835027):
    comm


doc_id = await api.write_document(board_id="programming",
                               name="점진적자살", password="1234", 
                               title="제목", contents="내용")
doc_id = await api.modify_document(board_id="programming", document_id=document_id, 
                          name="얄파고", pw="1234", 
                          title="수정된 제목", contents="수정된 내용")
com_id = await api.write_comment(board_id="programming", document_id=doc_id, 
                           name="점진적자살", password="1234", contents="설리")
await api.remove_document(board_id="programming", document_id=document_id, password="1234")

```
