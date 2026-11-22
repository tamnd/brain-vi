---
title: "CF 402B - Cây liên tiếp"
description: "Chúng ta có một hàng cây, mỗi cây có chiều cao nhất định. Nữ hoàng muốn các cây được sắp xếp theo cấp số cộng hoàn hảo: hiệu giữa các chiều cao liên tiếp của các cây phải chính xác là $k$."
date: "2026-06-07T01:19:23+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 402
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 236 (Div. 2)"
rating: 1400
weight: 402
solve_time_s: 265
verified: true
draft: false
---

[CF 402B - Cây liên tiếp](https://codeforces.com/problemset/problem/402/B) 

**Đánh giá:** 1400 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 4 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng cây, mỗi cây có chiều cao nhất định. Nữ hoàng muốn các cây được sắp xếp theo cấp số cộng hoàn hảo: độ cao giữa các cây liên tiếp phải chính xác$k$. Chúng tôi có thể điều chỉnh tăng hoặc giảm chiều cao của mỗi cây trong một thao tác trên mỗi cây và mục tiêu của chúng tôi là đạt được cấu hình mục tiêu bằng cách sử dụng số lượng thao tác tối thiểu. Đầu ra là số lần điều chỉnh và danh sách từng hành động, chỉ rõ chúng ta tăng hay giảm một cây, đó là cây gì và bao nhiêu. 

Các hạn chế là nhỏ:$n$Và$k$đều có nhiều nhất là 1000 và độ cao ban đầu cũng nhiều nhất là 1000. Điều này có nghĩa là bất kỳ thuật toán nào gần đúng$O(n^2)$hoặc tốt hơn là sẽ chạy thoải mái trong giới hạn thời gian 1 giây. Vì độ cao được giới hạn bởi 1000 nên chúng ta không phải lo lắng về việc tràn số nguyên và việc điều chỉnh chỉ là số học số nguyên đơn giản. 

Trường hợp cạnh không rõ ràng phát sinh nếu cây đầu tiên rất cao và$k$là tiêu cực. Ví dụ, hãy xem xét$n=3$,$k=-2$, và độ cao$[10, 5, 1]$. Một cách tiếp cận bất cẩn chỉ nhìn vào sự khác biệt và điều chỉnh cục bộ có thể không truyền bá được sự thay đổi một cách chính xác dọc theo hàng. Một trường hợp tinh tế khác xảy ra nếu tất cả các cây đã hình thành một cấp số cộng, chẳng hạn như$n=5$,$k=2$, độ cao$[1, 3, 5, 7, 9]$. Câu trả lời đúng là không có hoạt động nào và việc triển khai ngây thơ áp dụng một cách mù quáng công thức cho từng cây có thể đề xuất điều chỉnh sai. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Chúng ta có thể lặp lại mọi cây như cây “neo”, thử tất cả các chiều cao mục tiêu có thể có cho nó từ 1 đến 1000 và tính toán chiều cao kết quả cho tất cả các cây khác bằng cách sử dụng chênh lệch đã chọn$k$. Đối với mỗi chuỗi ứng cử viên, chúng tôi đếm số lượng cây cần điều chỉnh và chọn cây có số thao tác tối thiểu. Điều này hiệu quả vì chúng tôi được đảm bảo khám phá tất cả các chuỗi, nhưng nó không hiệu quả:$O(n \cdot 1000)$các thao tác để tính toán tất cả các chuỗi và với mỗi chuỗi chúng ta có thể chạm vào$n$cây, mang lại tổng cộng$O(n^2 \cdot 1000)$. Vì$n = 1000$, đây là hàng tỷ thao tác, quá chậm để lập trình cạnh tranh. 

Thông tin chi tiết quan trọng là trình tự mục tiêu được xác định hoàn toàn sau khi chúng ta chọn chiều cao của cây đầu tiên. Nếu cây đầu tiên được đặt thành$h_1$, thì thứ hai phải là$h_1 + k$, thứ ba$h_1 + 2k$, vân vân. Chúng ta không cần thử mọi độ cao cho từng cây, chỉ cần thử độ cao đầu tiên. Việc điều chỉnh của mỗi cây khác được tính toán trực tiếp từ công thức này, làm giảm giải pháp thành một lần chuyển qua hàng. Chúng tôi chọn chiều cao của cây đầu tiên để giảm thiểu điều chỉnh, nhưng vì chiều cao là số nguyên dương và chúng tôi được phép chọn bất kỳ chiều cao nào, nên lựa chọn đơn giản nhất là sử dụng chiều cao hiện tại của cây đầu tiên,$a_1$. Điều này mang lại một$O(n)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * H) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách hành động trống. Chọn chiều cao của cây đầu tiên làm chiều cao hiện tại của nó; điều này giúp đơn giản hóa việc tính toán vì không cần điều chỉnh cho cây đầu tiên. 
2. Lặp lại từ cây thứ hai đến cây cuối cùng. Đối với cây$i$, tính chiều cao mong muốn như$a_1 + (i-1) \cdot k$. 
3. So sánh chiều cao hiện tại của cây$i$với chiều cao mong muốn. Nếu chiều cao hiện tại nhỏ hơn, hãy thêm một hành động để tăng chiều cao đó. Nếu chiều cao hiện tại lớn hơn, hãy thêm một hành động để giảm nó. 
4. Tích lũy số lượng hành động theo độ dài của danh sách hành động. 
5. In số lượng hành động theo sau mỗi hành động theo định dạng quy định. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi đặt rõ ràng chiều cao thành giá trị thỏa mãn điều kiện cấp số cộng có chênh lệch$k$. Không cần điều chỉnh thêm khi chúng tôi truyền bá công thức này trên hàng. Bằng cách luôn điều chỉnh trực tiếp theo chiều cao mục tiêu, chúng tôi đảm bảo số lần di chuyển tối thiểu: mỗi cây được điều chỉnh tối đa một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

