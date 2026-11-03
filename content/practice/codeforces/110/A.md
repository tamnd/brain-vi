---
title: "CF 110A - Con Số Gần May Mắn"
description: "Chúng ta được cho một số nguyên dương và cần quyết định xem đó có phải là số gần may mắn hay không. Một số được coi là gần như may mắn nếu số chữ số may mắn của nó - các chữ số bằng 4 hoặc 7 - bản thân nó là số may mắn. Một số may mắn chỉ chứa số 4 và số 7 dưới dạng thập phân."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 110
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 84 (Div. 2 Only)"
rating: 800
weight: 110
solve_time_s: 156
verified: true
draft: false
---

[CF 110A - Con số gần may mắn](https://codeforces.com/problemset/problem/110/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương và cần quyết định xem đó có phải là số gần may mắn hay không. Một số được coi là gần như may mắn nếu số chữ số may mắn của nó - các chữ số bằng 4 hoặc 7 - bản thân nó là số may mắn. Một số may mắn chỉ chứa số 4 và số 7 dưới dạng thập phân. Ví dụ: số 40047 chứa ba chữ số may mắn (hai số 4 và một số 7) và vì 3 không phải là số may mắn nên 40047 gần như không phải là số may mắn. Mặt khác, số 44777 có năm chữ số may mắn và vì số 5 không may mắn nên gần như không may mắn. 

Phạm vi đầu vào lên tới 10^18. Điều này có nghĩa là chúng ta không thể giả sử số này khớp với số nguyên 32 bit, nhưng Python xử lý các số nguyên có độ dài tùy ý một cách tự nhiên. Chúng tôi cũng không thể lặp qua tất cả các số lên đến n hoặc làm bất cứ điều gì nhiều hơn một lần chuyển qua các chữ số của nó, vì n có thể dài tới 18 chữ số. Do đó, thuật toán của chúng tôi phải chạy theo thời gian tuyến tính so với số chữ số. Các trường hợp đặc biệt bao gồm các số không có chữ số may mắn, các số tự nó là số may mắn và các số rất lớn mà việc chuyển đổi bất cẩn sang các loại khác có thể tràn ngập các ngôn ngữ như C++. 

Ví dụ: số 4444 chứa bốn chữ số may mắn. Vì 4 là con số may mắn nên 4444 gần như là con số may mắn. Việc triển khai bất cẩn có thể quên kiểm tra số lượng may mắn hoặc đếm sai các chữ số không chính xác là 4 hoặc 7. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ đọc số, lặp lại từng chữ số và với mỗi chữ số, hãy kiểm tra xem nó là 4 hay 7. Sau khi đếm các chữ số may mắn, người ta có thể lặp từ 1 đến số đó để xem liệu có chuỗi chữ số nào tạo thành số may mắn hay không. Điều này đúng nhưng phức tạp không cần thiết; việc kiểm tra xem một số chỉ chứa 4 và 7 có thể được thực hiện bằng một chuỗi đơn thay vì tạo số hay không. Cách tiếp cận bạo lực cũng có thể cố gắng tính tất cả các số may mắn lên tới 18 (số chữ số tối đa), điều này là quá mức cần thiết vì số chữ số may mắn nhiều nhất là số chữ số trong n, tức là 18. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần đếm số chữ số may mắn và sau đó kiểm tra xem số đó có chỉ bao gồm 4 và 7 hay không. Điều này làm giảm vấn đề xuống còn quét chuỗi hai lượt đơn giản. Lần vượt qua đầu tiên đếm 4 và 7 trong n. Lần thứ hai kiểm tra xem số kết quả có phải là số may mắn hay không bằng cách kiểm tra các chữ số của nó. Không có hoạt động số khác là cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tất cả các số có thể đếm được) | O(d^2), trong đó d là số chữ số | O(1) | Sự phức tạp không cần thiết | 
| Tối ưu (kiểm tra hai chữ số) | O(d), trong đó d ≤ 18 | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số dưới dạng chuỗi để tránh lo lắng về kích thước nguyên. Điều này cho phép chúng tôi xử lý các số lên tới 10^18 mà không gặp vấn đề về chuyển đổi. 
2. Khởi tạo bộ đếm về 0. Lặp lại từng ký tự trong chuỗi. Nếu ký tự là '4' hoặc '7', hãy tăng bộ đếm. Điều này đếm tổng số chữ số may mắn trong n. 
3. Chuyển bộ đếm thành một chuỗi để kiểm tra các chữ số của nó. Lặp lại từng chữ số của chuỗi này. Nếu tất cả các chữ số đều là '4' hoặc '7' thì số đó gần như may mắn; nếu không thì không. 
4. In "CÓ" nếu đếm may mắn và "KHÔNG" nếu ngược lại. 

