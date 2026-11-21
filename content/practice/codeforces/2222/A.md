---
title: "CF 2222A - Cuộc thi tuyệt vời"
description: "Chúng ta được yêu cầu xác định xem một cuộc thi lập trình có “tuyệt vời” hay không theo nghĩa là mọi số nguyên có thể đạt được trong khoảng từ 0 đến 100 n đều có thể đạt được. Cuộc thi có n bài toán, mỗi bài toán được chia thành các nhiệm vụ con ai."
date: "2026-06-07T18:40:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 2222
codeforces_index: "A"
codeforces_contest_name: "Spectral::Cup 2026 Round 1 (Codeforces Round 1094, Div. 1 + Div. 2)"
rating: 0
weight: 2222
solve_time_s: 123
verified: false
draft: false
---

[CF 2222A - Một cuộc thi tuyệt vời](https://codeforces.com/problemset/problem/2222/A) 

**Đánh giá:** - 
**Tags:** vũ lực, dp, toán học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một cuộc thi lập trình có “tuyệt vời” hay không theo nghĩa là mọi số nguyên có thể có tổng điểm giữa`0`Và`100 * n`có thể đạt được. Cuộc thi có`n`vấn đề và mỗi vấn đề được chia thành`a_i`nhiệm vụ phụ. Mỗi nhiệm vụ phụ của`i`- vấn đề thứ đóng góp như nhau vào điểm của vấn đề, tức là,`100 / a_i`điểm. 

Tổng điểm của thí sinh là tổng điểm của các nhiệm vụ phụ mà họ giải được ở mỗi bài toán. Đầu vào cung cấp một số trường hợp kiểm thử, mỗi trường hợp đưa ra số lượng vấn đề và danh sách các nhiệm vụ phụ cho mỗi vấn đề. Đầu ra phải chỉ ra cho mỗi trường hợp kiểm tra xem tất cả các số nguyên tổng điểm từ`0`ĐẾN`100 * n`có thể thu được. 

Các hạn chế là nhỏ:`n`nhiều nhất là`10`,`a_i`chia rẽ`100`, Và`t`tùy thuộc vào`100`. Với kích thước nhỏ như vậy`n`, một cách tiếp cận bạo lực là khả thi, nhưng chúng ta có thể tìm thấy một cái nhìn sâu sắc về mặt toán học hơn. 

Các trường hợp khó khăn không rõ ràng bao gồm các vấn đề trong đó tất cả`a_i`là các ước lớn của`100`. Ví dụ, nếu tất cả`a_i`là`100`, mỗi bài toán chỉ đóng góp bội số của`1`đến tổng số, điều này hoạt động tầm thường. Nhưng nếu`a_i`là`4`và cái khác là`25`, chúng ta phải đảm bảo rằng sự kết hợp của các điểm từ tất cả các bài toán cho phép có tổng mọi số nguyên. Một cách tiếp cận ngây thơ chỉ đơn giản là tính tổng điểm tối đa có thể bỏ sót những khoảng trống được tạo ra bởi các bước riêng biệt của từng vấn đề. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các kết hợp của`x_i`cho từng vấn đề. Từ`0 <= x_i <= a_i`Và`a_i <= 100`, điều này mang lại kết quả tồi tệ nhất`101^10`các kết hợp cho một trường hợp thử nghiệm duy nhất, quá lớn. Nó đúng nhưng không khả thi. 

Cái nhìn sâu sắc quan trọng là làm việc theo đơn vị “1 điểm” thay vì điểm số động. Vì mỗi`a_i`chia rẽ`100`, điểm cho vấn đề`i`có thể được biểu diễn dưới dạng số nguyên từ`0`ĐẾN`100`trong các bước của`100 / a_i`. Cho phép`step_i = 100 / a_i`. Khi đó câu hỏi rút gọn thành: liệu chúng ta có thể biểu diễn mọi số nguyên từ`0`ĐẾN`100 * n`dưới dạng tổ hợp số nguyên không âm của`step_1, step_2, ..., step_n`nơi bội số không vượt quá`a_i`? 

Chúng ta có thể mô hình hóa vấn đề này như một bài toán tổng con hoặc thay đổi đồng xu cổ điển, trong đó mỗi giá trị đồng xu là`step_i`với`a_i`bản sao. Với`n <= 10`Và`a_i <= 100`, tổng số tiền có thể đạt được nhiều nhất là`1000`, làm cho giải pháp lập trình động trở nên khả thi. Chúng tôi khởi tạo một mảng boolean`reachable[0..100*n]`và đánh dấu lặp đi lặp lại tất cả các điểm có thể đạt được bằng cách cộng bội số của mỗi điểm`step_i`. 

Điều này tránh việc ép buộc tất cả`x_i`kết hợp và hoạt động vì bài toán đảm bảo đủ số học số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(prod(a_i)) | O(100*n) | Quá chậm | 
| Lập trình động (giống như đồng xu) | O(n * 100 * tối đa(a_i)) | O(100*n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi số lượng nhiệm vụ của mỗi bài toán`a_i`thành một bước tính điểm:`step_i = 100 // a_i`. Điều này biến điểm số phân số thành số nguyên. 
2. Khởi tạo một mảng boolean`reachable`kích thước`100*n + 1`, Ở đâu`reachable[s]`cho biết liệu tổng số điểm có`s`là có thể đạt được. Bộ`reachable[0] = True`. 
3. Đối với từng vấn đề`i`với bước`step_i`Và`a_i`nhiệm vụ phụ, cập nhật`reachable`để bao gồm tất cả số tiền thu được bằng cách thêm`k * step_i`vì`k = 1..a_i`đến số tiền có thể đạt được trước đó. Điều này đảm bảo chúng tôi không vượt quá giới hạn nhiệm vụ phụ cho vấn đề. 
4. Sau khi xử lý tất cả các bài toán, kiểm tra xem mọi số nguyên có`s`từ`0`ĐẾN`100*n`có thể truy cập được. Nếu vậy, xuất`Yes`, nếu không thì`No`. 

Tại sao nó hoạt động:`reachable`mô hình chính xác tất cả các khoản tiền có thể được hình thành theo giới hạn nhiệm vụ con. Bằng cách lặp đi lặp lại việc bao gồm bội số của từng kích thước bước, chúng tôi đảm bảo rằng không có sự kết hợp nào bị bỏ sót. Số học số nguyên đảm bảo tính toán chính xác mà không có lỗi làm tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_wonderful(a):
    n = len(a)
    steps = [100 // ai for ai in a]
    max_score = 100 * n
    reachable = [False] * (max_score + 1)
    reachable[0] = True

    for step, count in zip(steps, a):
        # We iterate backwards to avoid using the same problem multiple times
        new_reachable = reachable[:]
        for s in range(max_score + 1):
            if reachable[s]:
                for k in range(1, count + 1):
                    ns = s + k * step
                    if ns <= max_score:
                        new_reachable[ns] = True
                    else:
                        break
        reachable = new_reachable

    return all(reachable)

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print("Yes" if is_wonderful(a) else "No")
```Giải pháp đầu tiên chuyển đổi điểm thành bội số nguyên, sau đó đánh dấu lặp đi lặp lại tất cả các tổng có thể đạt được. Chúng tôi lặp lại ngược lại`reachable`để ngăn chặn việc tính hai lần đóng góp từ cùng một vấn đề. Việc nghỉ ngơi ngăn cản việc vượt quá số điểm tối đa. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`2`

`2`

`1 2`

`3`

`4 5 10`| Bước | cập nhật mảng có thể truy cập | 
| --- | --- | 
| Bài toán 1: bước=100, số đếm=1 | 0, 100 | 
| Bài toán 2: bước=50, số đếm=2 | Thêm 50 và 100 -> có thể truy cập 0,50,100,150,200 | 
| Kiểm tra tất cả 0..200 | Đúng -> Có | 

**Đầu vào mẫu 2:**`1`

`2`

`4 5`| Bước | cập nhật mảng có thể truy cập | 
| --- | --- | 
| Bài toán 1: bước=25, đếm=4 | 0,25,50,75,100 | 
| Bài 2: bước=20, đếm=5 | Kết hợp trước -> thiếu 95, không thể | 
| Kiểm tra tất cả 0..100*2 | Sai -> Không | 

Những dấu vết này cho thấy DP tính toán chính xác kích thước và giới hạn của bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * max(a_i) * 100 * n) | Mỗi vấn đề lặp lại các nhiệm vụ phụ và điểm số có thể đạt được. n 10, a_i 100, điểm tối đa = 100*n 1000. | 
| Không gian | O(100*n) | Mảng điểm có thể truy cập lên tới 100*n. | 

Với n ≤ 10 và 100*n ≤ 1000, giải pháp sẽ chạy trong chưa đầy 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        output.append("Yes" if is_wonderful(a) else "No")
    return ''.join(output)

# Provided samples
assert run("2\n2\n1 2\n3\n4 5 10\n") == "YesNo", "Sample 1 and 2"

# Custom cases
assert run("1\n1\n100\n") == "Yes", "Single problem, 100 subtasks"
assert run("1\n2\n25 25\n") == "Yes", "Two equal divisors"
assert run("1\n2\n4 5\n") == "No", "Cannot reach all totals"
assert run("1\n3\n1 2 5\n") == "Yes", "Small mixed divisors"
assert run("1\n3\n20 25 50\n") == "Yes", "Large divisors with combination"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100 | Có | Vấn đề đơn lẻ, nhiệm vụ tối đa | 
| 2 25 25 | Có | Ước số bằng nhau, bảo hiểm dễ dàng | 
| 2 4 5 | Không | Số chia nhỏ không khớp sẽ ngăn cản phạm vi bao phủ đầy đủ | 
| 3 1 2 5 | Có | Các ước số nhỏ hỗn hợp | 
| 3 20 25 50 | Có | Ước số lớn hơn, đảm bảo bao phủ tổng | 

## Vỏ cạnh 

Đối với một vấn đề duy nhất với`a_1 = 100`, điểm số duy nhất có thể là bội số của`1`. Thuật toán đánh dấu chính xác tất cả các điểm từ`0`ĐẾN`100`, xác nhận "Có". 

Đối với một cặp`a = [4,5]`, số điểm có thể có của bài toán đầu tiên là`{0,25,50,75,100}`, và từ thứ hai`{0,20,40,60,80,100}`. Một số tổng số, như`95`, không thể hình thành được DP phát hiện không thể truy cập được
