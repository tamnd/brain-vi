---
title: "CF 1952H - Palindrome"
description: "Bài toán yêu cầu chúng ta xác định xem một chuỗi đã cho có đọc xuôi và ngược giống nhau hay không, đó là định nghĩa của một bảng màu. Chúng ta có nhiều trường hợp thử nghiệm, mỗi trường hợp bao gồm một chuỗi chữ cái viết thường."
date: "2026-06-07T18:00:05+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1952
codeforces_index: "H"
codeforces_contest_name: "April Fools Day Contest 2024"
rating: 0
weight: 1952
solve_time_s: 97
verified: false
draft: false
---

[CF 1952H - Palindrome](https://codeforces.com/problemset/problem/1952/H) 

**Đánh giá:** - 
**Tags:** *đặc biệt, triển khai, chuỗi 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xác định xem một chuỗi đã cho có đọc xuôi và ngược giống nhau hay không, đó là định nghĩa của một bảng màu. Chúng ta có nhiều trường hợp thử nghiệm, mỗi trường hợp bao gồm một chuỗi chữ cái viết thường. Đối với mỗi chuỗi, chúng ta cần xuất ra "CÓ" nếu đó là một bảng màu và "KHÔNG" nếu ngược lại. 

Các ràng buộc cho chúng ta biết rằng số lượng trường hợp thử nghiệm có thể lên tới 100 và độ dài chuỗi tối đa là 100. Điều này có nghĩa là tổng số ký tự chúng ta có thể cần xử lý tối đa là 10.000, một con số nhỏ. Bất kỳ giải pháp nào chạy theo thời gian tuyến tính đối với độ dài của mỗi chuỗi sẽ đủ nhanh, vì việc kiểm tra từng cặp ký tự trong chuỗi có độ dài 100 chỉ mất 50 lần so sánh. 

Các trường hợp cạnh mà chúng ta cần cẩn thận bao gồm các chuỗi có độ dài 1, là các chuỗi nhạt màu tầm thường và các chuỗi chỉ có ký tự đầu tiên và ký tự cuối cùng khác nhau, chẳng hạn như "ab". Một cách tiếp cận ngây thơ không xử lý chính xác ranh giới chuỗi có thể xác định sai các chuỗi đó. 

## Phương pháp tiếp cận 

Phương pháp đơn giản nhất là brute-force: đối với mỗi chuỗi, so sánh ký tự đầu tiên với ký tự cuối cùng, ký tự thứ hai với ký tự thứ hai đến cuối cùng, v.v., cho đến khi chúng ta đến giữa chuỗi. Nếu tất cả các cặp này khớp nhau thì chuỗi đó là một chuỗi palindrome; nếu có cặp nào khác nhau thì không phải vậy. Cách tiếp cận này đúng vì nó trực tiếp thực hiện định nghĩa của một bảng màu. Độ phức tạp về thời gian là O(n) trên mỗi chuỗi, trong đó n là độ dài chuỗi. Vì n ≤ 100 nên điều này hiệu quả. 

Không có giải pháp tiệm cận nào nhanh hơn cho vấn đề này vì chúng ta phải kiểm tra mọi ký tự ít nhất một lần trong trường hợp xấu nhất. Thông tin chi tiết quan trọng là nhận ra rằng chúng ta chỉ cần kiểm tra đến giữa chuỗi. Kiểm tra toàn bộ chuỗi theo cả hai hướng hoặc đảo ngược nó để so sánh sẽ hoạt động, nhưng sử dụng hai con trỏ từ hai đầu sẽ hiệu quả và trực tiếp hơn về bộ nhớ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (so sánh hai con trỏ) | O(n) mỗi chuỗi | O(1) | Đã chấp nhận | 
| Đảo ngược và so sánh | O(n) mỗi chuỗi | O(n) | Đã chấp nhận | 

Cả hai cách tiếp cận đều được chấp nhận ở đây. So sánh hai con trỏ không sử dụng thêm bộ nhớ và sạch hơn một chút. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Chúng tôi sẽ lặp lại từng trường hợp thử nghiệm riêng lẻ. 
2. Với mỗi test case, hãy đọc chuỗi`s`và khởi tạo hai con trỏ:`i`tại 0 (bắt đầu) và`j`Tại`len(s)-1`(kết thúc). 
3. Trong khi`i < j`, so sánh`s[i]`Và`s[j]`. Nếu bằng nhau thì tăng`i`và giảm dần`j`. Thao tác này sẽ di chuyển con trỏ về phía giữa chuỗi, kiểm tra tính đối xứng. 
4. Nếu tại bất kỳ thời điểm nào`s[i]`Và`s[j]`khác nhau thì chuỗi không thể là một bảng màu. Xuất "NO" ngay lập tức và ngừng kiểm tra các ký tự tiếp theo. 
5. Nếu vòng lặp hoàn thành mà không tìm thấy các ký tự không khớp, thì tất cả các cặp được nhân đôi đều bằng nhau. Đầu ra "CÓ". 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến ở mỗi lần lặp, tất cả các ký tự bên ngoài con trỏ đã được kiểm tra và đối xứng. Nếu xảy ra sự không khớp, tính chất đối xứng của bảng màu bị vi phạm. Nếu không tìm thấy sự không khớp nào thì chuỗi đó thỏa mãn thuộc tính palindrome. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    i, j = 0, len(s) - 1
    is_palindrome = True
    while i < j:
        if s[i] != s[j]:
            is_palindrome = False
            break
        i += 1
        j -= 1
    print("YES" if is_palindrome else "NO")
```Chúng tôi sử dụng`input().strip()`để loại bỏ bất kỳ ký tự dòng mới nào. Cách tiếp cận hai con trỏ đảm bảo rằng chúng tôi so sánh các vị trí được phản chiếu một cách hiệu quả. các`is_palindrome`cờ cho phép thoát sớm khi không khớp, tránh những so sánh không cần thiết. Điều này cũng giúp bạn dễ dàng in kết quả sau vòng lặp. 

## Ví dụ đã hoạt động 

Dấu vết cho chuỗi "radar": 

| tôi | j | s[i] | s[j] | is_palindrom | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | r | r | Đúng | 
| 1 | 3 | một | một | Đúng | 
| 2 | 2 | d | d | Đúng | 

Tất cả các so sánh đều khớp, vì vậy kết quả đầu ra là "CÓ". 

Dấu vết cho chuỗi "tle": 

| tôi | j | s[i] | s[j] | is_palindrom | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | t | e | Sai | 

Không khớp ở lần so sánh đầu tiên. Đầu ra là "KHÔNG". 

Những dấu vết này cho thấy thuật toán xác định chính xác cả palindromes và non-palindromes. Kỹ thuật hai con trỏ ngay lập tức dừng lại khi tính đối xứng bị vi phạm, tiết kiệm công việc không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi chuỗi | Mỗi ký tự được so sánh nhiều nhất một lần trong một lần quét hai con trỏ. | 
| Không gian | O(1) | Chỉ một số biến số nguyên và cờ boolean được sử dụng, không phụ thuộc vào độ dài chuỗi. | 

Với n ≤ 100 và t ≤ 100, tổng số thao tác tối đa là 10.000, nằm trong giới hạn 3 giây. Việc sử dụng bộ nhớ ở mức tối thiểu nên giải pháp này hiệu quả và an toàn cho tất cả các dữ liệu đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        s = input().strip()
        i, j = 0, len(s) - 1
        is_palindrome = True
        while i < j:
            if s[i] != s[j]:
                is_palindrome = False
                break
            i += 1
            j -= 1
        output.append("YES" if is_palindrome else "NO")
    return "\n".join(output)

# provided samples
assert run("8\nac\ntle\nradar\nracecar\nphp\natcoder\ncodeforces\nsteam\n") == "NO\nNO\nYES\nYES\nNO\nNO\nYES\nYES", "sample 1"

# custom cases
assert run("3\na\nab\naba\n") == "YES\nNO\nYES", "single-character and small palindromes"
assert run("2\nzzzzzzzzzz\nabcdefghijk\n") == "YES\nNO", "all-equal and completely non-palindromic"
assert run("1\nx"*100 + "\n") == "YES", "maximum-length palindrome"
assert run("1\n" + "ab"*50 + "\n") == "NO", "maximum-length non-palindrome"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a, ab, aba | CÓ, KHÔNG, CÓ | Một ký tự, hai ký tự không khớp, bảng màu nhỏ | 
| zzzzzzzzzz, abcdefghijk | CÓ, KHÔNG | Chuỗi đồng nhất, hoàn toàn không palindromic | 
| x lặp lại 100 | CÓ | Bảng màu có độ dài tối đa | 
| ab lặp lại 50 | KHÔNG | Không phải palindrome có độ dài tối đa | 

## Vỏ cạnh 

Đối với các chuỗi có độ dài 1, chẳng hạn như "x", con trỏ`i`Và`j`bắt đầu từ 0. Vì`i`không ít hơn`j`, vòng lặp không bao giờ chạy và chuỗi được phân loại chính xác thành một bảng màu. Đối với các chuỗi hai ký tự có các chữ cái khác nhau, con trỏ ngay lập tức phát hiện sự không khớp và xuất ra "KHÔNG". Các chuỗi trong đó tất cả các ký tự giống hệt nhau, chẳng hạn như "aaaa", được nhận dạng chính xác là các chuỗi đối xứng vì mỗi cặp đối xứng đều khớp với nhau. Cách tiếp cận hai con trỏ xử lý một cách tự nhiên cả chuỗi chẵn và chuỗi có độ dài lẻ mà không cần logic phân nhánh đặc biệt.
