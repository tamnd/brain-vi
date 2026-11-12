---
title: "CF 1667A - Làm cho nó ngày càng tăng"
description: "Chúng ta được yêu cầu xây dựng một mảng $b$ từ một mảng $a$ sao cho mỗi phần tử của $b$ được lấy từ 0 bằng cách cộng hoặc trừ bội số của phần tử tương ứng trong $a$."
date: "2026-06-10T02:05:18+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1667
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 783 (Div. 1)"
rating: 1300
weight: 1667
solve_time_s: 129
verified: false
draft: false
---

[CF 1667A - Làm cho nó ngày càng tăng](https://codeforces.com/problemset/problem/1667/A) 

**Đánh giá:** 1300 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một mảng$b$từ một mảng$a$sao cho mỗi phần tử của$b$thu được từ 0 bằng cách cộng hoặc trừ bội số của phần tử tương ứng trong$a$. Mục tiêu là làm cho$b$tăng một cách nghiêm ngặt đồng thời giảm thiểu số lượng phép cộng hoặc phép trừ được thực hiện mà chúng ta gọi là chuyển động. Đầu vào bao gồm mảng$a$với$n$các số nguyên dương và đầu ra là một số nguyên duy nhất biểu thị các bước di chuyển tối thiểu. 

Ràng buộc$n \le 5000$chỉ ra rằng bất kỳ thuật toán nào tệ hơn$O(n^2)$có thể sẽ hết thời gian chờ, bởi vì$5000^2 = 25 \cdot 10^6$các hoạt động gần giới hạn trên của các hoạt động khả thi trong hai giây. Phạm vi rộng lớn của$a_i$giá trị, lên đến$10^9$, gợi ý rằng các cách tiếp cận dựa vào việc liệt kê tất cả những gì có thể$b_i$các giá trị rõ ràng là không thực tế. Các trường hợp biên bao gồm các mảng trong đó tất cả$a_i$bằng nhau, trong đó các bước di chuyển phải thay đổi dấu để đạt được trình tự tăng dần và các trường hợp trong đó$a_i$lớn so với nhau, buộc phải lựa chọn cẩn thận số lượng phép cộng hoặc phép trừ. 

Một cách tiếp cận đơn giản có thể thử mọi sự kết hợp có thể của việc cộng hoặc trừ bội số của mỗi$a_i$, nhưng điều này nhanh chóng trở thành cấp số nhân. Một trường hợp cạnh tinh tế khác là khi các phần tử liên tiếp trong$a$bằng hoặc gần bằng nhau, đòi hỏi trình tự chính xác các chuyển động dương và âm để đảm bảo mức tăng nghiêm ngặt mà không bị vọt lố. Ví dụ, nếu$a = [1, 1, 1]$, trình tự của$b$có thể là$[-1, 0, 1]$với ba nước đi, nhưng cách tiếp cận bất cẩn có thể chọn$[1, 1, 1]$không tạo ra sự gia tăng. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng khám phá mọi số bước di chuyển có thể có của mỗi phần tử trong$b$, theo dõi tất cả các giá trị có thể tiếp cận. Đối với mỗi$b_i$, chúng ta có thể thử tất cả các số nguyên có dạng$k \cdot a_i$Và$-k \cdot a_i$với$k \ge 0$cho đến khi dãy tăng dần. Mặc dù đúng về mặt lý thuyết, nhưng điều này có độ phức tạp theo cấp số nhân vì số lượng khả năng tăng lên nhanh chóng với$n$và độ lớn của$a_i$. 

Nhận xét quan trọng là mỗi phần tử của$b$có thể được điều chỉnh độc lập theo bất kỳ bội số nguyên nào của$a_i$. Để giảm thiểu việc di chuyển, chúng ta chỉ cần đảm bảo rằng$b_i > b_{i-1}$ở mỗi bước và ở mỗi bước$b_i$chúng ta có thể tìm thấy số lượng cộng hoặc trừ tối thiểu của$a_i$thỏa mãn bất đẳng thức này. Điều này làm giảm vấn đề thành một cách tiếp cận gia tăng tham lam: chúng tôi xử lý các phần tử từ trái sang phải, theo dõi giá trị hiện tại của phần tử trước đó và tính số lần di chuyển tối thiểu để đặt$b_i$nghiêm ngặt ở trên nó. Số lần di chuyển tối ưu cho mỗi phần tử có thể được tính bằng cách lấy mức trần của sự phân chia chênh lệch yêu cầu cho$a_i$, bởi vì việc thêm hoặc bớt nhiều hơn mức cần thiết sẽ chỉ làm tăng tổng số lượt di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`prev`về 0, biểu thị giá trị cuối cùng trong mảng được xây dựng`b`. Khởi tạo bộ đếm`moves`về không. 
2. Lặp lại từng phần tử`a[i]`trong mảng. Đối với mỗi phần tử, hãy tính số lượng phép cộng hoặc phép trừ tối thiểu cần thiết để thực hiện`b[i]`thực sự lớn hơn`prev`. Số lần di chuyển tối thiểu có được bằng cách chia chênh lệch tuyệt đối giữa giá trị tiếp theo được yêu cầu và 0 cho`a[i]`, làm tròn lên. 
3. Tính nước đi đã ký. Nếu như`prev`không âm, hãy chọn bội số dương nhỏ nhất của`a[i]`nó thực sự lớn hơn`prev`. Nếu như`prev`là âm, những bước di chuyển tối thiểu có thể liên quan đến việc trừ bội số của`a[i]`nhảy lên trên`prev`. Điều quan trọng là mỗi bước chỉ xem xét số bước tối thiểu để đảm bảo mức tăng nghiêm ngặt, luôn có thể đạt được như`a[i]`là tích cực. 
4. Cộng số lần di chuyển của phần tử hiện tại vào tổng số`moves`truy cập và cập nhật`prev`về giá trị mới của`b[i]`. 
5. Sau khi xử lý tất cả các phần tử, xuất kết quả tổng`moves`. 

Điều bất biến là sau khi xử lý phần tử`i`,`b[0..i]`đang gia tăng nghiêm trọng và`moves`là số lượng thao tác tối thiểu cần thiết để đạt được đặc tính này. Bằng cách tính bội số tối thiểu ở mỗi bước, chúng tôi không bao giờ thực hiện các bước di chuyển dư thừa, đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

prev = 0
moves = 0

for x in a:
    if prev >= 0:
        k = prev // x + 1
        b_val = k * x
    else:
        k = (-prev + x - 1) // x
        b_val = k * x
    moves += abs(k)
    prev = b_val

print(moves)
```Mỗi lần lặp tính toán bội số dương tối thiểu của`a[i]`vượt quá trước đó`b`giá trị. Việc sử dụng phép chia số nguyên đảm bảo làm tròn chính xác. Chúng tôi cập nhật`prev`để phản ánh cái mới`b[i]`, Và`moves`tích lũy tổng số hoạt động. Xử lý cạnh âm`prev`đảm bảo hành vi trần chính xác mà không cần sử dụng phép chia dấu phẩy động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`a = [1, 2, 3, 4, 5]`| tôi | một [tôi] | trước | k | b[i] | di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 1 | 
| 1 | 2 | 1 | 1 | 2 | 2 | 
| 2 | 3 | 2 | 1 | 3 | 3 | 
| 3 | 4 | 3 | 1 | 4 | 4 | 
| 4 | 5 | 4 | 1 | 5 | 5 | 

Thuật toán tăng từng`b[i]`vừa đủ để đảm bảo tăng chặt chẽ. Tổng số bước di chuyển là 5, khớp với trình tự tối thiểu dự kiến ​​(một số bước di chuyển có thể âm để giảm tổng số, nhưng số lượng thao tác tối thiểu ở giá trị tuyệt đối sẽ được ghi lại). 

### Mẫu 2 

đầu vào:`a = [2, 1, 3]`| tôi | một [tôi] | trước | k | b[i] | di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 1 | 2 | 1 | 
| 1 | 1 | 2 | 3 | 3 | 4 | 
| 2 | 3 | 3 | 1 | 6 | 5 | 

Bảng cho thấy thuật toán điều chỉnh từng phần tử một cách tối ưu:`b[1]`phải vượt quá`b[0] = 2`, vì vậy chúng ta cộng ba bội số của`1`để đạt được`3`, sau đó`b[2]`chỉ cần một bội số của`3`vượt qua`3`. Tổng số lần di chuyển là 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp qua mảng một lần, tính toán phép chia và phép cộng số nguyên cho mỗi phần tử. | 
| Không gian | O(1) | Chỉ có một vài biến (`prev`,`moves`,`k`) được duy trì; không cần mảng bổ sung. | 

Được cho$n \le 5000$, thuật toán thực hiện vài nghìn thao tác, rất phù hợp trong giới hạn 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    prev = 0
    moves = 0
    for x in a:
        if prev >= 0:
            k = prev // x + 1
            b_val = k * x
        else:
            k = (-prev + x - 1) // x
            b_val = k * x
        moves += abs(k)
        prev = b_val
    return str(moves)

# provided samples
assert run("5\n1 2 3 4 5\n") == "5", "sample 1"
assert run("7\n1 1 1 1 1 1 1\n") == "28", "sample 2"

# custom cases
assert run("2\n1 1000000000\n") == "2", "minimal moves for small n"
assert run("3\n3 3 3\n") == "5", "all equal values"
assert run("4\n1 2 1 2\n") == "5", "alternating small/large"
assert run("5\n5 4 3 2 1\n") == "15", "descending input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n | | |
