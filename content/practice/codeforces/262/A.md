---
title: "CF 262A - Roma và những con số may mắn"
description: "Roma có một bộ sưu tập các số nguyên dương và anh ấy rất thích thú với những con số có chữ số thập phân chỉ gồm 4 và 7. Đây được gọi là những con số may mắn. Nhiệm vụ là xác định từ bộ sưu tập của mình có bao nhiêu số có tối đa k chữ số may mắn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 262
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 160 (Div. 2)"
rating: 800
weight: 262
solve_time_s: 186
verified: false
draft: false
---

[CF 262A - Roma và những con số may mắn](https://codeforces.com/problemset/problem/262/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Roma có một bộ sưu tập các số nguyên dương và anh ấy rất thích thú với những con số có chữ số thập phân chỉ gồm 4 và 7. Đây được gọi là những con số may mắn. Nhiệm vụ là xác định, từ bộ sưu tập của mình, có bao nhiêu số có tối đa _k_ chữ số may mắn. Đầu vào cung cấp kích thước của bộ sưu tập _n_, số lượng chữ số may mắn tối đa được phép _k_ và sau đó là chính các số _n_. Đầu ra là một số nguyên duy nhất: số lượng các số đáp ứng ràng buộc chữ số may mắn. 

Các ràng buộc ở mức vừa phải: cả _n_ và _k_ có thể tối đa là 100 và mỗi số có thể lên tới 10^9. Điều này có nghĩa là chúng tôi có đủ khả năng để xử lý từng số riêng lẻ và kiểm tra từng chữ số mà không cần lo ngại về hiệu suất. Việc kiểm tra từng chữ số một cách đơn giản là khả thi vì trong trường hợp xấu nhất, 100 số có tối đa 10 chữ số sẽ dẫn đến khoảng 1.000 phép tính. Các trường hợp đặc biệt bao gồm các số không có chữ số may mắn, các số bao gồm toàn bộ các chữ số may mắn hoặc các tình huống trong đó _k_ lớn hơn số chữ số trong số lớn nhất. Ví dụ: nếu _n_ = 3, _k_ = 1 và các số là 4, 7, 47 thì kết quả đúng là 2 vì 47 có hai chữ số may mắn và vượt quá _k_. Việc triển khai bất cẩn có thể đếm sai các chữ số hoặc quên so sánh với _k_, đưa ra tổng sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Lặp lại qua từng số của Roma và với mỗi số, hãy đếm xem có bao nhiêu chữ số may mắn. Nếu số lượng không vượt quá _k_, hãy tăng tổng số đang chạy. Điều này có hiệu quả vì mỗi số có nhiều nhất là 10 chữ số và có nhiều nhất là 100 số, mang lại kết quả kiểm tra khoảng 1.000 chữ số. Thuật toán đúng về mặt xây dựng vì chúng tôi đếm rõ ràng các chữ số may mắn và so sánh chúng với ngưỡng. Hiệu suất không phải là vấn đề đáng lo ngại do giới hạn đầu vào nhỏ. 

Không cần có giải pháp tiệm cận nhanh hơn, bởi vì cách tiếp cận vũ phu đã đáp ứng được các ràng buộc. Tối ưu hóa duy nhất có thể là về mặt phong cách: chúng ta có thể chuyển đổi số thành chuỗi và sử dụng biểu thức trình tạo để đếm các chữ số may mắn trên một dòng thay vì sử dụng vòng lặp, nhưng logic cơ bản không thay đổi. Vấn đề này về cơ bản là về việc lặp lại và đếm cẩn thận chứ không phải về độ phức tạp của thuật toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * d) trong đó d là số chữ số 10 | O(1) | Đã chấp nhận | 
| Tối ưu | O(n * d) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các số nguyên _n_ và _k_ từ đầu vào và đọc danh sách các số. 
2. Khởi tạo bộ đếm`result`về không. Điều này sẽ giữ số lượng thỏa mãn điều kiện chữ số may mắn. 
3. Lặp lại từng số trong danh sách. 
4. Với mỗi số, khởi tạo bộ đếm cục bộ`lucky_count`về không. 
5. Kiểm tra từng chữ số của số hiện tại. Nếu chữ số là 4 hoặc 7, tăng dần`lucky_count`. 
6. Sau khi kiểm tra tất cả các chữ số, so sánh`lucky_count`tới _k_. Nếu như`lucky_count`≤ _k_, tăng`result`. 
7. Sau khi tất cả các số đã được xử lý, xuất ra giá trị của`result`. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi đếm rõ ràng các chữ số may mắn trên mỗi số và kiểm tra xem chúng có nằm trong ngưỡng cho phép hay không. Tính bất biến đó là`result`luôn phản ánh chính xác số lượng các số được xử lý cho đến nay thỏa mãn điều kiện. Vì mọi chữ số đều được kiểm tra và việc so sánh với _k_ là chính xác nên không có số nào có thể bị đếm sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
numbers = list(map(int, input().split()))
result = 0

