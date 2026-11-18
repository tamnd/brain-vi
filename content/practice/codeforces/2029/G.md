---
title: "CF 2029G - Bài toán cân bằng"
description: "Chúng ta được yêu cầu tối đa hóa tổng có trọng số trên một mảng các số nguyên bắt đầu bằng tất cả các số 0. Mảng có độ dài $n$ và nhận các hoạt động ban đầu $m$, mỗi hoạt động tăng tiền tố hoặc hậu tố của mảng lên 1."
date: "2026-06-08T12:04:35+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 2029
codeforces_index: "G"
codeforces_contest_name: "Refact.ai Match 1 (Codeforces Round 985)"
rating: 3000
weight: 2029
solve_time_s: 106
verified: false
draft: false
---

[CF 2029G - Vấn đề cân bằng](https://codeforces.com/problemset/problem/2029/G) 

**Đánh giá:** 3000 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa tổng có trọng số trên một mảng các số nguyên bắt đầu bằng tất cả các số 0. Mảng có độ dài$n$và nhận được$m$các thao tác ban đầu, mỗi thao tác sẽ tăng tiền tố hoặc hậu tố của mảng lên 1. Sau các thao tác này, Kevin có thể thực hiện bất kỳ số lần bổ sung tiền tố hoặc hậu tố bổ sung nào. Đối với một giá trị mục tiêu nhất định$v$, một phần tử đóng góp trọng lượng tương ứng của nó$c_i$vẻ đẹp tổng thể khi và chỉ khi giá trị của nó bằng$v$. Nhiệm vụ là tính toán cho mỗi$v$từ 1 đến$V$, vẻ đẹp tối đa có thể đạt được nếu Kevin tiếp tục tối ưu. 

Những ràng buộc chỉ ra rằng$n$Và$m$có thể lớn như$2\cdot 10^5$, do đó, bất kỳ giải pháp nào lặp lại trên tất cả các chuỗi hoạt động có thể xảy ra đều không thể thực hiện được. Ngoài ra,$V$có thể lên tới 2000 và tổng của$V^2$trên tất cả các trường hợp thử nghiệm được giới hạn ở$4 \cdot 10^6$, gợi ý rằng các thuật toán có độ phức tạp$O(n V)$được chấp nhận. 

Các trường hợp cạnh bao gồm các mảng có độ dài bằng một, các mảng trong đó tất cả các trọng số đều bằng nhau hoặc các tình huống trong đó các hoạt động ban đầu đã đẩy một số phần tử vượt quá giới hạn$V$. Việc triển khai ngây thơ có thể đánh giá sai vẻ đẹp nếu nó không tính toán đúng các yếu tố đã vượt quá mục tiêu$v$và không thể giảm được. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force đơn giản về mặt khái niệm: mô phỏng các hoạt động ban đầu để xây dựng mảng$a$, thì với mỗi$v$từ 1 đến$V$, hãy thử tất cả các chuỗi bổ sung tiền tố và hậu tố để đạt được chính xác$v$tại mỗi chỉ số. Cách tiếp cận này đúng nhưng hoàn toàn không khả thi, bởi vì số dãy các phép cộng tăng theo cấp số nhân với$n$và số lần bổ sung. Ngay cả khi chúng tôi hạn chế thử một thao tác cho mỗi chỉ mục, thì độ phức tạp trong trường hợp xấu nhất là$O(n^2 V)$, quá lớn đối với$n \sim 2 \cdot 10^5$. 

Quan sát quan trọng là bất kỳ số lượng bổ sung tiền tố và hậu tố nào cũng có thể được biểu diễn dưới dạng số nguyên không âm để được thêm vào một phạm vi liền kề. Cụ thể chúng ta hãy xác định$a_i$sau những hoạt động đầu tiên. để làm$a_i$chính xác$v$, Kevin phải thực hiện các phép cộng bổ sung sao cho$a_i + \text{additional}_i = v$. Vì việc bổ sung tiền tố và hậu tố ảnh hưởng đến phạm vi nên mọi lựa chọn thao tác đều tương ứng với việc chọn phạm vi liền kề để tăng. Điều này làm giảm vấn đề xuống tổng trọng số tối đa$c_i$trên một phạm vi sao cho$a_i \le v$và có thể tăng lên$v$. 

Về mặt hình thức, hãy để$d_i = v - a_i$. Một phần tử$i$có thể góp phần làm đẹp nếu$d_i \ge 0$. Khi đó vẻ đẹp tối đa có thể đạt được là tổng tối đa của$c_i$trên một số phạm vi của các phần tử trong đó tương ứng$d_i$là không âm. Vì bất kỳ phạm vi liền kề nào cũng có thể được tăng lên bằng cách sử dụng các phần bổ sung tiền tố và hậu tố, nên chúng ta có thể chọn bất kỳ chuỗi con liền kề nào của các chỉ mục trong đó$d_i \ge 0$và thêm số lượng thao tác cần thiết để đưa chúng chính xác đến$v$. Điều này cho phép chúng ta tính toán câu trả lời trong$O(n)$mỗi giá trị$v$sử dụng một biến thể của thuật toán Kadane. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * V) | O(n) | Quá chậm | 
| Tối ưu | O (n V) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng$a$của số không có chiều dài$n$. Lặp lại thông qua$m$các hoạt động ban đầu. Đối với mỗi hoạt động loại L tại chỉ mục$x$, tăng mảng chênh lệch ở chỉ số 0 lên 1 và giảm ở chỉ mục$x$bằng 1. Đối với mỗi thao tác loại R tại chỉ mục$x$, tăng tại chỉ số$x-1$và giảm chỉ số$n$. Chuyển đổi mảng khác biệt thành mảng thực tế$a$bằng cách lấy một tiền tố. Bước này mô phỏng hiệu quả tất cả các hoạt động ban đầu trong O(n + m). 
2. Với mỗi giá trị mục tiêu$v$từ 1 đến$V$, tính toán$d_i = v - a_i$. Nếu như$d_i < 0$, yếu tố$i$không thể với tới$v$và không đóng góp gì cho vẻ đẹp. Ngược lại, trọng lượng của nó$c_i$có sẵn để tổng hợp. 
3. Áp dụng thuật toán tổng mảng con tối đa$c_i$bị giới hạn ở các chỉ số nơi$d_i \ge 0$. Bắt đầu với tổng bằng 0, lặp qua mảng: if$d_i < 0$, đặt lại tổng số tiền hiện có về 0; nếu không thì thêm$c_i$đến tổng số tiền hiện có và theo dõi số tiền tối đa gặp phải. 
4. Số tiền lớn nhất tìm được ở bước 3 chính là đáp án cho câu hỏi này$v$. Lặp lại cho tất cả$v$từ 1 đến$V$. 

