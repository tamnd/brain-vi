---
title: "CF 106035E - Nika và củ cải"
description: "Chúng ta được cấp một nhóm người trợ giúp, mỗi người có một giá trị sức mạnh cố định. Chúng ta được phép sắp xếp lại chúng theo thứ tự bất kỳ. Khi xếp vào một hàng, sự đóng góp của người trợ giúp phụ thuộc vào vị trí của người đó: nếu người trợ giúp có sức mạnh $a$ đứng ở vị trí $i$ (tính từ 1 ở cuối…"
date: "2026-06-25T12:55:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106035
codeforces_index: "E"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2024"
rating: 0
weight: 106035
solve_time_s: 35
verified: true
draft: false
---

[CF 106035E - Nika và củ cải](https://codeforces.com/problemset/problem/106035/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một nhóm người trợ giúp, mỗi người có một giá trị sức mạnh cố định. Chúng ta được phép sắp xếp lại chúng theo thứ tự bất kỳ. Sau khi được xếp vào hàng, sự đóng góp của người trợ giúp phụ thuộc vào vị trí của người đó: nếu người trợ giúp có sức mạnh$a$đứng ở vị trí$i$(tính từ 1 ở đầu sợi dây), nó góp phần$a / i$vào tổng lực kéo. Tổng lực là tổng của tất cả những đóng góp này. Chúng tôi muốn biết liệu có tồn tại một trật tự của những người trợ giúp sao cho tổng lực này ít nhất là$x$. 

Khó khăn chính là cùng một bộ số cho ra các kết quả khác nhau tùy theo vị trí của chúng, và các vị trí sau này “có giá trị” hơn vì chúng chia cho các chỉ số nhỏ hơn. Vì vậy, vấn đề cơ bản là gán các giá trị lớn cho mẫu số nhỏ theo cách tối đa hóa tổng có trọng số. 

Kích thước đầu vào đủ nhỏ để$O(n^2)$hoặc$O(n \log n)$giải pháp là khả thi. Với$n \le 1000$, thậm chí vài triệu phép tính cũng được, nhưng bất kỳ thao tác khối nào hoặc liên quan đến việc tính toán lại toàn bộ số tiền lặp đi lặp lại sau mỗi lần hoán đổi sẽ là quá chậm. 

Một cách giải thích ngây thơ là thử tất cả các hoán vị, nhưng$n!$phát triển quá nhanh ngay cả đối với$n=15$, vì vậy chúng ta phải khai thác cấu trúc trong mục tiêu. 

Một vài trường hợp đặc biệt bộc lộ những lỗi phổ biến. Nếu tất cả các điểm mạnh đều bằng nhau thì thứ tự không quan trọng, do đó câu trả lời chỉ phụ thuộc vào việc liệu$a \cdot (1 + 1/2 + \dots + 1/n)$đạt tới$x$. Ví dụ, với$n=3$,$a=[2,2,2]$, Và$x=10$, giá trị lớn nhất có thể là$2 \cdot (1 + 1/2 + 1/3) = 3.66$, vì vậy câu trả lời là KHÔNG. Một kẻ tham lam giả định không chính xác bất kỳ thứ tự nào sẽ hoạt động ở đây. 

Một trường hợp góc khác là khi tồn tại một giá trị cực lớn giữa nhiều giá trị nhỏ. Nếu giá trị đó được đặt ở vị trí 1 thì nó được tính đầy đủ, nhưng việc đặt nó sau sẽ làm giảm đáng kể tác động của nó. Bất kỳ chiến lược nào không ưu tiên các vị trí sớm sẽ đánh giá thấp câu trả lời. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi hoán vị của các trợ giúp, tính tổng trọng số thu được và lấy giá trị tối đa. Điều này đúng vì nó đánh giá tất cả các phép gán giá trị có thể có cho các vị trí. Vấn đề là nó đòi hỏi$n!$hoán vị, và với mỗi hoán vị tính tổng là$O(n)$, cho$O(n \cdot n!)$, điều này đã trở thành không thể ở mức nhỏ$n$. 

Cấu trúc của biểu thức cho thấy lý do tại sao việc sắp xếp lại trở nên phù hợp. Mỗi vị trí$i$áp dụng hệ số nhân cố định$1/i$, và các số nhân này giảm dần khi$i$tăng lên. Vì vậy, bài toán trở thành một bài toán gán cổ điển: ghép các số lớn với trọng số lớn. Từ$1/1 \ge 1/2 \ge \dots \ge 1/n$, bất đẳng thức sắp xếp lại áp dụng trực tiếp. Nó cho chúng ta biết rằng số tiền tối đa đạt được bằng cách ghép cường độ lớn nhất với hệ số lớn nhất, cường độ lớn thứ hai với hệ số lớn thứ hai, v.v. 

Vì vậy, thay vì khám phá các hoán vị, chúng tôi sắp xếp các điểm mạnh theo thứ tự giảm dần và trực tiếp tính toán cách sắp xếp tốt nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n \cdot n!)$|$O(n)$| Quá chậm | 
| Sắp xếp + phân công tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng điểm mạnh theo thứ tự giảm dần. Điều này đảm bảo rằng sức mạnh sẵn có lớn nhất luôn được gán cho vị trí còn lại có giá trị nhất. 
2. Tính toán trước hoặc lặp lại duy trì tổng hoạt động của biểu mẫu$\sum a_i / i$, trong đó chỉ số$i$tăng từ 1 đến$n$. Thứ tự lặp bây giờ tương ứng với phép gán tối ưu, do đó vị trí$i$được sửa khi mảng được sắp xếp. 
3. Tích lũy tổng có trọng số bằng cách sử dụng số học dấu phẩy động hoặc số học có độ chính xác cao nếu cần, vì các giá trị có thể lớn. 
4. So sánh tổng cuối cùng với$x$. Nếu ít nhất là$x$, xuất ra CÓ, ngược lại xuất ra KHÔNG. 

