---
title: "CF 1409C - Khôi phục mảng khác"
description: "Nguyên nhân sâu xa của lỗi ở đây là do bộ khai thác thử nghiệm đang cố gắng gọi một hàm giải quyết() không được xác định trong cùng một ngữ cảnh."
date: "2026-06-11T07:39:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1409
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 667 (Div. 3)"
rating: 1200
weight: 1409
solve_time_s: 423
verified: false
draft: false
---

[CF 1409C - Một cách khôi phục mảng khác](https://codeforces.com/problemset/problem/1409/C) 

**Đánh giá:** 1200 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 7 phút 3 giây 
**Đã xác minh:** không 

##Giải pháp 
Nguyên nhân cốt lõi của sự cố ở đây là ** bộ khai thác thử nghiệm đang cố gắng gọi một`solve()`hàm không được xác định trong cùng ngữ cảnh**. Trong Python, nếu bạn xác định mã chính trong phạm vi toàn cục mà không gói nó trong một hàm, hãy gọi`solve()`sau này sẽ luôn thất bại với`NameError`hoặc`RuntimeError`. Điều này không liên quan đến chính thuật toán. 

Để khắc phục, chúng ta cần một **giải pháp Python 3 hoàn toàn độc lập** trong đó tất cả logic nằm trong một hàm có tên`solve()`và I/O nhanh được sử dụng. Bản thân thuật toán - cửa sổ trượt trên tất cả`(fret, note_index)`cặp - là chính xác, vì vậy chúng tôi giữ nguyên cách tiếp cận đó. 

Đây là giải pháp sửa chữa, hoàn chỉnh:```python
import sys
input = sys.stdin.readline

def solve():
    # read input
    a = list(map(int, input().split()))
    n = int(input())
    b = list(map(int, input().split()))

    # generate all possible frets for each note
    frets = []
    for i in range(n):
        for j in range(6):
            fret = b[i] - a[j]
            frets.append((fret, i))  # (fret_index, note_index)
    
    frets.sort()  # sort by fret number

    count = [0] * n  # how many times each note is in current window
    unique = 0       # number of unique notes in the window
    ans = float('inf')
    l = 0

    # sliding window to find minimal range covering all notes
    for r in range(len(frets)):
        fret, idx = frets[r]
        if count[idx] == 0:
            unique += 1
        count[idx] += 1

        # contract window from the left while all notes are covered
        while unique == n:
            ans = min(ans, frets[r][0] - frets[l][0])
            left_fret, left_idx = frets[l]
            count[left_idx] -= 1
            if count[left_idx] == 0:
                unique -= 1
            l += 1

    print(ans)

if __name__ == "__main__":
    solve()
```### Những điểm chính 

1. Chức năng`solve()`chứa tất cả logic, vì vậy bạn có thể gọi nó một cách an toàn trong khai thác thử nghiệm. 
2. Mỗi nốt nhạc`b[i]`có thể chơi trên bất kỳ dây nào`j`với sự băn khoăn`b[i] - a[j]`. 
3. Sắp xếp tất cả`(fret, note_index)`các cặp cho phép một cửa sổ trượt tìm ra sự khác biệt phím đàn tối thiểu một cách hiệu quả bao trùm tất cả các nốt. 
4.`count`Và`unique`theo dõi có bao nhiêu ghi chú riêng biệt trong cửa sổ hiện tại. 
5. Kết quả`ans`là sự khác biệt nhỏ nhất của phím đàn tối đa và tối thiểu trong một cửa sổ hợp lệ. 

### Cách chạy thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample test
assert run("1 4 100 10 30 5\n6\n101 104 105 110 130 200\n") == "0", "sample 1"
```Điều này khắc phục`RuntimeError`bởi vì`solve()`bây giờ đã được xác định. 

Nếu bạn muốn, tôi cũng có thể **xem từng bước mẫu này** để cho biết lý do tại sao câu trả lời là`0`với những bài tập băn khoăn. Điều này thường làm rõ logic cửa sổ trượt cho các trường hợp biên. Bạn có muốn tôi làm điều đó không?
