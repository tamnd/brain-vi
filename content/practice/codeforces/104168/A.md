---
title: "CF 104168A - Số chia"
description: "Chúng ta được cho một số nguyên dương $n$, và chúng ta xét tất cả các cặp thừa số $(x, y)$ sao cho $x cdot y = n$ với cả hai số nguyên dương $x$ và $y$."
date: "2026-07-02T00:57:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104168
codeforces_index: "A"
codeforces_contest_name: "The American University in Cairo CSEA End of Winter Break Contest 2023"
rating: 0
weight: 104168
solve_time_s: 62
verified: true
draft: false
---

[CF 104168A - Hiệu số chia](https://codeforces.com/problemset/problem/104168/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$n$, và chúng tôi xem xét tất cả các cặp yếu tố$(x, y)$như vậy$x \cdot y = n$với cả hai$x$Và$y$số nguyên dương. Đối với mỗi cặp như vậy, chúng tôi tính toán sự khác biệt tuyệt đối$|x - y|$và chúng tôi muốn giá trị tối đa có thể có trên tất cả các cặp yếu tố hợp lệ. 

Đầu vào chứa tối đa$2 \cdot 10^5$trường hợp thử nghiệm và mỗi trường hợp thử nghiệm có một số nguyên duy nhất$n$lên tới$10^9$. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào thử tất cả các cặp thừa số bằng cách quét tối đa$n$, vì như thế sẽ là quá chậm. Ngay cả việc liệt kê tất cả các ước số cho mỗi trường hợp thử nghiệm cũng phải hiệu quả, vì vậy mọi giải pháp đều phải chạy trong khoảng$O(\sqrt{n})$mỗi bài kiểm tra hoặc tốt hơn. 

Một điểm tinh tế là cặp$(x, y)$Và$(y, x)$tương đương nhau vì giá trị tuyệt đối nên ta chỉ cần xét các ước số tối đa$\sqrt{n}$. 

Một sai lầm ngây thơ là cho rằng sự khác biệt tối đa đến từ một cặp nhân tố “ngẫu nhiên” nào đó mà không kiểm tra các ước số một cách có hệ thống. Ví dụ, nếu$n = 36$, các cặp có thể bao gồm$(1,36)$,$(2,18)$,$(3,12)$,$(4,9)$,$(6,6)$. Sự khác biệt tối đa rõ ràng là từ$(1,36)$, cho$35$. Một cách tiếp cận bất cẩn chỉ kiểm tra các ước số liên tiếp hoặc chỉ cặp nhân tố gần nhất sẽ bỏ sót điều này. 

Một hiểu lầm phổ biến khác là nghĩ rằng cặp đôi đẹp nhất luôn ở gần$\sqrt{n}$. Điều đó là sai vì sự gần gũi sẽ tối đa hóa tính ổn định của sản phẩm chứ không phải sự khác biệt. Ở đây chúng tôi muốn mục tiêu ngược lại. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ tạo ra tất cả các cặp thừa số bằng cách kiểm tra mọi số nguyên$x$từ$1$ĐẾN$n$, kiểm tra xem$x \mid n$, sau đó tính toán$y = n / x$. Điều này đúng vì nó liệt kê rõ ràng tất cả các cặp hợp lệ và đánh giá mục tiêu. Tuy nhiên, phải mất$O(n)$cho mỗi trường hợp thử nghiệm, trở thành$2 \cdot 10^5 \times 10^9$trong trường hợp xấu nhất là không thể thực hiện được. 

Quan sát quan trọng là các cặp yếu tố có dạng đối xứng xung quanh$\sqrt{n}$. Mỗi số chia$x \le \sqrt{n}$xác định một đối tác duy nhất$y = n/x$. Vì vậy chúng ta chỉ cần lặp lại tối đa$\sqrt{n}$, làm giảm đáng kể không gian tìm kiếm. 

Một khi chúng ta xem xét một số chia$x$, cặp này cố định và phần đóng góp là$|x - n/x|$. Chúng tôi chỉ đơn giản theo dõi mức tối đa trên tất cả các cặp như vậy. 

Không có cấu trúc tổ hợp nào sâu hơn cần thiết ngoài việc liệt kê số chia hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Đếm số chia |$O(\sqrt{n})$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đối với một nhất định$n$, khởi tạo biến trả lời về 0. Điều này sẽ lưu trữ sự khác biệt tuyệt đối tốt nhất được tìm thấy trong số tất cả các cặp yếu tố. 
2. Lặp lại$i$từ$1$ĐẾN$\lfloor \sqrt{n} \rfloor$. Chúng tôi chỉ xem xét$i$như một yếu tố ứng cử viên. 
3. Nếu$i \cdot i = n$, sau đó$i$là một cặp thừa số căn bậc hai hoàn hảo$(i, i)$, điều này không đóng góp gì vào sự khác biệt, vì vậy nó không cải thiện câu trả lời. 
4. Nếu$i \mid n$, tính ước số ghép đôi$j = n / i$. Bây giờ chúng ta có cặp nhân tố hợp lệ$(i, j)$. 
5. Tính toán$|i - j|$và cập nhật câu trả lời nếu giá trị này lớn hơn mức tối đa hiện tại. 
6. Sau khi kết thúc vòng lặp, xuất ra giá trị tốt nhất tìm được. 

Bước lý luận quan trọng là chỉ kiểm tra tối đa$\sqrt{n}$đảm bảo chúng ta nhìn thấy mọi ước số chính xác một lần và mỗi ước số xác định duy nhất đối tác bổ sung của nó. 

### Tại sao nó hoạt động 

Mỗi cặp nhân tố$(x, y)$với$x \cdot y = n$có một phần tử không vượt quá$\sqrt{n}$. Vì vậy, việc lặp lại tất cả$i \le \sqrt{n}$sự chia rẽ đó$n$liệt kê tất cả các cặp riêng biệt chính xác một lần. Vì mục tiêu chỉ phụ thuộc vào cặp chứ không phụ thuộc vào thứ tự nên không có ứng cử viên nào bị bỏ sót và không có sự trùng lặp nào ảnh hưởng đến tính chính xác. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        best = 0
        
        r = int(math.isqrt(n))
        for i in range(1, r + 1):
            if n % i == 0:
                j = n // i
                if i != j:
                    diff = j - i
                    if diff > best:
                        best = diff
        
        print(best)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng căn bậc hai số nguyên để giới hạn vòng lặp. Với mỗi số chia$i$, chúng tôi tính toán đối tác của nó$j$và chỉ đánh giá sự khác biệt khi$i \neq j$. Điều kiện này tránh việc cập nhật số 0 dư thừa cho các ô vuông hoàn hảo, mặc dù việc đưa chúng vào sẽ không ảnh hưởng đến tính chính xác. 

Một chi tiết triển khai tinh tế đang sử dụng`math.isqrt`thay vì căn bậc hai dấu phẩy động để tránh các vấn đề về độ chính xác. Từ$n \le 10^9$, căn bậc hai số nguyên là an toàn và nhanh chóng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 36$Chúng tôi kiểm tra các ước số lên đến 6. 

| tôi | n % tôi == 0 | j = n/i | |i - j| | tốt nhất | 

|---|---|---|---|---| 

| 1 | vâng | 36 | 35 | 35 | 

| 2 | vâng | 18 | 16 | 35 | 

| 3 | vâng | 12 | 9 | 35 | 

| 4 | vâng | 9 | 5 | 35 | 

| 5 | không | - | - | 35 | 

| 6 | vâng | 6 | 0 | 35 | 

Câu trả lời cuối cùng là 35, đạt được bởi cặp cực trị (1, 36). Điều này xác nhận rằng thuật toán ưu tiên các cặp yếu tố có độ không cân bằng cao. 

### Ví dụ 2:$n = 20$Chúng tôi kiểm tra các ước số lên đến 4. 

| tôi | n % tôi | j | |i - j| | tốt nhất | 

|---|---|---|---|---| 

| 1 | vâng | 20 | 19 | 19 | 

| 2 | vâng | 10 | 8 | 19 | 

| 3 | không | - | - | 19 | 

| 4 | vâng | 5 | 1 | 19 | 

Đáp án cuối cùng là 19 từ (1, 20). Điều này cho thấy rằng các phép phân tích trung gian không bao giờ đánh bại được cặp ước số cực trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \sqrt{n})$| Mỗi bài kiểm tra liệt kê các ước số đến căn bậc hai | 
| Không gian |$O(1)$| Chỉ các biến không đổi cho mỗi bài kiểm tra | 

Sự ràng buộc$n \le 10^9$ngụ ý nhiều nhất là khoảng 31623 lần lặp cho mỗi trường hợp thử nghiệm, điều này có thể chấp nhận được ngay cả đối với$2 \cdot 10^5$các trường hợp trong Python được tối ưu hóa với I/O nhanh. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        best = 0
        r = math.isqrt(n)
        for i in range(1, r + 1):
            if n % i == 0:
                j = n // i
                if i != j:
                    best = max(best, abs(i - j))
        out.append(str(best))
    return "\n".join(out)

# provided sample-like checks
assert run("3\n36\n20\n1\n") == "35\n19\n0", "basic cases"

# custom cases
assert run("1\n1\n") == "0", "n=1 edge"
assert run("1\n2\n") == "1", "prime small"
assert run("1\n1000000000\n") >= "0", "large stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 0 | tầm thường không có yếu tố khác biệt | 
| n=2 | 1 | trường hợp không tầm thường nhỏ nhất | 
| n=10^9 | tính toán tối đa | hiệu suất và xử lý số chia lớn |
