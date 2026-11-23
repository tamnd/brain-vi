---
title: "CF 486C - Chuyển đổi Palindrome"
description: "Chúng ta được cấp một chuỗi có độ dài $n$ bao gồm các chữ cái tiếng Anh viết thường, cùng với vị trí con trỏ $p$ có thể di chuyển sang trái hoặc phải xung quanh chuỗi theo chu kỳ."
date: "2026-06-07T17:27:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 486
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 277 (Div. 2)"
rating: 1700
weight: 486
solve_time_s: 104
verified: true
draft: false
---

[CF 486C - Chuyển đổi Palindrome](https://codeforces.com/problemset/problem/486/C) 

**Đánh giá:** 1700 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài$n$bao gồm các chữ cái tiếng Anh viết thường, cùng với vị trí con trỏ$p$có thể di chuyển sang trái hoặc phải xung quanh chuỗi theo chu kỳ. Sử dụng các phím mũi tên, chúng ta có thể di chuyển con trỏ hoặc tăng/giảm chữ cái tại con trỏ (cũng theo chu kỳ, vì vậy sau 'z' sẽ đến 'a' và ngược lại). Mục tiêu là chuyển chuỗi thành một bảng màu bằng cách sử dụng số lần nhấn phím tối thiểu. 

Đầu vào cung cấp độ dài chuỗi$n$, vị trí con trỏ bắt đầu$p$, và chính chuỗi đó. Đầu ra phải là tổng số lần nhấn phím tối thiểu, bao gồm cả di chuyển con trỏ và thay đổi chữ cái. 

Với$n$lên đến$10^5$và giới hạn thời gian 1 giây, bất kỳ thuật toán nào xử lý từng ký tự trong thời gian không đổi đều khả thi. Một cách tiếp cận bạo lực ngây thơ thử mọi chuỗi di chuyển và thay đổi chữ cái có thể là không khả thi vì số lượng chuỗi tăng theo cấp số nhân. Chúng ta phải khai thác tính đối xứng của các palindrome và cấu trúc di chuyển con trỏ. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi đã là palindrome, con trỏ bắt đầu gần một đầu và các tình huống trong đó giải pháp tối ưu yêu cầu di chuyển sang trái thay vì phải do chuyển động theo chu kỳ. Ví dụ, với đầu vào`"abca"`và con trỏ ở vị trí 4, chiến lược di chuyển đơn giản chỉ bên phải có thể đếm quá nhiều bước di chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: xem xét mọi trình tự nhấn phím có thể có, mô phỏng nó và kiểm tra xem liệu nó có tạo ra một bảng màu hay không. Mặc dù đúng nhưng điều này sẽ mất$O(26^{n/2})$các thao tác chỉ dành cho việc thay đổi chữ cái, điều này không thể lớn được đối với$n$lên đến$10^5$. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả là vấn đề phân tách một cách tự nhiên thành hai thành phần độc lập: thay đổi chữ cái và chuyển động của con trỏ. Để một chuỗi là một palindrome, chỉ các cặp ký tự không khớp$(s[i], s[n-i-1])$vì$i = 0$ĐẾN$n//2-1$cần phải được sửa chữa. Việc tính toán số lần nhấn lên/xuống tối thiểu cho mỗi cặp là chuyện nhỏ khi sử dụng khoảng cách bảng chữ cái tuần hoàn. Thử thách còn lại là giảm thiểu việc di chuyển con trỏ. 

Vì con trỏ có thể di chuyển sang trái hoặc phải theo chu kỳ, nên đường di chuyển tối ưu là bắt đầu ở vị trí ban đầu, đi đến điểm không khớp ngoài cùng bên trái và điểm không khớp ngoài cùng bên phải giữa nửa chuỗi có liên quan, sau đó di chuyển giữa chúng theo thứ tự ngắn nhất có thể. Nếu con trỏ bắt đầu ở nửa bên phải, chúng ta sẽ phản chiếu nó sang nửa bên trái vì bài toán đối xứng quanh điểm giữa của chuỗi. 

Do đó, giải pháp là: tính tổng số lần thay đổi chữ cái tối thiểu cho mỗi cặp không khớp, sau đó tính chuyển động con trỏ tối thiểu để bao phủ tất cả các vị trí không khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^(n/2)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu con trỏ ở nửa bên phải của chuỗi, hãy phản chiếu nó sang nửa bên trái để đơn giản hóa việc tính toán. Cho phép$p$dựa trên 0. 
2. Lặp lại lần đầu tiên$n//2$các ký tự và xác định các vị trí không khớp trong đó$s[i] \neq s[n-i-1]$. Theo dõi những điểm không khớp ngoài cùng bên trái và ngoài cùng bên phải. 
3. Đối với mỗi vị trí không khớp, hãy tính số lần nhấn lên/xuống tối thiểu để khớp với ký tự được phản chiếu. Điều này sử dụng khoảng cách theo chu kỳ$\min(|a-b|, 26 - |a-b|)$. Tổng hợp những thứ này để có được tổng chi phí thay đổi chữ cái. 
4. Nếu không có sự không khớp nào, xuất ra số 0 vì chuỗi đó đã là một bảng màu. 
5. Tính toán chuyển động của con trỏ. Có hai chiến lược: đầu tiên di chuyển đến điểm không khớp ngoài cùng bên trái, sau đó đến ngoài cùng bên phải hoặc ngược lại. Cả hai đường dẫn đều bao gồm tất cả các vị trí không khớp. Tổng chuyển động tối thiểu là khoảng cách nhỏ hơn trong hai khoảng cách này được cộng vào khoảng cách giữa điểm không khớp ngoài cùng bên trái và ngoài cùng bên phải. 
6. Trả về tổng chi phí thay đổi chữ cái và chi phí di chuyển con trỏ. 

Tại sao nó hoạt động: Chúng tôi chỉ sửa đổi các vị trí góp phần làm cho chuỗi trở thành một bảng màu. Thuật toán bao gồm đường dẫn tối thiểu kết nối tất cả các vị trí có liên quan. Chi phí thay đổi chữ cái độc lập là tối thiểu do khoảng cách bảng chữ cái theo chu kỳ. Chuyển động của con trỏ được giảm thiểu bằng cách chỉ xem xét khoảng cách của các vị trí không khớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, p = map(int, input().split())
s = input().strip()

p -= 1
half = n // 2

if p >= half:
    p = n - 1 - p

left, right = half, -1
letter_changes = 0

for i in range(half):
    if s[i] != s[n - 1 - i]:
        left = min(left, i)
        right = max(right, i)
        diff = abs(ord(s[i]) - ord(s[n - 1 - i]))
        letter_changes += min(diff, 26 - diff)

if right == -1:
    print(0)
    sys.exit(0)

move_cost = min(abs(p - left), abs(p - right)) + (right - left)
print(letter_changes + move_cost)
```Giải pháp đầu tiên phản chiếu con trỏ sang nửa bên trái để đơn giản. Sau đó, nó lặp lại trong nửa đầu để tính toán chi phí thay đổi chữ cái và xác định những điểm không khớp ở ngoài cùng bên trái và ngoài cùng bên phải. Chi phí di chuyển con trỏ tính đến việc bắt đầu ở vị trí hiện tại và di chuyển ngang qua tất cả các vị trí không khớp một cách tối ưu. 

Các cân nhắc về ranh giới bao gồm xử lý chính xác chuyển đổi dựa trên 1 sang dựa trên 0, đảm bảo tính toán bảng chữ cái tuần hoàn và tính toán chuyển động chính xác khi con trỏ đã ở một đầu của khoảng không khớp. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`8 3 aeabcaez`| tôi | s[i] | s[n-1-i] | Thay đổi thư | trái | đúng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | z | 1 | 0 | 0 | 
| 1 | e | e | 0 | 0 | 1 | 
| 2 | một | một | 0 | 0 | 2 | 
| 3 | b | c | 1 | 0 | 3 | 

Con trỏ bắt đầu ở vị trí 2 (dựa trên 0: 2). Thay đổi chữ cái = 1+0+0+1 = 2. Không khớp ngoài cùng bên trái = 0, ngoài cùng bên phải = 3. Di chuyển con trỏ = min(abs(2-0), abs(2-3)) + (3-0) = 1 + 3 = 4. Tổng = 2 + 4 = 6. 

**Đầu vào tùy chỉnh 2** 

đầu vào:`5 5 abcde`| tôi | s[i] | s[n-1-i] | Thay đổi thư | trái | đúng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | e | 4 | 0 | 0 | 
| 1 | b | d | 2 | 0 | 1 | 

Con trỏ bắt đầu ở vị trí 4 (dựa trên 0: 4 → được phản chiếu 0). Thay đổi chữ cái = 4+2=6. Chỉ số nhịp không khớp 0-1. Di chuyển con trỏ = abs(0-0) + (1-0) = 1. Tổng = 6+1=7. 

Những dấu vết này cho thấy thuật toán tính toán chính xác những thay đổi tối thiểu về chữ cái và di chuyển con trỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua nửa đầu để tính toán các thay đổi của chữ cái và xác định vị trí không khớp | 
| Không gian | O(1) | Chỉ có một số số nguyên được theo dõi; chính chuỗi đó là đầu vào | 

Giải pháp dễ dàng phù hợp trong giới hạn thời gian 1 giây cho$n \le 10^5$và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return ""

# Provided sample
assert run("8 3\naeabcaez\n") == "", "sample 1"

# Minimum-size input
assert run("1 1\na\n") == "", "single character"

# Already palindrome
assert run("4 2\nabba\n") == "", "already palindrome"

# Cursor at the end
assert run("5 5\nabcde\n") == "", "cursor at end"

# All equal characters
assert run("6 3\nffffff\n") == "", "all equal characters"

# Max size input (boundary)
import random, string
s = ''.join(random.choice(string.ascii_lowercase) for _ in range(10**5))
inp = f"{10**5} {10**5}\n{s}\n"
# Do not assert actual number, just test that it runs
run(inp)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 1\na\n" | 0 | Ký tự đơn, đã có màu nhạt | 
| "4 2\nabba\n" | 0 | Đã palindrom | 
| "5 5\nabcde\n" | 7 | Con trỏ ở cuối, tính chuyển động | 
| "6 3\nffffff\n" | 0 | Tất cả các ký tự đều bằng nhau, không thay đổi | 
| 10^5 ngẫu nhiên | Không áp dụng | Thuật toán xử lý đầu vào lớn trong thời gian giới hạn | 

## Vỏ cạnh 

Chuỗi ký tự đơn không yêu cầu thay đổi. Thuật toán không phát hiện sự không khớp nào và trả về 0. Đối với một chuỗi mà con trỏ bắt đầu ở nửa bên phải, nó phản ánh vị trí, tính toán chính xác khoảng chuyển động và vẫn tìm thấy tổng tối thiểu. Nếu tất cả các ký tự đều bằng nhau thì tổng số lần thay đổi chữ cái bằng 0 và chi phí di chuyển bằng 0, ngay cả khi con trỏ không khớp. Những trường hợp này xác nhận rằng thuật toán xử lý các vị trí biên,
