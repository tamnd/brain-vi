---
title: "CF 2051B - Hành Trình"
description: "Monocarp đang thực hiện một chuyến đi bộ đường dài nhiều ngày và có mô hình khoảng cách lặp lại cho mỗi ngày: anh ấy đi bộ một km vào ngày đầu tiên, b vào ngày thứ hai, c vào ngày thứ ba, sau đó lặp lại chu kỳ đó vô thời hạn."
date: "2026-06-08T08:39:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 2051
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 995 (Div. 3)"
rating: 800
weight: 2051
solve_time_s: 91
verified: true
draft: false
---

[CF 2051B - Hành trình](https://codeforces.com/problemset/problem/2051/B) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Monocarp đang thực hiện một chuyến đi bộ đường dài nhiều ngày và có mô hình khoảng cách lặp đi lặp lại cho mỗi ngày: anh ấy đi bộ`a`km vào ngày đầu tiên,`b`vào ngày thứ hai,`c`vào ngày thứ ba, sau đó lặp lại chu kỳ đó vô thời hạn. Cho tổng khoảng cách mục tiêu`n`, chúng tôi được yêu cầu xác định chính xác ngày mà Monocarp đạt hoặc vượt`n`km tích lũy. Mỗi trường hợp thử nghiệm cho bốn số nguyên: khoảng cách mục tiêu`n`và khoảng cách trong chu kỳ ba ngày`a`,`b`,`c`. Đầu ra chỉ đơn giản là số ngày Monocarp hoàn thành hành trình của mình. 

Các ràng buộc cho phép lên tới 10.000 trường hợp thử nghiệm, mỗi trường hợp có`n`lên tới 1 tỷ và khoảng cách hàng ngày lên tới 1 triệu. Điều này có nghĩa là một cách tiếp cận đơn giản mô phỏng từng ngày một có thể thực hiện tới 1 tỷ thao tác cho một trường hợp thử nghiệm, quá chậm. Bất kỳ cách tiếp cận nào cũng phải tính đến mô hình lặp lại và khả năng xảy ra rất lớn`n`mà không cần lặp lại từng ngày riêng lẻ. 

Các trường hợp khó khăn phát sinh khi hành trình hoàn thành trong chu kỳ đầu tiên, đặc biệt là vào ngày đầu tiên. Ví dụ, nếu`n = 6`và quãng đường ngày đầu tiên`a = 6`, Monocarp kết thúc ngay lập tức. Một trường hợp cạnh khác là khi`n`là rất lớn, buộc phải thực hiện nhiều chu trình hoàn chỉnh trước khi chu trình từng phần cuối cùng góp phần đạt được`n`. Việc triển khai bất cẩn có thể cố gắng lặp lại từng ngày và hết thời gian hoặc xử lý không chính xác việc đếm từng cái một ở cuối chu kỳ một phần. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng hành trình hàng ngày, duy trì tổng quãng đường đang chạy và dừng lại khi tổng quãng đường đạt hoặc vượt quá`n`. Mặc dù điều này đúng nhưng trường hợp xấu nhất xảy ra khi`n = 10^9`và khoảng cách hàng ngày là nhỏ (ví dụ:`a = b = c = 1`). Thuật toán sẽ cần khoảng 1 tỷ lần lặp, điều này không thể thực hiện được trong vòng 1 giây. 

Quan sát quan trọng là khoảng cách lặp lại theo chu kỳ ba ngày. Tổng của một chu kỳ đầy đủ là`cycle_sum = a + b + c`. Nếu chúng ta có thể tính toán cần bao nhiêu chu kỳ hoàn chỉnh trước khi đạt được`n`, chúng ta có thể chia thành bội số của ba ngày thay vì lặp lại từng ngày. Sau khi hoàn thành đầy đủ các chu kỳ, chúng ta chỉ cần kiểm tra tối đa ba ngày nữa là đạt`n`. Điều này làm giảm vấn đề có thể xảy ra từ hàng tỷ lần lặp xuống chỉ còn một vài phép tính số học đơn giản cho mỗi trường hợp thử nghiệm. 

Cách tiếp cận bạo lực về mặt khái niệm đơn giản nhưng chậm, trong khi cách tiếp cận được tối ưu hóa tận dụng tính tuần hoàn của chuỗi để giảm đáng kể số lần tính toán. Chúng ta có thể kết hợp phép toán đơn giản với kiểm tra có điều kiện cho một phần chu trình ở cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm đối với n lớn | 
| Nhảy chu kỳ | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`n`,`a`,`b`,`c`cho một trường hợp thử nghiệm duy nhất. Chúng đại diện cho khoảng cách mục tiêu và khoảng cách đi bộ trong ba ngày. 
2. Tính tổng của một chu kỳ ba ngày:`cycle_sum = a + b + c`. Đây là tổng quãng đường Monocarp đi được trong ba ngày. 
3. Tính số chu kỳ ba ngày hoàn chỉnh có thể phù hợp trước khi vượt quá`n`:`full_cycles = (n - 1) // cycle_sum`. các`-1`đảm bảo chúng tôi tính toán chính xác ngày cuối cùng nếu tổng số điểm chính xác nằm trên ranh giới chu kỳ. nhân`full_cycles`bằng 3 để có được`days = full_cycles * 3`. Đây là số ngày sau tất cả các chu kỳ đầy đủ. 
4. Tính quãng đường tích lũy sau đủ chu kỳ:`distance_so_far = full_cycles * cycle_sum`. 
5. Lặp lại những ngày tiếp theo trong chu kỳ (tối đa ba ngày:`a`,`b`,`c`), cộng khoảng cách mỗi ngày vào`distance_so_far`. Sau mỗi lần thêm, kiểm tra xem`distance_so_far >= n`. Ngày đầu tiên thỏa mãn điều kiện này là đáp án. Tăng`days`với mỗi ngày bổ sung. 
6. Đầu ra`days`khi ngày Monocarp đạt ít nhất`n`km. 

Tại sao nó hoạt động: Bằng cách tận dụng tính chu kỳ của khoảng cách đi bộ, chúng ta có thể vượt qua toàn bộ chu kỳ ba ngày mà không yêu cầu mô phỏng từng ngày riêng lẻ. Chu kỳ một phần nhỏ, chỉ tối đa ba ngày nên việc lặp lại nó là không đáng kể. Điều bất biến là sau khi tính toán đầy đủ các chu kỳ,`distance_so_far`luôn nhỏ hơn`n`và ngày đầu tiên trong chu kỳ từng phần tiếp theo đẩy tổng lên trên`n`được đảm bảo là ngày hoàn thành chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, a, b, c = map(int, input().split())
    cycle_sum = a + b + c
    
    # number of full cycles
    full_cycles = (n - 1) // cycle_sum
    days = full_cycles * 3
    distance_so_far = full_cycles * cycle_sum
    
    for d in [a, b, c]:
        if distance_so_far >= n:
            break
        distance_so_far += d
        days += 1
    
    print(days)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó tính tổng chu kỳ ba ngày và số lượng chu kỳ hoàn chỉnh để nhảy. Sau đó, nó sẽ cộng thêm số ngày từ chu kỳ từng phần tiếp theo, kiểm tra sau mỗi lần bổ sung xem tổng khoảng cách có đạt đến không.`n`. Sự lặp lại kết thúc`[a, b, c]`đảm bảo chúng tôi không bao giờ vượt quá ba lần kiểm tra bổ sung. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n = 12, a = 1, b = 5, c = 3`| Bước | ngày | khoảng cách_so_far | Hành động | 
| --- | --- | --- | --- | 
| full_cycles | 1*3=3 | 9 | sau 1 chu kỳ đầy đủ 9 km | 
| ngày 4 | 4 | 9+1=10 | kiểm tra khoảng cách < n | 
| ngày 5 | 5 | 10+5=15 | khoảng cách >= n → dừng lại | 

Thuật toán trả về 5, khớp với kết quả mong đợi. 

**Ví dụ 2:**`n = 6, a = 6, b = 7, c = 4`| Bước | ngày | khoảng cách_so_far | Hành động | 
| --- | --- | --- | --- | 
| full_cycles | 0 | 0 | (n-1)//cycle_sum = 0 | 
| ngày 1 | 1 | 0+6=6 | khoảng cách >= n → dừng lại | 

Thuật toán trả về 1, đúng khi hoàn thành trong một ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ kiểm tra số học và tối đa ba ngày bổ sung | 
| Không gian | O(1) | Không có cấu trúc bổ sung, chỉ có số nguyên | 

Với tối đa 10.000 trường hợp thử nghiệm, tổng số thao tác vẫn rất nhỏ, thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, a, b, c = map(int, input().split())
        cycle_sum = a + b + c
        full_cycles = (n - 1) // cycle_sum
        days = full_cycles * 3
        distance_so_far = full_cycles * cycle_sum
        for d in [a, b, c]:
            if distance_so_far >= n:
                break
            distance_so_far += d
            days += 1
        output.append(str(days))
    return "\n".join(output)

# Provided samples
assert run("4\n12 1 5 3\n6 6 7 4\n16 3 4 1\n1000000000 1 1 1\n") == "5\n1\n6\n1000000000", "sample tests"

# Custom tests
assert run("1\n1 1 1 1\n") == "1", "minimum n"
assert run("1\n3 1 1 1\n") == "3", "exact 1 cycle"
assert run("1\n10 2 2 2\n") == "5", "partial cycle after full cycles"
assert run("1\n7 3 2 2\n") == "3", "finishes within first cycle"
assert run("1\n1000000 1000000 1000000 1000000\n") == "1", "large single step"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | Tối thiểu n hoàn thành vào ngày đầu tiên | 
| 3 1 1 1 | 3 | Hoàn thành chính xác vào cuối chu kỳ đầu tiên | 
| 10 2 2 2 | 5 | Chu kỳ một phần sau chu kỳ đầy đủ | 
| 7 3 2 2 | 3 | Hoàn thành trong chu kỳ đầu tiên | 
| 1000000 1000000 1000000 1000000 | 1 | Giá trị lớn kết thúc ngay lập tức | 

## Vỏ cạnh 

Khi nào`n`nhỏ hơn ngày đầu tiên, chẳng hạn như`n=1`Và`a=1, b=1, c=1`,`full_cycles = 0`, và lần lặp đầu tiên kết thúc`[a, b, c]`thỏa mãn ngay `khoảng cách_so_xa