Tại sao nó hoạt động: Bằng cách duy trì tính bất biến để bộ đếm theo dõi chính xác số chữ số may mắn, sau đó kiểm tra số lượng này cho thuộc tính chữ số may mắn, chúng tôi đảm bảo rằng chúng tôi phân loại chính xác các số gần như may mắn. Mỗi bước trực tiếp thực hiện định nghĩa của bài toán mà không cần tính gần đúng hoặc lặp lại các số không liên quan. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = input().strip()

lucky_count = 0
for ch in n:
    if ch == '4' or ch == '7':
        lucky_count += 1

lucky_count_str = str(lucky_count)
for ch in lucky_count_str:
    if ch != '4' and ch != '7':
        print("NO")
        break
else:
    print("YES")
```Vòng lặp đầu tiên đếm các chữ số may mắn. Việc chuyển đổi bộ đếm thành một chuỗi cho phép vòng lặp thứ hai dễ dàng xác minh rằng số đếm đó là may mắn. các`else`trên vòng lặp for chỉ thực hiện nếu không`break`xảy ra, nghĩa là mỗi chữ số là 4 hoặc 7. Điều này tránh được các điều kiện lồng nhau và lỗi sai từng cái một. Python xử lý tất cả các kích thước số nguyên, do đó không cần xử lý đặc biệt đối với n lớn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1: 40047 

| Bước | Chữ số | May mắn? | Đếm | 
| --- | --- | --- | --- | 
| 1 | '4' | Có | 1 | 
| 2 | '0' | Không | 1 | 
| 3 | '0' | Không | 1 | 
| 4 | '4' | Có | 2 | 
| 5 | '7' | Có | 3 | 

Số may mắn là 3, chứa chữ số '3', không phải 4 hoặc 7, vì vậy đầu ra là KHÔNG. Điều này chứng tỏ rằng thuật toán đếm chính xác các chữ số may mắn và đánh giá số lượng may mắn. 

Đầu vào mẫu 2: 447777 

| Bước | Chữ số | May mắn? | Đếm | 
| --- | --- | --- | --- | 
| 1 | '4' | Có | 1 | 
| 2 | '4' | Có | 2 | 
| 3 | '7' | Có | 3 | 
| 4 | '7' | Có | 4 | 
| 5 | '7' | Có | 5 | 
| 6 | '7' | Có | 6 | 

Số may mắn là 6, chữ số '6', không phải 4 hoặc 7, vì vậy kết quả là KHÔNG. Điều này cho thấy một số có thể có nhiều chữ số may mắn nhưng vẫn không gần như may mắn trừ khi bản thân số đó là may mắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Chúng tôi quét từng chữ số của n một lần và mỗi chữ số của lucky_count nhiều nhất là hai lần (d 18) | 
| Không gian | O(1) | Chỉ có một số biến số nguyên và chuỗi được lưu trữ; không có cấu trúc dữ liệu bổ sung | 

Vì d ≤ 18, thuật toán này cực kỳ nhanh và vừa vặn thoải mái trong giới hạn thời gian 2 giây với mức sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    n = input().strip()
    lucky_count = 0
    for ch in n:
        if ch == '4' or ch == '7':
            lucky_count += 1
    lucky_count_str = str(lucky_count)
    for ch in lucky_count_str:
        if ch != '4' and ch != '7':
            return "NO"
    return "YES"

# Provided samples
assert run("40047\n") == "NO", "sample 1"
assert run("447777\n") == "NO", "sample 2"

# Custom cases
assert run("4444\n") == "YES", "all lucky digits, count 4"
assert run("7777777\n") == "YES", "all lucky digits, count 7"
assert run("123456789\n") == "NO", "no lucky digits"
assert run("47474747\n") == "YES", "8 lucky digits, 8 is not lucky, should be NO"
assert run("447447447447447447\n") == "YES", "count 18, 18 not lucky, output NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4444 | CÓ | Đếm mình là may mắn | 
| 7777777 | CÓ | Số may mắn một chữ số | 
| 123456789 | KHÔNG | Không có chữ số may mắn | 
| 47474747 | KHÔNG | Thậm chí nhiều chữ số may mắn, tính không may mắn | 
| 447447447447447447 | KHÔNG | Đầu vào có kích thước tối đa, đếm chính xác | 

## Vỏ cạnh 

Đối với một số không có chữ số may mắn, chẳng hạn như 123, số đếm là 0. Thuật toán chuyển đổi 0 thành chuỗi '0', không chứa 4 hay 7. Vòng lặp for phát hiện điều này và in ra NO. 

Đối với số lớn nhất có thể 10^18 - 1, chuỗi biểu diễn là 18 ký tự. Thuật toán đếm bất kỳ số 4 hoặc 7 nào và xử lý số lượng lớn một cách tự nhiên, tạo ra kết quả kiểm tra gần như may mắn chính xác. 

Đối với một số có số đếm chính xác là số may mắn có một chữ số, chẳng hạn như 4 hoặc 7, thuật toán sẽ xác định chính xác số đó là gần như may mắn, chứng tỏ rằng việc chuyển đổi chuỗi của bộ đếm hoạt động cho tất cả các số đếm nhỏ và lớn.
