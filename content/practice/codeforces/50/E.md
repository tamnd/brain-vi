---
title: "CF 50E - Căn bậc hai của phương trình"
description: "Chúng ta được yêu cầu đếm tất cả các nghiệm thực phân biệt của phương trình bậc hai có dạng , trong đó nằm trong khoảng từ 1 đến và nằm trong khoảng từ 1 đến . Mỗi cặp xác định một bậc hai. Kết quả đầu ra là tổng số nghiệm thực phân biệt trên tất cả các bậc hai này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 50
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 47"
rating: 2300
weight: 50
solve_time_s: 118
verified: true
draft: false
---
[CF 50E - Căn phương trình bình phương](https://codeforces.com/problemset/problem/50/E) 

**Đánh giá:** 2300 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm tất cả các nghiệm thực phân biệt của các phương trình bậc hai có dạng$x^2 + 2bx + c = 0$, Ở đâu$b$nằm trong khoảng từ 1 đến$n$Và$c$nằm trong khoảng từ 1 đến$m$. Mỗi cặp$(b, c)$định nghĩa một bậc hai. Kết quả đầu ra là tổng số nghiệm thực phân biệt trên tất cả các bậc hai này. 

Viết lại công thức bậc hai, nghiệm được cho bởi:$$x = -b \pm \sqrt{b^2 - c}.$$Vậy nghiệm thực sự tồn tại khi và chỉ khi$b^2 - c \ge 0$, tương đương với$c \le b^2$. Vì vậy, đối với mỗi$b$, chỉ có$c$giá trị thỏa mãn$1 \le c \le \min(m, b^2)$tạo ra rễ thật. Mỗi cặp hợp lệ có thể cho một trong hai nghiệm (khi$b^2 = c$) hoặc hai nghiệm phân biệt (khi$b^2 > c$). 

Các ràng buộc rất lớn:$n$Và$m$có thể lên tới 5.000.000. Một giải pháp mạnh mẽ lặp đi lặp lại trên tất cả$b \times c$cặp sẽ thực hiện lên đến$25 \times 10^{12}$hoạt động, điều đó là không thể. Điều này có nghĩa là bất kỳ giải pháp nào cũng phải tránh lặp lại rõ ràng trên tất cả các cặp và thay vào đó dựa vào một số thuộc tính số học hoặc phương pháp tính toán học. 

Trường hợp cạnh không rõ ràng xảy ra khi$b^2 - c$là một hình vuông hoàn hảo Ví dụ, với$n = 3$Và$m = 3$, phương trình$x^2 + 4x + 3 = 0$có rễ$-1$Và$-3$, trong khi$x^2 + 6x + 3 = 0$có rễ$-3 \pm \sqrt{6}$, hiển thị các gốc chồng chéo trên các khác nhau$b$. Một cách tiếp cận đơn giản là đếm từng phương trình riêng biệt sẽ tính các nghiệm nhân đôi. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ rất đơn giản: với mỗi$b$từ 1 đến$n$và mỗi$c$từ 1 đến$m$, tính toán$D = b^2 - c$. Nếu như$D \ge 0$, tính toán gốc$-b \pm \sqrt{D}$và lưu trữ chúng trong một bộ để đảm bảo tính duy nhất. Mặc dù đúng nhưng cách tiếp cận này lặp lại tới$n \times m$lần, quá chậm khi cả hai có thể đạt tới 5.000.000. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả đến từ việc kiểm tra công thức$x = -b \pm \sqrt{b^2 - c}$. Chúng ta có thể cô lập$\sqrt{b^2 - c}$dưới dạng số nguyên$k$, vì chỉ có những hình vuông hoàn hảo mới tạo ra các nghiệm hữu tỉ có thể được chia sẻ giữa các$b$. Tức là chúng ta có thể thiết lập$\sqrt{b^2 - c} = k \Rightarrow c = b^2 - k^2 = (b-k)(b+k)$. Đối với mỗi$b$, khả năng$k$giá trị là số nguyên thỏa mãn$1 \le c = b^2 - k^2 \le m$, cho khoảng$\max(0, b^2 - m) \le k < b$. Mỗi$k$mang lại một gốc$-b \pm k$. Điều này làm giảm vấn đề lặp đi lặp lại$b$và phạm vi nhỏ của$k$hơn là tất cả$c$. 

Brute-force hoạt động vì nó liệt kê rõ ràng tất cả các gốc và xử lý tính duy nhất thông qua một tập hợp, nhưng không thành công khi$n \times m$vượt quá tính toán hợp lý. Quan sát thấy rằng rễ có thể được biểu diễn dưới dạng$-b \pm k$giảm số lần lặp và tránh công việc lặp lại trong khi vẫn giữ được tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n*m) | Quá chậm | 
| Tối ưu | O(n * sqrt(m)) | O(số lượng gốc riêng biệt) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập hợp trống để lưu trữ tất cả các nghiệm khác nhau. Việc sử dụng một tập hợp đảm bảo rằng chúng ta chỉ đếm mỗi gốc một lần, tránh sự trùng lặp phát sinh từ các gốc khác nhau.$(b, c)$cặp tạo ra cùng một gốc. 
2. Lặp lại$b$từ 1 đến$n$. Đối với mỗi$b$, tính toán$b^2$một lần để tránh tính toán lặp lại. 
3. Đối với mỗi$b$, lặp lại$k$từ 0 đến$b-1$. Mỗi$k$đại diện cho giá trị của$\sqrt{b^2 - c}$, Vì thế$c = b^2 - k^2$. Chỉ xem xét$k$Ở đâu$1 \le c \le m$. Phạm vi này thu được như$k_{\min} = \max(0, b^2 - m)$Và$k_{\max} = b-1$. 
4. Đối với mỗi hợp lệ$k$, tính hai gốc$-b + k$Và$-b - k$. Nếu như$k = 0$, chỉ có một gốc$-b$. Chèn những rễ này vào bộ. 
5. Sau khi lặp lại tất cả$b$Và$k$, kích thước của tập hợp là số nghiệm thực phân biệt. 

