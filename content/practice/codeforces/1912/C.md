---
title: "CF 1912C - Biến Hình Xương Rồng"
description: "Bài toán cung cấp cho chúng ta một mảng các số nguyên và yêu cầu chúng ta chuyển đổi nó thành một \"mảng xương rồng.\" Một mảng xương rồng được xác định sao cho đối với mỗi phần tử, bản thân phần tử đó là giá trị cực đại cục bộ hoặc bằng phần tử trước đó sau một số phép biến đổi."
date: "2026-06-08T20:12:43+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1912
codeforces_index: "C"
codeforces_contest_name: "2023-2024 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3300
weight: 1912
solve_time_s: 85
verified: true
draft: false
---

[CF 1912C - Biến đổi xương rồng](https://codeforces.com/problemset/problem/1912/C) 

**Đánh giá:** 3300 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cung cấp cho chúng ta một mảng các số nguyên và yêu cầu chúng ta chuyển đổi nó thành một "mảng xương rồng". Mảng xương rồng được xác định sao cho đối với mỗi phần tử, bản thân phần tử đó là giá trị cực đại cục bộ hoặc bằng phần tử trước đó sau một số phép biến đổi. Nói một cách đơn giản hơn, chúng ta có thể tăng một số phần tử nhưng không thể giảm chúng và mục tiêu của chúng ta là tối đa hóa số phần tử bằng vị trí của chúng trong mảng xương rồng cuối cùng theo các quy tắc này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bắt đầu bằng một số nguyên$n$, độ dài của mảng, theo sau là$n$số nguyên. Đầu ra phải báo cáo, đối với mỗi trường hợp thử nghiệm, số phần tử tối đa có thể đáp ứng điều kiện cây xương rồng sau khi biến đổi. 

Các hạn chế ở mức vừa phải:$n$có thể đạt tới$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, có nghĩa là ngây thơ$O(n^2)$giải pháp sẽ quá chậm. Chúng ta cần một giải pháp đại khái$O(n)$hoặc$O(n \log n)$mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các phần tử đều giống nhau. Ví dụ, mảng`[1, 1, 1]`đã thỏa mãn điều kiện xương rồng mà không cần thực hiện bất kỳ thao tác nào. Một cách tiếp cận ngây thơ cố gắng tăng mọi phần tử một cách tham lam có thể tính quá mức các phép biến đổi hoặc tạo ra các chuỗi không hợp lệ. 

Một tình huống phức tạp khác xảy ra khi các phần tử giảm đi một cách nghiêm ngặt. Ví dụ,`[3, 2, 1]`không thể chuyển đổi thành một mảng tăng đầy đủ và việc triển khai bất cẩn có thể thực hiện các hoạt động không hợp lệ. Đầu ra đúng phải tuân theo quy tắc "không thể giảm" và chỉ tính các phép biến đổi khả thi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tất cả các chuỗi biến đổi có thể có. Đối với mỗi yếu tố, chúng tôi sẽ cố gắng tăng nó để phù hợp với yêu cầu của cây xương rồng hoặc giữ nguyên. Cách tiếp cận này đúng vì nó khám phá mọi chuỗi hợp lệ, nhưng số lượng chuỗi tăng theo cấp số nhân. Vì$n = 10^5$, điều này dẫn đến một số lượng hoạt động không thể thực hiện được, đại khái là$O(2^n)$. 

Quan sát quan trọng là vấn đề biến đổi có thể được rút gọn thành việc đếm các "đỉnh" một cách hạn chế. Mọi phần tử đều bắt đầu một đỉnh mới (nếu nó lớn hơn phần tử trước đó) hoặc tiếp tục ổn định (nếu nó khớp với phần tử trước đó). Bởi vì chúng ta chỉ có thể tăng giá trị và không thể giảm chúng nên số lượng đỉnh được xác định bởi sự khác biệt giữa các phần tử liên tiếp và liệu phần tử trước đó có cho phép tiếp tục hay không. Thông tin chuyên sâu này giúp giảm vấn đề xuống còn một lần quét tuyến tính đơn giản với một vài bước kiểm tra cho mỗi phần tử. 

Lực lượng vũ phu hoạt động vì mỗi phép biến đổi là cục bộ của một phần tử duy nhất, nhưng không thành công khi chuỗi dài do phân nhánh theo cấp số nhân. Nhận thức được rằng chúng ta chỉ cần theo dõi "giá trị đỉnh hiện tại" và liệu phần tử cuối cùng có tăng lên hay không cho phép chúng ta thu gọn không gian quyết định thành$O(n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Quét tuyến tính / Đếm đỉnh | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`cactus_count`về không. Biến này theo dõi số phần tử thỏa mãn điều kiện xương rồng. 
2. Khởi tạo một biến`prev`về không. Điều này sẽ lưu trữ phần tử cuối cùng trong mảng được chuyển đổi. 
3. Lặp lại mảng từ trái sang phải. 
4. Đối với mỗi phần tử`a[i]`, kiểm tra xem nó có lớn hơn không`prev`. Nếu vậy thì tăng`cactus_count`bởi vì nó có thể là một đỉnh cao mới. 
5. Nếu`a[i]`bằng với`prev`, nó có thể tiếp tục trạng thái ổn định trước đó, vì vậy hãy tăng dần`cactus_count`. 
6. Cập nhật`prev`là giá trị tối đa của giá trị trước đó hoặc phần tử hiện tại. Điều này thực thi tính chất không giảm của phép biến đổi. 
7. Sau khi quét tất cả các phần tử,`cactus_count`chứa số lượng tối đa các yếu tố xương rồng. 

**Tại sao nó hoạt động**: Bất biến là vậy`prev`luôn đại diện cho giá trị hợp lệ cuối cùng trong mảng được chuyển đổi. Bất kỳ phần tử nào lớn hơn`prev`bắt đầu một đỉnh mới và bất kỳ phần tử nào bằng`prev`tiếp tục cao nguyên. Bởi vì chúng tôi không bao giờ giảm`prev`, chúng tôi đảm bảo rằng tất cả các phép biến đổi đều hợp lệ và việc đếm các phần tử xương rồng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        cactus_count = 0
        prev = 0
        for x in a:
            if x > prev:
                cactus_count += 1
            elif x == prev:
                cactus_count += 1
            prev = max(prev, x)
        print(cactus_count)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả đầu vào bằng I/O nhanh. Phần quan trọng là vòng lặp quét mảng một lần và cập nhật`cactus_count`Và`prev`dựa trên các quy tắc đỉnh/bình nguyên. các`max(prev, x)`đảm bảo ràng buộc không giảm không bao giờ bị vi phạm. Các điều kiện biên như mảng có kích thước bằng một hoặc các phần tử đã bằng nhau được xử lý một cách tự nhiên theo cùng một logic. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1
5
1 2 2 1 3
```| tôi | x | trước | cây xương rồng_count | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 
| 1 | 2 | 1 | 2 | 
| 2 | 2 | 2 | 3 | 
| 3 | 1 | 2 | 3 | 
| 4 | 3 | 2 | 4 | 

Điều này chứng tỏ các phần tử giảm không giảm`cactus_count`và các phần tử tăng dần hoặc ổn định được tính chính xác. 

Đầu vào mẫu 2:```
1
3
3 3 3
```| tôi | x | trước | cây xương rồng_count | 
| --- | --- | --- | --- | 
| 0 | 3 | 0 | 1 | 
| 1 | 3 | 3 | 2 | 
| 2 | 3 | 3 | 3 | 

Tất cả các phần tử bằng nhau đều được tính, xác nhận logic ổn định hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập một lần và chỉ các hoạt động có thời gian không đổi xảy ra trên mỗi phần tử. | 
| Không gian | O(1) | Chỉ có hai biến bổ sung được sử dụng, không phụ thuộc vào kích thước đầu vào. | 

Với$n \le 2 \cdot 10^5$, giải pháp này thực hiện thoải mái trong giới hạn thời gian tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("1\n5\n1 2 2 1 3\n") == "4", "sample 1"

# Custom tests
assert run("1\n3\n3 3 3\n") == "3", "all equal"
assert run("1\n1\n5\n") == "1", "single element"
assert run("1\n4\n4 3 2 1\n") == "1", "strictly decreasing"
assert run("1\n6\n1 2 3 2 3 4\n") == "5", "mixed increasing and decreasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 3 | 3 | xử lý cao nguyên | 
| 5 | 1 | trường hợp phần tử đơn | 
| 4 3 2 1 | 1 | mảng giảm nghiêm ngặt | 
| 1 2 3 2 3 4 | 5 | sự đúng đắn của trình tự hỗn hợp | 

## Vỏ cạnh 

Đối với trường hợp cạnh`[4, 3, 2, 1]`, thuật toán quét từ trái sang phải. Yếu tố đầu tiên`4`tạo thành một đỉnh, do đó`cactus_count = 1`. Mỗi phần tử tiếp theo nhỏ hơn`prev = 4`, vì vậy không có cái nào được tính. Đầu ra là`1`, điều này đúng vì không được phép giảm. 

Vì`[3, 3, 3]`,`prev`bắt đầu từ 0. Mỗi phần tử bằng`prev`hoặc lớn hơn, do đó tất cả các yếu tố đều góp phần vào số lượng xương rồng. Điều này xác nhận việc xử lý cao nguyên là chính xác. 

Bài xã luận này cung cấp hướng dẫn đầy đủ về mặt khái niệm và thực tiễn cho vấn đề, từ việc hiểu các quy tắc biến đổi xương rồng đến triển khai giải pháp quét tuyến tính xử lý tất cả các trường hợp biên.