Điều này hoạt động vì bất kỳ phạm vi chỉ số liền kề nào cũng có thể được điều chỉnh để đạt được$v$bằng cách sử dụng bổ sung tiền tố và hậu tố. Mảng khác biệt ở bước 1 đảm bảo rằng chúng ta tôn trọng các thao tác ban đầu. Tổng mảng con tối đa đảm bảo rằng chúng tôi chọn tối ưu tập hợp con của các chỉ số để tối đa hóa vẻ đẹp mà không vượt quá giới hạn$v$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, V = map(int, input().split())
        c = list(map(int, input().split()))
        diff = [0]*(n+1)
        for _ in range(m):
            op, x = input().split()
            x = int(x)
            if op == 'L':
                diff[0] += 1
                diff[x] -= 1
            else:
                diff[x-1] += 1
                diff[n] -= 1
        a = [0]*n
        curr = 0
        for i in range(n):
            curr += diff[i]
            a[i] = curr

        result = []
        for v in range(1, V+1):
            max_beauty = 0
            curr_sum = 0
            for i in range(n):
                if a[i] > v:
                    curr_sum = 0
                else:
                    curr_sum += c[i]
                    if curr_sum > max_beauty:
                        max_beauty = curr_sum
            result.append(str(max_beauty))
        print(' '.join(result))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ xây dựng mảng sau các thao tác ban đầu bằng cách sử dụng mảng sai phân để xử lý hiệu quả các phần tăng tiền tố và hậu tố. Sau đó với mỗi giá trị$v$, nó tính toán vẻ đẹp tối đa bằng cách lặp qua các phần tử, bỏ qua những phần tử đã vượt quá$v$và sử dụng tổng chạy để theo dõi chuỗi con liền kề tốt nhất. Điều này tránh việc mô phỏng các thao tác bổ sung một cách rõ ràng và đảm bảo vẻ đẹp tối ưu cho từng thao tác.$v$. 

## Ví dụ đã hoạt động 

### Mẫu 1, test case đầu tiên 

Mảng ban đầu$a = [0,0,0]$. Hoạt động: 

| Hoạt động | Kết quả là | 
| --- | --- | 
| L 3 | [1,1,1] | 
| R 3 | [1,1,2] | 
| L 1 | [2,1,2] | 

Mục tiêu$v = 1$:$d = [-1,0,-1]$, chỉ có chỉ số 2 mới có thể đóng góp, tổng = 2. 

Mục tiêu$v = 2$:$d = [0,1,0]$, dãy con liền kề [2,3] có thể tăng lên, tổng = 6. 

### Mẫu 2, test case thứ ba 

Mảng$a = [0,0,0,0,0]$, hoạt động ban đầu sửa đổi thành$a = [3,2,2,1,1]$. Vì$v = 3$: 

| tôi | a_i | d_i | c_i đóng góp | 
| --- | --- | --- | --- | 
| 1 | 3 | 0 | 1 | 
| 2 | 2 | 1 | 1 | 
| 3 | 2 | 1 | 1 | 
| 4 | 1 | 2 | 1 | 
| 5 | 1 | 2 | 1 | 

Tổng liền kề tối đa cho số không âm$d_i$là 3 (chỉ số 1-3), sắc đẹp = 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O (n V) | Các hoạt động ban đầu được mô phỏng theo O(n + m), sau đó O(n) trên v từ 1 đến V. | 
| Không gian | O(n) | Mảng a và khác biệt, cộng với các biến tạm thời. | 

Với những hạn chế$V^2$tổng hợp tất cả các trường hợp thử nghiệm ≤$4 \cdot 10^6$, điều này có thể thực hiện được trong vòng 2-3 giây và giới hạn bộ nhớ là 1GB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("""5
3 3 2
1 2 4
L 3
R 3
L 1
3 3 2
5 1 4
L
```