for num in numbers:
    lucky_count = sum(1 for digit in str(num) if digit == '4' or digit == '7')
    if lucky_count <= k:
        result += 1

print(result)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý tốc độ đầu vào lập trình cạnh tranh tiêu chuẩn. Mỗi số được chuyển đổi thành một chuỗi để chúng ta có thể kiểm tra từng chữ số. Biểu thức máy phát điện`sum(1 for digit in str(num) if digit == '4' or digit == '7')`đếm các chữ số may mắn một cách rõ ràng và tránh các vòng lặp thủ công. Sự so sánh với`k`đảm bảo chỉ những số đáp ứng điều kiện mới đóng góp vào kết quả cuối cùng. Việc sử dụng số nguyên sẽ tránh mọi nguy cơ tràn và kích thước nhỏ của số đảm bảo không gây tắc nghẽn hiệu suất. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
3 4
1 2 4
```| số | chữ số được kiểm tra | may mắn_đếm | lucky_count <= k? | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | '1' | 0 | Đúng | 1 | 
| 2 | '2' | 0 | Đúng | 2 | 
| 4 | '4' | 1 | Đúng | 3 | 

Tất cả các số đều có tối đa 4 chữ số may mắn nên kết quả cuối cùng là 3. 

**Mẫu 2:** 

đầu vào:```
3 2
447 4 77
```| số | chữ số được kiểm tra | may mắn_đếm | lucky_count <= k? | kết quả | 
| --- | --- | --- | --- | --- | 
| 447 | '4','4','7' | 3 | Sai | 0 | 
| 4 | '4' | 1 | Đúng | 1 | 
| 77 | '7','7' | 2 | Đúng | 2 | 

Chỉ có 4 và 77 thỏa mãn điều kiện, mang lại kết quả là 2. 

Những dấu vết này xác nhận thuật toán đếm chính xác các chữ số may mắn và so sánh chúng với ngưỡng _k_. Các trường hợp cạnh như số chính xác trên ranh giới của _k_ được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * d) ≤ O(1000) | Mỗi số trong số n có nhiều nhất là 10 chữ số. | 
| Không gian | O(1) | Chỉ các bộ đếm và danh sách số tạm thời được lưu trữ. | 

Với các ràng buộc, điều này chạy tốt trong giới hạn 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    numbers = list(map(int, input().split()))
    result = 0
    for num in numbers:
        lucky_count = sum(1 for digit in str(num) if digit == '4' or digit == '7')
        if lucky_count <= k:
            result += 1
    return str(result)

# provided samples
assert run("3 4\n1 2 4\n") == "3", "sample 1"
assert run("3 2\n447 4 77\n") == "2", "sample 2"

# custom cases
assert run("1 0\n4\n") == "0", "single number exceeds k"
assert run("1 1\n4\n") == "1", "single number equals k"
assert run("5 3\n4 7 44 77 447\n") == "4", "mixed small numbers"
assert run("3 10\n4444 7777 4747\n") == "3", "all numbers below high k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0\n4 | 0 | số đơn vượt quá k | 
| 1 1\n4 | 1 | số đơn bằng k | 
| 5 3\n4 7 44 77 447 | 4 | hỗn hợp các số nhỏ, một số vượt quá k | 
| 3 10\n4444 7777 4747 | 3 | mọi số dưới k cao | 

## Vỏ cạnh 

Đối với một số vượt quá _k_, chẳng hạn như`n=1, k=0, numbers=[4]`, thuật toán đếm chính xác 1 chữ số may mắn, so sánh nó với 0 và bỏ qua việc tăng kết quả, tạo ra kết quả 0. Đối với các số có kích thước tối đa như`n=100, k=100, numbers=[777777777]*100`, mỗi số có 9 chữ số, đều may mắn. Tổng của mỗi số là 9, tức là ≤ 100, do đó tất cả các số đều được tính, cho kết quả đầu ra là 100. Thuật toán xử lý chính xác số 0, ranh giới và đầu vào tối đa mà không có lỗi riêng lẻ.
