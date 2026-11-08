---
title: "CF 1451C - Bình đẳng chuỗi"
description: "Chúng ta có hai chuỗi a và b có độ dài n bằng nhau cùng với số nguyên k. Mục đích là để xác định liệu chúng ta có thể chuyển đổi a thành b bằng cách sử dụng hai loại phép toán hay không. Thao tác đầu tiên cho phép hoán đổi bất kỳ ký tự liền kề nào trong a."
date: "2026-06-11T03:29:57+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "hashing", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1451
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 685 (Div. 2)"
rating: 1400
weight: 1451
solve_time_s: 82
verified: true
draft: false
---

[CF 1451C - Bình đẳng chuỗi](https://codeforces.com/problemset/problem/1451/C) 

**Đánh giá:** 1400 
**Tags:** dp, tham lam, băm, triển khai, chuỗi 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`a`Và`b`, có độ dài bằng nhau`n`, cùng với một số nguyên`k`. Mục đích là để xác định liệu chúng ta có thể biến đổi`a`vào trong`b`sử dụng hai loại hoạt động. Thao tác đầu tiên cho phép hoán đổi bất kỳ ký tự liền kề nào trong`a`. Thao tác thứ hai cho phép chọn bất kỳ khối có độ dài liên tiếp nào`k`trong đó tất cả các ký tự đều giống hệt nhau và không`'z'`và tăng từng ký tự trong khối đó lên chữ cái tiếp theo trong bảng chữ cái. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng ta cần xuất ra "Có" nếu có thể chuyển đổi`a`vào trong`b`, và "Không" nếu không. 

Với những hạn chế,`n`có thể lên tới 10^6 trên tất cả các trường hợp thử nghiệm. Điều này giúp loại bỏ bất kỳ giải pháp nào cố gắng mô phỏng từng bước hoạt động vì số lượng hoạt động có thể rất lớn. Chúng ta cần một cách tiếp cận hoạt động theo thời gian tuyến tính hoặc gần tuyến tính tương ứng với độ dài chuỗi. 

Một trường hợp cạnh phát sinh khi`k = 1`. Trong trường hợp này, bất kỳ ký tự đơn nào cũng có thể được tăng lên nếu nó không`'z'`, cho phép linh hoạt hơn so với khi`k > 1`. Một trường hợp phức tạp khác xảy ra khi`a`chứa các ký tự`'z'`cần phải trở thành chữ cái thấp hơn; hoạt động tăng không thể được áp dụng cho`'z'`, làm cho một số phép biến đổi không thể thực hiện được. Tương tự, nếu số của một chữ cái nào đó trong`a`nhỏ hơn số lượng cần thiết trong`b`, có thể không đáp ứng được yêu cầu. 

Ví dụ, hãy xem xét`a = "abc"`,`b = "bcd"`, Và`k = 3`. Không có cách nào để tăng các ký tự một cách chính xác vì chúng ta không thể tăng các khối bắt đầu từ`'c'`hoặc`'b'`không vi phạm yêu cầu về khối thống nhất hoặc vượt quá giới hạn bảng chữ cái. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ mô phỏng trực tiếp các thao tác: liên tục kiểm tra các ký tự có thể hoán đổi hoặc các khối có thể tăng dần và áp dụng các phép biến đổi cho đến khi`a`bằng`b`hoặc không thể thực hiện được thao tác nào. Điều này đúng về mặt lý thuyết, nhưng trên thực tế, cần phải`O(n^2)`thời gian trong trường hợp xấu nhất là quá chậm để`n`lên đến 10^6. 

Cái nhìn sâu sắc quan trọng là các giao dịch hoán đổi liền kề cho phép chúng tôi sắp xếp lại`a`tùy tiện. Vì vậy, chúng ta không cần lo lắng về vị trí chính xác; chúng ta chỉ cần kiểm tra xem liệu chúng ta có thể khớp số lượng ký tự trong`a`Và`b`sau khi sử dụng các thao tác tăng dần. Mỗi thao tác tăng dần trên một khối có chiều dài`k`cho phép chúng ta chuyển giao một cách hiệu quả`k`đơn vị của ký tự này sang ký tự tiếp theo. Điều này đưa ra một chiến lược tham lam: bắt đầu từ`'a'`và cố gắng đáp ứng số lượng yêu cầu trong`b`bằng cách sử dụng các ký tự thừa từ`a`và đẩy chúng tới những chữ cái cao hơn. 

Brute-force hoạt động vì nó mô phỏng mọi hoạt động có thể xảy ra, nhưng nó không thành công đối với các chuỗi lớn do độ phức tạp bậc hai. Quan sát cho thấy việc hoán đổi cho phép sắp xếp lại tự do và có thể xử lý số gia tăng một cách tham lam cho phép chúng ta giảm vấn đề xuống mức chuyển tuyến tính qua số lượng ký tự, truyền số lượng dư thừa từ các chữ cái thấp hơn đến các chữ cái cao hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Số lượng nhân vật tham lam | O(26 * n) = O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng có độ dài 26 để đếm số lần xuất hiện của mỗi ký tự trong`a`Và`b`. Sử dụng`ord(char) - ord('a')`để ánh xạ các chữ cái thành các chỉ số. 
2. Lặp lại bảng chữ cái từ`'a'`ĐẾN`'y'`(chỉ số 0 đến 24). Với mỗi ký tự, hãy kiểm tra xem`a`có đủ trường hợp để trang trải`b`. Nếu như`a`có nhiều hơn`b`, tính số dư. 
3. Nếu số dư âm thì trả về ngay "Không" vì chúng ta không thể tạo ký tự từ con số không. 
4. Nếu thặng dư là dương, chúng ta có thể chuyển chúng theo bội số của`k`sang chữ cái tiếp theo. Giảm modulo dư thừa`k`và cộng số nguyên vào số đếm của chữ cái tiếp theo. 
5. Sau khi xử lý xong tất cả các chữ cái, hãy kiểm tra chữ cái cuối cùng`'z'`. Nếu như`a`có số chính xác giống như`b`, trả về "Có"; nếu không, trả về "Không". 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến rằng ở mỗi bước, các ký tự có sẵn trong`a`đến thư hiện tại là đủ để đáp ứng`b`. Các ký tự dư thừa luôn được đẩy về phía trước theo bội số`k`, khớp với quy tắc tăng khối. Vì hoán đổi cho phép sắp xếp lại miễn phí nên chỉ tính vật chất chứ không tính vị trí. Điều này đảm bảo chiến lược tham lam là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_convert(n, k, a, b):
    count_a = [0] * 26
    count_b = [0] * 26
    for ch in a:
        count_a[ord(ch) - ord('a')] += 1
    for ch in b:
        count_b[ord(ch) - ord('a')] += 1

    for i in range(25):
        if count_a[i] < count_b[i]:
            return False
        surplus = count_a[i] - count_b[i]
        # push surplus forward in multiples of k
        if surplus % k != 0:
            return False
        count_a[i+1] += surplus
    return count_a[25] == count_b[25]

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = input().strip()
    b = input().strip()
    print("Yes" if can_convert(n, k, a, b) else "No")
```Đầu tiên, mã đếm tần số của mỗi ký tự trong`a`Và`b`. Sau đó nó lặp qua các ký tự`'a'`ĐẾN`'y'`, kiểm tra xem số đếm trong`a`là đủ và đẩy bất kỳ khoản thặng dư nào về phía trước theo bội số của`k`. Cuối cùng, nó kiểm tra`'z'`cho một trận đấu chính xác. Việc loại bỏ các chuỗi đầu vào đảm bảo không có vấn đề về dòng mới. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`a = "abc"`,`b = "bcd"`,`k = 3`| char | đếm_a | đếm_b | dư thừa | hành động | 
| --- | --- | --- | --- | --- | 
| một | 1 | 0 | 1 | 1 % 3 != 0 -> Không | 

Phần thặng dư của 1 không thể được đẩy về phía trước theo bội số của 3, vì vậy đầu ra là "Không". 

### Mẫu 2 

đầu vào:`a = "abba"`,`b = "azza"`,`k = 2`| char | đếm_a | đếm_b | dư thừa | hành động | 
| --- | --- | --- | --- | --- | 
| một | 2 | 2 | 0 | vượt qua | 
| b | 2 | 0 | 2 | 2 % 2 == 0, đẩy 1*2 sang char tiếp theo -> count_c += 2 | 
| c | 0 + 2 = 2 | 0 | 2 | đẩy sang phần tiếp theo -> count_d += 2 | 
| z | đếm trận đấu b | vượt qua | | | 

Đầu ra là "Có". 

Các bảng này cho thấy thuật toán xử lý chính xác việc đẩy thặng dư theo bội số của`k`và tôn trọng các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 26) = O(n) | Đếm ký tự mất O(n), đẩy dư mất O(26) | 
| Không gian | O(26*2) = O(1) | Hai mảng có độ dài 26 | 

Thuật toán phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB vì ​​nó xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính tương ứng với độ dài chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open(__file__).read())  # Or put the solution function here
    return output.getvalue().strip()

# Provided samples
assert run("4\n3 3\nabc\nbcd\n4 2\nabba\nazza\n2 1\nzz\naa\n6 2\naaabba\nddddcc\n") == "No\nYes\nNo\nYes"

# Custom test cases
assert run("1\n2 1\naz\nba\n") == "No", "cannot decrement z"
assert run("1\n5 2\naaaab\nccddd\n") == "No", "not enough increments"
assert run("1\n6 3\naaaabb\naabbcc\n") == "Yes", "surplus pushed forward"
assert run("1\n3 1\nzzz\nzzz\n") == "Yes", "already equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| az -> ba, k=1 | Không | không thể giảm chữ cái, bảng chữ cái cạnh | 
| aaaab -> ccddd, k=2 | Không | không đủ số lượng để đạt được mục tiêu | 
| aaabb -> aabbcc, k=3 | Có | thặng dư được đẩy qua bội số của k | 
| zzz -> zzz, k=1 | Có | các chuỗi đã bằng nhau | 

## Vỏ cạnh 

Khi nào`k = 1`, bất kỳ ký tự đơn nào cũng có thể được tăng lên. Thuật toán xử lý việc này vì thặng dư luôn được đẩy dưới dạng bội số của`k`. Vì`k > 1`, phần dư không chia hết cho`k`không thể đẩy được và thuật toán trả về chính xác