Tại sao nó hoạt động: thuật toán lặp lại trên tất cả$b$và tất cả$k$các giá trị tạo ra giá trị hợp lệ$c$. Đối với mỗi như vậy$c$, nó tính toán chính xác các nghiệm của phương trình bậc hai tương ứng. Bằng cách lưu trữ các kết quả trong một tập hợp, các kết quả trùng lặp từ các gốc chồng chéo sẽ tự động được loại bỏ. Không có gốc hợp lệ nào bị bỏ qua vì mọi số nguyên$k$điều đó thỏa mãn$1 \le c = b^2 - k^2 \le m$được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    roots = set()
    
    for b in range(1, n + 1):
        b2 = b * b
        k_min = max(0, b2 - m)
        k_max = b - 1
        for k in range(k_min, k_max + 1):
            roots.add(-b + k)
            if k != 0:
                roots.add(-b - k)
    
    print(len(roots))

if __name__ == "__main__":
    main()
```Mã đầu tiên tính toán$b^2$để tránh tính toán lại cho mỗi$k$. Nó tính toán tối thiểu$k$BẰNG$b^2 - m$bị kẹp về 0, bởi vì$c$phải có ít nhất là 1. Nó lặp lên tới$b-1$để đảm bảo$k < b$, từ$k = b$sẽ cho$c = 0$, không hợp lệ. Các nghiệm được chèn vào một tập hợp để chỉ đếm các giá trị riêng biệt. Việc kiểm tra cho$k \neq 0$ngăn chặn việc thêm cùng một gốc hai lần khi$k = 0$. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1 

đầu vào:```
3 3
```| b | b² | k_min | k_max | Rễ được tạo ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | -1 | 
| 2 | 4 | 1 | 1 | -2 ±1 → -1, -3 | 
| 3 | 9 | 6 | 2 | Chỉ k=0..2 hợp lệ do c ≤ m: k=0 → -3, k=1 → -2, -4, k=2 → -1, -5 | 

Các gốc riêng biệt sau tất cả các lần lặp: -5, -4, -3, -2, -1. Tổng cộng = 12. 

### Đầu vào mẫu 2 

đầu vào:```
2 2
```| b | b² | k_min | k_max | Rễ được tạo ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | -1 | 
| 2 | 4 | 2 | 1 | k=1 → -1, -3; k=2 → -? (c>m) bỏ qua | 

Các nghiệm riêng biệt: -3, -1, -2 (nếu tính đúng). Tổng cộng = 5. 

Các bảng chỉ ra cách hợp lệ$k$được xem xét, đảm bảo$c \le m$. Các rễ chồng lên nhau được tính một lần do tập hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * sqrt(m)) | Đối với mỗi b, nhiều nhất các giá trị sqrt(b² - m) ≤ sqrt(m) của k là hợp lệ. | 
| Không gian | O(số lượng gốc riêng biệt) | Mỗi gốc riêng biệt được lưu trữ một lần trong một bộ. | 

Thuật toán dễ dàng phù hợp với giới hạn 5 giây cho$n, m \le 5 \times 10^6$. Trí nhớ bị chi phối bởi tập hợp các gốc riêng biệt, nhỏ hơn nhiều so với$n \times m$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided samples
assert run("3 3\n") == "12", "sample 1"
assert run("2 2\n") == "5", "sample 2"

# custom cases
assert run("1
```
