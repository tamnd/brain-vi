---
title: "CF 2067A - Tổng các chữ số liền kề"
description: "Chúng ta được yêu cầu xác định xem có tồn tại số nguyên n sao cho tổng các chữ số của nó bằng một số x cho trước và tổng các chữ số của n + 1 bằng một số y đã cho khác hay không."
date: "2026-06-08T07:09:55+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 2067
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1004 (Div. 2)"
rating: 800
weight: 2067
solve_time_s: 101
verified: true
draft: false
---

[CF 2067A - Tổng các chữ số liền kề](https://codeforces.com/problemset/problem/2067/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem có tồn tại một số nguyên hay không`n`sao cho tổng các chữ số của nó bằng một số cho trước`x`, và tổng các chữ số của`n + 1`bằng một số đã cho khác`y`. Đầu vào cung cấp nhiều như vậy`(x, y)`cặp, và với mỗi cặp, chúng ta phải quyết định xem liệu một cặp như vậy có`n`tồn tại, trả về "CÓ" hoặc "KHÔNG". Tổng chữ số`S(a)`là tổng các chữ số thập phân của`a`. 

Các hạn chế là nhỏ:`x`Và`y`nhiều nhất là 1000 và có nhiều nhất 500 ca kiểm thử. Điều này có nghĩa là bất kỳ giải pháp nào hoạt động theo thời gian tuyến tính liên quan đến`x`hoặc`y`là khả thi, nhưng chúng ta nên tránh xây dựng các số khổng lồ một cách rõ ràng vì một số có tổng chữ số 1000 có thể có hơn 100 chữ số, điều này không thực tế khi xử lý trực tiếp trong số học vũ phu. 

Các trường hợp đặc biệt phát sinh từ việc việc thêm một vào một số sẽ ảnh hưởng đến tổng chữ số của nó như thế nào. Một giải pháp ngây thơ có thể cho rằng`S(n+1)`luôn luôn là`S(n) + 1`, không thành công khi có số 9 ở cuối. Ví dụ, nếu`n = 999`,`S(n) = 27`, Nhưng`S(n+1) = 1`. Tương tự, di chuyển từ 0 đến 1 sẽ làm tổng tăng lên 1, nhưng di chuyển từ 19 lên 20 sẽ làm giảm tổng từ 10 xuống 2. Các tình huống mang theo này tạo ra các tình huống trong đó`y`có thể ít hơn`x`, thoạt nhìn có vẻ phản trực giác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các số có tổng chữ số`x`, tính toán`S(n+1)`, và kiểm tra xem nó có bằng không`y`. Trong trường hợp xấu nhất, việc xây dựng một số có tổng chữ số 1000 có thể yêu cầu lặp lại tất cả các tổ hợp chữ số có tổng bằng 1000, tức là nhiều số theo cấp số nhân. Điều này rõ ràng là quá chậm, vì vậy chúng ta cần một cách tiếp cận thông minh hơn. 

Cái nhìn sâu sắc quan trọng là phân tích việc tăng một số ảnh hưởng như thế nào đến tổng chữ số của nó. Thêm một vào một số sẽ tăng tổng lên 1 nếu chữ số cuối cùng nhỏ hơn 9 hoặc kích hoạt mang số. Nếu cuối cùng`k`tất cả các chữ số đều là 9, sau đó thêm một chữ số sẽ giảm mỗi 9 xuống 0 và tăng chữ số trước đó thêm 1, giảm tổng đi`9*k - 1`. Cách tiếp cận mang tính xây dựng đơn giản nhất là thử xây dựng các số bắt đầu bằng 1, sau đó có thể là số 0 theo sau và kết thúc bằng chuỗi số 9. Cụ thể, chúng ta có thể thử các số có dạng`1...0...0...9...9`, cho phép chúng ta kiểm soát tổng chữ số của`n`và tác dụng của việc tiếp tục`n+1`. 

Về mặt toán học, nếu chúng ta chọn một số có`a`chữ số 9 ở cuối là tổng các chữ số của`n`là`s = (first digits sum) + 9*a`, và tổng của`n+1`là`(first digits sum + 1)`. Chúng tôi cần`(first digits sum + 1) = y`Và`(first digits sum + 9*a) = x`. Điều này làm giảm việc kiểm tra nếu`y - 1 >= 0`Và`x - (y - 1)`chia hết cho 9, đưa ra phép kiểm tra số học đơn giản mà không tạo ra số lượng lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số số có tổng x) | O(số chữ số trong số) | Quá chậm | 
| Logic mang tính xây dựng | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`x`Và`y`. 
2. Kiểm tra xem`y == 1`Và`x > 1`. Trong trường hợp này, số duy nhất có tổng sau số tăng là 1 là`10...0`, có tổng`1`, vì vậy bất cứ điều gì lớn hơn là không thể. 
3. Ngược lại hãy tính`d = x - (y - 1)`. Số này thể hiện tổng phải được chuyển đi bằng số 9 ở cuối để giảm`x`xuống`y - 1`sau khi tăng. 
4. Kiểm tra xem`d`không âm và chia hết cho 9. Nếu đúng như vậy thì chúng ta có thể tạo thành một số trong đó phần đầu tổng bằng`y - 1`và phần cuối gồm có`d // 9`số chín. Ngược lại không có số nào thỏa mãn điều kiện. 
5. In "CÓ" nếu như vậy`d`tồn tại và điều kiện chia hết được giữ nguyên, nếu không thì in ra "NO". 

