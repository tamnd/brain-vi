---
title: "CF 1492A - Ba vận động viên bơi lội"
description: "Chúng ta có ba người bơi liên tục bơi qua một bể bơi và quay trở lại, mỗi người vào một khoảng thời gian cố định riêng. Người bơi thứ nhất mất một phút để hoàn thành một vòng, người thứ hai b phút và người thứ ba c phút."
date: "2026-06-10T22:20:25+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1492
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 704 (Div. 2)"
rating: 800
weight: 1492
solve_time_s: 114
verified: true
draft: false
---

[CF 1492A - Ba vận động viên bơi lội](https://codeforces.com/problemset/problem/1492/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba người bơi liên tục bơi qua một bể bơi và quay trở lại, mỗi người vào một khoảng thời gian cố định riêng. Người bơi đầu tiên mất`a`phút để hoàn thành một chuyến đi khứ hồi, chuyến thứ hai`b`phút, và thứ ba`c`phút. Bạn đến hồ bơi`p`phút sau khi bắt đầu và mục tiêu là xác định xem bạn phải đợi bao nhiêu phút cho đến khi có ít nhất một người bơi đến được phía bên trái của hồ bơi. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm độc lập. Với mỗi số, chúng ta được cho bốn số nguyên`p`,`a`,`b`, Và`c`. Mỗi giá trị này có thể lớn tới 10^18, có nghĩa là bất kỳ mô phỏng lực mạnh nào lặp đi lặp lại theo thời gian quay trở lại của mỗi người bơi lội sẽ không khả thi. Chúng ta cần một phương pháp có thể làm việc trực tiếp với những con số lớn này. 

Các trường hợp khó khăn không rõ ràng phát sinh khi bạn đến đúng lúc người bơi ở phía bên trái hoặc khi thời gian chờ đợi lâu hơn một số khoảng thời gian của người bơi. Ví dụ, nếu`p = 10`Và`a = 2, b = 5, c = 10`, bạn đến đúng lúc người bơi thứ ba có mặt ở đó, vì vậy thời gian chờ đợi sẽ là`0`. Một giải pháp bất cẩn chỉ kiểm tra bội số tương lai mà không xem xét sự bằng nhau sẽ báo cáo sai sự chờ đợi tích cực. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là tạo ra tất cả thời gian quay trở lại của mỗi người bơi cho đến thời điểm hiện tại.`p`và hơn thế nữa, sau đó quét về phía trước để tìm lần đầu tiên lớn hơn hoặc bằng`p`. Về nguyên tắc thì điều này đúng nhưng nó nhanh chóng trở nên không thực tế vì`p`có thể là 10^18 và các dấu chấm`a, b, c`cũng có thể rất lớn. Lặp lại hàng triệu hoặc hàng tỷ bước là quá chậm. 

Quan sát quan trọng là thời gian quay về của mỗi vận động viên bơi lội tạo thành một chuỗi số học bắt đầu từ 0 với một bước bằng chu kỳ của họ. Để tìm thời gian quay lại tiếp theo không nhỏ hơn`p`, chúng ta có thể tính toán nó bằng toán học thay vì lặp lại. Đối với một vận động viên bơi lội có kinh nguyệt`x`, lần trở lại tiếp theo sau`p`là bội số nhỏ nhất của`x`lớn hơn hoặc bằng`p`. Nếu như`p`chia hết cho`x`, lần trở lại tiếp theo là`p`chính nó. Nếu không thì đó là`(p // x + 1) * x`. Khi chúng ta có thời gian quay lại tiếp theo của mỗi người bơi, thời gian chờ là tối thiểu`(next_return_time - p)`trên cả ba vận động viên bơi lội. 

Điều này làm giảm vấn đề từ việc lặp qua các chuỗi có khả năng rất lớn đến việc tính toán số học trong thời gian không đổi cho mỗi người bơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(p / phút(a, b, c)) | O(1) | Quá chậm | 
| Tính Toán | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, hãy đọc các số nguyên`p`,`a`,`b`, Và`c`. 
3. Đối với mỗi kỳ bơi`x`TRONG`[a, b, c]`, tính toán lần tiếp theo người bơi ở phía bên trái không sớm hơn`p`. Nếu như`p`chia hết cho`x`, lần sau là`p`. Ngược lại hãy tính`next_time = ((p // x) + 1) * x`. 
4. Tính thời gian chờ của mỗi người bơi như sau:`next_time - p`. 
5. Câu trả lời cho trường hợp thử nghiệm là thời gian chờ tối thiểu của ba người bơi. 
6. In kết quả cho từng test. 

Tại sao nó hoạt động: Phép tính số học đảm bảo chúng ta tìm được bội số đầu tiên của thời gian của mỗi vận động viên bơi lội không nhỏ hơn`p`. Trừ`p`đưa ra thời gian chờ đợi chính xác. Lấy số lượng tối thiểu đối với tất cả những người bơi lội sẽ đảm bảo chúng ta tìm được người đến sớm nhất. Vì số học với số nguyên lên tới 10^18 được hỗ trợ nên không cần phép tính gần đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def next_arrival(p, x):
    if p % x == 0:
        return p
    return ((p // x) + 1) * x

t = int(input())
for _ in range(t):
    p, a, b, c = map(int, input().split())
    wait_times = [next_arrival(p, a) - p,
                  next_arrival(p, b) - p,
                  next_arrival(p, c) - p]
    print(min(wait_times))
```chức năng`next_arrival`xử lý việc tính toán bội số tiếp theo một cách rõ ràng, tránh các lỗi sai sót khi`p`chính xác chia hết cho thời gian của người bơi. Vòng lặp chính đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và thực hiện tất cả các phép tính bằng số học số nguyên, đảm bảo tính chính xác cho các giá trị rất lớn. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên`9 5 4 8`, quá trình tính toán được tiến hành như sau: 

| Vận động viên bơi lội | Giai đoạn`x`| Bội số tiếp theo ≥ 9 | Thời gian chờ đợi | 
| --- | --- | --- | --- | 
| một | 5 | 10 | 1 | 
| b | 4 | 12 | 3 | 
| c | 8 | 16 | 7 | 

Thời gian chờ tối thiểu là`1`. 

Đối với mẫu thứ hai`2 6 10 9`: 

| Vận động viên bơi lội | Giai đoạn`x`| Bội số tiếp theo ≥ 2 | Thời gian chờ đợi | 
| --- | --- | --- | --- | 
| một | 6 | 6 | 4 | 
| b | 10 | 10 | 8 | 
| c | 9 | 9 | 7 | 

Thời gian chờ tối thiểu là`4`. 

Những dấu vết này xác nhận thuật toán tính toán chính xác thời gian đến tiếp theo và chọn chính xác thời gian chờ tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm ba phép tính số học và mức tối thiểu. | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được sử dụng cho mỗi trường hợp thử nghiệm. | 

Với`t`lên tới 1000, giải pháp này dễ dàng chạy trong giới hạn 1 giây ngay cả với các giá trị lên tới 10^18. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assuming solution above saved in solution.py
    return output.getvalue().strip()

# provided samples
assert run("4\n9 5 4 8\n2 6 10 9\n10 2 5 10\n10 9 9 9\n") == "1\n4\n0\n8", "sample 1"

# custom cases
assert run("1\n1 1 1 1\n") == "0", "all swimmers arrive immediately"
assert run("1\n10 3 7 11\n") == "1", "general case with different periods"
assert run("1\n1000000000000000000 1 2 3\n") == "0", "p divisible by smallest period"
assert run("1\n5 10 10 10\n") == "5", "all periods larger than p"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 0 | Có mặt đúng giờ xuất phát, tất cả các vận động viên bơi lội đều có mặt | 
| 10 3 7 11 | 1 | Trường hợp chung với các thời kỳ khác nhau | 
| 10^18 1 2 3 | 0 | Xử lý rất lớn`p`với sự xuất hiện của người bơi lội ngay lập tức | 
| 5 10 10 10 | 5 | Tất cả thời gian bơi lội lớn hơn thời gian đến | 

## Vỏ cạnh 

Nếu bạn đến đúng lúc người bơi đang ở phía bên trái,`p % x == 0`đảm bảo thời gian chờ bằng không. Ví dụ, với đầu vào`p=10`Và`a=2`,`b=5`,`c=10`, tính toán cho người bơi thứ ba cho`next_time = 10`, Và`next_time - p = 0`. Kết quả đầu ra chính xác tối thiểu đối với tất cả những người bơi lội`0`. 

Khi tất cả các khoảng thời gian đều lớn hơn`p`, chẳng hạn như`p=5`Và`a=b=c=10`, phép tính mang lại`next_time = 10`cho tất cả, và thời gian chờ đợi là`5`, phản ánh chính xác rằng bạn phải đợi cho đến khi người bơi đầu tiên quay lại. 

Phương pháp này xử lý nhất quán tất cả các kết hợp thời gian đến liên quan đến thời gian của người bơi.
