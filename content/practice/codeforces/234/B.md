---
title: "CF 234B - Đọc"
description: "Chúng tôi được yêu cầu giúp Vasya chọn những giờ tốt nhất để đọc sách giáo khoa trên chuyến tàu. Chuyến tàu kéo dài n giờ và mỗi giờ có mức ánh sáng nhất định trong khoảng từ 0 đến 100. Vasya muốn đọc đúng k giờ."
date: "2026-06-04T10:06:45+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 234
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 145 (Div. 2, ACM-ICPC Rules)"
rating: 1000
weight: 234
solve_time_s: 104
verified: true
draft: false
---

[CF 234B - Đọc](https://codeforces.com/problemset/problem/234/B) 

**Đánh giá:** 1000 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giúp Vasya chọn những giờ tốt nhất để đọc sách giáo khoa trên chuyến tàu. Chuyến tàu kéo dài _n_ giờ và mỗi giờ có mức ánh sáng nhất định từ 0 đến 100. Vasya muốn đọc chính xác _k_ giờ. Mục tiêu của anh ấy là chọn _k_ giờ sao cho giờ tối nhất trong số các giờ đã chọn càng sáng càng tốt. Nói cách khác, nếu anh ta đọc trong giờ với mức ánh sáng 20, 30 và 40 thì mức tối thiểu trong số này, 20, phải được tối đa hóa trong tất cả các nhóm _k_ giờ có thể có. 

Đầu vào cung cấp số giờ _n_, số giờ đọc _k_ và mức độ ánh sáng cho tất cả các giờ. Đầu ra trước tiên phải báo cáo mức ánh sáng tối thiểu tối đa có thể đạt được, sau đó là chỉ số giờ mà Vasya nên chọn. Có thể tồn tại nhiều giải pháp tối ưu. 

Các ràng buộc ở mức vừa phải: _n_ có thể lên tới 1000. Điều này có nghĩa là một$O(n \log n)$hoặc$O(n^2)$cách tiếp cận là khả thi, nhưng bất cứ điều gì theo cấp số nhân như$O(n^k)$sẽ là quá chậm. Vì mức độ ánh sáng nằm trong khoảng từ 0 đến 100 nên chúng ta có thể khai thác phạm vi nhỏ này nhưng cách tiếp cận đơn giản hơn là tập trung vào việc lựa chọn giờ thay vì tần số giá trị. 

Các trường hợp đặc biệt bao gồm khi tất cả các giờ có cùng ánh sáng, khi số giờ đọc _k_ bằng _n_ hoặc khi giá trị ánh sáng tối đa và tối thiểu bằng nhau. Cách tiếp cận bất cẩn có thể cố gắng chọn các giờ liên tiếp hoặc không xử lý được việc lập chỉ mục chính xác, dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ xem xét mọi sự kết hợp của _k_ giờ trong số _n_, tính toán ánh sáng tối thiểu cho mỗi sự kết hợp và chọn ánh sáng tối thiểu cao nhất. Điều này đúng nhưng không khả thi: số lượng kết hợp là$\binom{n}{k}$, tăng cực kỳ nhanh (với _n_ = 1000 và _k_ = 500 thì nó rất lớn về mặt thiên văn). 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải kiểm tra tất cả các kết hợp. Vì chúng ta chỉ quan tâm đến việc tối đa hóa ánh sáng tối thiểu trong số các giờ đã chọn nên bộ tối ưu phải bao gồm _k_ giờ sáng nhất. Bằng cách sắp xếp giờ theo mức độ ánh sáng và chọn _k_ lớn nhất, chúng tôi đảm bảo mức tối thiểu lớn nhất có thể. Ánh sáng tối thiểu trong số những giờ đã chọn này chỉ đơn giản là giá trị nhỏ nhất trong tập hợp con đã chọn, sẽ là giá trị$k$-thứ lớn nhất trong toàn bộ mảng. Sau khi chọn những giờ này, chúng tôi xuất ra chỉ số ban đầu của chúng. 

Cách tiếp cận này làm giảm vấn đề thành một nhiệm vụ sắp xếp và lựa chọn. Sắp xếp mất$O(n \log n)$thời gian và việc chọn _k_ hàng đầu và truy xuất các chỉ số sẽ mất$O(n)$. Điều này là hiệu quả cho các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n chọn k × k) | O(k) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị đầu vào: _n_ và _k_, theo sau là danh sách các mức ánh sáng. 
2. Ghép nối từng mức ánh sáng với chỉ số dựa trên 1 ban đầu của nó. Điều này bảo toàn vị trí giờ sau khi sắp xếp. 
3. Sắp xếp danh sách các cặp theo thứ tự mức độ ánh sáng giảm dần. Điều này đảm bảo rằng phần tử _k_ đầu tiên là giờ có mức ánh sáng cao nhất. 
4. Lấy cặp _k_ đầu tiên từ danh sách đã sắp xếp. Đây là những giờ tối ưu để đọc. 
5. Trích xuất mức ánh sáng tối thiểu trong số các giờ đã chọn, đây là phần tử cuối cùng trong tập con _k_ trên cùng vì danh sách được sắp xếp theo thứ tự giảm dần. 
6. Thu thập các chỉ số của giờ đã chọn từ các cặp và xuất chúng theo thứ tự bất kỳ. 

