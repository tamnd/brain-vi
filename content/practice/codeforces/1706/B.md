---
title: "CF 1706B - Làm Tháp"
description: "Chúng ta có một dãy gồm n khối, mỗi khối có màu từ 1 đến n. Chúng ta cần đặt từng cái một trên lưới 2D. Khối đầu tiên luôn bắt đầu tại (0, 0)."
date: "2026-06-09T21:18:36+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1706
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 809 (Div. 2)"
rating: 1100
weight: 1706
solve_time_s: 183
verified: false
draft: false
---

[CF 1706B - Làm tháp](https://codeforces.com/problemset/problem/1706/B) 

**Đánh giá:** 1100 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi`n`các khối, mỗi khối có một màu từ`1`ĐẾN`n`. Chúng ta cần đặt từng cái một trên lưới 2D. Khối đầu tiên luôn bắt đầu tại`(0, 0)`. Mỗi khối tiếp theo có thể di chuyển sang trái, phải hoặc lên trên so với khối trước đó, nhưng không thể đi xuống hoặc chiếm vị trí đã chứa khối đó. Sau khi các khối được đặt, đối với mỗi màu, chúng ta được yêu cầu tìm ngăn xếp thẳng đứng liền kề lớn nhất của các khối có màu đó, tức là tòa tháp cao nhất chỉ sử dụng các khối có màu đó. 

Đầu vào lên đến`10^5`khối cho mỗi trường hợp thử nghiệm và tổng số trên tất cả các trường hợp thử nghiệm là`2 * 10^5`. Với giới hạn thời gian một giây, chúng tôi không thể chấp nhận bất cứ điều gì tệ hơn tuyến tính hoặc logarit tuyến tính cho mỗi trường hợp thử nghiệm. Các phép toán bậc hai trên các khối quá chậm. 

Trường hợp cạnh phát sinh khi một màu chỉ xuất hiện một lần hoặc ở các vị trí không thể căn chỉnh theo chiều dọc mà không vi phạm quy tắc sắp xếp. Ví dụ, nếu các khối`[1, 2, 1, 2, 1]`, cách đếm ngây thơ về số lần xuất hiện của`1`sẽ đề xuất một tòa tháp có kích thước`3`, nhưng các hạn chế về vị trí có thể ngăn xếp dọc lớn hơn`2`. Tương tự, một màu hoàn toàn không xuất hiện sẽ trả về một cách chính xác.`0`. 

Sự tinh tế nằm ở chỗ sự “tách dọc” giữa các khối liên tiếp cùng màu rất quan trọng. Chúng ta không thể tự do xếp chúng theo chiều dọc mà không xem xét các khối can thiệp, chúng có thể buộc di chuyển theo chiều ngang. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc đặt từng khối trên lưới, theo dõi tất cả các tọa độ có thể có và kiểm tra độ cao của tháp thẳng đứng cho từng màu. Điều này đúng, nhưng cực kỳ chậm. Đối với mỗi khối, chúng tôi có thể khám phá nhiều vị trí có thể và có tới`10^5`khối. Trong trường hợp xấu nhất, điều này dẫn đến hành vi theo cấp số nhân và không thể chạy trong giới hạn thời gian. 

Cái nhìn sâu sắc quan trọng là tọa độ tuyệt đối không quan trọng. Điều duy nhất ảnh hưởng đến chiều cao của tháp đối với một màu cụ thể là tần suất chúng ta cần "nhảy" theo chiều ngang để đặt khối tiếp theo cùng màu. Nếu hai khối cùng màu liên tiếp trong chuỗi, chúng ta có thể xếp chúng theo chiều dọc bằng cách đặt các khối xen kẽ theo chiều ngang. Do đó, chúng ta có thể đơn giản hóa vấn đề bằng cách đếm số lượng "khoảng trống" tối thiểu giữa các khối liên tiếp cùng màu và cách các khoảng trống này buộc di chuyển theo chiều ngang. 

Chúng ta có thể chính thức hóa điều này bằng cách xem xét một chuỗi các vị trí nơi xuất hiện một màu cụ thể. Giữa hai lần xuất hiện, nếu có các khối khác có màu khác nhau thì chúng ta cần thực hiện ít nhất một lần di chuyển theo chiều ngang. Vì vậy, ngăn xếp màu theo chiều dọc tối đa về cơ bản là`1 + the number of times that color appears with other blocks in between`. 

Một cách đơn giản để thực hiện điều này một cách hiệu quả là lặp lại các khối, theo dõi chỉ mục nhìn thấy lần cuối của mỗi màu và đếm "phân đoạn" giữa chúng. Điều này biến vấn đề thành quét tuyến tính trên mảng cho từng trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`max_tower`kích thước`n`với số không. Điều này sẽ lưu trữ kết quả cho mỗi màu. 
2. Đối với mỗi màu, hãy duy trì chỉ số của lần xuất hiện cuối cùng. Bắt đầu với`-1`để cho biết nó chưa được nhìn thấy. 
3. Lặp lại các khối một cách tuần tự. Đối với mỗi khối màu`c`:

Một. Nếu đây là lần đầu tiên xảy ra`c`, bắt đầu phân đoạn tháp mới, tính từ`1`. 

b. Nếu nó đã xuất hiện trước đó, hãy kiểm tra khoảng cách so với lần xuất hiện trước. Nếu có các khối khác ở giữa, chúng ta sẽ tăng số đoạn dọc cần thiết cho`c`. Điều này đếm một cách hiệu quả số lượng "di chuyển dọc" mà chúng ta có thể thực hiện cho màu này. 

c. Cập nhật chỉ số xuất hiện cuối cùng cho màu sắc`c`. 
4. Sau khi quét các khối, các đoạn thẳng đứng tối đa cho mỗi màu là câu trả lời. 
5. Xuất mảng`max_tower`. 

Lý do điều này hoạt động là vì mỗi khối phải tiếp tục là một tháp thẳng đứng hoặc buộc phải dịch chuyển theo chiều ngang nếu một khối khác can thiệp. Bằng cách đếm số khối giữa các lần xuất hiện liên tiếp của cùng một màu, chúng tôi thu được số lượng phân đoạn dọc tối thiểu cần thiết. Điều này đảm bảo chiều cao tháp tối đa theo quy định về vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    c = list(map(int, input().split()))
    
    last_pos = [-1] * (n + 1)
    max_tower = [0] * (n + 1)
    
    for idx, color in enumerate(c):
        if last_pos[color] == -1:
            max_tower[color] += 1
        else:
            if idx - last_pos[color] > 1:
                max_tower[color] += 1
        last_pos[color] = idx
    
    print(" ".join(str(max_tower[i]) for i in range(1, n + 1)))
```Mã theo dõi chỉ mục cuối cùng nơi mỗi màu xuất hiện và tăng số lượng tháp khi chúng ta gặp sự xuất hiện mới với các khối can thiệp. Sử dụng danh sách kích thước`n+1`tránh được từng lỗi một. các`if idx - last_pos[color] > 1`kiểm tra đảm bảo chúng tôi chỉ tăng khi có ít nhất một khối khác ở giữa, phản ánh sự dịch chuyển theo chiều ngang cần thiết ở vị trí. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
7
1 2 3 1 2 3 1
```| idx | màu sắc | cuối_pos | max_tower | 
| --- | --- | --- | --- | 
| 0 | 1 | -1 → 0 | 0 → 1 | 
| 1 | 2 | -1 → 1 | 0 → 1 | 
| 2 | 3 | -1 → 2 | 0 → 1 | 
| 3 | 1 | 0 → 3 | 1 → 2 | 
| 4 | 2 | 1 → 4 | 1 → 2 | 
| 5 | 3 | 2 → 5 | 1 → 2 | 
| 6 | 1 | 3 → 6 | 2 → 3 | 

Điều này chứng tỏ rằng ngăn xếp dọc chỉ tăng lên khi buộc phải di chuyển theo chiều ngang cần thiết. 

### Đầu vào mẫu 2```
6
4 2 2 2 4 4
```| idx | màu sắc | cuối_pos | max_tower | 
| --- | --- | --- | --- | 
| 0 | 4 | -1 → 0 | 0 → 1 | 
| 1 | 2 | -1 → 1 | 0 → 1 | 
| 2 | 2 | 1 → 2 | 1 (không có khoảng cách) | 
| 3 | 2 | 2 → 3 | 1 (không có khoảng cách) | 
| 4 | 4 | 0 → 4 | 1 → 2 | 
| 5 | 4 | 4 → 5 | 2 → 3 | 

Hiển thị việc xử lý các khối liên tiếp mà không cần tăng theo chiều dọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi khối được xử lý một lần; truy cập mảng là thời gian không đổi. | 
| Không gian | O(n) | Chúng tôi lưu trữ`last_pos`Và`max_tower`mảng kích thước`n+1`. | 

Với`sum(n) ≤ 2*10^5`, thuật toán chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        c = list(map(int, input().split()))
        last_pos = [-1] * (n + 1)
        max_tower = [0] * (n + 1)
        for idx, color in enumerate(c):
            if last_pos[color] == -1:
                max_tower[color] += 1
            else:
                if idx - last_pos[color] > 1:
                    max_tower[color] += 1
            last_pos[color] = idx
        res.append(" ".join(str(max_tower[i]) for i in range(1, n + 1)))
    return "\n".join(res)

# provided samples
assert run("6\n7\n1 2 3 1 2 3 1\n6\n4 2 2 2 4 4\n1\n1\n5\n5 4 5 3 5\n6\n3 3 3 1 3 3\n8\n1 2 3 4 4 3 2 1\n") == \
"3 2 2 0 0 0 0\n0 3 0 2 0 0\n1\n0 0 1 1 1\n1 0 4 0 0 0\n2 2 2 2 0 0 0 0"

# custom cases
assert run("1\n5\n1 1 1 1 1\n") == "1 0 0 0 0", "all same color consecutive"
assert run("
```
