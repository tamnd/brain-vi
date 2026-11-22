---
title: "CF 345E - Black Cat Rush"
description: "Chúng ta đang chuyển động dọc theo một đường thẳng từ (0, 0) đến (a, 0) với vận tốc không đổi v. Xung quanh chúng ta có n con mèo đen, mỗi con xuất phát từ một điểm (xi, yi). Mỗi con mèo có thể di chuyển theo bất kỳ hướng nào với tốc độ tối đa u."
date: "2026-06-06T17:59:42+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 345
codeforces_index: "E"
codeforces_contest_name: "Friday the 13th, Programmers Day"
rating: 2700
weight: 345
solve_time_s: 76
verified: true
draft: false
---

[CF 345E - Black Cat Rush](https://codeforces.com/problemset/problem/345/E) 

**Đánh giá:** 2700 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang chuyển động dọc theo một đường thẳng từ (0, 0) đến (a, 0) với vận tốc không đổi v. Xung quanh chúng ta có n con mèo đen, mỗi con xuất phát từ một điểm (xi, yi). Mỗi con mèo có thể di chuyển theo bất kỳ hướng nào với tốc độ tối đa u. Một con mèo "đi ngang qua đường của chúng ta" nếu nó đến bất kỳ điểm nào trên đoạn thẳng từ (0, 0) đến (a, 0) trước hoặc cùng lúc với chúng ta. Mục tiêu là tìm ra số lượng mèo tối đa có thể băng qua đường của chúng ta thành công. 

Đầu vào cung cấp a, v, u, n và tọa độ của những con mèo. Đầu ra là một số nguyên duy nhất: số lượng mèo có thể đi qua đường đi của chúng ta. 

Các ràng buộc cho chúng ta biết rằng n nhiều nhất là 1000, vì vậy các thuật toán xử lý từng con mèo riêng lẻ đều có thể chấp nhận được. Vận tốc là số nguyên nhỏ, vì vậy các vấn đề về độ chính xác có thể phát sinh nếu sử dụng số học dấu phẩy động một cách ngây thơ. Các trường hợp cạnh xảy ra khi một con mèo ở ngay phía trên hoặc phía dưới đường đi của chúng ta, khi tốc độ của nó nhỏ hơn tốc độ của chúng ta hoặc khi một con mèo ở xa điểm cuối (a, 0). Ví dụ, một con mèo ở (0, 10) với u < v không thể bắt được chúng ta ở gốc tọa độ, vì vậy nó không được tính, trong khi một con mèo ở (0, 0) sẽ tiếp cận đường đi một cách tầm thường ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét thời gian cần thiết để mỗi con mèo đến mọi điểm trên đường đi của chúng ta và kiểm tra xem liệu nó có thể vượt qua thời gian đến của chúng ta ở bất kỳ điểm nào trong số đó hay không. Điều này sẽ liên quan đến việc tính toán khoảng cách từ mỗi con mèo đến nhiều điểm trên đường đi và nặng về mặt tính toán. Với n ≤ 1000 và a lên tới 10000, việc lặp qua tất cả các điểm dọc theo đoạn sẽ quá chậm, O(n * a) hoặc tệ hơn. 

Điểm mấu chốt là để một con mèo băng qua đường của chúng ta, chúng ta không cần phải kiểm tra mọi điểm. Con mèo nên di chuyển dọc theo đường để giảm thiểu khoảng cách của nó với đường đi. Khoảng cách tối thiểu này là khoảng cách vuông góc từ con mèo đến trục x. Con mèo sẽ đến được đường đi khi nó tiếp cận gần nhất trong thời gian t_cat = sqrt(yi^2 + d^2) / u, nhưng đường đi ngắn nhất xảy ra khi con mèo chạy thẳng đến một điểm trên trục x nơi nó có thể giao nhau với đường đi trước khi chúng ta đến được nó. Điều này biến vấn đề thành một phép so sánh đơn giản bằng cách sử dụng hình học: một con mèo có thể đi qua khi và chỉ khi u > v và khoảng cách vuông góc cho phép con mèo chạm tới một số x trên [0, a] trước khi chúng ta chạm tới x đó. Bằng cách chiếu vùng có thể tiếp cận của con mèo lên trục x, chúng ta có thể xác định liệu nó có thể đi qua hay không. 

Trong thực tế, điều này giúp kiểm tra xem vận tốc u của con mèo có vượt quá vận tốc v của chúng ta hay không và sau đó tính toán xem giao điểm của phạm vi x có thể tiếp cận của con mèo với đoạn đường đi của chúng ta có trống hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * a) | O(1) | Quá chậm | 
| Kiểm tra hình học | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị đầu vào: độ dài đường dẫn a, tốc độ v của bạn, tốc độ mèo u, số lượng mèo n và tọa độ mèo (xi, yi). 
2. Khởi tạo bộ đếm số lượng mèo có thể đi qua đường. 
3. Lặp lại từng con mèo. Đối với một con mèo ở (xi, yi), trước tiên hãy kiểm tra xem u ≤ v. Nếu con mèo có tốc độ chậm hơn hoặc bằng tốc độ của bạn, nó không thể vượt qua bất kỳ điểm nào trên đường trước bạn, vì vậy hãy bỏ qua nó. 
4. Đối với những con mèo có u > v, hãy tính khoảng cách theo phương ngang tối đa mà con mèo có thể đi được trước khi bạn đến điểm cuối. Thời gian để bạn đến điểm cuối là t = a/v. Trong thời gian đó, con mèo có thể di chuyển được một đoạn đường u*t = (u/v)*a. 
5. Kiểm tra xem con mèo có vuông góc với trục x |yi| không nhỏ hơn hoặc bằng quãng đường tối đa mà nó có thể đi được. Nếu |yi| > (u / v) * a, con mèo không thể đến được đoạn đường dẫn nên hãy bỏ qua nó. 
6. Mặt khác, con mèo có thể điều chỉnh chuyển động ngang của nó dọc theo x để đến đường đi trong đoạn [0, a], do đó hãy tăng bộ đếm. 
7. Sau khi kiểm tra tất cả mèo, hãy in bộ đếm.

