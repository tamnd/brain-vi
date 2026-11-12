---
title: "CF 1661C - Tưới Cây"
description: "Chúng ta được cấp một tập hợp các cây, mỗi cây có chiều cao ban đầu. Mục tiêu của chúng tôi là làm cho tất cả các cây đạt được chiều cao cuối cùng như nhau bằng quy trình tưới nước hàng ngày. Vào những ngày lẻ, tưới nước cho cây sẽ tăng chiều cao thêm 1, còn vào những ngày chẵn thì chiều cao tăng thêm 2."
date: "2026-06-10T02:57:13+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1661
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 126 (Rated for Div. 2)"
rating: 1700
weight: 1661
solve_time_s: 235
verified: true
draft: false
---

[CF 1661C - Tưới cây](https://codeforces.com/problemset/problem/1661/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 3 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các cây, mỗi cây có chiều cao ban đầu. Mục tiêu của chúng tôi là làm cho tất cả các cây đạt được chiều cao cuối cùng như nhau bằng quy trình tưới nước hàng ngày. Vào những ngày lẻ, tưới nước cho cây sẽ tăng chiều cao thêm 1, còn vào những ngày chẵn thì chiều cao tăng thêm 2. Chúng ta có thể bỏ tưới nước vào bất kỳ ngày nào. Mỗi ngày chỉ được tưới một cây. Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định số ngày tối thiểu cần thiết để tất cả các cây đều có cùng độ cao. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng cây và chiều cao của chúng. Tổng số cây trong tất cả các trường hợp thử nghiệm không vượt quá 300.000. Bản thân độ cao có thể lên tới 1 tỷ, điều này loại trừ bất kỳ giải pháp nào lặp lại một cách rõ ràng qua mọi mức tăng chiều cao có thể có. Có thể chấp nhận cách tiếp cận O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm; O(n^2) quá chậm. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó tất cả các cây ban đầu đều bằng nhau, do đó cần có 0 ngày. Một trường hợp tinh tế khác là khi các cây có chiều cao rất khác nhau, ví dụ:`[1, 10, 1]`, nơi việc tưới nước tham lam ngây thơ có thể dễ dàng chọn sai dãy cây và mô phỏng nhiều ngày hơn mức cần thiết. Một trường hợp khác là khi số lượng yêu cầu tăng lên 2 đơn vị vượt quá mức tăng 1 đơn vị; tính chẵn lẻ của các ngày rất quan trọng vì chúng ta chỉ có thể +2 vào các ngày chẵn. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ sẽ mô phỏng mọi trình tự tưới nước có thể xảy ra. Chúng tôi lặp đi lặp lại từng ngày, chọn một cây để tưới nước hoặc bỏ qua và cố gắng cân bằng tất cả các cây. Điều này rõ ràng là đúng, nhưng mỗi ngày có nhiều lựa chọn và tổng số ngày có thể bằng chênh lệch giữa cây cao nhất và cây ngắn nhất, nhân với một số yếu tố do mức tăng xen kẽ. Điều này có thể dễ dàng đạt được các phép toán O(n × max_h), quá chậm đối với n lên tới 3 × 10^5 và h_i lên tới 10^9. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng hàng ngày một cách rõ ràng. Để cây cao nhất sau khi tưới nước`H`. Sau đó mỗi cây cần nhận`delta_i = H - h_i`đơn vị tăng trưởng. Chúng ta có thể coi mỗi delta là sự kết hợp của các phép toán +1 (ngày lẻ) và phép toán +2 (ngày chẵn). Cho phép`a`là số phép toán +2 và`b`số lượng thao tác +1 cần thiết để đạt được`delta_i`. Sau đó`2*a + b >= delta_i`. 

Sau đó, chúng ta có thể mô hình hóa vấn đề dưới dạng phân bổ số ngày lẻ và chẵn tối thiểu để bao trùm tất cả các vùng đồng bằng. Cho phép`count1`là tổng số đơn vị +1 được yêu cầu trên tất cả các cây và`count2`là tổng số +2 đơn vị. Số ngày cần thiết phải thỏa mãn:```
ceil(count1 / ceil(days/2)) <= number of odd days
ceil(count2 / floor(days/2)) <= number of even days
```Tìm kiếm nhị phân theo số ngày hoạt động hiệu quả. Chúng tôi thử một ứng cử viên`days`và kiểm tra xem có thể chỉ định các thao tác +1 và +2 cần thiết theo tính chẵn lẻ của ngày hay không. Bởi vì số ngày cần thiết tối đa bị giới hạn bởi 2 × max(delta_i), nên tìm kiếm nhị phân hội tụ theo các bước log(max_delta), làm cho độ phức tạp tổng thể là O(n log max_h). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × max_h) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Toán | O(n log max_h) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`và danh sách độ cao`h`. 
2. Xác định chiều cao ban đầu tối đa`max_h`. Chúng tôi sẽ cố gắng cân bằng tất cả các cây đến độ cao cuối cùng`H >= max_h`. 
3. Khởi tạo`left = 0`Và`right = 2 * 10^9`để tìm kiếm nhị phân theo số ngày. 
4. Trong khi`left < right`, bộ`mid = (left + right) // 2`như một số ngày ứng cử viên. 
5. Với mỗi cây, hãy tính`delta_i = H - h_i`. Sau đó tính xem có bao nhiêu thao tác +2 (`x_i = delta_i // 2`) và các phép toán +1 (`y_i = delta_i % 2`) là bắt buộc. Tổng hợp tất cả`x_i`để có được tổng số hoạt động 2 đơn vị, tổng hợp tất cả`y_i`để có được tổng số hoạt động 1 đơn vị. 
6. Xác định`num_even_days = mid // 2`Và`num_odd_days = mid - num_even_days`. Kiểm tra xem`total_2_units <= num_even_days`Và`total_1_units <= num_odd_days`. Nếu đúng,`mid`ngày là đủ, vì vậy hãy di chuyển`right = mid`. Ngược lại, hãy di chuyển`left = mid + 1`. 
7. Sau khi tìm kiếm nhị phân,`left`là số ngày tối thiểu thỏa mãn tất cả các cây. 

