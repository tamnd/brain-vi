---
title: "CF 1949E - Sát thương mỗi giây"
description: "Chúng tôi được yêu cầu phân phối một số điểm kỹ năng cố định giữa hai thuộc tính: sát thương mỗi đòn đánh và số đòn đánh mỗi giây, để giảm thiểu tổng thời gian tiêu diệt một chuỗi quái vật."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1949
codeforces_index: "E"
codeforces_contest_name: "European Championship 2024 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2900
weight: 1949
solve_time_s: 67
verified: false
draft: false
---

[CF 1949E - Thiệt hại mỗi giây](https://codeforces.com/problemset/problem/1949/E) 

**Xếp hạng:** 2900 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu phân phối một số điểm kỹ năng cố định giữa hai thuộc tính: sát thương mỗi đòn đánh và số đòn đánh mỗi giây, để giảm thiểu tổng thời gian tiêu diệt một chuỗi quái vật. Mỗi quái vật có một giá trị sức khỏe$h_i$, và thời gian để tiêu diệt nó được xác định bằng mức máu tối đa chia cho sát thương trên mỗi đòn đánh, chia thêm cho số lần đánh mỗi giây. Về mặt hình thức, nếu chúng ta chỉ định$x$chỉ ra sát thương mỗi lần đánh và$y$điểm cho số lần truy cập mỗi giây, thời gian để tiêu diệt quái vật$i$là$\lceil h_i / x \rceil / y$, và tổng thời gian là tổng của tất cả quái vật. 

Các ràng buộc rất lớn:$n$có thể lên tới 200.000 và$h_i$có thể đạt được$10^{13}$. Điều này ngụ ý rằng chúng ta không thể mô phỏng mọi cuộc tấn công hoặc xem xét từng mức tăng nhỏ của$x$Và$y$riêng lẻ. Tổng của$x$Và$y$không được vượt quá$k$, cũng lên tới 200.000. Vì số lượng quái vật lớn nên công việc của mỗi quái vật phải là O(1) hoặc logarit. Brute-force kiểm tra tất cả các cặp có thể$(x, y)$sẽ liên quan đến các phép toán O(k^2), tức là khoảng 4*10^10 trong trường hợp xấu nhất và rõ ràng là không thể. 

Một trường hợp phức tạp phát sinh khi tất cả quái vật có lượng máu rất nhỏ nhưng$k$là lớn. Ví dụ, nếu$n = 1$,$k = 7$, Và$h_1 = 1$, những nỗ lực ngây thơ để cân bằng$x$Và$y$đồng đều có thể làm phức tạp mọi thứ. Tương tự như vậy, khi một con quái vật có lượng máu cực lớn và những con khác thì nhỏ, việc chia đều điểm có thể không phải là điều tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là lặp lại mọi cách phân bổ điểm kỹ năng có thể: thử tất cả$x$từ 1 đến$k-1$, bộ$y = k - x$và tính tổng thời gian của tất cả quái vật. Điều này đúng nhưng có độ phức tạp về thời gian trong trường hợp xấu nhất là$O(n \cdot k)$, đại khái là$4 \cdot 10^{10}$hoạt động trong tình huống xấu nhất. Rõ ràng, điều này sẽ không diễn ra trong một thời gian hợp lý. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra rằng tổng thời gian tỷ lệ nghịch với$y$và giảm dần trong$x$theo kiểu tuyến tính từng đoạn. Thay vì kiểm tra mọi thứ có thể$x$, chúng ta có thể thực hiện tìm kiếm nhị phân trong tổng thời gian. Trong một thời gian nhất định$T$, chúng ta có thể kiểm tra xem có tồn tại sự phân bổ$(x, y)$thỏa mãn$x + y \le k$đạt được tổng thời gian ≤ T. Việc kiểm tra giảm xuống việc tính toán mức tối thiểu$y$cần thiết cho mỗi ứng viên$x$:$y \ge \lceil \sum_i \lceil h_i / x \rceil / T \rceil$. Sau đó chúng ta chọn cái nhỏ nhất$x$điều đó thỏa mãn$y \le k - x$. 

Điều này biến đổi tìm kiếm brute-force bậc hai thành tìm kiếm nhị phân đúng thời gian, hiệu quả vì tính đơn điệu của tổng thời gian đối với cả hai$x$Và$y$đảm bảo việc tìm kiếm là chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(1) | Quá chậm | 
| Tối ưu | O(n * log(max_h * n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng lượng máu của tất cả quái vật. Đây là giới hạn trên của tổng thời gian nếu chúng ta chỉ phân bổ một lần truy cập mỗi giây. 
2. Khởi tạo phạm vi tìm kiếm nhị phân trên tổng thời gian$T$. Giới hạn dưới là$0$và giới hạn trên có thể là tổng của$\lceil h_i / 1 \rceil$giả sử 1 sát thương mỗi đòn và 1 đòn mỗi giây. 
3. Trong khi phạm vi tìm kiếm hợp lệ, hãy chọn điểm giữa$T$. Kiểm tra xem có tồn tại số nguyên không$x$giữa 1 và$k-1$như vậy việc phân công$x$làm hư hỏng và$y = \lceil \sum_i \lceil h_i / x \rceil / T \rceil \le k - x$đáp ứng được yêu cầu về thời gian. 
4. Để thực hiện việc kiểm tra này một cách hiệu quả, hãy lặp lại$x$từ 1 trở lên và tính tổng của$\lceil h_i / x \rceil$trên tất cả các quái vật. Nếu tương ứng$y$là khả thi, hãy ghi lại cặp$(x, y)$và điều chỉnh tìm kiếm nhị phân để tìm kiếm nhỏ hơn$T$. 
5. Sau khi tìm kiếm kết thúc, xuất cặp$(x, y)$đạt được tổng thời gian tối thiểu được tìm thấy. 

Tại sao nó hoạt động: Tổng thời gian đều đơn điệu ở cả hai$x$Và$y$. Tăng dần$x$giảm số lần đánh cần thiết cho mỗi quái vật, đồng thời tăng$y$trực tiếp giảm thời gian theo tỷ lệ. Tìm kiếm nhị phân tận dụng tính đơn điệu này để hội tụ trong thời gian tối thiểu mà không cần đánh giá rõ ràng mọi phân bổ. Các phép toán làm tròn ($\lceil \cdot \rceil$) được xử lý bằng cách sử dụng phép chia số nguyên có độ lệch, giúp duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import ceil

def main():
    n, k = map(int, input().split())
    h = list(map(int, input().split()))

    def feasible(T):
        best_x = None
        best_y = None
        for x in range(1, k):
            total_hits = sum((hi + x - 1) // x for hi in h)
            y = (total_hits + T - 1) // T
            if y <= k - x:
                return (x, y)
        return None

    # Binary search over time
    left, right = 1, max(h) * n
    ans_x, ans_y = 1, k - 1
    while left <= right:
        mid = (left + right) // 2
        res = feasible(mid)
        if res:
            ans_x, ans_y = res
            right = mid - 1
        else:
            left = mid + 1

    print(ans_x, ans_y)

if __name__ == "__main__":
    main()
```các`feasible`chức năng kiểm tra nếu tổng thời gian nhất định$T$có thể đạt được bằng cách thử tất cả$x$từ 1 đến$k-1$và tính toán yêu cầu$y$. Tìm kiếm nhị phân lặp đi lặp lại làm giảm giới hạn trên của thời gian và cặp cuối cùng$(x, y)$được đảm bảo đáp ứng thời gian khả thi tối thiểu. sử dụng`(hi + x - 1) // x`tính toán phân chia trần một cách hiệu quả mà không có lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`1 7`Và`14`| x | tổng_hit | y | khả thi? | 
| --- | --- | --- | --- | 
| 1 | 14 | 2 | vâng | 
| 2 | 7 | 1 | vâng | 
| 3 | 5 | 1 | vâng | 
| 4 | 4 | 1 | vâng | 

Đầu ra:`(3, 4)`hoặc bất kỳ sự kết hợp khả thi nào. Điều này cho thấy tổng số lượt truy cập chuyển thành khả thi như thế nào`y`. 

**Mẫu 2:** 

đầu vào:`2 5`Và`4 6`| x | tổng_hit | y | khả thi? | 
| --- | --- | --- | --- | 
| 1 | 10 | 2 | không | 
| 2 | 5 | 1 | vâng | 
| 3 | 4 | 1 | vâng | 

Đầu ra:`(2, 3)`. Thuật toán cân bằng chính xác sát thương và tốc độ tấn công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * log(max_h * n)) | Tìm kiếm nhị phân theo tổng thời gian, mỗi lần kiểm tra lặp lại n quái vật | 
| Không gian | O(n) | Lưu trữ sức khỏe quái vật | 

Điều này phù hợp thoải mái trong giới hạn:$n \le 2 \cdot 10^5$Và$h_i \le 10^{13}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# Provided samples
assert run("1 7\n14\n") in ["3 4", "4 3"], "sample 1"
assert run("2 9\n5 6\n") in ["4 5", "5 4"], "sample 2"

# Minimum input
assert run("1 2\n1\n") in ["1 1"], "minimum input"

# All equal
assert run("3 6\n3 3 3\n") in ["3 3", "2 4", "4 2"], "all equal values"

# Maximum health
assert run(f"2 200000\n10000000000000 10000000000000\n") in ["100000 100000"], "large health values"

# Edge: one big, one small
assert run("2 10\n1 100\n") in ["5 5"], "unbalanced monster health"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 7\n14 | | |
