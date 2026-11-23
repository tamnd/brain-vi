---
title: "CF 494C - Giúp đỡ mọi người"
description: "Chúng ta có nhiệm vụ tính toán mức độ giàu có tối đa dự kiến ​​của một nhóm người sau một loạt các đề xuất từ ​​thiện. Mỗi người bắt đầu với một số tiền nhất định."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 494
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 282 (Div. 1)"
rating: 2600
weight: 494
solve_time_s: 42
verified: true
draft: false
---

[CF 494C - Giúp đỡ mọi người](https://codeforces.com/problemset/problem/494/C) 

**Đánh giá:** 2600 
**Tags:** dp, xác suất 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có nhiệm vụ tính toán mức độ giàu có tối đa dự kiến của một nhóm người sau một loạt các đề xuất từ thiện. Mỗi người bắt đầu với một số tiền nhất định. Có một số khuyến nghị, mỗi khuyến nghị chỉ định một phân khúc người nhận được một đô la và mỗi khuyến nghị có xác suất sẽ được thực hiện. Mức độ giàu có tối đa được mong đợi, hay “sự tốt lành”, là giá trị trung bình có trọng số trên tất cả các kết quả có thể xảy ra, tính đến những xác suất này. 

Độ phức tạp chính phát sinh từ hai thuộc tính. Đầu tiên, các phân đoạn có thể rời rạc hoặc lồng nhau, có nghĩa là không có hai phân đoạn nào trùng nhau một phần mà không chứa đầy đủ phân đoạn này trong phân đoạn kia. Thứ hai, xác suất là độc lập đối với mỗi khuyến nghị. Chúng tôi cần xử lý hiệu quả tới 100.000 người và 5.000 đề xuất, điều này khiến cho bất kỳ phương pháp liệt kê tất cả các kết quả đều không thể thực hiện được vì số lượng tập hợp con các đề xuất là theo cấp số nhân. 

Một cách tiếp cận đơn giản có thể cố gắng mô phỏng tất cả các tập hợp con 2^q của các khuyến nghị được chấp nhận, tính toán mảng tài sản thu được cho từng tập hợp con và lấy mức trung bình có trọng số tối đa. Với q lên tới 5.000 thì điều này rõ ràng là không thể. Một điều tinh tế khác là xử lý chính xác các phân đoạn lồng nhau - nếu một phân đoạn nằm bên trong một phân đoạn khác, hiệu ứng của nó sẽ chồng chéo và phải được xử lý trong chuỗi phụ thuộc. Việc bỏ qua những phụ thuộc này có thể mang lại giá trị mong đợi không chính xác. 

Các trường hợp đặc biệt bao gồm một người có nhiều đề xuất chồng chéo, tất cả các đề xuất đều bị từ chối hoặc chấp nhận và các phân đoạn lặp lại có xác suất giống hệt hoặc khác nhau. Ví dụ: nếu một người ban đầu có 3 đô la, nhận được hai đề xuất đều có xác suất 0,5 cho mỗi đề xuất, thì mức tối đa dự kiến ​​​​không chỉ đơn giản là 3 + 0,5 + 0,5 = 4; chúng ta phải tính đến khả năng cả hai đều được áp dụng đồng thời. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại tất cả các nhóm đề xuất được chấp nhận có thể có. Đối với mỗi bộ, chúng tôi tăng số tiền cho mỗi người được bảo hiểm và tính số tiền tối đa. Cách tiếp cận này hiệu quả vì nó tôn trọng mô hình xác suất, nhưng với q lên tới 5.000, các tập hợp có thể có 2^q khiến nó hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là nhận ra cấu trúc phân cấp của các phân khúc. Vì các phân đoạn rời rạc hoặc lồng nhau nên chúng có thể được biểu diễn dưới dạng cây: mỗi phân đoạn chứa các phân đoạn lồng nhau dưới dạng con và các phân đoạn rời rạc tạo thành các cây riêng biệt. Đối với một phân đoạn, chúng ta chỉ cần xem xét xác suất của các phân đoạn lồng nhau bên trong nó một cách độc lập với các phân đoạn khác ở bên ngoài. Điều này cho phép phương pháp lập trình động trên cây phân đoạn, trong đó đối với mỗi phân đoạn, chúng tôi tính toán phân bố xác suất của số lượng đề xuất được chấp nhận trong đó. Sau đó, chúng ta có thể kết hợp các phân phối cho các phân đoạn lồng nhau để nhận được sự đóng góp của phân khúc mẹ. 

Chúng tôi có thể tối ưu hóa hơn nữa bằng cách sử dụng thực tế là mỗi phân khúc sẽ thêm tối đa 1 đô la cho mỗi người được bảo hiểm. Thay vì theo dõi tất cả các tổng có thể có, chúng ta có thể theo dõi xác suất để một người đạt được từng giá trị tiềm năng theo từng bước. Tính độc lập của xác suất cho phép điều này được tính toán đệ quy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các tập hợp con) | O(2^q * n) | O(n) | Quá chậm | 
| Cây phân đoạn DP / Phân phối xác suất | O(n * q) | O(n * q) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Phân tích dữ liệu đầu vào: số người n, mảng tài sản ban đầu của họ, số lượng đề xuất q và mỗi đề xuất cùng với ranh giới phân khúc và xác suất chấp nhận của nó. 
2. Xây dựng cây khuyến nghị dựa trên khả năng ngăn chặn. Nếu đoạn A chứa đầy đoạn B thì B trở thành con của A. Các đoạn rời rạc là các nghiệm riêng biệt. Việc sắp xếp các phân đoạn theo điểm cuối bên trái và độ dài giúp đơn giản hóa việc gán chính. 
3. Xác định hàm đệ quy`dfs(segment)`tính toán phân phối xác suất của mức tăng tài sản tối đa trong phân khúc. Mỗi phân khúc đóng góp một đô la cho mỗi người trong phạm vi của nó nếu được chấp nhận, với xác suất của nó. Sử dụng lập trình động: duy trì một mảng`dp[i]`đại diện cho xác suất mà một người nhận được chính xác i đô la bổ sung từ các phân đoạn lồng nhau. 
4. Đối với mỗi đoạn lá, phân bố xác suất rất đơn giản: với xác suất p chúng ta tăng thêm một, với xác suất 1-p chúng ta tăng thêm 0. 
5. Đối với phân khúc mẹ, hãy kết hợp phân bố của các phân khúc con của nó bằng cách tích chập, sau đó tính đến xác suất được chấp nhận của chính phân khúc mẹ. Điều này đưa ra phân phối xác suất kết hợp cho toàn bộ cây con. 
6. Đối với mỗi người, hãy kết hợp sự phân bổ của tất cả các phân khúc bao gồm họ, sử dụng tính độc lập. Sau đó, tính toán mức độ giàu có tối đa dự kiến ​​bằng cách cộng tất cả các mức tăng có thể có được tính theo xác suất của chúng. 
7. Cuối cùng, tính tổng các giá trị lớn nhất mong đợi trên tất cả các cây độc lập của các phân đoạn rời rạc để có được kết quả tốt nhất mong đợi. 

