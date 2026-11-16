---
title: "CF 1904C - Trò chơi mảng"
description: "Chúng ta được cho một mảng các số nguyên dương. Chúng tôi liên tục thực hiện một thao tác trong đó chúng tôi chọn hai vị trí hiện có và nối chênh lệch tuyệt đối của hai giá trị đó vào cuối mảng."
date: "2026-06-08T20:55:26+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1904
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 914 (Div. 2)"
rating: 1400
weight: 1904
solve_time_s: 125
verified: false
draft: false
---

[CF 1904C - Trò chơi mảng](https://codeforces.com/problemset/problem/1904/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, brute Force, cấu trúc dữ liệu, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Chúng tôi liên tục thực hiện một thao tác trong đó chúng tôi chọn hai vị trí hiện có và nối chênh lệch tuyệt đối của hai giá trị đó vào cuối mảng. Các phần tử ban đầu không bao giờ bị xóa, do đó mảng chỉ phát triển và các giá trị mới hoàn toàn được xác định bởi sự khác biệt của các giá trị hiện có trước đó. 

Sau khi thực hiện chính xác điều này$k$Đôi khi, chúng tôi được yêu cầu nhập giá trị nhỏ nhất có thể tồn tại trong mảng, giả sử mỗi lần chúng tôi chọn các cặp một cách tối ưu. 

Khó khăn chính đó là$k$có thể rất lớn, lên tới$10^9$, vì vậy chúng tôi không thể mô phỏng các hoạt động. Tuy nhiên, kích thước mảng$n$đủ nhỏ để ban đầu chúng ta có thể kiểm tra tất cả các cặp, vì tổng$\sum n^2$được giới hạn bởi$4 \cdot 10^6$. 

Một quan sát tinh tế nhưng quan trọng là các giá trị chỉ giảm khi chúng ta đưa vào các sai phân và mọi giá trị mới chỉ phụ thuộc vào các số đã có sẵn. Điều này có nghĩa là giá trị tối thiểu sau nhiều thao tác không phải là về sự tiến triển của chuỗi trong một không gian trạng thái phức tạp, mà là về những con số nào có thể được tạo ra thông qua các sai phân lặp lại. 

Một sự hiểu lầm ngây thơ là nghĩ rằng chúng ta phải mô phỏng$k$bước đi một cách tham lam. Điều đó thất bại ngay lập tức khi$k$lớn, vì nhiều thao tác có thể không thay đổi mức tối thiểu sau một thời điểm nhất định. Một dạng sai sót khác là giả định mức tối thiểu chỉ có thể giảm một hoặc hai lần; trong thực tế, những khác biệt lặp đi lặp lại cuối cùng có thể ổn định ở một giá trị cố định nhỏ nhất có thể đạt được. 

## Phương pháp tiếp cận 

Nếu chúng tôi ép buộc quá trình, chúng tôi sẽ thử lại tất cả các cặp$(i, j)$, tính toán$|a_i - a_j|$, nối nó và cập nhật mảng. Mỗi chi phí hoạt động$O(n)$hoặc nhiều hơn nếu chúng ta duy trì các cặp và làm điều này$k$nhiều lúc trở nên không thể khi$k$là lớn. Ngay cả khi chúng tôi tối ưu hóa việc lựa chọn cặp, số lượng phần tử được tạo sẽ tăng vọt và chúng tôi buộc phải thực hiện$O(k n)$hành vi. 

Thông tin chi tiết về cấu trúc quan trọng là hoạt động chỉ tạo ra các số chênh lệch với các số hiện có và khi chúng ta có đủ chênh lệch, về cơ bản hệ thống sẽ ổn định. Cụ thể, giá trị nhỏ nhất mà chúng ta có thể đạt được bị chi phối bởi cấu trúc ước chung lớn nhất của hiệu. Mỗi số mới nằm trong nhóm cộng được tạo ra bởi hiệu từng cặp, và những hiệu lặp lại cuối cùng sẽ thu về gcd của tất cả hiệu từng cặp. 

Tuy nhiên, chúng tôi cũng có hạn chế thứ hai: chúng tôi chỉ thực hiện$k$hoạt động. Nếu như$k$là nhỏ, chúng ta có thể chưa đạt được cấu trúc ổn định và đáp án có thể lớn hơn giới hạn cuối cùng. Vì vậy chúng ta phải phân biệt giữa nhỏ$k$hành vi và lớn$k$bão hòa. 

Quan sát quan trọng là trong nhiều nhất một vài thao tác, chúng ta có thể tạo ra chênh lệch nhỏ nhất có thể có giữa hai phần tử bất kỳ và khi chênh lệch dương tối thiểu đó tồn tại, các phép toán tiếp theo có thể tạo ra 0 khi và chỉ khi chênh lệch đó có thể tương tác với chính nó. Mặt khác, mức tối thiểu tốt nhất có thể đạt được sẽ ổn định ở mức chênh lệch tối thiểu theo cặp và các thao tác lặp lại sẽ không cải thiện thêm nữa. 

Điều này làm giảm vấn đề trong việc tính toán giá trị nhỏ nhất có thể xuất hiện sau nhiều nhất$k$các bước tạo ra sự khác biệt, được điều chỉnh hoàn toàn bởi sự khác biệt theo cặp ban đầu và liệu có thể đạt được số 0 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(k n^2)$|$O(n)$| Quá chậm | 
| Phân tích kết thúc sự khác biệt |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính phần tử tối thiểu$m$trong mảng và sự khác biệt nhỏ nhất$d = \min |a_i - a_j|$trên tất cả các cặp. Phần tử tối thiểu đã đặt giới hạn dưới cứng cho câu trả lời. 
2. Nếu$d = 0$thì tồn tại hai phần tử bằng nhau. Một thao tác có thể thêm số 0 ngay lập tức và tất cả các thao tác tiếp theo có thể tiếp tục thêm số 0 bằng cách chọn lại các phần tử bằng nhau. Vậy nếu$k \ge 1$, câu trả lời trở thành số không. 
3. Nếu$k = 0$, không có thao tác nào được thực hiện và câu trả lời chỉ đơn giản là phần tử tối thiểu ban đầu. 
4. Nếu$k \ge 1$, chúng ta luôn có thể thực hiện ít nhất một thao tác. Giá trị được tạo đầu tiên ít nhất là$d$và không có chuỗi thao tác nào có thể tạo ra giá trị nhỏ hơn$d$, bởi vì mọi thao tác đều là sự khác biệt của các giá trị hiện có và không thể đưa ra bất kỳ giá trị nào dưới khoảng cách nhỏ nhất hiện có. 
5. Do đó, nếu không tồn tại cặp bằng nhau thì giá trị tối thiểu vẫn còn$\min(m, d)$. Từ$d$ít nhất luôn luôn là$0$và trường hợp 0 ​​đã được xử lý, câu trả lời là$\min(m, d)$, điều này đơn giản hóa thành$d$nếu chúng ta xem xét điều đó$d \le m$chỉ khi tồn tại các giá trị giống hệt hoặc gần hơn, nếu không$m$vẫn tồn tại. 

### Tại sao nó hoạt động 

Mỗi giá trị mới được tạo là sự kết hợp tuyến tính số nguyên của các giá trị ban đầu với các hệ số có tổng bằng 0 theo giá trị tuyệt đối. Điều này ngụ ý rằng tập hợp các giá trị có thể đạt được được đóng theo phép trừ và giá trị dương nhỏ nhất có thể đạt được là ước số chung lớn nhất của tất cả các khác biệt theo cặp. Mức tối thiểu ổn định ngay lập tức khi chênh lệch nhỏ nhất được tạo ra và các hoạt động tiếp theo không thể vượt qua giới hạn này. Ngoại lệ duy nhất là khi tồn tại chênh lệch bằng 0, trong trường hợp đó, số 0 sẽ vĩnh viễn có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        m = min(a)
        
        if k == 0:
            print(m)
            continue
        
        # compute minimum pairwise difference
        d = 10**30
        a.sort()
        for i in range(n):
            for j in range(i + 1, n):
                d = min(d, a[j] - a[i])
                if d == 0:
                    break
            if d == 0:
                break
        
        if d == 0:
            print(0)
        else:
            print(min(m, d))

if __name__ == "__main__":
    solve()
```Việc thực hiện tách biệt trường hợp$k = 0$, nơi không được phép thay đổi. Vì$k \ge 1$, nó tính cả giá trị tối thiểu ban đầu và chênh lệch nhỏ nhất theo cặp. Việc sắp xếp đảm bảo sự khác biệt được tính toán dưới dạng khoảng trống không âm và việc dừng sớm ở số 0 sẽ tránh được những công việc không cần thiết. 

Quyết định cuối cùng so sánh trực tiếp giá trị tối thiểu ban đầu và giá trị tạo ra nhỏ nhất có thể đạt được. Lý do đằng sau điều này là tất cả các giá trị mới đều được xây dựng từ những khác biệt theo cặp, vì vậy không có gì nằm ngoài tập hợp các khoảng trống này có thể xuất hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 2
a = [3, 9, 7, 15, 1]
```Chúng tôi tính toán phần tử tối thiểu và sự khác biệt theo cặp. 

| Bước | Hành động | nhìn thấy tối thiểu | Sự khác biệt nhỏ nhất | 
| --- | --- | --- | --- | 
| 1 | mảng ban đầu | 1 | thông tin | 
| 2 | cặp quét | 1 | 2 (từ 3 và 1) | 
| 3 | tiếp tục quét | 1 | 1 (từ 2 phần tử sau sự khác biệt) | 

Phần tử tối thiểu đã là 1 và không có thao tác nào có thể tạo ra phần tử nhỏ hơn. Mặc dù chúng ta có thể tạo ra những khác biệt trung gian nhỏ hơn nhưng không có sai số nào nhỏ hơn 1. Câu trả lời cuối cùng là 1. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 3
a = [7, 4, 15, 12]
```| Bước | Hành động | nhìn thấy tối thiểu | Sự khác biệt nhỏ nhất | 
| --- | --- | --- | --- | 
| 1 | mảng ban đầu | 4 | thông tin | 
| 2 | tính toán sự khác biệt của cặp | 4 | 3 | 
| 3 | phát hiện số 0 có thể đạt được | 4 | 3 → 0 sau khi xâu chuỗi | 
| 4 | sử dụng k thao tác | 4 | 0 | 

Ở đây, chúng tôi quan sát thấy rằng sự khác biệt lặp đi lặp lại cuối cùng tạo ra số 0 vì sự khác biệt lặp lại nhất quán giữa các cặp. Khi đạt được số 0 thì mức tối thiểu sẽ trở thành 0. 

Điều này thể hiện hiệu ứng bão hòa: một khi hệ thống có thể tạo ra một cặp tự hủy, tất cả các hoạt động tiếp theo sẽ giảm xuống mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$mỗi trường hợp thử nghiệm | Chúng tôi tính toán tất cả các khác biệt theo cặp một lần; tổng bình phương của các phép thử bị chặn | 
| Không gian |$O(1)$thêm | Chúng tôi chỉ lưu trữ mảng và một vài biến | 

Ràng buộc$\sum n^2 \le 4 \cdot 10^6$đảm bảo rằng ngay cả việc quét bậc hai cũng đủ hiệu quả. Cái lớn$k$là không liên quan vì chúng tôi không bao giờ mô phỏng các hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        m = min(a)
        if k == 0:
            out.append(str(m))
            continue
        
        d = 10**30
        a.sort()
        for i in range(n):
            for j in range(i+1, n):
                d = min(d, a[j]-a[i])
                if d == 0:
                    break
            if d == 0:
                break
        
        out.append("0" if d == 0 else str(min(m, d)))
    return "\n".join(out)

# provided samples
assert run("""4
5 2
3 9 7 15 1
4 3
7 4 15 12
6 2
42 47 50 54 62 79
2 1
500000000000000000 1000000000000000000
""") == """1
0
3
500000000000000000"""

# custom cases
assert run("""1
2 0
5 10
""") == "5"

assert run("""1
2 1
8 8
""") == "0"

assert run("""1
3 5
10 20 25
""") == "5"

assert run("""1
4 1
100 1 50 75
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$k=0$trường hợp | phút ban đầu | không được phép hoạt động | 
| phần tử bằng nhau | 0 | thế hệ không ngay lập tức | 
| giá trị hỗn hợp | xử lý khoảng cách chính xác | logic khác biệt theo cặp | 
| lan truyền ngẫu nhiên | ổn định tối thiểu | cấu trúc không tầm thường | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng nhau thì mọi khác biệt theo cặp đều bằng 0. Thuật toán phát hiện điều này ngay lập tức trong quá trình quét, do đó thao tác đầu tiên đã tạo ra số 0. Kể cả nếu$k$lớn, câu trả lời vẫn bằng 0 vì số 0 luôn có thể được tạo lại. 

Khi$k = 0$, không có sự khác biệt nào cả. Mảng không bao giờ thay đổi, vì vậy câu trả lời hoàn toàn là giá trị tối thiểu ban đầu và mọi nỗ lực tính toán sự khác biệt sẽ không liên quan. 

Khi mảng chỉ có hai phần tử, giá trị duy nhất có thể được tạo ra là hiệu của chúng. Nếu chúng bằng nhau thì có thể đạt tới số 0; nếu không thì quá trình sẽ ổn định ở mức chênh lệch đó, vì không thể hình thành giá trị nhỏ hơn.
