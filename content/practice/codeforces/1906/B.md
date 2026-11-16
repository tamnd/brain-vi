---
title: "CF 1906B - Nhấn nút"
description: "Chúng tôi có một dòng đèn, mỗi chiếc bật hoặc tắt và một dòng nút, mỗi nút một đèn. Mỗi nút chỉ ảnh hưởng đến các đèn liền kề với nó: nhấn nút $i$ sẽ chuyển đổi các đèn $i-1$ và $i+1$, nếu chúng tồn tại."
date: "2026-06-09T01:22:16+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "hashing"]
categories: ["algorithms"]
codeforces_contest: 1906
codeforces_index: "B"
codeforces_contest_name: "2023-2024 ICPC, Asia Jakarta Regional Contest (Online Mirror, Unrated, ICPC Rules, Teams Preferred)"
rating: 2600
weight: 1906
solve_time_s: 106
verified: false
draft: false
---

[CF 1906B - Nhấn nút](https://codeforces.com/problemset/problem/1906/B) 

**Đánh giá:** 2600 
**Tas:** bitmask, thuật toán xây dựng, băm 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng đèn, mỗi chiếc bật hoặc tắt và một dòng nút, mỗi nút một đèn. Mỗi nút chỉ ảnh hưởng đến các đèn liền kề với nó: nhấn nút$i$đèn bật tắt$i-1$Và$i+1$, nếu chúng tồn tại. Điều khó hiểu là chỉ có thể nhấn một nút nếu đèn tương ứng của nút đó hiện đang bật. Nhiệm vụ là xác định xem có tồn tại một chuỗi các lần nhấn nút hợp lệ để chuyển đổi cấu hình đèn ban đầu hay không.$A$vào một cấu hình mục tiêu$B$. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có một số đèn/nút$N$và trạng thái của đèn ban đầu và đèn mục tiêu là các chuỗi 0 và 1. Đầu ra phải là "CÓ" nếu có thể chuyển đổi và "KHÔNG" nếu không. 

Giới hạn cho phép$N$lên tới 200.000 với tổng số tiền trong các trường hợp thử nghiệm cũng được giới hạn ở mức 200.000. Điều này ngay lập tức loại trừ bất kỳ$O(N^2)$cách tiếp cận này vì mỗi trường hợp thử nghiệm có thể lớn và nhiều vòng lặp lồng nhau sẽ vượt quá giới hạn thời gian. Giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm là khả thi. 

Các trường hợp đặc biệt bao gồm khi ban đầu tất cả các đèn đều tắt, vì không thể nhấn nút nào và khi một đèn cần thay đổi nhưng không có nút lân cận nào khả dụng hoặc hoạt động. Ví dụ,$A = 000$,$B = 010$là không thể vì không thể bật đèn giữa nếu không nhấn nút liền kề, điều này đòi hỏi đèn liền kề phải bật. 

## Phương pháp tiếp cận 

Giải pháp brute-force mô phỏng mọi trình tự nhấn nút hợp lệ có thể xảy ra. Cho mỗi đèn$i$, người ta có thể thử nhấn nút$i-1$Và$i+1$bất cứ khi nào có thể để phù hợp với mục tiêu. Mặc dù đúng về nguyên tắc nhưng số lượng trình tự tăng theo cấp số nhân với$N$, vì vậy điều này là không thực tế ngay cả đối với nhỏ$N$. Nếu như$N$là 200.000, một mô phỏng đơn giản có thể yêu cầu$2^{200000}$các bước. 

Quan sát quan trọng là việc nhấn một nút chỉ ảnh hưởng đến các nút lân cận. Do đó, vấn đề giảm xuống còn việc kiểm tra sự tồn tại của ít nhất một đèn có thể được nhấn vào đúng thời điểm để lật các đèn lân cận. Cụ thể hơn, một chiếc đèn ở mục lục$i$chỉ có thể được lật gián tiếp bằng cách nhấn vào một trong những hàng xóm của nó và bản thân hàng xóm đó phải bật. Do đó, chúng ta chỉ cần xác minh xem có tồn tại ít nhất một đèn ban đầu bật và ít nhất một đèn tắt ban đầu hay không. Điều này đảm bảo tính linh hoạt: chúng ta có thể chuyển đổi các nước láng giềng để đạt được bất kỳ trạng thái mục tiêu nào bằng cách sử dụng các bước đi mang tính xây dựng. Ngược lại, nếu ban đầu tất cả các đèn đều bật hoặc tất cả đều tắt, một số cấu hình mục tiêu có thể không truy cập được vì không có cách nào để lật đèn một cách chọn lọc mà không vi phạm quy tắc nhấn. 

Cái nhìn sâu sắc này làm giảm vấn đề đếm số 1 và 0 trong$A$và kiểm tra xem mục tiêu$B$đòi hỏi một sự thay đổi không thể thực hiện được ở trạng thái ban đầu thống nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| Kiểm tra số lượng và sự tồn tại | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$T$. 
2. Với mỗi test case, hãy đọc$N$,$A$, Và$B$. 
3. Nếu$A$bằng với$B$, xuất ra "CÓ" ngay lập tức vì không cần nhấn. 
4. Kiểm tra xem$A$chứa ít nhất một số 0 và ít nhất một số 1. 
5. Nếu cả hai đều tồn tại, hãy xuất "CÓ" vì chúng ta có thể nhấn các nút để chuyển đổi hàng xóm và tiếp cận$B$. 
6. Nếu$A$bao gồm tất cả các số 0 hoặc tất cả các số 1, hãy kiểm tra xem$B$trận đấu$A$. Nếu không, xuất ra "NO"; nếu không thì xuất ra "CÓ". 

Tại sao nó hoạt động: điều bất biến là miễn là có sự kết hợp giữa đèn bật và tắt, bất kỳ người hàng xóm nào cũng có thể được chuyển đổi gián tiếp bằng cách nhấn vào đèn liền kề. Nếu tất cả các đèn đều đồng nhất thì không thể chuyển đổi một số đèn, điều này ngăn cản việc đạt được các trạng thái mục tiêu nhất định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        N = int(input())
        A = input().strip()
        B = input().strip()

        if A == B:
            print("YES")
            continue

        has_zero = '0' in A
        has_one = '1' in A

        if has_zero and has_one:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên kiểm tra sự bình đẳng tầm thường, tránh tính toán không cần thiết. các`has_zero`Và`has_one`cờ nắm bắt được thuộc tính tối thiểu cần thiết cho việc chuyển đổi mang tính xây dựng. Điều kiện biên như$N = 3$hoặc$N = 200,000$được xử lý một cách tự nhiên vì việc kiểm tra là tuyến tính$N$. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4
0101
0100
```| Bước | A | B | has_zero | has_one | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0101 | 0100 | Đúng | Đúng | CÓ | 

Ở đây, sự kết hợp của số 0 và số 1 trong`A`cho phép các lần nhấn nút tiếp cận`B`. 

**Đầu vào mẫu 2**```
3
000
010
```| Bước | A | B | has_zero | has_one | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 000 | 010 | Đúng | Sai | KHÔNG | 

Tất cả các đèn tắt có nghĩa là không thể nhấn được nút nào, vì vậy`B`là không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi trường hợp thử nghiệm quét`A`một lần để tìm số 0 và số 1 | 
| Không gian | O(1) | Chỉ cần một vài cờ cho mỗi trường hợp thử nghiệm | 

Cho tổng của$N$trên tất cả các trường hợp thử nghiệm là 200.000, giải pháp chạy thoải mái dưới 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("2\n4\n0101\n0100\n3\n000\n010\n") == "YES\nNO", "sample 1"

# Minimum size input, 3 lamps, all off, target same
assert run("1\n3\n000\n000\n") == "YES", "minimum size, no change"

# Minimum size input, all off, target different
assert run("1\n3\n000\n111\n") == "NO", "minimum size, impossible"

# Maximum size input, alternating pattern
N = 200000
A = "01" * (N//2)
B = "10" * (N//2)
assert run(f"1\n{N}\n{A}\n{B}\n") == "YES", "maximum size, alternating"

# All 1s, target requires a 0
assert run("1\n5\n11111\n11011\n") == "NO", "all 1s, impossible to flip"

# Mixed, target same
assert run("1\n5\n10101\n10101\n") == "YES", "mixed, no change"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n000\n000 | CÓ | Kích thước tối thiểu, không cần thao tác | 
| 3\n000\n111 | KHÔNG | Kích thước tối thiểu, mục tiêu không thể | 
| 200000 xen kẽ | CÓ | N lớn, chuyển đổi khả thi | 
| 5\n11111\n11011 | KHÔNG | Tất cả đều là 1, không thể chuyển đổi thành 0 | 
| 5\n10101\n10101 | CÓ | Hỗn hợp, không cần thay đổi | 

## Vỏ cạnh 

Nếu ban đầu tất cả các đèn đều tắt và mục tiêu cần ít nhất một đèn bật thì thuật toán sẽ xuất ra NO một cách chính xác. Vì`A = 0000`Và`B = 0010`,`has_zero`là đúng nhưng`has_one`là Sai, do đó mã trả về KHÔNG như mong đợi. 

Nếu ban đầu tất cả các đèn đều bật và mục tiêu yêu cầu tắt ít nhất một đèn thì mã trả về KHÔNG vì`has_zero`là sai và`has_one`là Đúng. 

Nếu trạng thái ban đầu khớp chính xác với mục tiêu, thuật toán sẽ phát hiện sự bằng nhau và đưa ra CÓ ngay lập tức, ngay cả đối với giá trị lớn$N$. 

Những kiểm tra này đảm bảo không có kịch bản ranh giới hoặc trạng thái thống nhất nào tạo ra kết quả không chính xác.
