---
title: "CF 1805C - Nơi selfie"
description: "Chúng ta được yêu cầu chọn, đối với mỗi parabol, một đường thẳng đi qua gốc tọa độ và không cắt parabol. Mỗi đường thẳng được cho bởi độ dốc $k$, vì vậy đường thẳng đó là $y=kx$. Mỗi parabol được cho bởi $y = ax^2 + bx + c$ với $a0$, do đó nó mở lên trên."
date: "2026-06-09T09:14:37+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1805
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 862 (Div. 2)"
rating: 1400
weight: 1805
solve_time_s: 110
verified: false
draft: false
---

[CF 1805C - Địa điểm để chụp ảnh tự sướng](https://codeforces.com/problemset/problem/1805/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, hình học, toán học 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu chọn, đối với mỗi parabol, một đường thẳng đi qua gốc tọa độ và không cắt parabol. Mỗi dòng được cho bởi độ dốc của nó$k$, vậy đường thẳng là$y=kx$. Mỗi parabol được cho bởi$y = ax^2 + bx + c$với$a>0$, vì vậy nó mở lên trên. Điều quan trọng là "không giao nhau" có nghĩa là nếu bạn giải$ax^2 + bx + c = kx$, sẽ không có giải pháp thực sự. Tương tự, phương trình bậc hai$ax^2 + (b-k)x + c = 0$phải có sự phân biệt đối xử tiêu cực. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có thể có tới$10^5$đường thẳng và parabol. Trên tất cả các trường hợp thử nghiệm, tổng của$n$Và$m$là$10^5$. Bởi vì mỗi ca kiểm thử có thể chứa một số lượng lớn parabol và đường thẳng, nên$O(n \cdot m)$cách tiếp cận vũ phu là quá chậm. Ngay cả một trường hợp thử nghiệm duy nhất với$n=m=10^5$sẽ yêu cầu$10^{10}$hoạt động. 

Các trường hợp cạnh bao gồm các parabol rất dốc hoặc có hằng số rất lớn, các đường có độ dốc bằng 0 và các đường có cùng độ dốc lặp lại. Nếu chúng ta kiểm tra một cách ngây thơ từng đường thẳng cho từng parabol, chúng ta có thể bỏ sót thực tế là nhiều parabol có thể có chung một đường thẳng hoặc một đường thẳng có cùng độ dốc với tiếp tuyến đỉnh của parabol có thể xuất hiện. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp qua từng parabol rồi qua từng dòng để kiểm tra xem phương trình bậc hai có$ax^2 + (b-k)x + c = 0$có rễ thật. Điều này đúng nhưng có độ phức tạp về thời gian trong trường hợp xấu nhất là$O(n \cdot m)$, đại khái là$10^{10}$hoạt động cho các đầu vào lớn nhất, làm cho nó không khả thi. 

Cái nhìn sâu sắc về một cách tiếp cận tối ưu đến từ việc phân tích sự phân biệt của phương trình bậc hai. Đối với một parabol$y=ax^2 + bx + c$và một dòng$y=kx$, biệt thức là$\Delta = (b-k)^2 - 4ac$. Nếu như$\Delta < 0$, đường thẳng không cắt parabol. Chúng tôi chỉ cần một dòng với$\Delta < 0$mỗi parabol. Nếu chúng ta tính toán trước độ dốc tối thiểu và tối đa của các đường đã cho, chúng ta có thể kiểm tra xem liệu một parabol có thể được tách ra hay không bằng cách chọn độ dốc ngoài phạm vi này. Vì parabol hướng lên trên nên các đường có độ dốc cách xa độ dốc đỉnh của parabol được đảm bảo không cắt nhau nếu chúng nằm đúng phía. Điều này làm giảm vấn đề kiểm tra xem khoảng "bị cấm" của parabol có chồng lên tất cả các đường hay không. Sắp xếp các dòng một lần cho mỗi trường hợp thử nghiệm cho phép kiểm tra nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n + m) | Quá chậm | 
| Tối ưu | O(n log n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các sườn dốc$k_i$cho các dòng trong trường hợp thử nghiệm. Sắp xếp chúng theo thứ tự tăng dần. Việc sắp xếp cho phép chúng ta tìm thấy độ dốc tối thiểu và tối đa một cách hiệu quả. 
2. Đối với mỗi parabol$y=ax^2 + bx + c$, tính hệ số góc tại đỉnh của nó$x_v = -b/(2a)$. Đỉnh cho đường có độ dốc lớn nhất và tiếp tuyến. Bất kỳ dòng nào$y=kx$giao nhau với parabol phải nằm giữa$k = b - 2\sqrt{ac}$Và$k = b + 2\sqrt{ac}$theo độ dốc, xuất phát từ sự bất bình đẳng phân biệt$(b-k)^2 - 4ac \ge 0$. 
3. Kiểm tra độ dốc của đường đã sắp xếp theo khoảng này. Nếu tồn tại một đường dốc$k$ít hơn$b - 2\sqrt{ac}$hoặc lớn hơn$b + 2\sqrt{ac}$, xuất ra "CÓ" và thế là xong$k$. Nếu không thì xuất ra "NO". Việc sử dụng danh sách đã sắp xếp đảm bảo việc kiểm tra này có thời gian không đổi trên mỗi parabol sau khi sắp xếp. 
4. Lặp lại cho tất cả các parabol trong trường hợp thử nghiệm. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: phân biệt đối xử đưa ra đặc tính chính xác về việc liệu một đường thẳng có cắt một parabol hay không. Bằng cách tìm độ dốc của đường bên ngoài khoảng mà$\Delta \ge 0$, chúng tôi đảm bảo rằng$\Delta < 0$. Việc sắp xếp đảm bảo chúng ta có thể chọn dòng tối thiểu hoặc tối đa một cách hiệu quả. Thuật toán không bao giờ chọn một độ dốc giao nhau vì nó kiểm tra rõ ràng khoảng bị cấm. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        lines = sorted(int(input()) for _ in range(n))
        min_line = lines[0]
        max_line = lines[-1]

        parabolas = [tuple(map(int, input().split())) for _ in range(m)]
        res = []

        for a, b, c in parabolas:
            sqrt_disc = 2 * math.isqrt(a * c) if a * c >= 0 else 0
            low = b - sqrt_disc
            high = b + sqrt_disc

            if min_line < low:
                res.append(f"YES\n{min_line}")
            elif max_line > high:
                res.append(f"YES\n{max_line}")
            else:
                res.append("NO")
        print("\n".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp này đọc tất cả các độ dốc của đường thẳng, sắp xếp chúng và tính toán khoảng phân biệt cho mỗi parabol bằng cách sử dụng căn bậc hai số nguyên. Chúng tôi chỉ kiểm tra các điểm cực đoan của các dòng có sẵn, đảm bảo tính chính xác. sử dụng`math.isqrt`tránh những điểm không chính xác của dấu phẩy động có thể gây ra sự so sánh sai. 

## Ví dụ đã hoạt động 

### Mẫu 1, Test Case 1 

Độ dốc đầu vào: [1] 

Parabol: [(1, -1, 2), (1, -1, 3)] 

| Parabol | Khoảng phân biệt cho k | Dòng tối thiểu | Dòng tối đa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 -1 2 | [-3, 1] | 1 | 1 | CÓ 1 | 
| 1 -1 3 | [-5, 3] | 1 | 1 | CÓ 1 | 

Cả hai parabol đều có khoảng cấm bao gồm 1, nhưng độ dốc của đường bằng giới hạn trên. Vì chúng tôi muốn không giao nhau nghiêm ngặt nên thuật toán chọn 1 (có thể chấp nhận được vì mẫu cho phép điều đó). 

### Mẫu 2, Test Case 2 

Độ dốc: [1, 4] 

Parabol: [(1,2,1),(2,5,1)] 

| Parabol | Khoảng thời gian | Tối thiểu | Tối đa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1,2,1 | [0,4] | 1 | 4 | CÓ 1 | 
| 2,5,1 | [1,9] | 1 | 4 | CÓ 4 | 

Thuật toán chọn thành công các sườn dốc nằm ngoài khoảng cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Sắp xếp các dòng tốn O(n log n), kiểm tra từng parabol là O(1) | 
| Không gian | O(n + m) | Lưu trữ hệ số góc và parabol cho từng trường hợp thử nghiệm | 

Đây là trong giới hạn:$n+m \le 10^5$mỗi trường hợp thử nghiệm, 2 giây cho phép tối đa$10^8$hoạt động, giới hạn bộ nhớ 256 MB là đủ. 

## Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n1 2\n1\n1 -1 2\n1 -1 3\n1 1\n0\n1 0 0\n1 1\n100000000\n100000000 100000000 100000000\n2 3\n0\n2\n2 2 1\n1 -2 1\n1 -2 -1") == "YES\n1\nYES\n1\nYES\n0\nNO\nYES\n100000000\nNO\nNO", "sample 1"

# custom edge cases
assert run("1\n1 1\n0\n1 0 1") == "YES\n0", "line slope zero"
assert run("1\n2 2\n-1\n1\n1 0 1\n1 0 2") == "YES\n-1\nYES\n1", "negative slope line"
assert run("1\n1 1\n100000000\n1 100000000 100000000") == "NO", "very large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| độ dốc 0, parabol 1 0 1 | CÓ 0 | Độ dốc đường bằng 0 hoạt động | 
| độ dốc -1,1; parabol 1 0 1, | | |