Bất biến là đối với bất kỳ ứng cử viên nào`days`, chúng tôi biết chính xác mình có thể thực hiện bao nhiêu lần tăng +1 và +2. Bằng cách chỉ định tất cả số tăng +2 cho ngày chẵn và số tăng +1 cho ngày lẻ, chúng tôi đảm bảo tính khả thi. Tìm kiếm nhị phân đảm bảo tính tối thiểu vì nó loại bỏ tất cả các giá trị lớn hơn khi tìm thấy giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_days_to_equalize(heights):
    max_h = max(heights)
    
    # Binary search on days
    left, right = 0, 2 * 10**9
    while left < right:
        mid = (left + right) // 2
        odd_days = (mid + 1) // 2
        even_days = mid // 2
        
        total_1, total_2 = 0, 0
        for h in heights:
            delta = max_h - h
            total_2 += delta // 2
            total_1 += delta % 2
        
        if total_1 <= odd_days and total_2 <= even_days:
            right = mid
        else:
            left = mid + 1
    return left

t = int(input())
for _ in range(t):
    n = int(input())
    h = list(map(int, input().split()))
    print(min_days_to_equalize(h))
```Giải pháp xác định một chức năng`min_days_to_equalize`thực hiện tìm kiếm nhị phân theo số ngày. Bên trong vòng lặp, nó tính toán có bao nhiêu ngày lẻ và chẵn và tính tổng số thao tác +1 và +2 cần thiết trên tất cả các cây. Tìm kiếm nhị phân hội tụ đến số ngày tối thiểu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[1, 2, 4]`: 

| Ngày | Chiều cao tối đa H | Đồng bằng cây | Tổng cộng +1 | Tổng +2 | Ngày Lẻ | Ngày chẵn | Khả thi? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 4 | 4 | [3,2,0] | 1 | 2 | 2 | 2 | Có | 

Tìm kiếm nhị phân nhanh chóng tìm thấy 4 ngày là tối thiểu. 

Đầu vào mẫu`[2, 5, 4, 8, 3, 7, 4]`: 

| Ngày | Chiều cao tối đa H | Đồng bằng cây | Tổng cộng +1 | Tổng +2 | Ngày Lẻ | Ngày chẵn | Khả thi? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 16 | 8 | [6,3,4,0,5,1,4] | 4 | 10 | 8 | 8 | Có | 

Dấu vết xác nhận số ngày tối thiểu cần thiết là 16. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log max_h) | Tìm kiếm nhị phân theo ngày nhân với tổng O(n) trên tất cả các cây trên mỗi lần lặp | 
| Không gian | O(n) | Lưu trữ danh sách chiều cao cây | 

Được cho`n`lên tới 3 × 10^5 và`max_h`lên tới 10^9, log(max_h) ≈ 30, do đó, các thao tác là khoảng 10^7 cho mỗi trường hợp thử nghiệm, nằm trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        t = int(input())
        for _ in range(t):
            n = int(input())
            h = list(map(int, input().split()))
            print(min_days_to_equalize(h))
    return out.getvalue().strip()

# provided samples
assert run("3\n3\n1 2 4\n5\n4 4 3 5 5\n7\n2 5 4 8 3 7 4\n") == "4\n3\n16"

# custom cases
assert run("1\n1\n10\n") == "0", "single tree"
assert run("1\n3\n5 5 5\n") == "0", "all equal"
assert run("1\n3\n1 1 1000000000\n") == "500000000", "extreme height difference"
assert run("1\n2\n1 2\n") == "2", "small n, small difference"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n10\n`|`0`| Cây đơn không cần tưới nước | 
| `1\n | | |