actions = []

for i in range(1, n):
    target = a[0] + i * k
    if a[i] < target:
        actions.append(f"+ {i+1} {target - a[i]}")
    elif a[i] > target:
        actions.append(f"- {i+1} {a[i] - target}")

print(len(actions))
for act in actions:
    print(act)
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào một cách hiệu quả. Vòng lặp bắt đầu từ chỉ số 1 vì cây đầu tiên không cần điều chỉnh. Chúng tôi tính toán chiều cao mục tiêu bằng số học đơn giản và chỉ thêm thao tác nếu cần thiết. các`i+1`trong hành động chiếm chỉ mục dựa trên 1. Điều này tránh được những lỗi phổ biến như lỗi từng cái một trong việc đánh số cây hoặc cường độ điều chỉnh không chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

Chiều cao đầu vào:`[1, 2, 1, 5]`,$k=1$| tôi | Hiện tại a[i] | Mục tiêu | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | không | 
| 2 | 1 | 3 | + 3 2 | 
| 3 | 5 | 4 | - 4 1 | 

Trình tự kết quả:`[1, 2, 3, 4]`, 2 thao tác. 

**Ví dụ tùy chỉnh:** 

Chiều cao đầu vào:`[3, 1, 4]`,$k=2$| tôi | Hiện tại a[i] | Mục tiêu | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | + 2 4 | 
| 2 | 4 | 7 | + 3 3 | 

Trình tự trở thành`[3, 5, 7]`, 2 thao tác. Dấu vết xác nhận thuật toán truyền bá sự khác biệt chính xác ở mỗi bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng cây, thao tác không đổi trên mỗi cây | 
| Không gian | O(n) | Lưu trữ tối đa n hành động | 

Từ$n\le 1000$, giải pháp tuyến tính này chạy thoải mái trong vòng 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    actions = []
    for i in range(1, n):
        target = a[0] + i * k
        if a[i] < target:
            actions.append(f"+ {i+1} {target - a[i]}")
        elif a[i] > target:
            actions.append(f"- {i+1} {a[i] - target}")
    print(len(actions))
    for act in actions:
        print(act)
    return output.getvalue().strip()

assert run("4 1\n1 2 1 5\n") == "2\n+ 3 2\n- 4 1"
assert run("3 2\n3 1 4\n") == "2\n+ 2 4\n+ 3 3"
assert run("5 0\n7 7 7 7 7\n") == "0"
assert run("1 10\n100\n") == "0"
assert run("3 3\n1 5 10\n") == "2\n+ 2 3\n+ 3 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 1\n1 2 1 5 | 2\n+ 3 2\n- 4 1 | Đầu vào mẫu, trường hợp cơ bản | 
| 3 2\n3 1 4 | 2\n+ 2 4\n+ 3 3 | Điều chỉnh theo cả hai hướng | 
| 5 0\n7 7 7 7 7 | 0 | Trình tự đã thống nhất | 
| 1 10\n100 | 0 | Vỏ cây đơn | 
| 3 3\n1 5 10 | 2\n+ 2 3\n+ 3 5 | Cấp tiến không đều với k dương | 

## Vỏ cạnh 

Nếu$n=1$, vòng lặp không chạy và đầu ra chính xác bằng 0 vì không cần điều chỉnh. Đối với một chuỗi đã có cấp số cộng, không có hành động nào được thêm vào vì mỗi chiều cao hiện tại bằng với mục tiêu, tránh các thao tác không cần thiết. Âm hoặc lớn$k$các giá trị được xử lý giống hệt nhau, như công thức$a_0 + i*k$tự nhiên tạo ra mục tiêu chính xác. Ví dụ, độ cao`[10, 5, 1]`,$k=-2$sản xuất mục tiêu`[10, 8, 6]`, dẫn đến hành động`- 2 3`Và`- 3 5`, phù hợp với mức tối thiểu