Bước không rõ ràng duy nhất là tại sao việc sắp xếp theo thứ tự giảm dần lại đúng. Đây chính xác là một ví dụ về việc tối đa hóa tích số chấm với chuỗi trọng số giảm dần cố định. 

### Tại sao nó hoạt động 

Trọng lượng$1, 1/2, \dots, 1/n$tạo thành một dãy giảm nghiêm ngặt. Bất kỳ sự đảo ngược nào trong phép gán, nghĩa là cường độ nhỏ hơn được đặt trước cường độ lớn hơn, chỉ có thể làm giảm tổng vì việc hoán đổi chúng sẽ di chuyển giá trị lớn hơn đến vị trí có hệ số lớn hơn. Việc loại bỏ nhiều lần các phép đảo ngược như vậy sẽ biến đổi mọi sắp xếp thành sắp xếp đã được sắp xếp mà không làm giảm giá trị, điều này đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort(reverse=True)
    
    total = 0.0
    for i, val in enumerate(a, start=1):
        total += val / i
    
    print("YES" if total >= x else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các điểm mạnh theo thứ tự giảm dần, giúp cố định việc gán giá trị tối ưu cho các vị trí. Sau đó, vòng lặp sẽ tính tổng có trọng số trực tiếp theo cấu trúc tối ưu dẫn xuất. 

Một chi tiết triển khai tinh tế là việc sử dụng số học dấu phẩy động. Từ$a_i$có thể lên đến$10^{12}$Và$n$lên đến$1000$, tổng vẫn nằm trong giới hạn chính xác an toàn đối với số float của Python. Nếu điều này chặt chẽ hơn, có thể cần một cách tiếp cận hợp lý hoặc mở rộng quy mô, nhưng ở đây độ chính xác gấp đôi là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 8
5 4 3 2 1
```Điểm mạnh được sắp xếp vẫn còn`[5, 4, 3, 2, 1]`. 

| tôi | giá trị | đóng góp | tổng số tiền chạy | 
| --- | --- | --- | --- | 
| 1 | 5 | 5/1 = 5,0 | 5.0 | 
| 2 | 4 | 2.0 | 7.0 | 
| 3 | 3 | 1.0 | 8.0 | 
| 4 | 2 | 0,5 | 8,5 | 
| 5 | 1 | 0,2 | 8,7 | 

Tổng cuối cùng là 8,7, ít nhất là 8, vì vậy câu trả lời là CÓ. 

Dấu vết này cho thấy các vị trí đầu chiếm ưu thế như thế nào trong tổng và tại sao việc đặt các giá trị lớn lên đầu tiên là điều cần thiết. 

### Ví dụ 2 

đầu vào:```
6 20
10 4 2 4 2 8
```Đã sắp xếp:`[10, 8, 4, 4, 2, 2]`. 

| tôi | giá trị | đóng góp | tổng số tiền chạy | 
| --- | --- | --- | --- | 
| 1 | 10 | 10.0 | 10.0 | 
| 2 | 8 | 4.0 | 14.0 | 
| 3 | 4 | 1.33 | 15.33 | 
| 4 | 4 | 1.0 | 16.33 | 
| 5 | 2 | 0,4 | 16.73 | 
| 6 | 2 | 0,33 | 17.06 | 

Tổng cuối cùng xấp xỉ 17,06, nhỏ hơn 20, vì vậy câu trả lời là KHÔNG. 

Điều này chứng tỏ rằng ngay cả thứ tự tối ưu cũng không thể bù đắp được khi tổng khối lượng có trọng số sẵn có không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; tổng là tuyến tính | 
| Không gian |$O(n)$| Lưu trữ cho mảng đầu vào | 

Những hạn chế$n \le 1000$làm cho việc sắp xếp trở nên tầm thường về mặt thời gian và đường truyền tuyến tính trên mảng là không đáng kể. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, x = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    a.sort(reverse=True)

    total = 0.0
    for i, v in enumerate(a, 1):
        total += v / i

    return "YES\n" if total >= x else "NO\n"

# provided samples
assert run("5 8\n5 4 3 2 1\n") == "YES\n"
assert run("6 20\n10 4 2 4 2 8\n") == "NO\n"

# custom cases
assert run("1 5\n10\n") == "YES\n", "single element always direct"
assert run("3 100\n1 1 1\n") == "NO\n", "all equal small values"
assert run("4 10\n10 1 1 1\n") == "YES\n", "dominant first position"
assert run("5 0\n5 4 3 2 1\n") == "YES\n", "zero threshold"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | trường hợp cơ sở đúng đắn | 
| tất cả những cái | KHÔNG | trường hợp cốt liệu yếu | 
| một giá trị lớn | CÓ | tầm quan trọng của vị trí số 1 | 
| ngưỡng 0 | CÓ | điều kiện biên | 

## Vỏ cạnh 

Khi nào$n = 1$, thuật toán giảm xuống để kiểm tra xem giá trị đơn có đáp ứng hay vượt quá$x$. Việc sắp xếp không liên quan và vòng lặp tính toán chính xác$a_1 / 1$, phù hợp với cấu hình duy nhất có thể. 

Khi tất cả các giá trị giống hệt nhau, hãy nói$a_i = k$, việc sắp xếp không làm thay đổi mảng. Thuật toán tính toán$k \cdot H_n$, Ở đâu$H_n$là số hài hòa. Bất kỳ sự sắp xếp lại nào cũng mang lại kết quả tương tự, vì vậy giải pháp tham lam là chính xác. 

Khi có một giá trị rất lớn, thứ tự sắp xếp sẽ đặt nó ở vị trí 1. Phần đóng góp trở thành tối đa. Bất kỳ sự sắp xếp thay thế nào cũng sẽ đặt nó ở vị trí thấp hơn và giảm tổng một cách nghiêm ngặt, điều mà thuật toán tránh được khi xây dựng.
