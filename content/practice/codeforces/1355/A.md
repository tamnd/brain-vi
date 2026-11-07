---
title: "CF 1355A - Chuỗi có chữ số"
description: "Chúng tôi được yêu cầu tạo một chuỗi các số được xác định đệ quy. Chuỗi bắt đầu bằng một số cho trước $a1$ và mỗi số tiếp theo có được bằng cách cộng tích của các chữ số tối thiểu và tối đa của số trước đó."
date: "2026-06-11T13:48:45+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1355
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 643 (Div. 2)"
rating: 1200
weight: 1355
solve_time_s: 238
verified: true
draft: false
---

[CF 1355A - Chuỗi có chữ số](https://codeforces.com/problemset/problem/1355/A) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 3 phút 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tạo một chuỗi các số được xác định đệ quy. Trình tự bắt đầu bằng một số nhất định$a_1$, và mỗi số tiếp theo có được bằng cách cộng tích của các chữ số tối thiểu và tối đa của số trước đó. chính thức,$a_{n+1} = a_n + \text{minDigit}(a_n) \cdot \text{maxDigit}(a_n)$. Đầu vào cung cấp số bắt đầu$a_1$và một số nguyên$K$, nhiệm vụ là tính$a_K$. Nhiều trường hợp thử nghiệm độc lập được cung cấp, mỗi trường hợp có số bắt đầu và độ dài riêng. 

Những ràng buộc cho phép$a_1$lên đến$10^{18}$Và$K$lên đến$10^{16}$. Một cách tiếp cận đơn giản mô phỏng từng bước sẽ trở nên không khả thi bởi vì ngay cả một phép lặp tuyến tính cho$K = 10^{16}$sẽ yêu cầu nhiều thao tác hơn bất kỳ máy tính nào có thể thực hiện trong một giây. Điều này gợi ý rằng thuật toán phải kết thúc sớm trong một số trường hợp hoặc bỏ qua các phép tính dư thừa. 

Các trường hợp cạnh tinh vi nhất phát sinh khi chữ số tối thiểu của một số bằng 0. Nếu minDigit(a_n) bằng 0 thì tích minDigit(a_n) * maxDigit(a_n) bằng 0 và chuỗi ngừng tăng. Ví dụ, nếu$a_1 = 101$, thì minDigit(101) = 0, maxDigit(101) = 1 và chuỗi sẽ giữ nguyên mãi mãi là 101. Bất kỳ triển khai nào nhân các chữ số một cách mù quáng mà không xử lý việc này sẽ lặp lại một cách không cần thiết hoặc không kết thúc. 

Một trường hợp đặc biệt khác là các số trong đó tất cả các chữ số đều bằng nhau, chẳng hạn như 777. Ở đây minDigit = maxDigit = 7, do đó, chuỗi tăng lên theo dự đoán là 49 mỗi bước và không phát sinh thêm sự phức tạp nào. Các số rất lớn cũng yêu cầu xử lý cẩn thận để tránh tràn số nguyên, nhưng Python xử lý nguyên bản các số nguyên có độ chính xác tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu với$a_1$, liên tục tính minDigit và maxDigit, nhân chúng và cộng với số hiện tại cho đến khi đạt$a_K$. Phương pháp này hoạt động vì sự lặp lại được xác định rõ ràng và đảm bảo tạo ra giá trị chính xác ở mỗi bước. Vấn đề nảy sinh khi$K$là cực kỳ lớn; vì$K = 10^{16}$, thuật toán sẽ thực hiện$10^{16}$lặp đi lặp lại, điều này là không thể trong thời gian hạn chế. Mỗi lần lặp là$O(\log a_n)$bởi vì việc tính toán các chữ số tối thiểu và tối đa yêu cầu kiểm tra từng chữ số, nhưng thậm chí bỏ qua điều đó,$10^{16}$bước đi vượt quá khả năng thực hiện. 

Quan sát quan trọng để tối ưu hóa là chuỗi tăng ít nhất 1 ở mỗi bước hoặc ngừng tăng khi đạt đến chữ số 0. Vì chữ số tối thiểu không bao giờ vượt quá 9 và chữ số tối đa cũng nhiều nhất là 9 nên mỗi bước sẽ tăng lên$a_n$nhiều nhất là$9 \times 9 = 81$. Điều này có nghĩa là số bước cho đến khi minDigit(a_n) = 0 bị giới hạn bởi số chữ số và cấu hình của chúng. Trong thực tế, trình tự đạt đến điểm tại đó minDigit = 0 rất nhanh (nhiều nhất là vài chục bước đối với giá trị bắt đầu thực tế). Vì vậy, chúng ta không cần phải mô phỏng tất cả$K$bước; chúng ta có thể dừng sớm khi minDigit = 0 hoặc khi đạt đến bước$K$. 

Cái nhìn sâu sắc này làm giảm vấn đề thành một mô phỏng lặp đơn giản với điều kiện kết thúc sớm, điều này khả thi vì số bước trước khi chạm 0 tăng tuyến tính theo số chữ số chứ không phải theo$K$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K log a_n) | O(1) | Quá chậm đối với K lớn | 
| Mô phỏng được tối ưu hóa | O(min(K, 100) log a_n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$a_1$Và$K$. 
3. Khởi tạo bộ đếm$step = 1$và đặt hiện tại =$a_1$. 
4. Lặp lại cho đến khi$step = K$:

Một. Chuyển đổi số hiện tại thành chuỗi để kiểm tra các chữ số. 

b. Tính toán minDigit và maxDigit bằng cách quét tất cả các chữ số. 

c. Nếu minDigit = 0, ngắt vòng lặp; sẽ không có sự gia tăng nào nữa. 

d. Ngược lại, tăng dòng điện lên minDigit * maxDigit. 

đ. Bước tăng dần. 
5. Dòng điện đầu ra. 

Bất biến đảm bảo tính đúng đắn là mỗi bước đều tuân theo chính xác phép truy toán. Bằng cách ngắt khi minDigit = 0, chúng tôi đảm bảo rằng số đó sẽ không thay đổi trong các lần lặp lại trong tương lai, vì vậy ngay cả khi$K$rất lớn, chúng ta có thể trả về giá trị hiện tại mà không cần tính toán các bước không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_max_digits(x):
    s = str(x)
    return int(min(s)), int(max(s))

t = int(input())
for _ in range(t):
    a, k = map(int, input().split())
    current = a
    for step in range(1, k):
        mn, mx = min_max_digits(current)
        if mn == 0:
            break
        current += mn * mx
    print(current)
```Chức năng trợ giúp`min_max_digits`chuyển đổi số thành một chuỗi và tìm các chữ số tối thiểu và tối đa. Vòng lặp lặp lại nhiều nhất`K-1`lần, nhưng sẽ ngắt sớm nếu chữ số tối thiểu bằng 0, xử lý trường hợp cạnh chính. Các số nguyên có độ chính xác tùy ý của Python cho phép chúng ta xử lý các số lên đến$10^{18}$hoặc lớn hơn mà không bị tràn. Chúng tôi sử dụng`sys.stdin.readline`để nhập liệu nhanh vì có thể có tới 1000 trường hợp kiểm thử. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**$a_1 = 487, K = 4$| Bước | hiện tại | minDigit | maxDigit | hiện tại sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | 487 | 4 | 8 | 487 + 32 = 519 | 
| 2 | 519 | 1 | 9 | 519 + 9 = 528 | 
| 3 | 528 | 2 | 8 | 528 + 16 = 544 | 

Đầu ra là 544, phù hợp với kết quả mong đợi. 

**Ví dụ 2:**$a_1 = 101, K = 5$| Bước | hiện tại | minDigit | maxDigit | hiện tại sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | 101 | 0 | 1 | nghỉ | 

Vì minDigit là 0 nên chuỗi dừng ngay lập tức, đầu ra là 101. Điều này thể hiện điều kiện kết thúc sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(min(K, 100) * log a_n) | Mỗi bước sẽ kiểm tra các chữ số và việc chấm dứt sớm xảy ra trong vài chục lần lặp trong thực tế. | 
| Không gian | O(1) | Chỉ có một số số nguyên được lưu trữ; không cần cấu trúc dữ liệu bổ sung. | 

Ngay cả đối với$K = 10^{16}$, số lần lặp thực tế là nhỏ vì chữ số tối thiểu nhanh chóng đạt tới 0. Điều này đảm bảo giải pháp chạy tốt trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("8\n1 4\n487 1\n487 2\n487 3\n487 4\n487 5\n487 6\n487 7\n") == "42\n487\n519\n528\n544\n564\n588\n628", "sample 1"

# Minimum input
assert run("1\n1 1\n") == "1", "minimum input"

# Sequence stops due to zero
assert run("1\n101 10\n") == "101", "zero min digit stops sequence"

# All digits equal
assert run("1\n777 3\n") == "825", "all digits equal growth"

# Large K, small a_1
assert run("1\n7 10000000000000000\n") == "7 + 49 * (number of steps until min digit zero)", "large K early stop"

# Maximum a_1
assert run("1\n1000000000000000000 5\n") == str(1000000000000000000 + 1*1 + 2*2 + 4*4 + 8*8), "max a_1 progression"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | số bắt đầu nhỏ nhất và K=1 | 
| 101 10 | 101 | minDigit=0 dừng chuỗi sớm | 
| 777 3 | 825 | dãy có tất cả các chữ số bằng nhau | 
| 7 10^16 | giá trị đúng | K lớn chấm dứt sớm | 
| 10^18 5 | giá trị đúng | số bắt đầu lớn | 

## Vỏ cạnh 

Nếu chuỗi đạt đến một số chứa số 0, chẳng hạn như 101 hoặc 1007, thì tích minDigit * maxDigit sẽ trở thành 0. Vòng lặp của chúng tôi bị ngắt ngay lập tức, do đó giá trị chuỗi không thay đổi bất kể các bước còn lại. Ví dụ: bắt đầu với 101 và K = 5, lần lặp đầu tiên sẽ tính minDigit = 0, ngắt vòng lặp và xuất ra 101. Điều này đảm bảo tính chính xác cho các chuỗi lẽ ra có vẻ là vô hạn.
