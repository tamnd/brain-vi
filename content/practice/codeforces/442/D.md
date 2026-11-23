---
title: "CF 442D - Adam Và Cây"
description: "Cây bắt đầu với một đỉnh gốc duy nhất. Mỗi thao tác thêm một lá mới vào một đỉnh hiện có. Sau mỗi lần cộng, chúng ta phải tính giá trị nhỏ nhất có thể có của đại lượng sau. Chúng tôi tô màu các cạnh của cây."
date: "2026-06-07T15:53:31+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 442
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 253 (Div. 1)"
rating: 2600
weight: 442
solve_time_s: 229
verified: false
draft: false
---

[CF 442D - Adam và Cây](https://codeforces.com/problemset/problem/442/D) 

**Đánh giá:** 2600 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 3 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cây bắt đầu với một đỉnh gốc duy nhất. Mỗi thao tác thêm một lá mới vào một đỉnh hiện có. Sau mỗi lần cộng, chúng ta phải tính giá trị nhỏ nhất có thể có của đại lượng sau. 

Chúng tôi tô màu các cạnh của cây. Các cạnh có cùng màu phải tạo thành một đường dẫn được kết nối và không có đỉnh nào có thể có nhiều hơn hai cạnh liên quan cùng màu. Đối với bất kỳ đỉnh nào, hãy nhìn vào đường đi từ đỉnh đó đến gốc và đếm xem có bao nhiêu màu khác nhau xuất hiện trên đường đi đó. Chi phí của việc tô màu là số lượng tối đa như vậy trên tất cả các đỉnh. Chúng tôi muốn chi phí tối thiểu có thể. 

Đầu vào không mô tả một cây tĩnh. Nó mô tả một chuỗi các phần chèn vào. Nếu như`p[i] = v`, thì đỉnh`i + 1`được đính kèm như một đứa trẻ mới của`v`. Sau mỗi lần chèn, chúng tôi đưa ra chi phí tối ưu của cây hiện tại. 

Khó khăn lớn nhất là hạn chế`n ≤ 10^6`. Một giải pháp tính toán lại bất cứ điều gì quan trọng sau mỗi lần chèn sẽ bị loại trừ ngay lập tức. Thậm chí`O(height)`công việc cho mỗi lần chèn có thể nguy hiểm nếu nó xảy ra quá thường xuyên. Tổng công việc phải ở rất gần với tuyến tính. 

Có một số cách dễ dàng để đi sai. 

Xét một gốc có hai lá con:```
1
├─2
└─3
```Câu trả lời là`2`, không`1`. Một màu không thể đồng thời tiếp tục vào cả hai nhánh. Bất kỳ màu nào cũng cần thêm một cấp độ màu sau khi đường dẫn chia thành hai cây con có độ khó như nhau. 

Bây giờ hãy xem xét một chuỗi:```
1
└─2
   └─3
      └─4
```Câu trả lời vẫn là`1`. Một màu duy nhất có thể bao phủ toàn bộ chuỗi. Việc đếm độ sâu hoặc chiều cao sẽ tạo ra kết quả không chính xác`4`. 

Một trường hợp tế nhị khác là:```
1
├─2
│  └─4
└─3
```Câu trả lời là`2`, không`3`. Chỉ có một nhánh là sâu. Màu sắc được sử dụng trên nhánh nặng có thể tiếp tục đi qua gốc nên độ khó không tăng trừ khi có ít nhất hai nhánh khó như nhau. 

Những ví dụ này hướng tới một đại lượng đệ quy phụ thuộc vào giá trị con lớn nhất và vào số lượng con đạt được mức tối đa đó. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xây dựng cây sau mỗi lần chèn và tính toán lại màu tối ưu từ đầu. 

Một quan sát hữu ích là mỗi lớp màu là một đường dẫn. Khi một đường đi đến một đỉnh phân nhánh, nó có thể tiếp tục đi qua nhiều nhất một con. Tất cả các cây con khác phải bắt đầu một phân đoạn màu mới. Điều này dẫn đến tối ưu hóa đệ quy trên cây. 

Nếu chúng ta tính toán giá trị tối ưu cho mọi nút từ đầu sau mỗi lần chèn, trường hợp xấu nhất sẽ trở thành bậc hai. Một cây có một triệu phần chèn vào khiến điều này hoàn toàn không khả thi. 

Điểm mấu chốt là chi phí tối ưu chính xác là số Horton-Strahler của cây có gốc. 

Cho phép`f(v)`là sự đóng góp chi phí tối ưu của cây con bắt nguồn từ`v`. 

Đối với một chiếc lá,`f(v) = 1`. 

Đối với một nút nội bộ, hãy`m`là giá trị lớn nhất trong số các con của nó. 

Nếu chỉ có một đứa trẻ có giá trị`m`, thì đường màu tốt nhất có thể tiếp tục đi qua thành phần con đó, vì vậy:```
f(v) = m
```Nếu ít nhất hai con có giá trị`m`, thì không thể tránh khỏi việc có thêm một mức màu:```
f(v) = m + 1
```This is precisely the Strahler recurrence.

 Phần động trở nên dễ dàng hơn nhiều vì số Strahler chỉ có thể tăng lên. Trên cây có nhiều nhất`10^6 + 1`đỉnh thì số Strahler không bao giờ vượt quá`21`, kể từ khi nhận được giá trị`k`yêu cầu ít nhất`2^(k-1)`lá. 

Giá trị con chỉ thay đổi một số lần nhỏ trong toàn bộ quá trình. Điều này mang lại tổng số cập nhật gần như tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) được khấu hao, với log n 21 | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi đỉnh, hãy lưu trữ giá trị Strahler hiện tại của nó`val[v]`. 
2. For every vertex store`mx[v]`, giá trị Strahler lớn nhất trong số các con của nó. 
3. Với mỗi đỉnh duy trì có bao nhiêu con hiện có mỗi giá trị có thể. Vì giá trị lớn nhất có thể là nhiều nhất`21`, một phạm vi cố định nhỏ là đủ. 
4. Khi một lá mới được tạo ra, giá trị của nó là`1`. 
5. Gắn chiếc lá vào lá mẹ của nó. Từ quan điểm của cha mẹ, một đứa con có giá trị`1`đã xuất hiện. 
6. Cập nhật bộ đếm của cha và tính lại giá trị của nó. 