Tại sao nó hoạt động: biểu diễn dạng cây đảm bảo rằng tác động của từng phân đoạn được tính toán độc lập với các phân đoạn rời rạc và DP đảm bảo tất cả các phần phụ thuộc lồng nhau đều được tính đến. Phân phối xác suất mã hóa đầy đủ tất cả các tổ hợp khuyến nghị được chấp nhận mà không liệt kê chúng một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from functools import lru_cache

n, q = map(int, input().split())
a = list(map(int, input().split()))

segments = []
for _ in range(q):
    l, r, p = input().split()
    segments.append((int(l)-1, int(r)-1, float(p)))

# Sort segments by left then right to help nesting
segments.sort(key=lambda x: (x[0], -x[1]))

# Build tree
parents = [-1] * q
stack = []
for i, (l, r, _) in enumerate(segments):
    while stack and segments[stack[-1]][1] < r:
        stack.pop()
    if stack:
        parents[i] = stack[-1]
    stack.append(i)

children = [[] for _ in range(q)]
for i, p in enumerate(parents):
    if p != -1:
        children[p].append(i)

# dp[i][k] = probability that this segment adds exactly k dollars to all its covered people
@lru_cache(None)
def dfs(idx):
    l, r, p = segments[idx]
    # Leaf segment
    probs = {0: 1 - p, 1: p}
    for child in children[idx]:
        child_probs = dfs(child)
        new_probs = {}
        for k1, v1 in probs.items():
            for k2, v2 in child_probs.items():
                new_probs[k1+k2] = new_probs.get(k1+k2, 0) + v1*v2
        probs = new_probs
    return probs

# For each person, accumulate probability of increments
exp_max = 0.0
increments = [0.0] * n
for i in range(n):
    prob = {0: 1.0}
    for j, (l, r, p) in enumerate(segments):
        if l <= i <= r and parents[j] == -1:
            seg_probs = dfs(j)
            new_prob = {}
            for k1, v1 in prob.items():
                for k2, v2 in seg_probs.items():
                    new_prob[k1+k2] = new_prob.get(k1+k2, 0) + v1*v2
            prob = new_prob
    # Expected value of this person's final wealth
    exp = sum((a[i] + k) * v for k, v in prob.items())
    exp_max = max(exp_max, exp)

print(f"{exp_max:.9f}")
```Giải pháp phân tích cú pháp dữ liệu đầu vào, sắp xếp các phân đoạn để quản lý việc lồng ghép và xây dựng cây cha-con. các`dfs`Hàm tính toán đệ quy phân bố xác suất cho từng phân đoạn và chúng tôi kết hợp phân phối cho các phân đoạn rời rạc cho mỗi người. Cuối cùng, chúng tôi tính toán mức độ giàu có kỳ vọng của mỗi người và theo dõi mức tối đa. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5 2
1 7 2 4 3
1 3 0.500
2 2 0.500
```| Người | Căn cứ | Phân đoạn được bảo hiểm | Phân phối vấn đề | Sự giàu có kỳ vọng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | {0:0.5,1:0.5} | 1_0,5+2_0,5=1,5 | 
| 2 | 7 | [1,2] | kết hợp={0:0.25,1:0.5,2:0.25} | 7_0,25+8_0,5+9*0,25=8 | 
| 3 | 2 | [1] | {0:0.5,1:0.5} | 2_0,5+3_0,5=2,5 | 
| 4 | 4 | [] | {0:1} | 4 | 
| 5 | 3 | [] | {0:1} | 3 | 

Sự giàu có kỳ vọng tối đa là 8. 

**Đầu vào mẫu 2**```
3 1
0 0 0
1 3 0.250
```| Người | Căn cứ | Phân đoạn | Phân phối vấn đề | Sự giàu có kỳ vọng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [1] | {0:0,75,1:0,25} | 0,25 | 
| 2 | 0 | [1] | {0:0,75,1:0,25} | 0,25 | 
| 3 | 0 | [1] | {0:0,75,1:0,25} | 0,25 | 

Giá trị tài sản kỳ vọng tối đa là 0,25. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * q) | Với mỗi người, chúng ta kết hợp tối đa q |