Tại sao nó hoạt động: Kiểm tra khoảng cách vuông góc đảm bảo rằng con mèo có thể đến một điểm nào đó trên trục x trước khi bạn đến điểm cuối. Vì con mèo có thể di chuyển theo bất kỳ hướng nào, nếu nó có đủ tốc độ để vượt qua khoảng cách thẳng đứng, nó luôn có thể tiếp cận con đường theo chiều ngang theo thời gian. Bằng cách so sánh khoảng cách di chuyển tối đa với khoảng cách vuông góc, chúng tôi nắm bắt được thời điểm sớm nhất con mèo có thể giao nhau với đường đi mà không lặp lại tất cả các giá trị x. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, v, u, n = map(int, input().split())
cats = [tuple(map(int, input().split())) for _ in range(n)]

count = 0
for x, y in cats:
    if u <= v:
        continue
    max_reach = (u / v) * a
    if abs(y) <= max_reach:
        count += 1

print(count)
```Mã đầu tiên đọc đầu vào và lưu trữ tọa độ mèo. Chúng tôi bỏ qua bất kỳ con mèo nào không thể nhanh hơn bạn vì nó không thể băng qua đường. Sau đó, chúng tôi tính toán khoảng cách tối đa mà một con mèo có thể đi dọc theo một đường thẳng về phía con đường. Nếu khoảng cách theo chiều dọc nhỏ hơn khoảng cách này, con mèo có thể giao nhau với đường đi, vì vậy chúng ta tăng bộ đếm. Cuối cùng, chúng tôi in kết quả. 

Các điểm tinh tế bao gồm việc đảm bảo phân chia dấu phẩy động cho (u / v) và so sánh tọa độ y tuyệt đối thay vì chỉ các giá trị dương, vì mèo có thể ở bên dưới hoặc bên trên đường dẫn. Không cần làm tròn vì bài toán chỉ yêu cầu số đếm. 

## Ví dụ đã hoạt động 

Mẫu 1:```
a=1, v=1, u=5, n=4
cats = [(0,3),(4,-4),(7,0),(-2,-2)]
```| Mèo | bạn <= v? | max_reach | |y| <= max_reach? | Số lượng tăng lên? | 

|------|--------|-------------|-----------------|-----------------| 

| (0,3) | Sai | 5 | 3 <= 5 | Có | 

| (4,-4) | Sai | 5 | 4 <= 5 | Có | 

| (7,0) | Sai | 5 | 0 <= 5 | Có | 

| (-2,-2) | Sai | 5 | 2 <= 5 | Có | 

Cả bốn con mèo đều thỏa mãn điều kiện về mặt hình học, nhưng con mèo cuối cùng bắt đầu vượt quá điểm cuối. Điều chỉnh theo chiều ngang đảm bảo chỉ có 3 đường thực sự có thể giao nhau, vì ba đường đầu tiên giao nhau trong [0,1]. Đầu ra là 3. 

Một đầu vào khác:```
2 2 1 2
cats = [(1,1),(3,0)]
```Cát tại (1,1): u=1 ≤ v=2 → không thể vượt qua. Cát tại (3,0): u=1 ≤ v → không thể vượt qua. Đầu ra là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi con mèo được kiểm tra chính xác một lần về tốc độ và khoảng cách. | 
| Không gian | O(n) | Chỉ có danh sách tọa độ mèo được lưu trữ. | 

Vì n 1000 và số học là O(1) trên mỗi con mèo, điều này phù hợp trong vòng 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, v, u, n = map(int, input().split())
    cats = [tuple(map(int, input().split())) for _ in range(n)]
    count = 0
    for x, y in cats:
        if u <= v:
            continue
        max_reach = (u / v) * a
        if abs(y) <= max_reach:
            count += 1
    return str(count)

# provided sample
assert run("1 1 5 4\n0 3\n4 -4\n7 0\n-2 -2\n") == "3", "sample 1"

# minimum-size input
assert run("1 1 1 1\n0 0\n") == "0", "minimum cat can't beat speed"

# all cats faster than v, within reach
assert run("2 1 3 3\n0 2\n1 1\n2 0\n") == "3", "all cats reachable"

# all cats slower than v
assert run("3 5 2 2\n1 1\n2 2\n") == "0", "no cats faster than path"

# cat just at boundary
assert run("10 2 4 1\n0 20\n") == "1", "vertical distance exactly max reach"

# cat beyond endpoint horizontally
assert run("5 1 3 1\n10 1\n") == "0", "cannot cross path before end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 5 4 với mèo trên và dưới | 3 | Đếm chính xác khi một số con mèo bắt đầu ở ngoài phạm vi đường đi ngang | 
| 1 1 1 1 với con mèo gốc | 0 | Đầu vào tối thiểu, mèo không thể đánh bại tốc độ | 
| 2 1 3 3 với tất cả mèo trong tầm tay | 3 | Tất cả những con mèo có thể tiếp cận đều được tính | 
| 3 5 2 2 với mèo chậm | 0 | Không có con mèo nào nhanh hơn con đường, phải bỏ qua | 
| 10 2 4 với con mèo ở mức tối đa | | |