Nếu như`mx`là giá trị con lớn nhất và`cnt[mx]`là số trẻ đạt được điều đó thì:```
val = 1                         if mx = 0
val = mx                        if cnt[mx] = 1
val = mx + 1                    if cnt[mx] ≥ 2
```7. Nếu giá trị của cha không thay đổi thì dừng lại. Không có gì ở trên nó có thể thay đổi. 
8. Nếu giá trị của cha mẹ thay đổi từ`old`ĐẾN`new`, di chuyển đến cha mẹ của nó. Đối với tổ tiên đó, một giá trị con đã thay đổi từ`old`ĐẾN`new`. 
9. Lặp lại cho đến khi giá trị không thay đổi hoặc đạt đến nghiệm. 
10. Sau khi xử lý việc chèn, xuất ra`val[root]`. 

Lý do điều này hiệu quả là vì giá trị chỉ tăng lên. Một đỉnh có thể di chuyển tối đa khoảng 20 giá trị khác nhau trong suốt vòng đời của cây, do đó tổng số lượng cập nhật được truyền đi là nhỏ. 

### Tại sao nó hoạt động 

Phép truy toán Strahler nắm bắt chính xác ràng buộc về màu sắc. 

Một đường dẫn màu có thể tiếp tục đi qua nhiều nhất một cây con. Nếu có một con khó nhất duy nhất, con đường tối ưu sẽ tiếp tục đi qua nó và không cần thêm mức màu nào. Nếu có hai hoặc nhiều con khó nhất, chỉ một trong số chúng có thể kế thừa đường dẫn màu hiện tại, buộc một mức màu khác ở đâu đó bên dưới. Điều đó làm tăng giá trị yêu cầu lên một. 

Các bộ đếm được duy trì luôn biểu thị tập hợp nhiều giá trị con hiện tại. Mỗi bản cập nhật sửa đổi chính xác một phần đóng góp con, do đó việc tính toán lại phần tử gốc từ các bộ đếm này sẽ duy trì kết quả lặp lại. Chỉ truyền bá khi giá trị thay đổi là đủ vì tổ tiên chỉ phụ thuộc vào giá trị con. Do đó, giá trị gốc sau mỗi lần chèn chính xác là chi phí tô màu tối ưu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    max_nodes = n + 2
    MAXR = 22  # Strahler number never exceeds 21 for n <= 1e6

    parent = array('I', [0]) * max_nodes
    val = bytearray(max_nodes)
    mx = bytearray(max_nodes)

    cnt = array('I', [0]) * (max_nodes * MAXR)

    val[1] = 1

    ans = []

    for node, par in enumerate(p, start=2):
        parent[node] = par
        val[node] = 1

        v = par
        old_child = 0
        new_child = 1

        while v:
            base = v * MAXR

            if old_child:
                cnt[base + old_child] -= 1

            cnt[base + new_child] += 1

            if new_child > mx[v]:
                mx[v] = new_child

            old_val = val[v]

            m = mx[v]
            if m == 0:
                new_val = 1
            else:
                new_val = m + (cnt[base + m] >= 2)

            if new_val == old_val:
                break

            val[v] = new_val

            old_child = old_val
            new_child = new_val
            v = parent[v]

        ans.append(str(val[1]))

    sys.stdout.write(" ".join(ans))

if __name__ == "__main__":
```
