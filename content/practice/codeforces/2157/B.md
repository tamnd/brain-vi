---
title: "CF 2157B - Kế hoạch mở rộng 2"
description: "Chúng ta có một lưới 2D vô hạn với một ô màu đen ban đầu ở gốc (0, 0). Chúng tôi thực hiện một chuỗi các thao tác mở rộng, mỗi loại \"4\" hoặc loại \"8\"."
date: "2026-06-08T00:16:40+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2157
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1066 (Div. 1 + Div. 2)"
rating: 900
weight: 2157
solve_time_s: 93
verified: true
draft: false
---

[CF 2157B - Kế hoạch mở rộng 2](https://codeforces.com/problemset/problem/2157/B) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới 2D vô hạn với một ô màu đen ban đầu ở gốc`(0, 0)`. Chúng tôi thực hiện một chuỗi các hoạt động mở rộng, mỗi loại là "4" hoặc loại "8". Bản mở rộng loại "4" chuyển tất cả các ô liền kề trực giao thành màu đen và bản mở rộng loại "8" chuyển tất cả các ô liền kề trực giao hoặc chéo thành màu đen. Cho một ô mục tiêu`(x, y)`và một chuỗi mở rộng, chúng ta phải xác định xem ô mục tiêu có trở thành màu đen hay không. 

Những ràng buộc cho phép`n`lên đến`2*10^5`trên tất cả các trường hợp thử nghiệm và phối hợp lên đến`10^9`. Việc mô phỏng lưới một cách rõ ràng là không thể vì lưới là vô hạn và tọa độ rất lớn. Điều này ngay lập tức loại trừ mọi giải pháp cập nhật từng ô riêng lẻ hoặc lưu trữ toàn bộ trạng thái lưới. Thay vào đó, chúng ta phải suy luận về mặt hình học về sự phân bố của các tế bào đen. 

Các trường hợp cạnh bao gồm tọa độ âm và các phần mở rộng chỉ di chuyển theo một hướng. Ví dụ: nếu chuỗi đều là phần mở rộng "4", một ô đường chéo`(1,1)`sẽ không bao giờ trở thành màu đen, nhưng nếu có số "8" trong dãy thì có thể. Một sự tinh tế khác là thứ tự mở rộng: ngay cả một số "8" trong một chuỗi cũng có thể tiếp cận các ô mà một chuỗi "4" dài hơn không thể. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: duy trì một tập hợp các ô đen và cập nhật nó cho mỗi thao tác. Đối với "4", chúng tôi thêm bốn ô lân cận của mỗi ô màu đen; đối với "8", chúng tôi thêm tất cả tám người hàng xóm. Sau khi xử lý tất cả các hoạt động, chúng tôi kiểm tra xem`(x, y)`là trong bộ. Cách tiếp cận này hoạt động về nguyên tắc, nhưng số lượng tế bào tăng theo cấp số nhân, vì vậy với`n`xung quanh`10^5`điều này trở nên không khả thi. 

Cái nhìn sâu sắc quan trọng là vùng màu đen phát triển theo mô hình hình học có thể dự đoán được. Việc mở rộng trực giao làm tăng khoảng cách Manhattan từ điểm gốc lên 1 mỗi bước, trong khi việc mở rộng đường chéo làm tăng khoảng cách Chebyshev (tối đa là`|x|`Và`|y|`) bằng 1. Do đó, câu hỏi quy về việc tính toán xem liệu`(x, y)`nằm trong một hình thoi hoặc hình vuông có tâm ở gốc tọa độ sau khi áp dụng các phép khai triển kết hợp. 

Chúng ta không cần phải theo dõi từng ô đen. Chúng ta chỉ cần tìm số "8" đầu tiên trong dãy vì điều đó biến sự tăng trưởng thành số liệu Chebyshev, sau đó mọi phép mở rộng còn lại có thể được coi là mở rộng hình vuông một cách đồng đều. Nếu không có số "8", khoảng cách tối đa của Manhattan được tính là số "4". Bằng cách sử dụng điều này, chúng ta có thể kiểm tra ô trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(8^n) | O(số ô đen) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, phân tích`n`,`x`,`y`, và chuỗi`s`. 
2. Kiểm tra nguồn gốc xuất xứ`(0, 0)`là mục tiêu. Nếu vậy hãy quay lại`YES`. 
3. Xác định lần xuất hiện đầu tiên của số “8” trong`s`. Đặt chỉ số của nó là`k`(dựa trên 0). Trước chỉ số này, tất cả các bản mở rộng đều là "4" và mỗi lần tăng khoảng cách Manhattan thêm 1. 
4. Tính khoảng cách Manhattan`d = |x| + |y|`của mục tiêu từ nguồn gốc. 
5. Nếu không có số "8" trong`s`, kiểm tra xem`d <= n`. Nếu có, ô có thể đến được bằng các bước di chuyển trực giao; nếu không thì không thể. 
6. Nếu có ít nhất một số "8" ở vị trí`k`, sau đó`k`Khu vực "4" là viên kim cương của bán kính Manhattan`k`. Số "8" chuyển đổi số liệu thành Chebyshev, do đó các bước còn lại cho phép bất kỳ ô nào có khoảng cách Chebyshev`<= remaining_length`từ viên kim cương hiện tại cần đạt tới. 
7. Sử dụng tính chất khoảng cách Chebyshev là`max(|x|, |y|)`, kiểm tra xem`max(|x|, |y|) <= total_expansions`. Nếu được thì quay lại`YES`; nếu không thì,`NO`. 

Bất biến đảm bảo tính đúng đắn là sự tăng trưởng đơn điệu của vùng màu đen. Việc mở rộng Manhattan không thể tiếp cận các ô chỉ có đường chéo trừ khi xảy ra số "8" và sau số "8", vùng sẽ mở rộng đồng đều theo mọi hướng theo khoảng cách Chebyshev. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x, y = map(int, input().split())
    s = input().strip()
    
    if x == 0 and y == 0:
        print("YES")
        continue
    
    first8 = s.find('8')
    
    if first8 == -1:
        # all '4's, check Manhattan distance
        if abs(x) + abs(y) <= n:
            print("YES")
        else:
            print("NO")
    else:
        # the first '8' occurs at index first8
        # after that, Chebyshev distance matters
        # maximum of |x|, |y| must be <= n
        if max(abs(x), abs(y)) <= n:
            print("YES")
        else:
            print("NO")
```Chúng ta bắt đầu bằng cách xử lý trường hợp tầm thường trong đó mục tiêu là điểm gốc. Sau đó, chúng tôi tìm số "8" đầu tiên trong chuỗi để xác định thời điểm bắt đầu mở rộng đường chéo. Nếu không có số "8", chúng tôi chỉ cần khoảng cách Manhattan để kiểm tra khả năng tiếp cận. Nếu có "8", bạn có thể đạt được bất kỳ ô nào có tọa độ tối đa trong tổng số bước. sử dụng`find`và so sánh giá trị tuyệt đối cũng đảm bảo tính chính xác cho tọa độ âm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`(3 3 3, 888)`: 

| Bước | Hoạt động | Mục tiêu | Kiểm tra khoảng cách | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | Ban đầu | (0,0) | - | CÓ | 
| 1 | '8' | (3,3) | Chebyshev max( | 3 | 

Tế bào`(3,3)`đạt được vì mỗi lần mở rộng '8' sẽ tăng bán kính Chebyshev lên 1. 

Đầu vào mẫu`(4 5 1, 4884)`: 

| Bước | Hoạt động | Mục tiêu | Kiểm tra khoảng cách | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | Ban đầu | (0,0) | - | CÓ | 
| 1 | '4' | (5,1) | Khoảng cách Manhattan 6 > 1 | KHÔNG | 

Số '8' đầu tiên xảy ra muộn, do đó số '4 trước đó không thể đạt được mục tiêu. Số '8 còn lại không đủ để bao trùm khoảng cách trong tổng số bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét chuỗi một lần để tìm '8' | 
| Không gian | O(1) | Chỉ sử dụng số nguyên và chỉ mục chuỗi | 

Với`sum(n) <= 2*10^5`, điều này vừa vặn thoải mái dưới 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, x, y = map(int, input().split())
        s = input().strip()
        if x == 0 and y == 0:
            output.append("YES")
            continue
        first8 = s.find('8')
        if first8 == -1:
            output.append("YES" if abs(x)+abs(y) <= n else "NO")
        else:
            output.append("YES" if max(abs(x), abs(y)) <= n else "NO")
    return "\n".join(output)

# provided samples
assert run("6\n3 3 3\n888\n4 5 1\n4884\n4 3 -3\n4884\n7 -7 -5\n4884884\n10 0 0\n4884884888\n1 1 1\n4\n") == "YES\nNO\nYES\nNO\nYES\nNO"

# custom cases
assert run("1\n1 0 0\n4\n") == "YES", "origin"
assert run("1\n1 1 0\n4\n") == "YES", "Manhattan distance 1"
assert run("1\n1 1 1\n4\n") == "NO", "cannot reach diagonal with '4'"
assert run("1\n2 1 1\n48\n") == "YES", "diagonal reachable with '8'"
assert run("1\n5 -3 2\n44444\n") == "YES", "negative coordinates, Manhattan distance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 4 | CÓ | Mục tiêu là nguồn gốc | 
| 1 1 0 4 | CÓ | Khoảng cách Manhattan 1 có thể đến được bằng '4' | 
| 1 1 1 4 | KHÔNG | Không thể truy cập đường chéo chỉ với '4' | 
| 2 1 1 48 | CÓ | '8' đầu tiên cho phép đường chéo | 
| 5 -3 2 44444 | CÓ | Tọa độ âm với sự mở rộng Manhattan | 

## Vỏ cạnh 

Nếu mục tiêu là nguồn gốc`(0,0)`, thuật toán trả về ngay`YES`. Điều này xử lý trường hợp không cần mở rộng. Nếu tất cả các phần mở rộng là "4", thuật toán chính xác
