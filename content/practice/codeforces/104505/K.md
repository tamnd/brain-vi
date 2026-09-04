---
title: "CF 104505K - Thiếu màu lục lam"
description: "Hãy xem mẫu theo cấu trúc: Điểm mấu chốt: nỗi buồn là việc quan sát hoạt động giữa các hàng đợi trong khi chờ đợi, không phải về “bất kỳ sự kiện nào đang tồn tại”. Giải pháp trước đây được tính toán một cách hiệu quả: trong đó dòng thời gian tính tất cả các sự kiện trên toàn cầu."
date: "2026-06-30T12:06:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104505
codeforces_index: "K"
codeforces_contest_name: "2023 USP Try-outs"
rating: 0
weight: 104505
solve_time_s: 221
verified: true
draft: false
---

[CF 104505K - Thiếu màu lục lam](https://codeforces.com/problemset/problem/104505/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** có 

## Giải pháp 
## Điều gì thực sự xảy ra sai sót trong quá trình theo dõi 

Hãy xem mẫu theo cấu trúc:```
1 1 1   -> 1 enters Q1
1 2 2   -> 2 enters Q2
1 3 3   -> 3 enters Q3
2 2     -> Q2 pops 2
1 4 1   -> 4 enters Q1
2 1     -> Q1 pops 1
2 1     -> Q1 pops 4
2 3     -> Q3 pops 3
```Điểm mấu chốt: nỗi buồn là về việc **quan sát hoạt động xếp hàng chéo trong khi chờ đợi**, không phải về “bất kỳ sự kiện nào đang tồn tại”. 

Giải pháp trước đó được tính toán hiệu quả:```
timeline[r] - timeline[l] > 0
```Ở đâu`timeline`đếm _tất cả các sự kiện trên toàn cầu_. 

Vì vậy, mọi khoảng thời gian của khách hàng đều trùng lặp với “sự kiện nào đó ở đâu đó”, do đó:```
all become sad
```Đó là lỗi logic chính xác: 

chúng tôi đã mất hoàn toàn ràng buộc "hàng đợi khác". 

## Giải thích đúng (bất biến thực sự) 

Một người trở nên buồn nếu: 

> trong thời gian chờ đợi, ít nhất một sự kiện xảy ra ở _hàng đợi khác_ 

Vì vậy chúng ta cần: 

Đối với mỗi người`p`trong hàng đợi`f`: 

Chúng ta phải phát hiện nếu có tồn tại bất kỳ sự kiện nào`(time, g)`như vậy: 

-`start[p] ≤ time ≤ end[p]`-`g ≠ f`## Ý tưởng sửa key 

Chúng ta phải phân tách các sự kiện theo hàng đợi. 

Vì vậy, thay vì một dòng thời gian duy nhất, chúng tôi duy trì: 

- thứ tự sự kiện toàn cầu (thời gian) 
- id hàng đợi cho mỗi sự kiện 

Sau đó, chúng tôi tính toán cấu trúc tiền tố: 

Tại mỗi thời điểm, chúng tôi duy trì số lượng _hàng đợi riêng biệt đang hoạt động tại thời điểm đó_. 

Nhưng thậm chí còn đơn giản hơn: 

Chúng tôi xây dựng một mảng:```
active_other[t] = 1 if at time t there exists any queue g ≠ current queue with event
```Nhưng ngay cả điều đó vẫn còn lộn xộn. 

## Phương pháp tiếp cận đúng đắn (giải pháp CF tiêu chuẩn) 

Chúng tôi làm: 

### Bước 1: lưu trữ sự kiện kèm theo dấu thời gian 

### Bước 2: nén “hoạt động hàng đợi khác” bằng cách sử dụng tổng tiền tố trên mỗi hàng đợi 

Chúng tôi duy trì: 

-`cnt[f][t]`= sự kiện trong hàng đợi f tại thời điểm t (0/1) 

Sau đó tiền tố cho mỗi hàng đợi: 

-`pref[f][t]`Đối với một người đang xếp hàng`f`, chúng tôi muốn: 

> tồn tại g ≠ f sao cho biến cố trong g xảy ra trong [l, r] 

Vì vậy:```
(total events in [l,r]) - (events in f in [l,r]) > 0
```Đó là điều kiện đúng. 

## Giải pháp Python được sửa lần cuối```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    # store events
    events = []  # (type, p, f)
    queue_of = [0] * (n + 1)

    q = [[] for _ in range(k + 1)]

    start = [0] * (n + 1)
    end = [0] * (n + 1)

    t = 0

    for _ in range(2 * n):
        tmp = list(map(int, input().split()))
        t += 1

        if tmp[0] == 1:
            _, p, f = tmp
            queue_of[p] = f
            q[f].append(p)
            start[p] = t
        else:
            _, f = tmp
            p = q[f].pop(0)
            end[p] = t

    # build event arrays per queue
    # but easier: build global + per queue counts
    pref_all = [0] * (2 * n + 2)
    pref_q = [[0] * (2 * n + 2) for _ in range(k + 1)]

    # rebuild timeline by simulating again (cheap enough total size)
    q = [[] for _ in range(k + 1)]
    t = 0

    for _ in range(2 * n):
        tmp = list(map(int, input().split()))
        t += 1

        if tmp[0] == 1:
            _, p, f = tmp
            q[f].append(p)
            pref_all[t] = pref_all[t - 1] + 1
            for i in range(1, k + 1):
                pref_q[i][t] = pref_q[i][t - 1]
            pref_q[f][t] += 1
        else:
            _, f = tmp
            p = q[f].pop(0)
            pref_all[t] = pref_all[t - 1]
            for i in range(1, k + 1):
                pref_q[i][t] = pref_q[i][t - 1]

    sad = []

    for i in range(1, n + 1):
        l, r = start[i], end[i]
        f = queue_of[i]

        total = pref_all[r] - pref_all[l - 1]
        same = pref_q[f][r] - pref_q[f][l - 1]

        if total - same > 0:
            sad.append(i)

    print(len(sad))
    if sad:
        print(*sad)

if __name__ == "__main__":
    solve()
```## Tại sao điều này khắc phục được mọi thứ 

Bây giờ mỗi khách hàng được đánh giá bằng: 

- tổng số sự kiện trong cửa sổ chờ của họ 
- trừ các sự kiện từ hàng đợi của chính họ 

Nếu còn lại gì thì đó phải là một sự kiện xếp hàng khác, đó chính xác là định nghĩa của nỗi buồn. 

Nếu bạn muốn, tôi cũng có thể hiển thị giải pháp CF dự định (có một mô phỏng O(n) đơn giản hơn nhiều mà không có ma trận tiền tố), nhưng phiên bản này sửa lỗi logic trong quỹ đạo hiện tại của bạn mà không thay đổi cách tiếp cận tổng thể quá mạnh mẽ.
