---
title: "CF 104767I - Natatorium"
description: "Chúng ta có diện tích bề mặt mục tiêu $C$, được đảm bảo là tích của hai số nguyên tố phân biệt. Ngoài ra, chúng ta còn được cung cấp một danh sách các độ dài cạnh có sẵn của $M$, trong đó mọi phần tử trong danh sách cũng là số nguyên tố."
date: "2026-06-28T20:08:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104767
codeforces_index: "I"
codeforces_contest_name: "2023-2024 CTU Open Contest"
rating: 0
weight: 104767
solve_time_s: 63
verified: true
draft: false
---

[CF 104767I - Natatorium](https://codeforces.com/problemset/problem/104767/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một diện tích bề mặt mục tiêu$C$, được đảm bảo là tích của hai số nguyên tố phân biệt. Bên cạnh đó, chúng tôi được cung cấp một danh sách$M$độ dài cạnh có sẵn, trong đó mọi phần tử trong danh sách cũng là số nguyên tố. 

Nhiệm vụ là chọn hai số nguyên tố khác nhau từ danh sách này sao cho tích của chúng bằng$C$. Vì nhóm phải là hình chữ nhật chứ không phải hình vuông nên chúng ta không thể chọn cùng một số hai lần và hai số nguyên tố được chọn phải khác nhau. Đầu ra chỉ đơn giản là cái nhỏ hơn trước, tiếp theo là cái lớn hơn. 

Ràng buộc$C \le 10^{18}$ngụ ý rằng chúng ta không thể phân tích nó bằng cách sử dụng phép chia thử đơn giản cho đến$C$, nhưng nói chung chúng ta không cần phân tích đầy đủ vì danh sách đã hạn chế các ứng cử viên. Số lượng ứng viên$M \le 2 \cdot 10^5$gợi ý rằng việc quét tuyến tính hoặc gần tuyến tính trên danh sách là có thể chấp nhận được, nhưng mọi phép tính bậc hai trên danh sách sẽ không cần thiết vì cấu trúc của bài toán cho phép xác nhận trực tiếp phần bù. 

Sự tinh tế quan trọng là cả hai yếu tố của$C$được đảm bảo xuất hiện trong danh sách nên chúng ta không cần tìm các ước số tùy ý mà chỉ cần tìm các truy vấn thành viên trong tập hợp đã cho. 

Một sai lầm ngây thơ là thử tất cả các cặp trong danh sách và kiểm tra sản phẩm của chúng. Điều đó sẽ đòi hỏi$O(M^2)$hoạt động, mà tại$M = 2 \cdot 10^5$dẫn đến$4 \cdot 10^{10}$kiểm tra và hoàn toàn không khả thi. 

Một lỗi khác có thể xảy ra là lặp lại các số nguyên tố và kiểm tra tính chia hết$C \bmod P_i = 0$, sau đó cố gắng tìm$C / P_i$thông qua tìm kiếm tuyến tính. Nếu không băm, điều này thoái hóa thành$O(M^2)$lại. Cách tiếp cận đúng phải thực hiện việc kiểm tra tư cách thành viên liên tục. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử từng cặp số nguyên tố trong danh sách và kiểm tra xem tích của chúng có bằng không$C$. Điều này đúng vì giải pháp được đảm bảo tồn tại trong danh sách. Tuy nhiên, nó kiểm tra tất cả$\binom{M}{2}$cặp, quá chậm khi$M$là lớn. 

Chúng ta có thể cải thiện điều này bằng cách thay đổi quan điểm. Thay vì chọn hai số và nhân chúng, chúng ta có thể cố định một số$p$và tính xem cái còn lại phải là gì:$q = C / p$. Từ$C$được đảm bảo là tích của hai số nguyên tố phân biệt, nếu$p$thì là một trong số đó$q$cũng phải là số nguyên tố trong danh sách. 

Điều này làm giảm vấn đề thành vấn đề kiểm tra tư cách thành viên: với mỗi$p$, kiểm tra xem$C$chia hết cho$p$, và nếu vậy hãy kiểm tra xem$C/p$tồn tại trong danh sách. Việc sử dụng bộ băm thực hiện cả hai thao tác$O(1)$trung bình, biến toàn bộ giải pháp thành quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force |$O(M^2)$|$O(1)$| Quá chậm | 
| Kiểm tra bổ sung bộ băm |$O(M)$|$O(M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta dựa vào thực tế là cặp đúng chính xác là hai thừa số nguyên tố của$C$và cả hai đều được đảm bảo có mặt trong danh sách đầu vào. 

1. Đọc$C$,$M$, và danh sách các số nguyên tố. Chúng tôi lưu trữ tất cả các số nguyên tố trong bộ băm để tra cứu theo thời gian liên tục. Điều này cho phép chúng ta nhanh chóng kiểm tra xem phần bổ sung ứng viên có tồn tại hay không. 
2. Lặp lại từng số nguyên tố$p$trong danh sách. Mỗi$p$là chiều dài cạnh tiềm năng của bể bơi. 
3. Kiểm tra xem$p$chia rẽ$C$. Nếu không, nó không thể là một phần của cặp đúng, vì cả hai thừa số hợp lệ phải nhân chính xác với$C$. 
4. Nếu$p$chia rẽ$C$, tính toán$q = C / p$. Đây là đối tác duy nhất có thể cho$p$trong một giải pháp hợp lệ. 
5. Kiểm tra xem$q$tồn tại trong tập các số nguyên tố cho phép. Nếu đúng như vậy thì chúng ta đã tìm được độ dài hai cạnh cần tìm. 
6. Xuất cặp theo thứ tự sắp xếp sao cho giá trị nhỏ hơn xuất hiện trước. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào cấu trúc của$C$. Từ$C$là tích của hai số nguyên tố phân biệt nên có đúng hai ước nguyên tố. Bất kỳ giải pháp hợp lệ nào cũng phải sử dụng chính xác hai số đó. Vì vậy, bất kỳ$p$từ danh sách chia$C$và mang lại một yếu tố bổ sung$q$cũng xuất hiện trong danh sách phải là một trong hai câu trả lời đúng. Bộ băm đảm bảo chúng tôi phát hiện tư cách thành viên mà không có sự mơ hồ hoặc lặp lại và điều kiện phân biệt được tự động thỏa mãn vì các yếu tố được đảm bảo phân biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    C = int(input())
    M = int(input())
    arr = list(map(int, input().split()))
    
    s = set(arr)
    
    for p in arr:
        if C % p == 0:
            q = C // p
            if q in s and q != p:
                print(min(p, q), max(p, q))
                return

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đặt tất cả các độ dài cạnh có sẵn vào một bộ, cho phép kiểm tra thành phần liên tục để tìm phần bù. Sau đó chúng tôi quét qua từng ứng cử viên$p$. Kiểm tra khả năng chia$C \bmod p = 0$lọc sớm các số nguyên tố không liên quan, tránh việc tra cứu tập hợp không cần thiết. 

Khi tìm thấy một ước số hợp lệ, chúng tôi tính toán phần bù của nó và xác minh rằng nó tồn tại trong tập hợp. điều kiện$q \ne p$thực thi yêu cầu rằng nhóm không phải là hình vuông, mặc dù bài toán đảm bảo các số nguyên tố khác nhau, vì vậy đây chủ yếu là một biện pháp bảo vệ an toàn. 

Chúng tôi ngay lập tức in và kết thúc sau khi tìm thấy cặp hợp lệ, vì tính duy nhất của hệ số hóa đảm bảo không tồn tại giải pháp hợp lệ thứ hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
15
5
7 2 5 11 3
```Cặp đúng là$3$Và$5$. 

| Bước | p | C % p == 0 | q = C/p | q trong bộ | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 7 | không | - | - | bỏ qua | 
| 2 | 2 | không | - | - | bỏ qua | 
| 3 | 5 | vâng | 3 | vâng | trở lại (3, 5) | 

Dấu vết này cho thấy chỉ có những yếu tố thực sự của$C$vượt qua bộ lọc chia hết và kiểm tra phần bù xác nhận tính đúng đắn ngay lập tức. 

### Ví dụ 2 

đầu vào:```
21
4
2 3 5 7
```Đây$C = 21 = 3 \cdot 7$. 

| Bước | p | C % p == 0 | q = C/p | q trong bộ | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | không | - | - | bỏ qua | 
| 2 | 3 | vâng | 7 | vâng | trở về (3, 7) | 

Thuật toán tìm cặp ngay khi nó đạt đến một trong các số nguyên tố hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M)$| Mỗi phần tử được xử lý một lần với các phép toán băm liên tục | 
| Không gian |$O(M)$| Đặt lưu trữ tất cả các số nguyên tố ứng cử viên | 

Quét tuyến tính là đủ cho$M \le 2 \cdot 10^5$và mức sử dụng bộ nhớ nhỏ vì chúng tôi chỉ lưu trữ danh sách đầu vào trong tập hợp băm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    C = int(input())
    M = int(input())
    arr = list(map(int, input().split()))
    s = set(arr)

    for p in arr:
        if C % p == 0:
            q = C // p
            if q in s and q != p:
                return f"{min(p,q)} {max(p,q)}\n"

    return ""

# provided sample
assert run("15\n5\n7 2 5 11 3\n") == "3 5\n"

# custom cases
assert run("21\n4\n2 3 5 7\n") == "3 7\n", "basic factor pair"
assert run("77\n5\n11 7 2 3 5\n") == "7 11\n", "unordered pair"
assert run("143\n4\n11 13 17 19\n") == "11 13\n", "larger primes"
assert run("6\n3\n2 3 5\n") == "2 3\n", "smallest valid case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 21 trường hợp | 3 7 | logic bổ sung cơ bản | 
| trường hợp 77 | 7 11 | đặt hàng độc lập | 
| trường hợp 143 | 11 13 | số nguyên tố không tầm thường | 
| 6 trường hợp | 2 3 | trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi cặp nhân tố hợp lệ xuất hiện theo thứ tự ngược lại trong danh sách đầu vào. Ví dụ: 

đầu vào:```
21
4
7 2 3 5
```Ở đây, thuật toán vẫn hoạt động chính xác. Đầu tiên nó kiểm tra 7, tính toán$21/7 = 3$, và tìm thấy nó trong tập hợp, vì vậy nó xuất ra$3 7$. Ràng buộc đặt hàng được xử lý tại thời điểm đầu ra bằng cách sử dụng`min`Và`max`, vì vậy thứ tự đầu vào không thành vấn đề. 

Một trường hợp khác là khi một số nguyên tố không có thừa số chia hết$C$không chính xác trong số học số nguyên do lo ngại tràn. Trong Python điều này không thể xảy ra, nhưng trong các ngôn ngữ có chiều rộng cố định, cần phải sử dụng cẩn thận các số nguyên 64 bit vì$C$có thể lên đến$10^{18}$. 

Trường hợp cuối cùng là khi danh sách đầu vào chứa cả hai yếu tố nhưng một yếu tố xuất hiện sớm và một yếu tố xuất hiện muộn. Thuật toán có thể kết thúc sớm hoặc muộn tùy theo thứ tự quét, nhưng độ chính xác không bị ảnh hưởng vì bất kỳ lần gặp hợp lệ nào cũng ngay lập tức mang lại giải pháp duy nhất.
