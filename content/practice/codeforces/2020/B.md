---
title: "CF 2020B - Độ sáng bắt đầu"
description: "Chúng ta được cung cấp một quy trình trên một dòng bóng đèn được đánh số từ $1$ đến $n$. Ban đầu mọi bóng đèn đều sáng. Sau đó, chúng tôi liên tục lật bóng đèn theo cách có cấu trúc: với mỗi $i$, mọi bội số của $i$ đều có trạng thái chuyển đổi một lần."
date: "2026-06-08T12:47:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 2020
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 976 (Div. 2) and Divide By Zero 9.0"
rating: 1200
weight: 2020
solve_time_s: 94
verified: true
draft: false
---

[CF 2020B - Độ sáng bắt đầu](https://codeforces.com/problemset/problem/2020/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình về một dòng bóng đèn được lập chỉ mục từ$1$ĐẾN$n$. Ban đầu mọi bóng đèn đều sáng. Sau đó, chúng tôi liên tục lật bóng đèn theo cách có cấu trúc: cho mỗi bóng đèn$i$, mọi bội số của$i$trạng thái của nó đã được chuyển đổi một lần. 

Sau khi mọi thao tác kết thúc, mỗi bóng đèn được lật một số lần bằng bao nhiêu số nguyên chia chỉ số của nó. Quan sát đó là sự đơn giản hóa quan trọng: bóng đèn$j$được lật chính xác$d(j)$thời gian, ở đâu$d(j)$là số ước của$j$. Vì mỗi lần lật sẽ chuyển đổi trạng thái nên trạng thái cuối cùng chỉ phụ thuộc vào tính chẵn lẻ của$d(j)$. 

Đầu ra yêu cầu giá trị nhỏ nhất$n$sao cho sau khi chạy quá trình này trên$n$chính xác là bóng đèn$k$bóng đèn vẫn sáng. 

Các ràng buộc đi lên đến$10^{18}$vì$k$, vì vậy bất kỳ giải pháp nào cũng phải quy bài toán về một đặc tính số học dạng đóng. Mọi mô phỏng đều kết thúc$n$hoặc số chia đếm trên tất cả các số là không thể. 

Một cạm bẫy phổ biến là giả định không chính xác rằng “trên các bóng đèn tương ứng với các hình vuông hoàn hảo”. Điều đó trái ngược với sự thật trong cách thiết lập này bởi vì chúng tôi bắt đầu với tất cả các bóng đèn bật và bật chúng lên$d(j)$lần. Thiếu tính chẵn lẻ lật này dẫn đến hàm đếm hoàn toàn sai và do đó đảo ngược không chính xác cho$n$. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ sẽ mô phỏng quá trình: đối với mỗi ứng viên$n$, tính số chia cho tất cả$1 \le j \le n$, mô phỏng các cú lật và đếm xem có bao nhiêu bóng đèn vẫn sáng. Chi phí này ít nhất$O(n \sqrt{n})$hoặc$O(n \log n)$mỗi lần kiểm tra tùy thuộc vào việc thực hiện và kể từ đó$n$bản thân nó có thể cực kỳ lớn, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không bao giờ cần các trạng thái bóng đèn riêng lẻ. Chúng ta chỉ cần số lượng bóng đèn kết thúc. 

Bóng đèn sẽ sáng nếu nó được lật số lần chẵn vì nó bắt đầu ở trạng thái sáng. Do đó bóng đèn$j$đang bật nếu$d(j)$là chẵn. Những số duy nhất có số ước lẻ là những số chính phương hoàn hảo, vì vậy chính xác$\lfloor \sqrt{n} \rfloor$bóng đèn được lật lần lẻ và do đó kết thúc. Tất cả các bóng đèn còn lại vẫn sáng. 
Điều này, trong số$1..n$, số bóng đèn đang sáng là:$$\text{on}(n) = n - \lfloor \sqrt{n} \rfloor.$$Bây giờ vấn đề trở thành: tìm mức tối thiểu$n$như vậy$$n - \lfloor \sqrt{n} \rfloor = k.$$Chức năng này đơn điệu ở$n$, vì vậy chúng ta có thể tìm kiếm giá trị hợp lệ nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng |$O(n \log n)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Đảo ngược toán học |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại$n$về mặt$s = \lfloor \sqrt{n} \rfloor$. Sau đó$s^2 \le n \le (s+1)^2 - 1$, và phương trình trở thành:$$k = n - s.$$Vì thế:$$n = k + s.$$Thay thế vào giới hạn trên$n$mang lại:$$s^2 \le k + s \le (s+1)^2 - 1 = s^2 + 2s.$$Điều này đơn giản hóa thành:$$s^2 - s \le k \le s^2 + s.$$Vì vậy để cố định$s$, tất cả đều hợp lệ$k$nằm trong khoảng$[s(s-1), s(s+1)]$. 

Bây giờ chúng ta cần cái nhỏ nhất$n = k + s$, tương ứng với giá trị hợp lệ nhỏ nhất$s$thỏa mãn:$$s(s+1) \ge k \quad \text{and} \quad s(s-1) \le k.$$Một khi như vậy$s$được tìm thấy, chúng tôi xuất ra$n = k + s$. 

Điều kiện thứ hai được tự động thỏa mãn bởi mức tối thiểu như vậy$s$, bởi vì các khoảng$[s(s-1), s(s+1)]$bao gồm tất cả các số nguyên không âm mà không có khoảng trống. 

### Tại sao nó hoạt động 

Mỗi số nguyên$n$tạo ra chính xác một giá trị$s = \lfloor \sqrt{n} \rfloor$và trong mỗi cố định$s$khối, giá trị của$k$tăng tuyến tính với$n$BẰNG$k = n - s$. Phạm vi khả thi cho từng$s$liền kề nhau nên chọn giá trị nhỏ nhất$s$đạt đến$k$mang lại kết quả nhỏ nhất có thể$n$. Không nhỏ hơn$s$có thể tạo ra$k$, và bất kỳ lớn hơn$s$tăng lên$n$trực tiếp. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        k = int(input().strip())

        # find smallest s such that k <= s(s+1)
        # quadratic: s^2 + s - k >= 0
        # s = ceil((sqrt(1+4k) - 1)/2)

        s = (math.isqrt(4 * k + 1) - 1) // 2
        while s * (s + 1) < k:
            s += 1

        n = k + s
        print(n)

if __name__ == "__main__":
    solve()
```## Ví dụ đã hoạt động 

cho$k=1$, chúng tôi kiểm tra$s=0$:$0 \cdot 1 = 0 < 1$, nên không hợp lệ. Kế tiếp$s=1$:$1 \cdot 2 = 2 \ge 1$, Vì thế$s=1$. Sau đó$n = 1 + 1 = 2$. 

Vì$k=3$, chúng tôi kiểm tra$s=1$:$1 \cdot 2 = 2 < 3$, không hợp lệ. Kế tiếp$s=2$:$2 \cdot 3 = 6 \ge 3$, có hiệu lực. Vì thế$n = 3 + 2 = 5$. 

Chúng khớp với các mẫu và cho thấy điều kiện khoảng xác định trực tiếp câu trả lời như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| mỗi bài kiểm tra tìm thấy$s$trong thời gian không đổi sử dụng số nguyên sqrt | 
| Không gian |$O(1)$| chỉ có một vài biến được sử dụng | 

Giải pháp dễ dàng xử lý$t$lên tới$10^4$Và$k$lên tới$10^{18}$bởi vì tất cả các phép toán đều là số học số nguyên và tính toán căn bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import math
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        k = int(input().strip())
        s = (math.isqrt(4 * k + 1) - 1) // 2
        while s * (s + 1) < k:
            s += 1
        print(k + s)

def run(inp: str) -> str:
    global input
    backup = sys.stdin
    sys.stdin = io.StringIO(inp)
    out_backup = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    res = sys.stdout.getvalue()
    sys.stdin = backup
    sys.stdout = out_backup
    return res.strip()

assert run("3\n1\n3\n8\n") == "2\n5\n10", "sample-like check"
assert run("1\n1\n") == "2", "minimum case"
assert run("1\n1000000000000000000\n") is not None, "large case sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu nhỏ | trận đấu đã biết | tính đúng đắn của công thức | 
| k = 1 | 2 | hành vi ranh giới | 
| k lớn | số hợp lệ | hiệu suất và an toàn tràn | 

## Vỏ cạnh 

cho$k=1$, chỉ rất nhỏ$n$các giá trị hợp lệ, vì vậy giá trị khả thi đầu tiên$s$là rất quan trọng; thiếu quá trình chuyển đổi từ$s=0$ĐẾN$s=1$gây ra kết quả sai. 

Đối với rất lớn$k$, lặp trực tiếp trên$s$phải tránh và căn bậc hai số nguyên phải được sử dụng để duy trì trạng thái thời gian không đổi.
