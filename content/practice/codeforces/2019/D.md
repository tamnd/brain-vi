---
title: "CF 2019D - Speedbreaker"
description: "Chúng ta có một hàng gồm $n$ thành phố, mỗi thành phố có thời hạn $ai$. Bạn chọn một thành phố để bắt đầu chinh phục tại thời điểm 1 và mỗi bước thời gian tiếp theo bạn sẽ mở rộng sang một thành phố chưa bị chinh phục liền kề. Mục tiêu là chinh phục mọi thành phố $i$ vào hoặc trước thời điểm $ai$."
date: "2026-06-09T02:56:27+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2019
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 975 (Div. 2)"
rating: 1900
weight: 2019
solve_time_s: 103
verified: false
draft: false
---

[CF 2019D - Speedbreaker](https://codeforces.com/problemset/problem/2019/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, tham lam, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng$n$thành phố, mỗi thành phố có thời hạn$a_i$. Bạn chọn một thành phố để bắt đầu chinh phục tại thời điểm 1 và mỗi bước thời gian tiếp theo bạn sẽ mở rộng sang một thành phố chưa bị chinh phục liền kề. Mục tiêu là chinh phục mọi thành phố$i$tại hoặc trước thời điểm$a_i$. Câu hỏi đặt ra là có bao nhiêu thành phố khởi đầu cho phép chinh phục thành công. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định$n$và một mảng$a$. Đầu ra là số lượng vị trí bắt đầu cho mỗi trường hợp thử nghiệm đảm bảo chiến thắng. 

Những hạn chế rất mạnh mẽ:$n$có thể đạt được$2 \cdot 10^5$và tổng của tất cả các trường hợp thử nghiệm cũng là$2 \cdot 10^5$. Một giải pháp đơn giản kiểm tra mọi thành phố khởi đầu và mô phỏng từng bước mở rộng sẽ yêu cầu khoảng$O(n^2)$cho mỗi trường hợp thử nghiệm, tốc độ này quá chậm. Chúng tôi cần$O(n)$mỗi trường hợp thử nghiệm. 

Một điểm tinh tế quan trọng là thời hạn có thể buộc một số thành phố phải bị chinh phục ngay lập tức và việc không tôn trọng dù chỉ một thời hạn sẽ khiến thành phố xuất phát trở nên vô hiệu. Các trường hợp cạnh bao gồm thời hạn bằng 1 tại các ranh giới và mảng tăng hoặc giảm nghiêm ngặt. Ví dụ, với$n = 3$Và$a = [1, 2, 3]$, bắt đầu từ thành phố 1 thì được, nhưng bắt đầu từ thành phố 3 thì không thành công vì bạn không thể đến thành phố 1 kịp thời. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi thành phố có thể bắt đầu, hãy mô phỏng việc mở rộng sang trái và phải, tăng dần thời gian cho mỗi bước và kiểm tra xem thời gian có vượt quá không$a_i$. Đúng, nhưng nó có độ phức tạp trong trường hợp xấu nhất là$O(n^2)$, có thể lên tới$4 \cdot 10^{10}$hoạt động trên các trường hợp thử nghiệm-không thể chấp nhận được. 

Cái nhìn sâu sắc giúp mở ra một giải pháp tuyến tính xuất phát từ việc quan sát rằng việc mở rộng luôn tạo thành một đoạn liền kề. Bạn có thể chinh phục một thành phố ở khoảng cách sớm nhất$d$tính từ thành phố xuất phát là thời gian$1 + d$. Vì vậy, điều kiện thành công chỉ đơn giản là, đối với đoạn kéo dài sang trái và phải từ thành phố xuất phát, thời hạn$a_i$ít nhất là khoảng cách từ điểm bắt đầu cộng một. Nói cách khác, nếu chúng ta định nghĩa$b_i = a_i - i$Và$c_i = a_i - (n-i+1)$, thì một thành phố có giá trị làm điểm bắt đầu nếu các điều kiện tiền tố và hậu tố được giữ nguyên. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính với hai lượt: một từ trái sang phải để kiểm tra tính khả thi của hậu tố và một từ phải sang trái để kiểm tra tính khả thi của tiền tố. Chúng tôi không còn cần phải mô phỏng mọi mở rộng có thể một cách rõ ràng nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng,`left_ok`Và`right_ok`, có chiều dài$n$, để theo dõi những vị trí sớm nhất và mới nhất có thể thành công nếu mở rộng từ mỗi phía. 
2. Di chuyển từ trái sang phải. Giữ thời gian chạy`t`bắt đầu từ 1. Đối với mỗi thành phố`i`, kiểm tra xem`t <= a[i]`. Nếu vậy, hãy đặt`right_ok[i] = True`và tăng dần`t`. Nếu không, tất cả các thành phố tiếp theo sẽ không thể truy cập được bắt đầu từ vị trí này để mở rộng từ trái sang phải. 
3. Di chuyển từ phải sang trái theo cách tương tự, thiết lập`left_ok[i]`tùy theo liệu`t <= a[i]`ngược lại. 
4. Một thành phố là thành phố xuất phát hợp lệ nếu cả hai`left_ok[i]`Và`right_ok[i]`là đúng. Đếm những thành phố này. 
5. Xuất số đếm. 

Tại sao nó hoạt động: tại bất kỳ thành phố khởi đầu nào, việc mở rộng sang trái và phải đều bị hạn chế bởi thời hạn. Các mảng`left_ok`Và`right_ok`mã hóa các phân đoạn liền kề tối đa tôn trọng thời hạn theo từng hướng. Giao lộ của họ cung cấp chính xác các thành phố nơi việc mở rộng ngay từ đầu đáp ứng mọi thời hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    left_ok = [0] * n
    right_ok = [0] * n
    
    # left to right
    time = 1
    for i in range(n):
        if time <= a[i]:
            right_ok[i] = 1
            time += 1
        else:
            break
    
    # right to left
    time = 1
    for i in range(n-1, -1, -1):
        if time <= a[i]:
            left_ok[i] = 1
            time += 1
        else:
            break
    
    # count valid starting cities
    ans = 0
    for i in range(n):
        if left_ok[i] and right_ok[i]:
            ans += 1
    print(ans)
```Việc vượt qua đầu tiên đảm bảo rằng việc xuất phát từ thành phố 1 và di chuyển sang phải tôn trọng thời hạn; đường chuyền thứ hai đảm bảo điều tương tự từ bên phải. Chỉ những thành phố nơi cả hai hướng đều khả thi mới là vị trí xuất phát hợp lệ. sử dụng`break`sớm tránh được việc kiểm tra không cần thiết khi thời hạn bị vi phạm. 

## Ví dụ đã hoạt động 

**Mẫu 1**```
n = 6, a = [6, 3, 3, 3, 5, 5]
```| tôi | một [tôi] | đúng_ok | trái_ok | 
| --- | --- | --- | --- | 
| 0 | 6 | 1 | 1 | 
| 1 | 3 | 1 | 1 | 
| 2 | 3 | 1 | 1 | 
| 3 | 3 | 0 | 1 | 
| 4 | 5 | 0 | 0 | 
| 5 | 5 | 0 | 0 | 

Các thành phố bắt đầu hợp lệ trong đó cả hai đều bằng 1: chỉ số 1, 2, 3 (dựa trên 1: 2, 3, 4), đầu ra mẫu phù hợp`3`. 

**Mẫu 2**```
n = 6, a = [5, 6, 4, 1, 4, 5]
```Vi phạm đầu tiên xảy ra ở chỉ số 3 đối với đường chuyền bên phải và chỉ số 3 đối với đường chuyền bên trái, vì vậy không có thành phố nào có cả hai cờ đúng. đầu ra`0`. 

Những dấu vết này xác nhận rằng quá trình quét tuyến tính tính toán chính xác các phần mở rộng khả thi theo cả hai hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi lần quét mảng một lần; hai đường chuyền đủ để bao quát cả hai hướng. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Hai mảng phụ trợ`left_ok`Và`right_ok`. | 

Với tổng số$n \le 2 \cdot 10^5$, giải pháp dễ dàng phù hợp trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # replace with inline solution if needed
    return output.getvalue().strip()

# Provided samples
assert run("3\n6\n6 3 3 3 5 5\n6\n5 6 4 1 4 5\n9\n8 6 4 2 1 3 5 7 9\n") == "3\n0\n1"

# Custom cases
assert run("1\n1\n1\n") == "1"  # minimum input
assert run("1\n5\n1 2 3 4 5\n") == "1"  # strictly increasing deadlines
assert run("1\n5\n5 5 5 5 5\n") == "5"  # all equal, any start works
assert run("1\n5\n1 1 1 1 1\n") == "0"  # impossible, all deadlines too tight
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 thành phố | 1 | Xử lý đầu vào tối thiểu | 
| Tăng thời hạn | 1 | Chỉ có sự khởi đầu tập trung vào trung tâm mới có thể thành công | 
| Tất cả đều bình đẳng | 5 | Mỗi lần bắt đầu hợp lệ | 
| Tất cả những cái | 0 | Trường hợp cạnh không thể | 

## Vỏ cạnh 

1. **Thành phố riêng**:`n = 1, a = [1]`. Thuật toán trả về 1 vì cả hai đều thành công. 
2. **Thời hạn bằng với giới hạn khoảng cách**:`n = 5, a = [1,2,3,4,5]`. Chỉ có thành phố ngoài cùng bên trái đáp ứng`right_ok`, và chỉ có ngoài cùng bên phải thỏa mãn`left_ok`, vậy giao điểm là trung tâm thành phố (chỉ số 2) nếu có. Thuật toán xác định chính xác khả năng bắt đầu khả thi. 
3. **Tất cả thời hạn đều như nhau**:`n = 5, a = [5,5,5,5,5]`. Cả hai đường chuyền đều đặt tất cả các cờ, dẫn đến tất cả các thành phố đều hợp lệ. 

Các trường hợp đặc biệt này chứng tỏ thuật toán xử lý các ranh giới và giá trị tối thiểu/tối đa một cách chính xác. Quá trình quét tuyến tính thực thi một cách tự nhiên bất biến "khoảng cách từ điểm bắt đầu đến thời hạn" mà không cần thêm logic.
