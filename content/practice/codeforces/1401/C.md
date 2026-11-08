---
title: "CF 1401C - Mảng đơn thuần"
description: "Chúng ta được cho một mảng các số nguyên dương. Hoạt động được phép là hoán đổi giữa hai phần tử nếu ước số chung lớn nhất (GCD) của hai phần tử đó bằng phần tử tối thiểu của mảng."
date: "2026-06-11T08:48:38+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1401
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 665 (Div. 2)"
rating: 1300
weight: 1401
solve_time_s: 554
verified: true
draft: false
---

[CF 1401C - Mảng đơn thuần](https://codeforces.com/problemset/problem/1401/C) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, toán học, lý thuyết số, sắp xếp 
**Thời gian giải:** 9 phút 14 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Hoạt động được phép là hoán đổi giữa hai phần tử nếu ước số chung lớn nhất (GCD) của hai phần tử đó bằng phần tử tối thiểu của mảng. Mục đích là để xác định xem liệu chúng ta có thể sắp xếp lại mảng thành thứ tự không giảm bằng cách sử dụng các hoán đổi này hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi test case có độ dài`n`và một mảng`n`số nguyên. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu mảng có thể được sắp xếp theo thứ tự không giảm theo quy tắc hoặc "KHÔNG" nếu ngược lại. 

Hạn chế chính là tổng số tiền`n`trên tất cả các trường hợp thử nghiệm nhiều nhất là 100.000. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng các giao dịch hoán đổi một cách rõ ràng vì giải pháp đó có thể yêu cầu tối đa O(n^2) thao tác cho mỗi trường hợp thử nghiệm, tốc độ này quá chậm. 

Trường hợp cạnh tinh tế phát sinh khi mảng không giảm hoặc chứa nhiều bản sao của phần tử tối thiểu. Ví dụ, nếu mảng là`[2, 2, 3]`, nó đã được sắp xếp rồi. Nếu mảng là`[3, 6, 2, 9]`, thì chỉ những phần tử chia hết cho tối thiểu (2) mới có thể được hoán đổi. Cố gắng sắp xếp một cách ngây thơ mà không xem xét hạn chế về khả năng chia hết sẽ tạo ra "CÓ" hoặc "KHÔNG" không chính xác. 

Một trường hợp phức tạp khác là khi một phần tử nhỏ hơn mức tối thiểu nhưng xuất hiện muộn hơn. Coi như`[4, 6, 2, 5]`. Số 5 không thể trao đổi với bất kỳ phần tử nào có thể giúp nó đạt đến vị trí được sắp xếp bởi vì`gcd(5, any other number)`không bao giờ là 2 nên câu trả lời phải là "KHÔNG". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các giao dịch hoán đổi hợp lệ có thể có, nhưng điều này không khả thi vì có thể có tới 10^5 phần tử và số lượng giao dịch hoán đổi có thể là O(n^2). Cũng sẽ rất khó để đảm bảo việc chấm dứt vì các giao dịch hoán đổi có thể lan rộng. 

Thông tin chi tiết quan trọng là phần tử tối thiểu xác định “nhóm có thể hoán đổi”. Bất kỳ phần tử nào chia hết cho mức tối thiểu đều có thể được hoán đổi tự do với nhau thông qua một loạt các hoán đổi được phép. Do đó, hạn chế duy nhất xảy ra đối với các phần tử không chia hết cho mức tối thiểu. Các phần tử này phải có thứ tự tương đối chính xác trong mảng so với các phần tử khác không chia hết cho mức tối thiểu. 

Sử dụng quan sát này, cách tiếp cận tối ưu là: 

1. Tính phiên bản đã sắp xếp của mảng. 
2. Lặp lại trong mảng và đối với bất kỳ phần tử nào không chia hết cho giá trị tối thiểu, hãy kiểm tra xem nó đã ở vị trí được sắp xếp hay chưa. Nếu không thì không thể di chuyển vào đúng vị trí và câu trả lời là "KHÔNG". 
3. Nếu tất cả các phần tử đều chia hết cho giá trị tối thiểu hoặc đã ở vị trí được sắp xếp của chúng thì mảng có thể được sắp xếp bằng cách sử dụng các hoán đổi liên quan đến giá trị tối thiểu. 

Điều này làm giảm vấn đề xuống còn một lần quét đơn giản và kiểm tra GCD/khả năng chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Kiểm tra sắp xếp + chia hết | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`và mảng`a`. 
2. Tính phần tử tối thiểu`mn`TRONG`a`. 
3. Xây dựng một bản sao đã được sắp xếp của`a`, gọi nó`sorted_a`. 
4. Lặp qua các chỉ số của`a`. 

1. Nếu`a[i]`bằng`sorted_a[i]`, Tiếp tục. 
2. Nếu`a[i]`chia hết cho`mn`, nó có thể được di chuyển tự do, tiếp tục. 
3. Nếu không,`a[i]`không chia hết cho`mn`và không đúng chỗ. Trả về "KHÔNG" cho trường hợp thử nghiệm này. 
5. Nếu vòng lặp hoàn thành mà không trả về "KHÔNG", hãy trả về "CÓ". 

Tại sao nó hoạt động: tính bất biến là bất kỳ phần tử nào chia hết cho mức tối thiểu đều có thể được hoán đổi tùy ý thông qua một chuỗi các hoán đổi liên quan đến mức tối thiểu. Các phần tử không chia hết cho mức tối thiểu phải ở đúng vị trí được sắp xếp vì không có phép hoán đổi hợp lệ nào có thể di chuyển chúng. Điều này đảm bảo tính chính xác vì mọi sự sắp xếp lại cần thiết chỉ liên quan đến các phần tử có thể hoán đổi và tất cả các hạn chế đều được kiểm tra rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_sort(a):
    mn = min(a)
    sorted_a = sorted(a)
    for original, target in zip(a, sorted_a):
        if original != target and original % mn != 0:
            return "NO"
    return "YES"

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(can_sort(a))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên tính toán mức tối thiểu. Phiên bản được sắp xếp đóng vai trò là cấu hình đích. Vòng lặp đảm bảo rằng bất kỳ phần tử nào không ở đúng vị trí đều phải chia hết cho mức tối thiểu; nếu không, việc sắp xếp là không thể. sử dụng`zip`cho phép so sánh từng cặp rõ ràng. Điều này sử dụng O(n log n) cho mỗi trường hợp thử nghiệm do sắp xếp, hiệu quả với các ràng buộc. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 2```
6
4 3 6 6 2 9
```| Chỉ mục | một [tôi] | được sắp xếp_a[i] | a[i] % mn == 0? | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 2 | 4 % 2 == 0 | được | 
| 1 | 3 | 3 | - | đã đúng rồi | 
| 2 | 6 | 4 | 6 % 2 == 0 | được | 
| 3 | 6 | 6 | - | đã đúng rồi | 
| 4 | 2 | 6 | 2 % 2 == 0 | được | 
| 5 | 9 | 9 | - | đã đúng rồi | 

Tất cả các phần tử có thể được di chuyển một cách chính xác thông qua các hoán đổi liên quan đến 2. Đầu ra: CÓ. 

### Đầu vào mẫu 4```
7 5 2 2 4
```| Chỉ mục | một [tôi] | được sắp xếp_a[i] | a[i] % mn == 0? | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | 2 | 7 % 2 != 0 | KHÔNG | 

Đầu ra là KHÔNG vì 7 không chia hết cho tối thiểu (2) và không ở vị trí đích của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế; lặp qua mảng là O(n) | 
| Không gian | O(n) | Lưu trữ mảng đã sắp xếp | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm là 10^5, tổng độ phức tạp vừa vặn trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided samples
assert run("4\n1
```
