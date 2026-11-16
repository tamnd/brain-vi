---
title: "CF 1927A - Làm cho nó trắng"
description: "Chúng ta được cung cấp một dải ô nhỏ nằm ngang, mỗi ô có thể có màu đen hoặc trắng. Mục tiêu của chúng tôi là làm cho mọi ô trở nên trắng bằng cách sơn lại một đoạn liền kề. Thao tác sơn lại sẽ biến mọi ô đen trong đoạn đó thành màu trắng và giữ nguyên các ô trắng."
date: "2026-06-08T18:52:45+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1927
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 923 (Div. 3)"
rating: 800
weight: 1927
solve_time_s: 100
verified: true
draft: false
---

[CF 1927A - Làm cho nó trắng](https://codeforces.com/problemset/problem/1927/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dải ô nhỏ nằm ngang, mỗi ô có thể có màu đen hoặc trắng. Mục tiêu của chúng tôi là làm cho mọi ô trở nên trắng bằng cách sơn lại một đoạn liền kề. Thao tác sơn lại sẽ biến mọi ô đen trong đoạn đó thành màu trắng và giữ nguyên các ô trắng. Thách thức là xác định độ dài tối thiểu của đoạn để đảm bảo toàn bộ dải trở thành màu trắng. Đầu vào chỉ định nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số ô và màu ban đầu dưới dạng chuỗi 'B' và 'W'. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị độ dài phân đoạn tối thiểu được yêu cầu. 

Các ràng buộc rất nhỏ: số lượng ô trong bất kỳ dải nào nhiều nhất là 10 và có thể có tới 10.000 trường hợp thử nghiệm. Độ dài dải nhỏ cho phép chúng tôi xem xét các giải pháp kiểm tra tất cả các phân đoạn liền kề có thể có mà không cần lo ngại về hiệu suất. Các trường hợp cạnh phát sinh khi các ô màu đen nằm ở ranh giới hoặc tập trung ở các điểm cực trị. Ví dụ: nếu một ô màu đen ở vị trí ngoài cùng bên trái hoặc ngoài cùng bên phải thì đoạn tối thiểu phải che phủ ô đó; nếu các ô đen nằm rải rác thì đoạn tối thiểu phải kéo dài từ ô đen đầu tiên đến ô đen cuối cùng. 

Việc triển khai bất cẩn có thể chỉ đơn giản là đếm số lượng ô đen hoặc giả sử ô đen đầu tiên từ bên trái xác định độ dài đoạn, điều này có thể thất bại khi các ô đen được phân tách bằng các ô trắng. Ví dụ: trong dải "BWWB", đoạn chính xác kéo dài từ ô đen đầu tiên đến ô đen cuối cùng, có độ dài 4 chứ không chỉ 1 hoặc 2. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi phân đoạn liền kề có thể có và kiểm tra xem việc sơn đoạn đó có biến tất cả các ô thành màu trắng hay không. Vì độ dài dải tối đa là 10 nên điều này bao gồm tối đa 55 phân đoạn cho mỗi trường hợp thử nghiệm (tổng từ 1 đến n), điều này hoàn toàn có thể chấp nhận được. Mỗi lần kiểm tra là tuyến tính theo n, do đó, điều này mang lại khoảng 550 thao tác cho mỗi trường hợp thử nghiệm và lên tới khoảng 5,5 triệu thao tác trên 10.000 trường hợp thử nghiệm, đủ nhanh. Phương pháp này đúng vì nó xem xét rõ ràng tất cả các phân đoạn có thể, nhưng nó nặng nề một cách không cần thiết đối với một bài toán đơn giản như vậy. 

Cách tiếp cận tối ưu khai thác quan sát rằng chỉ có các ô đen ngoài cùng bên trái và ngoài cùng bên phải mới quan trọng. Bất kỳ đoạn nào ngắn hơn khoảng cách giữa ô đen đầu tiên và ô đen cuối cùng không thể bao phủ tất cả các ô đen. Do đó, đoạn tối thiểu phải bắt đầu ở hoặc trước màu đen ngoài cùng bên trái và kết thúc ở hoặc sau màu đen ngoài cùng bên phải. Độ dài tối ưu là chỉ số của màu đen cuối cùng trừ đi chỉ số của màu đen đầu tiên cộng một. Cái nhìn sâu sắc tham lam này làm giảm vấn đề thành một lần duy nhất để tìm các ô đen đầu tiên và cuối cùng, đưa ra giải pháp O(n) cho mỗi trường hợp thử nghiệm, cực kỳ nhanh với n 10. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Được chấp nhận do n nhỏ | 
| Tham lam (đen đầu tiên cuối cùng) | O(n) | O(1) | Đã được chấp nhận, tối ưu | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số ô n và chuỗi s đại diện cho màu sắc. 
2. Khởi tạo hai biến,`left`Và`right`, để theo dõi chỉ số của các ô đen ngoài cùng bên trái và ngoài cùng bên phải. Bắt đầu`left`tại n và`right`ở -1 để đảm bảo chúng sẽ bị ghi đè. 
3. Lặp chuỗi từ trái sang phải. Khi tìm thấy một ô màu đen ở chỉ mục i, hãy cập nhật`left`ĐẾN`min(left, i)`Và`right`ĐẾN`max(right, i)`. 
4. Sau vòng lặp,`left`giữ chỉ số của ô đen đầu tiên và`right`giữ chỉ số của ô đen cuối cùng. Đoạn tối thiểu bao phủ tất cả các ô đen có chiều dài`right - left + 1`. 
5. Xuất ra độ dài tính toán cho test case. 

Thuật toán này hoạt động vì bất kỳ đoạn nào ngắn hơn khoảng cách giữa ô đen đầu tiên và ô đen cuối cùng sẽ để lại ít nhất một ô đen không che. Việc bao phủ chính xác từ ô đen đầu tiên đến ô đen cuối cùng đảm bảo tất cả các ô đen đều được bao gồm và tránh việc sơn lại không cần thiết các ô đã có màu trắng bên ngoài phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        left = n
        right = -1
        for i, c in enumerate(s):
            if c == 'B':
                left = min(left, i)
                right = max(right, i)
        print(right - left + 1)
```Giải pháp đọc số lượng trường hợp thử nghiệm và sau đó lặp lại trên từng dải. Các ô đen ngoài cùng bên trái và ngoài cùng bên phải được xác định bằng một lần truyền qua chuỗi. Độ dài phân đoạn tối thiểu được tính bằng cách trừ các chỉ số và cộng một. sử dụng`strip()`đảm bảo rằng bất kỳ dòng mới nào ở cuối từ`input()`không can thiệp vào việc lập chỉ mục. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** "WBBWBW" 

| Chỉ mục | 0 | 1 | 2 | 3 | 4 | 5 | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tế bào | W | B | B | W | B | W | 
| trái | 1 | 1 | 1 | 1 | 1 | 1 | 
| đúng | 1 | 2 | 2 | 2 | 4 | 4 | 

Màu đen đầu tiên ở chỉ số 1, màu đen cuối cùng ở chỉ số 4, do đó độ dài đoạn tối thiểu = 4 - 1 + 1 = 4. 

**Ví dụ 2:** "BWWB" 

| Chỉ mục | 0 | 1 | 2 | 3 | 
| --- | --- | --- | --- | --- | 
| Tế bào | B | W | W | B | 
| trái | 0 | 0 | 0 | 0 | 
| đúng | 0 | 0 | 0 | 3 | 

Độ dài đoạn = 3 - 0 + 1 = 4. 

Những dấu vết này xác nhận cách tiếp cận tham lam xác định chính xác khoảng bao phủ tất cả các ô màu đen. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·t) | Mỗi dải có độ dài n được quét một lần cho t ca kiểm thử. Với n ≤ 10, điều này hiệu quả. | 
| Không gian | O(1) | Chỉ cần một vài biến cho mỗi trường hợp thử nghiệm; không có cấu trúc dữ liệu bổ sung. | 

Với những hạn chế, giải pháp này thực thi thoải mái trong giới hạn 2 giây ngay cả đối với 10.000 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
# helper function
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("8\n6\nWBBWBW\n1\nB\n2\nWB\n3\nBBW\n4\nBWWB\n6\nBWBWWB\n6\nWWBBWB\n9\nWBWBWWWBW\n") == "4\n1\n1\n2\n4\n6\n4\n7", "sample 1"

# custom cases
assert run("2\n3\nBBB\n4\nWWBB") == "3\n2", "all blacks, trailing blacks"
assert run("1\n5\nBWWWB") == "5", "blacks at both ends"
assert run("1\n6\nWWBWWW") == "1", "single black in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "BBB" | 3 | Tất cả người da đen phải được sơn | 
| "WWBB" | 2 | Người da đen ở cuối được đếm chính xác | 
| "BWWWB" | 5 | Người da đen ở cả hai đầu yêu cầu toàn bộ phân khúc | 
| "WWBWWW" | 1 | Tay cầm đơn màu đen | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi các ô màu đen được phân tách bằng các ô màu trắng. Ví dụ: "BWWB" có màu đen ở chỉ số 0 và 3. Một cách tiếp cận đơn giản chỉ đếm số lượng ô đen sẽ đề xuất không chính xác một đoạn có độ dài 2. Thuật toán tính toán chính xác đoạn tối thiểu là 3 - 0 + 1 = 4. Một trường hợp khác là một ô đen duy nhất trong dải, ví dụ: "WBBW". Thuật toán tính toán`left = right = 2`, tạo ra đoạn có độ dài 1, điều này đúng. Cách tiếp cận này xử lý chính xác tất cả các vị trí và khoảng cách của các ô màu đen.