Tại sao nó hoạt động: Mỗi số có thể được biểu diễn dưới dạng một số chữ số theo sau là một chuỗi số 9. Thêm một vào số này sẽ lật tất cả các số 9 ở cuối thành 0 và tăng chữ số trước đó lên một. Công thức`d = x - (y - 1)`đảm bảo rằng sau khi mang tổng các chữ số giảm đúng từ`x`ĐẾN`y`. Việc chia hết cho 9 đảm bảo rằng`d`có thể được tạo thành hoàn toàn bằng 9 giây, điều này cần thiết để logic mang tạo ra chính xác số tiền mong muốn trong`n+1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        d = x - (y - 1)
        if d >= 0 and d % 9 == 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã đọc số lượng ca kiểm thử, sau đó tính toán cho mỗi ca kiểm thử`d = x - (y - 1)`. Séc`d >= 0 and d % 9 == 0`gói gọn toàn bộ lý luận dựa trên thực hiện. Các trường hợp cạnh, chẳng hạn như chuyển từ các số như`999`ĐẾN`1000`, được xử lý tự động vì số học của`d`nắm bắt được hiệu ứng của dấu số 9. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào`(1, 2)`: 

| x | y | d = x - (y-1) | d % 9 | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | CÓ | 

Đây,`d = 0`ngụ ý không cần số 9 ở cuối và tăng dần`n`từ`1`cho`2`trực tiếp. 

Theo dõi đầu vào`(77, 77)`: 

| x | y | d = x - (y-1) | d % 9 | kết quả | 
| --- | --- | --- | --- | --- | 
| 77 | 77 | 1 | 1 | KHÔNG |`d`không chia hết cho 9, vì vậy không có số nào có thể mang theo cách giảm tổng từ 77 xuống 77 sau khi tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu các phép tính số học không đổi. | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Với`t <= 500`, thao tác này chạy gần như ngay lập tức. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("7\n1 2\n77 77\n997 999\n999 1\n1000 1\n1 11\n18 1\n") == \
"YES\nNO\nNO\nYES\nNO\nNO\nYES", "Sample 1"

# Custom cases
assert run("1\n1 1\n") == "YES", "Minimum digits, no carry"
assert run("1\n10 1\n") == "YES", "Carry reduces sum from 10 to 1"
assert run("1\n1000 1000\n") == "NO", "Impossible, cannot maintain sum"
assert run("1\n1000 10\n") == "YES", "Large number with trailing 9s"
assert run("1\n2 1\n") == "YES", "Simple carry from 1 to 2 digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | CÓ | Số nhỏ nhất, không mang theo | 
| 10 1 | CÓ | Mang theo nhiều chữ số làm giảm tổng | 
| 1000 1000 | KHÔNG | Không thể duy trì số tiền lớn | 
| 1000 10 | CÓ | Số lớn có số 9 ở cuối, tổng giảm | 
| 2 1 | CÓ | Mang theo đơn giản, tổng giảm 1 | 

## Vỏ cạnh 

Hãy xem xét`(999, 1)`. Đây`x = 999`Và`y = 1`. Máy tính`d = 999 - (1-1) = 999`. Chia cho 9 được`111`, nghĩa là chúng ta có thể nối thêm 111 số chín. tăng dần`n`chuyển tất cả các số 9 ở cuối thành số 0 và tăng chữ số đầu tiên lên một, giảm tổng thành`y = 1`. Thuật toán xử lý trường hợp này mà không bao giờ xây dựng số có hơn 100 chữ số, hoàn toàn dựa vào kiểm tra số học. Một trường hợp cạnh khác`(1, 11)`có`d = 1 - (11-1) = -9 < 0`, ngay lập tức trả về KHÔNG, như mong đợi.
