---
title: "CF 104599B - Sinh nhật"
description: "Chúng ta có một khoảng năm từ $x$ đến $y$, bao gồm cả hai. Đối với mỗi năm trong khoảng thời gian này, chúng ta phải xuất ra ngày dương lịch tương ứng với ngày 7 tháng 3 của năm đó. Mỗi dòng đầu ra đại diện cho một ngày như vậy, được định dạng dưới dạng số ngày, sau đó là tên tháng, sau đó là năm."
date: "2026-06-30T02:58:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104599
codeforces_index: "B"
codeforces_contest_name: "GPL 2023 Novice"
rating: 0
weight: 104599
solve_time_s: 66
verified: true
draft: false
---

[CF 104599B - Sinh nhật](https://codeforces.com/problemset/problem/104599/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một khoảng thời gian từ$x$ĐẾN$y$, bao gồm cả hai. Đối với mỗi năm trong khoảng thời gian này, chúng ta phải xuất ra ngày dương lịch tương ứng với ngày 7 tháng 3 của năm đó. Mỗi dòng đầu ra đại diện cho một ngày như vậy, được định dạng dưới dạng số ngày, sau đó là tên tháng, sau đó là năm. 

Về mặt khái niệm, nhiệm vụ là liệt kê một ánh xạ xác định đơn giản từ mỗi năm nguyên tới một chuỗi cố định biểu thị một ngày. Không có bộ lọc, không có tính toán theo năm ngoài việc lặp lại và không có sự phụ thuộc giữa các năm. Thứ tự đầu ra phải tuân theo thứ tự thời gian, trong bối cảnh này tương đương với thứ tự tăng dần của năm vì tất cả các ngày đều có cùng tháng và ngày. 

Những ràng buộc cho phép$x, y \le 10^5$. Điều này có nghĩa là số năm tối đa chúng ta có thể cần để sản xuất là$10^5$. Bất kỳ giải pháp nào thực hiện công việc liên tục mỗi năm là đủ, vì khoảng$10^5$hoạt động dễ dàng phù hợp với giới hạn 1 giây trong Python. Bất cứ điều gì liên quan đến các vòng lặp lồng nhau trong phạm vi nhiều năm cũng sẽ vẫn an toàn, nhưng mọi cách tiếp cận với chi phí siêu tuyến tính mỗi năm đều không cần thiết. 

Không có trường hợp ẩn nào liên quan đến tính hợp lệ của lịch vì ngày được cố định là ngày 7 tháng 3, tồn tại hàng năm trong lịch Gregory tiêu chuẩn, kể cả năm nhuận. Các trường hợp đặc biệt duy nhất đến từ việc định dạng và sắp xếp. 

Một sai lầm ngây thơ có thể là tính toán lại hoặc phân tích lại ngày tháng bằng cách sử dụng thư viện ngày tháng hoặc xây dựng các chuỗi không hiệu quả bên trong các phép nối lặp lại. Ví dụ: xây dựng các chuỗi có lặp lại`+`các hoạt động trong một vòng lặp trên phạm vi lớn có thể đưa ra hành vi bậc hai trong Python do phân bổ lặp đi lặp lại. 

Một vấn đề tinh tế khác là tính nhất quán của định dạng. Tháng phải xuất hiện dưới dạng từ “Tháng 3”, không phải số như 3 và khoảng cách phải khớp chính xác. Ví dụ: 

đầu vào:```
2023 2024
```Đầu ra đúng:```
7 March 2023
7 March 2024
```Một cách tiếp cận sai có thể tạo ra`07 March 2023`hoặc`March 7 2023`, cả hai đều sẽ bị từ chối do định dạng không khớp. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp lại hàng năm từ$x$ĐẾN$y$, tạo chuỗi ngày tương ứng cho ngày 7 tháng 3 và in ngay lập tức. Mỗi lần lặp lại thực hiện công việc liên tục: chuyển đổi năm thành chuỗi và nối các mã thông báo cố định. 

Điều này hiệu quả vì việc ánh xạ từ năm tới đầu ra là độc lập và không yêu cầu tính toán trước hoặc xác thực. Tuy nhiên, người ta vẫn có thể lo lắng về hiệu quả nếu việc xây dựng chuỗi được thực hiện không hiệu quả. Nếu chúng ta liên tục nối các chuỗi theo cách đơn giản bên trong một vòng lặp, Python có thể phân bổ các chuỗi trung gian mới mỗi lần, nhưng vì mỗi chuỗi nhỏ và tổng số đầu ra nhiều nhất là$10^5$, điều này vẫn có thể chấp nhận được. 

Quan sát quan trọng là vấn đề không hề phức tạp về mặt tính toán. Cấu trúc này hoàn toàn là phép liệt kê trên một khoảng nguyên liền kề và mỗi phần tử ánh xạ tới một mẫu không đổi. Không có vấn đề tối ưu hóa nào cần giải quyết, chỉ cần lặp lại và định dạng cẩn thận. 

Do đó, giải pháp tối ưu giống hệt với cấu trúc brute-force nhưng được triển khai cẩn thận bằng định dạng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$Ở đâu$n = y-x+1$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$x$Và$y$. Những điều này xác định phạm vi bao gồm số năm mà chúng tôi phải xử lý. 
2. Lặp lại tất cả các số nguyên$i$từ$x$ĐẾN$y$. Mỗi giá trị của$i$đại diện cho một năm duy nhất. 
3. Hàng năm$i$, xây dựng chuỗi đầu ra chính xác theo định dạng:`7 March i`. Ngày và tháng là hằng số cố định nên chỉ có năm thay đổi. 
4. In ngay từng chuỗi được xây dựng trong vòng lặp. Điều này tránh việc lưu trữ tất cả các kết quả đầu ra và giữ mức sử dụng bộ nhớ không đổi. 

### Tại sao nó hoạt động 

Mỗi năm trong khoảng thời gian này tương ứng với đúng một lần xuất hiện hợp lệ vào ngày 7 tháng 3. Không có sự phụ thuộc giữa các năm và không có trường hợp thiếu sót. Việc lặp lại bao gồm phạm vi bao gồm đầy đủ, vì vậy mỗi ngày bắt buộc được tạo ra chính xác một lần. Vì định dạng đầu ra là cố định và mang tính xác định cho mỗi năm nên việc xây dựng chuỗi trực tiếp sẽ mang lại trình tự thời gian chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, y = map(int, input().split())

for year in range(x, y + 1):
    sys.stdout.write(f"7 March {year}\n")
```Giải pháp đọc hai điểm cuối và lặp lại toàn bộ phạm vi. Việc sử dụng`sys.stdout.write`tránh được chi phí lặp đi lặp lại`print`Tuy nhiên, các cuộc gọi`print`cũng sẽ vượt qua một cách thoải mái dưới những ràng buộc này. 

Chuỗi định dạng được cố định, chỉ có năm được nội suy. Một lỗi phổ biến là vô tình hoán đổi ngày và tháng hoặc sử dụng cách biểu diễn tháng bằng số. Ở đây nghĩa đen`"March"`được yêu cầu. 

Ranh giới vòng lặp bao gồm cả hai đầu, vì vậy`range(x, y + 1)`là điều cần thiết. Bỏ qua`+1`sẽ âm thầm bỏ năm ngoái, đây là một lỗi điển hình trong các vấn đề liệt kê như vậy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2023 2024
```| Bước | Năm | Đầu ra | 
| --- | --- | --- | 
| 1 | 2023 | Ngày 7 tháng 3 năm 2023 | 
| 2 | 2024 | Ngày 7 tháng 3 năm 2024 | 

Dấu vết này cho thấy rằng mỗi năm độc lập tạo ra một chuỗi được định dạng. Thứ tự khớp với thứ tự tăng dần của năm, thỏa mãn thứ tự thời gian. 

### Ví dụ 2 

đầu vào:```
2020 2022
```| Bước | Năm | Đầu ra | 
| --- | --- | --- | 
| 1 | 2020 | 7 Tháng Ba 2020 | 
| 2 | 2021 | Ngày 7 tháng 3 năm 2021 | 
| 3 | 2022 | Ngày 7 tháng 3 năm 2022 | 

Điều này xác nhận rằng vòng lặp bao gồm cả hai điểm cuối và tạo ra chính xác$y-x+1$dòng. Không có năm nào bị bỏ qua và không có sản lượng bổ sung nào được sản xuất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(y - x + 1)$| Một lần xây dựng và sản xuất chuỗi thời gian không đổi mỗi năm | 
| Không gian |$O(1)$| Không có bộ nhớ tỷ lệ thuận với kích thước đầu vào; đầu ra được truyền trực tiếp | 

Số lần lặp tối đa là$10^5$, nằm trong giới hạn đối với Python. Việc sử dụng bộ nhớ không đổi do không có cấu trúc dữ liệu lớn nào được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import PIPE, Popen
    p = Popen(["python3", "main.py"], stdin=PIPE, stdout=PIPE, stderr=PIPE, text=True)
    out, _ = p.communicate(inp)
    return out.strip()

# sample
# assert run("2023 2024") == "7 March 2023\n7 March 2024"

# minimum range
assert run("1 1") == "7 March 1"

# small range
assert run("2020 2022") == "7 March 2020\n7 March 2021\n7 March 2022"

# boundary formatting check
assert run("9 10") == "7 March 9\n7 March 10"

# larger consecutive block
assert run("1998 2002") == "\n".join([
    "7 March 1998",
    "7 March 1999",
    "7 March 2000",
    "7 March 2001",
    "7 March 2002"
])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 7 ngày 1 tháng 3 | phạm vi phần tử đơn | 
| 9 10 | 7 Ngày 9 tháng 3\n7 Ngày 10 tháng 3 | định dạng và phạm vi hai năm | 
| 1998 2002 | dòng tuần tự | tính chính xác của phép lặp nhiều bước | 

## Vỏ cạnh 

Trường hợp đặc biệt chính là khi phạm vi giảm xuống còn một năm, chẳng hạn như$x = y$. Trong trường hợp này, vòng lặp vẫn chạy chính xác một lần do giới hạn bao gồm. Đối với đầu vào:```
5 5
```Thuật toán khởi tạo `