Tại sao nó hoạt động: Sắp xếp giờ theo ánh sáng đảm bảo rằng bất kỳ bộ nào có mức ánh sáng nhỏ hơn sẽ không mang lại mức tối thiểu tốt hơn. Bằng cách lấy các giá trị _k_ hàng đầu, chúng tôi đảm bảo rằng giá trị nhỏ nhất trong tập hợp con này càng lớn càng tốt. Các chỉ số được bảo toàn bằng cách lưu trữ chúng cùng với các mức ánh sáng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
lights = list(map(int, input().split()))

# Pair light levels with their original indices
hours = [(lights[i], i+1) for i in range(n)]

# Sort descending by light level
hours.sort(reverse=True, key=lambda x: x[0])

# Take top k hours
selected = hours[:k]

# Minimum light among chosen hours
min_light = min(l[0] for l in selected)

# Extract indices
indices = [l[1] for l in selected]

print(min_light)
print(" ".join(map(str, indices)))
```Giải pháp này đọc dữ liệu, ghép các mức ánh sáng với chỉ số giờ để giữ nguyên vị trí, sắp xếp các cặp theo thứ tự ánh sáng giảm dần và chọn _k_ trên cùng. Ánh sáng tối thiểu được trích xuất bằng cách sử dụng tính năng tích hợp sẵn của Python`min`, mặc dù nó cũng có thể là phần tử cuối cùng của danh sách _k_ được sắp xếp hàng đầu. Các chỉ số được in theo thứ tự bất kỳ. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:`5 3\n20 10 30 40 10`| Bước | giờ (sắp xếp) | đã chọn | phút_light | chỉ số | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [(20,1),(10,2),(30,3),(40,4),(10,5)] | - | - | - | 
| Sau khi sắp xếp | [(40,4),(30,3),(20,1),(10,2),(10,5)] | [(40,4),(30,3),(20,1)] | 20 | [4,3,1] | 

Đầu ra:```
20
4 3 1
```Điều này xác nhận việc lựa chọn giờ 4,3,1 với ánh sáng tối thiểu 20. 

Trường hợp tùy chỉnh 2: 

đầu vào:`4 4\n50 50 50 50`| Bước | giờ (sắp xếp) | đã chọn | phút_light | chỉ số | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [(50,1),(50,2),(50,3),(50,4)] | - | - | - | 
| Sau khi sắp xếp | [(50,1),(50,2),(50,3),(50,4)] | tất cả | 50 | [1,2,3,4] | 

Đầu ra:```
50
1 2 3 4
```Tất cả các giờ đều sáng như nhau, do đó, bất kỳ tập hợp con nào có kích thước _k_ đều có mức tối thiểu như nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp danh sách chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Lưu trữ các cặp mức ánh sáng và chỉ số | 

Cho n 1000, giá trị này phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    lights = list(map(int, input().split()))
    hours = [(lights[i], i+1) for i in range(n)]
    hours.sort(reverse=True, key=lambda x: x[0])
    selected = hours[:k]
    min_light = min(l[0] for l in selected)
    indices = [l[1] for l in selected]
    return f"{min_light}\n{' '.join(map(str, indices))}"

# Provided sample
assert run("5 3\n20 10 30 40 10\n") == "20\n4 3 1"

# Minimum-size input
assert run("1 1\n0\n") == "0\n1"

# Maximum-size input (all 100)
assert run("1000 1000\n" + " ".join(["100"]*1000) + "\n") == "100\n" + " ".join(str(i) for i in range(1,1001))

# All-equal values, k<n
assert run("5 2\n10 10 10 10 10\n") == "10\n1 2"

# Mixed values, k<n
assert run("6 3\n5 1 9 2 8 3\n") == "5\n3 5 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n0`|`0\n1`| Xử lý đầu vào nhỏ nhất | 
|`1000 1000\n100...`|`100\n1..1000`| Xử lý đầu vào có kích thước tối đa | 
|`5 2\n10 10 10 10 10`|`10\n1 2`| Tất cả các giá trị đều bằng nhau, k<n | 
|`6 3\n5 1 9 2 8 3`|`5\n3 5 1`| Giá trị hỗn hợp chung, không liên tiếp | 

## Vỏ cạnh 

Khi _k_ bằng _n_, thuật toán sẽ chọn tất cả các giờ một cách tự nhiên. Đối với đầu vào`4 4\n50 50 50 50`, thuật toán sắp xếp giờ nhưng _k_ trên cùng bao gồm mỗi giờ. Ánh sáng tối thiểu là 50 và chỉ số`[1,2,3,4]`được trả về một cách chính xác. Không có lỗi nào xảy ra vì các chỉ số được lưu trữ dưới dạng 1 ngay từ đầu. 

Khi tất cả các mức ánh sáng giống hệt nhau, mọi tập hợp con có kích thước _k_ đều hợp lệ. Thuật toán vẫn chọn _k_ giờ đầu tiên sau khi sắp xếp, điều này có thể chấp nhận được theo báo cáo vấn đề. Đối với đầu vào`5 3\n10 10 10 10 10`, thuật toán xuất ra `[1,2,
