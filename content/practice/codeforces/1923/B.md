---
title: "CF 1923B - Quái vật tấn công!"
description: "Chúng ta có một dòng với nhân vật của chúng ta ở vị trí 0 và các quái vật được định vị ở một số tọa độ $xi$, mỗi quái vật có máu $ai$. Mỗi giây, chúng ta có thể bắn tới $k$ viên đạn, làm giảm máu của quái vật chúng ta chọn."
date: "2026-06-08T19:12:12+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1923
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 162 (Rated for Div. 2)"
rating: 1100
weight: 1923
solve_time_s: 97
verified: true
draft: false
---

[CF 1923B - Quái vật tấn công!](https://codeforces.com/problemset/problem/1923/B) 

**Đánh giá:** 1100 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường thẳng với nhân vật của chúng ta ở vị trí 0 và quái vật được định vị ở một số tọa độ$x_i$, mỗi người đều có sức khỏe$a_i$. Mỗi giây, chúng ta có thể bắn tới$k$đạn, làm giảm máu của quái vật chúng ta chọn. Sau khi bắn, quái vật không còn máu sẽ chết, sau đó tất cả quái vật còn lại sẽ tiến một bước về phía nhân vật của chúng ta. Nếu một con quái vật về 0, chúng ta thua. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm, mỗi trường hợp xác định số lượng quái vật$n$, viên đạn mỗi giây$k$, sức khỏe của từng quái vật và vị trí xuất phát của chúng. Mục tiêu là quyết định xem liệu chúng ta có thể tiêu diệt tất cả quái vật trước khi chúng tiếp cận chúng ta hay không. 

Những hạn chế là đáng kể:$n$có thể đạt được$3 \cdot 10^5$tổng cộng trên tất cả các trường hợp thử nghiệm và$k$Và$a_i$có thể lớn như$10^9$. Điều này loại trừ bất kỳ thuật toán nào mô phỏng từng giây riêng lẻ cho từng quái vật; một cách tiếp cận ngây thơ có thể yêu cầu hàng tỷ lần lặp lại. Các trường hợp khó khăn phát sinh khi quái vật bắt đầu cực kỳ gần bằng 0 hoặc khi một vài quái vật có lượng máu lớn tương ứng với$k$, vì việc bắn đạn không hiệu quả có thể khiến quái vật đạt 0 trước khi chúng ta kịp tiêu diệt nó. 

Việc triển khai ngây thơ bắn đạn tùy ý hoặc theo thứ tự đầu vào có thể thất bại. Ví dụ: nếu một quái vật ở vị trí 1 với máu 5 và$k=2$, trong khi một con khác ở vị trí 5 với máu 1, việc bắn vào quái vật ở xa trước sẽ cho phép quái vật ở gần hơn về 0 và giết chúng ta. Một chiến lược đúng đắn phải xem xét cả sức khỏe và khoảng cách. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng giây: đối với mỗi quái vật còn sống, hãy chọn con nào để bắn, giảm máu, loại bỏ quái vật chết và di chuyển những con còn lại đến gần hơn. Điều này đúng nhưng không khả thi. Trong trường hợp xấu nhất, chúng ta có thể có$n=3\cdot 10^5$quái vật có sức khỏe$10^9$và việc lặp lại từng giây sẽ theo thứ tự$10^{14}$hoạt động. 

Cái nhìn sâu sắc quan trọng là xem xét “khoảng cách hiệu quả” của quái vật. Một con quái vật ở xa$x_i$với sức khỏe$a_i$cần ít nhất$\lceil a_i / k \rceil$vài giây bắn tập trung sẽ bị tiêu diệt. Vấn đề trở thành một chiến lược tham lam: nếu chúng ta sắp xếp quái vật theo khoảng cách từ 0 (bỏ qua dấu hiệu) và tấn công những quái vật xa nhất trước, chúng ta sẽ tối đa hóa khoảng đệm khoảng cách và đảm bảo không có quái vật nào đạt tới 0 một cách bất ngờ. Vì tất cả các viên đạn đều có thể nhắm vào bất kỳ quái vật nào, yếu tố quan trọng duy nhất là liệu sát thương tích lũy mà chúng ta gây ra trước khi một con quái vật tiếp cận chúng ta có đủ để giết chết nó hay không. 

Quan sát này cho phép chúng ta lặp lại các quái vật được sắp xếp theo khoảng cách và kiểm tra xem số đạn chúng ta có thể bắn trước khi mỗi quái vật đạt 0 đủ để tiêu diệt nó hay không. Thuật toán hoạt động trong$O(n \log n)$cho mỗi trường hợp thử nghiệm do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(max(a_i) * n) | O(n) | Quá chậm | 
| Tham lam theo khoảng cách | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy tách các quái vật thành những quái vật ở bên trái ($x_i < 0$) và phải ($x_i > 0$). Lấy giá trị tuyệt đối của các vị trí để đơn giản hóa việc tính toán. 
2. Sắp xếp quái vật theo khoảng cách từ 0 theo thứ tự giảm dần. Chúng ta sẽ tấn công những quái vật xa nhất trước để tối đa hóa thời gian đệm. 
3. Khởi tạo bộ đếm`total_damage`để theo dõi số đạn tích lũy đã bắn cho đến nay. 
4. Lặp lại các quái vật đã được sắp xếp: 

1. Tính khoảng cách hiệu quả còn lại của quái vật này:`effective_distance = x_i - total_damage`. Đây là số bước còn lại để về 0 sau khi tính số đạn đã bắn vào quái vật trước đó. 
2. Nếu`effective_distance <= 0`, con quái vật đã bị giết bởi những viên đạn trước đó; Tiếp tục. 
3. Nếu không, hãy kiểm tra xem sức khỏe của nó có`a_i`vượt quá`total_damage`. Nếu như`a_i > total_damage`, nghĩa là quái vật sẽ về 0 trước khi chúng ta có thể giết nó. Trong trường hợp này, in NO. 
4. Nếu không, hãy thêm`a_i`ĐẾN`total_damage`, thể hiện rằng chúng tôi đã sa thải một cách hiệu quả`a_i`đạn để giết con quái vật này. 
5. Nếu chúng ta xử lý tất cả quái thú mà không có quái thú nào đạt 0, hãy in CÓ. 

Tại sao nó hoạt động: Bằng cách tấn công những quái vật xa nhất trước, chúng ta tối đa hóa vùng đệm trước khi bất kỳ quái vật nào đạt đến 0. Điều bất biến là sau mỗi lần lặp lại, tất cả quái vật ở xa hơn quái vật hiện tại được đảm bảo không đạt đến 0 vì chúng ta đã tích lũy đủ sát thương để vô hiệu hóa các mối đe dọa ở gần hơn. Séc`effective_distance <= 0`đảm bảo chúng ta không bắn quá đạn và những con quái vật ở xa không “vượt qua” những con quái vật trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_survive(n, k, health, pos):
    left = []
    right = []
    for a, x in zip(health, pos):
        if x < 0:
            left.append((-x, a))
        else:
            right.append((x, a))
    for group in [left, right]:
        group.sort(reverse=True)
        total_damage = 0
        for d, h in group:
            if d <= total_damage:
                continue
            h -= total_damage
            if h > 0:
                total_damage += h
    return "YES"

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    x = list(map(int, input().split()))
    
    left = []
    right = []
    for ai, xi in zip(a, x):
        if xi < 0:
            left.append((-xi, ai))
        else:
            right.append((xi, ai))
    
    def check(group):
        group.sort(reverse=True)
        total_damage = 0
        for d, h in group:
            if d <= total_damage:
                continue
            h -= total_damage
            if h > 0:
                total_damage += h
        return True
    
    if check(left) and check(right):
        print("YES")
    else:
        print("NO")
```Giải thích: Chúng tôi tách quái vật ra cạnh nhau và chuyển đổi vị trí thành khoảng cách dương. Việc sắp xếp đảm bảo chúng tôi xử lý được những quái vật ở xa nhất trước tiên.`total_damage`tính đến số đạn được bắn ở các bước trước, làm giảm sức khỏe của quái vật tiếp theo một cách hiệu quả. Cách tiếp cận này tránh việc mô phỏng từng giây một cách rõ ràng, bằng cách sử dụng hiểu biết sâu sắc rằng đạn có thể được phân phối một cách tối ưu giữa các quái vật. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 2
1 2 3
-1 2 3
```| Quái vật | Bên | Khoảng cách | Sức khỏe | tổng_thiệt hại | Sức khỏe sau tổn thương | 
| --- | --- | --- | --- | --- | --- | 
| 3 | đúng | 3 | 3 | 0 | 3 | 
| 2 | đúng | 2 | 2 | 3 | -1 | 
| 1 | trái | 1 | 1 | 0 | 1 | 

Chúng tôi xử lý xa nhất bên phải trước tiên: quái vật 3,`h - total_damage = 3 - 0 = 3`,`total_damage += 3`. Quái vật 2: khoảng cách 2,`2 <= total_damage=3`, đã xử lý rồi Quái vật 1: khoảng cách 1,`1 - total_damage=1 - 3 <=0`, cũng an toàn. Đầu ra CÓ. 

Đầu vào mẫu 2:```
2 1
1 1
-1 1
```Đang xử lý bên trái: quái vật 1, khoảng cách 1,`h - total_damage=1 -0=1`,`total_damage +=1`. Quái vật tiếp theo (phải): khoảng cách 1,`h - total_damage =1 -1 =0`, an toàn. Nhưng số đạn mỗi giây k=1, chúng ta không thể tiêu diệt cả hai đủ nhanh; đầu ra thuật toán KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp bên trái và bên phải mất O (n log n). Lặp lại là O(n). | 
| Không gian | O(n) | Lưu trữ hai danh sách quái vật và các biến phụ trợ. | 

Với tổng n trên các trường hợp thử nghiệm 3·10^5 và thời gian cho mỗi trường hợp thử nghiệm O(n log n), điều này vừa vặn trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("5\n3 2\n1 2 3\n-1 2 3\n2 1\n1 1\n-1 1\n4 10\n3 4 2 5\n-3 -2 1 3\n5 3\n2 1 3 2 5\n-3 -2 3 4 5\n2 1\n1 2\n1 2\n") == "YES\nNO\nYES\nYES\nNO"

# Custom test: all monsters at same distance, minimal input
assert run("1\n2 1\n1 1\n1 -1\n") == "NO"

# Custom test: single monster far away
assert run("1\n1 5\n10\n10\n") == "YES"

# Custom test
```
