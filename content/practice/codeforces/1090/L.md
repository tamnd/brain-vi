---
title: "CF 1090L - Đại học Berland"
description: "Có t sinh viên và n bài giảng. Một sinh viên đỗ nếu họ tham dự ít nhất k bài giảng. Các bài giảng xen kẽ giữa hai khán phòng. Các bài giảng có chỉ số lẻ được tổ chức ở khán phòng đầu tiên, nơi có thể chứa tối đa một học sinh."
date: "2026-06-13T04:06:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1090
codeforces_index: "L"
codeforces_contest_name: "2018-2019 Russia Open High School Programming Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2000
weight: 1090
solve_time_s: 421
verified: true
draft: false
---

[CF 1090L - Đại học Berland](https://codeforces.com/problemset/problem/1090/L) 

**Xếp hạng:** 2000 
**Tags:** tham lam, toán học 
**Thời gian giải:** 7 phút 1 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`t`sinh viên và`n`bài giảng. Một học sinh đậu nếu họ tham dự ít nhất`k`bài giảng. 

Các bài giảng xen kẽ giữa hai khán phòng. Các bài giảng có chỉ số lẻ được tổ chức ở khán phòng thứ nhất, có thể chứa tối đa`a`sinh viên. Các bài giảng với chỉ số chẵn được tổ chức ở khán phòng thứ hai, có thể chứa tối đa`b`sinh viên. 

Chúng tôi được tự do quyết định sinh viên nào sẽ tham dự mỗi bài giảng, miễn là không vượt quá sức chứa của khán phòng tương ứng. Một sinh viên có thể tham dự bất kỳ tập hợp con của bài giảng. 

Nhiệm vụ là xác định số lượng sinh viên tối đa có thể theo học ít nhất`k`bài giảng. 

Các giá trị có thể lớn như$10^9$, điều này ngay lập tức loại trừ mọi hành vi mô phỏng đối với học sinh hoặc bài giảng. Thậm chí một$O(n)$thuật toán là không thể bởi vì`n`bản thân nó có thể là$10^9$. Giải pháp chỉ được sử dụng một số lượng phép tính số học không đổi. 

Phần tế nhị của vấn đề là việc tham dự bài giảng không bị ràng buộc trước với các sinh viên cụ thể. Chúng ta hoàn toàn có thể sắp xếp lại việc tham dự từ bài giảng này sang bài giảng khác. Điều quan trọng là tổng số chỗ ngồi có sẵn trên tất cả các bài giảng của từng loại khán phòng. 

### Trường hợp cạnh không rõ ràng 

Hãy xem xét:```
10 3 4 4 5
```Chỉ có ba bài giảng, nhưng để vượt qua cần phải tham dự năm bài giảng. Không có học sinh nào có thể vượt qua được. 

Câu trả lời đúng là:```
0
```Một giải pháp bất cẩn chỉ đếm tổng số ghế có sẵn có thể tạo ra câu trả lời tích cực không chính xác. 

Bây giờ hãy xem xét:```
5 4 5 3 3
```Có hai bài giảng trong mỗi loại khán phòng. 

Tổng số chỗ còn trống:```
2 * 5 + 2 * 3 = 16
```Mỗi học sinh đỗ cần có ba lần tham dự. 

Từ$16 / 3 = 5$, cả năm học sinh đều có thể đỗ. 

Câu trả lời đúng là:```
5
```Ví dụ này cho thấy câu trả lời không chỉ được xác định bởi khán phòng nhỏ hơn. 

Một trường hợp quan trọng khác là:```
100000 100000 100000 100000 1
```Việc vượt qua chỉ cần một bài giảng. 

Mỗi sinh viên có thể tham dự đúng một bài giảng và vượt qua. 

Câu trả lời là:```
100000
```Số lượng sinh viên tự nó trở thành yếu tố hạn chế. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ cố gắng phân công học sinh vào các bài giảng và theo dõi số lượng bài giảng mà mỗi học sinh tham dự. Điều này nhanh chóng trở nên bất khả thi vì cả số lượng sinh viên và số lượng bài giảng có thể lên tới$10^9$. 

Quan sát quan trọng là danh tính của học sinh không quan trọng. Chúng tôi chỉ quan tâm đến việc có bao nhiêu suất tham dự. 

Giả sử chúng ta muốn chính xác`x`học sinh đỗ. 

Mỗi học sinh này phải tham dự ít nhất`k`bài giảng. Với tất cả các học sinh đỗ, chúng tôi cần ít nhất:$$x \cdot k$$chỗ tham dự. 

Có bao nhiêu suất tham dự? 

Cho phép$$o = \left\lceil \frac{n}{2} \right\rceil$$là số lượng bài giảng có chỉ số lẻ và$$e = \left\lfloor \frac{n}{2} \right\rfloor$$là số lượng bài giảng được lập chỉ mục chẵn. 

Tổng số chỗ trống trong toàn bộ khóa học là$$S = o \cdot a + e \cdot b.$$Nếu chúng ta muốn`x`học sinh đậu, chúng tôi cần$$xk \le S.$$Thoạt nhìn điều này gợi ý$$x \le \left\lfloor \frac{S}{k} \right\rfloor.$$Tuy nhiên, có một hạn chế nữa. Một sinh viên không thể tham dự nhiều hơn tất cả các bài giảng có sẵn. 

Một học sinh có thể tham dự nhiều nhất`n`bài giảng. Nếu như`k > n`, vượt qua là không thể. 

Nếu như`k ≤ n`, thì các suất tham dự luôn có thể được phân bổ cho các học sinh được chọn. Hạn chế duy nhất còn lại là chúng tôi không thể vượt qua nhiều hơn`t`sinh viên. 

Do đó câu trả lời trở thành:$$\min\left(t,\left\lfloor\frac{S}{k}\right\rfloor\right).$$Ngoại lệ duy nhất là khi`k > n`, trong trường hợp đó câu trả lời là 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân công tham dự Brute Force | Không thể có những hạn chế | Không thể | Quá chậm | 
| Đếm số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số bài giảng ở giảng đường thứ nhất:$$o = \frac{n+1}{2}$$dùng phép chia số nguyên. 
2. Tính số bài giảng ở giảng đường thứ hai:$$e = \frac{n}{2}.$$3. Nếu`k > n`, đầu ra`0`. 

Không học sinh nào có thể tham dự nhiều hơn`n`bài giảng. 
4. Tính tổng số suất tham dự:$$S = o \cdot a + e \cdot b.$$5. Tính số học sinh tối đa có thể nhận được ít nhất`k`tham dự:$$\left\lfloor \frac{S}{k} \right\rfloor.$$6. Câu trả lời cuối cùng là giá trị này nhỏ hơn và`t`, vì chỉ có`t`sinh viên có sẵn. 

### Tại sao nó hoạt động 

Mỗi sinh viên đi qua tiêu thụ ít nhất`k`chỗ tham dự. Vì có chính xác`S`thời gian tham dự có sẵn trong toàn bộ khóa học, không quá$\lfloor S/k \rfloor$học sinh có thể vượt qua. 

Ngược lại, khi`k ≤ n`, bất kỳ bộ sưu tập nào của`x`học sinh với$$xk \le S$$có thể được phân công đủ lượt nghe giảng. Nguồn lực duy nhất đang được tiêu thụ là năng lực tham dự và tổng công suất chính xác là`S`. Vì một học sinh có thể tham dự tối đa tất cả`n`bài giảng, không có nút thắt bổ sung. 

Như vậy số học sinh đậu tối đa có thể thực hiện được chính xác là$$\min\left(t,\left\lfloor\frac{S}{k}\right\rfloor\right).$$## Giải pháp Python```python
import sys
input = sys.stdin.readline

t, n, a, b, k = map(int, input().split())

if k > n:
    print(0)
else:
    odd_lectures = (n + 1) // 2
    even_lectures = n // 2

    total_slots = odd_lectures * a + even_lectures * b

    print(min(t, total_slots // k))
```Việc thực hiện tuân theo đạo hàm toán học trực tiếp. 

Kiểm tra đầu tiên xử lý tình huống duy nhất không thể xảy ra, cụ thể là yêu cầu số lượng người tham dự bài giảng nhiều hơn mức tồn tại trong khóa học. 

Số lượng bài giảng chẵn và lẻ được tính bằng số học số nguyên. Đối với số lẻ`n`, khán phòng đầu tiên được nhận thêm một bài giảng vì việc đánh số bài giảng bắt đầu từ một. 

Tất cả các phép tính đều vừa vặn thoải mái bên trong số nguyên 64 bit. Giá trị lớn nhất có thể là khoảng$$10^9 \cdot 10^9 = 10^{18},$$vẫn an toàn trong Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 3 4 4 3
```| Biến | Giá trị | 
| --- | --- | 
| t | 10 | 
| n | 3 | 
| một | 4 | 
| b | 4 | 
| k | 3 | 
| bài giảng kỳ quặc | 2 | 
| thậm chí các bài giảng | 1 | 
| tổng số vị trí | 12 | 
| tổng_slots // k | 4 | 
| trả lời | 4 | 

Đầu ra:```
4
```Chỉ có mười hai cơ hội tham dự. Mỗi học sinh đỗ cần có ba học sinh, vì vậy tối đa bốn học sinh có thể đỗ. 

### Ví dụ 2 

đầu vào:```
100 9 6 3 6
```| Biến | Giá trị | 
| --- | --- | 
| t | 100 | 
| n | 9 | 
| một | 6 | 
| b | 3 | 
| k | 6 | 
| bài giảng kỳ quặc | 5 | 
| thậm chí các bài giảng | 4 | 
| tổng số vị trí | 42 | 
| tổng_slots // k | 7 | 
| trả lời | 7 | 

Đầu ra:```
7
```Tổng cộng có bốn mươi hai vị trí tham dự. Vì mỗi học sinh đậu cần có sáu người tham dự nên có đúng bảy học sinh có thể đậu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Bộ nhớ bổ sung liên tục | 

Những hạn chế đạt tới$10^9$nên mọi giải pháp tùy thuộc vào số lượng sinh viên hoặc số giảng viên sẽ không khả thi. Một nghiệm số học theo thời gian không đổi dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    t, n, a, b, k = map(int, input().split())

    if k > n:
        return "0"

    odd = (n + 1) // 2
    even = n // 2
    total = odd * a + even * b

    return str(min(t, total // k))

# provided samples
assert run("10 3 4 4 3\n") == "4"
assert run("10 3 4 4 5\n") == "0"
assert run("100000 100000 100000 100000 1\n") == "100000"
assert run("5 4 5 3 3\n") == "5"
assert run("100 9 6 3 6\n") == "7"

# custom cases
assert run("1 1 1 1 1\n") == "1"
assert run("1 1 1 1 2\n") == "0"
assert run("100 2 1 1 1\n") == "2"
assert run("1000000000 1000000000 1000000000 1000000000 1000000000\n") == "1000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1`|`1`| Phiên bản hợp lệ tối thiểu | 
|`1 1 1 1 2`|`0`|`k > n`không thể | 
|`100 2 1 1 1`|`2`| Câu trả lời về giới hạn số lượng sinh viên | 
| Lớn$10^9$giá trị |`1000000000`| An toàn tràn | 

## Vỏ cạnh 

Hãy xem xét:```
10 3 4 4 5
```Thuật toán kiểm tra đầu tiên`k > n`. Từ`5 > 3`, không có sinh viên nào có thể tham dự đủ bài giảng. Nó ngay lập tức trả về:```
0
```Bây giờ hãy xem xét:```
5 4 5 3 3
```Chúng tôi tính toán:$$o = 2,\quad e = 2$$Và$$S = 2 \cdot 5 + 2 \cdot 3 = 16.$$Sau đó:$$\left\lfloor \frac{16}{3} \right\rfloor = 5.$$Vì có đúng 5 học sinh nên đáp án là:```
5
```Cuối cùng hãy xem xét:```
100000 100000 100000 100000 1
```Mỗi suất điểm danh có thể tạo ra một sinh viên đỗ vì chỉ cần một bài giảng. Công thức cho giá trị lớn hơn rất nhiều so với số học sinh nên kết quả cuối cùng`min(t, ...)`giới hạn chính xác câu trả lời tại:```
100000
```Điều này chứng tỏ tại sao số lượng học sinh phải được đưa vào tính toán cuối cùng.
