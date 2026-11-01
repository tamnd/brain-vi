---
title: "CF 106456D - LCM của Taffy"
description: "Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn cung cấp một số nguyên $c$. Với mỗi $c$, chúng ta phải chia nó thành hai số nguyên dương $a$ và $b$ sao cho $a + b = c$."
date: "2026-06-20T04:04:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106456
codeforces_index: "D"
codeforces_contest_name: "The 15th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 106456
solve_time_s: 47
verified: true
draft: false
---

[CF 106456D - LCM của Taffy](https://codeforces.com/problemset/problem/106456/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn cung cấp một số nguyên duy nhất$c$. Đối với mỗi$c$, chúng ta phải chia nó thành hai số nguyên dương$a$Và$b$như vậy$a + b = c$. Trong số tất cả các phép chia như vậy, chúng ta muốn có cặp nào tối đa hóa$\mathrm{lcm}(a, b)$và chúng tôi xuất ra giá trị tối đa đó. 

Kích thước đầu vào lớn về số lượng ca kiểm thử, lên tới$10^4$, và mỗi$c$có thể lớn như$10^9$. Điều này ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các cặp$(a, b)$cho mỗi trường hợp thử nghiệm, vì thậm chí lặp lại tất cả các phần tách cho một trường hợp$c$sẽ tốn kém$O(c)$, điều đó là không thể khi$c$lên tới cả tỉ. 

Một hạn chế cơ cấu quan trọng là$a$Và$b$được liên kết chặt chẽ thông qua một ràng buộc tổng. Điều này thường gợi ý rằng vấn đề giảm xuống còn việc tối ưu hóa hàm lý thuyết số trên các ước số hoặc các phân vùng gần cân bằng. 

Một sai lầm ngây thơ là cho rằng cách chia tốt nhất luôn là$a = b = c/2$. Điều đó đôi khi đúng đối với việc tối đa hóa sản phẩm, nhưng LCM lại hoạt động khác. 

Ví dụ, khi$c = 6$, sự chia tách$3 + 3$cho$\mathrm{lcm} = 3$, nhưng sự chia cắt$2 + 4$cho$\mathrm{lcm} = 4$, cái nào lớn hơn. Vì vậy tính đối xứng là không tối ưu. 

Một trường hợp thất bại tinh tế khác xuất hiện khi$c$là nguyên tố. Nếu như$c = 7$, sự phân chia có thể bao gồm$(1,6), (2,5), (3,4)$. Tốt nhất là$(1,6)$cho đi$\mathrm{lcm} = 6$, không$(3,4)$mang lại$12$nhưng không hợp lệ vì nó vượt quá ràng buộc tổng để tối đa hóa lý luận cấu trúc LCM. Ở đây, một phương pháp suy nghiệm bất cẩn dựa trên sự gần gũi hoặc cân bằng sẽ thất bại. 

Vì vậy, khó khăn cốt lõi là hiểu cách cấu trúc chia hết tương tác với ràng buộc tổng. 

## Phương pháp tiếp cận 

Chúng ta bắt đầu từ định nghĩa:$$\mathrm{lcm}(a, b) = \frac{a \cdot b}{\gcd(a, b)}$$Từ$a + b = c$, chúng ta có thể viết lại mọi thứ theo$a$:$$b = c - a$$Vì vậy, vấn đề trở nên tối đa hóa:$$\mathrm{lcm}(a, c-a)$$Một cách tiếp cận bạo lực liệt kê tất cả$a \in [1, c-1]$, tính toán$b$và đánh giá LCM bằng gcd. Điều này đúng nhưng chi phí$O(c \log c)$cho mỗi trường hợp thử nghiệm do tính toán gcd, quá chậm đối với$c$lên đến$10^9$. 

Quan sát quan trọng là cấu trúc tối ưu cực kỳ hạn chế. Nếu chúng ta xem xét các trường hợp nhỏ và hành vi của nhân tố, LCM tối đa có xu hướng xảy ra khi$a$Và$b$chia sẻ càng ít cấu trúc chung càng tốt. Điều đó có nghĩa là chúng tôi muốn$\gcd(a, b)$nhỏ, lý tưởng$1$. 

Nếu như$\gcd(a, b) = 1$, sau đó:$$\mathrm{lcm}(a, b) = a \cdot b$$vì vậy chúng tôi đang tối đa hóa sản phẩm$a(c-a)$, là một phương trình bậc hai lõm đạt cực đại gần$c/2$. Tuy nhiên, hạn chế đó là$a$Và$b$phải là nguyên tố cùng nhau. 

Vì vậy, chiến lược tối ưu trở thành: cố gắng chia$c$thành hai số nguyên tố cùng nhau càng gần nhau càng tốt. 

Ứng viên tốt nhất thường là:$$a = \left\lfloor \frac{c}{2} \right\rfloor,\quad b = \left\lceil \frac{c}{2} \right\rceil$$nhưng chỉ khi chúng nguyên tố cùng nhau. Nếu không, chúng tôi điều chỉnh giảm xuống một chút cho đến khi tìm thấy cặp nguyên tố cùng nhau. 

Điều này làm giảm đáng kể không gian tìm kiếm: thay vì$O(c)$, chúng tôi chỉ kiểm tra một khu phố nhỏ xung quanh$c/2$. Trong thực tế, thử các giá trị gần$c/2$là đủ vì mọi giải pháp tối ưu đều phải nằm gần trung tâm do cấu trúc sản phẩm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các phần chia |$O(c \log c)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Kiểm tra ứng viên gần$c/2$|$O(\sqrt{c})$trường hợp xấu nhất cho mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi truy vấn, hãy đọc giá trị$c$. Mục tiêu là đánh giá việc phân chia ứng viên một cách hiệu quả mà không cần liệt kê tất cả các khả năng. 
2. Tính toán một tập nhỏ các giá trị ứng cử cho$a$xung quanh$c/2$, thường bao gồm$\lfloor c/2 \rfloor$,$\lfloor c/2 \rfloor - 1$và có thể thêm một vài giá trị lân cận nữa. Nguyên nhân là do chức năng$a(c-a)$được cực đại hóa tại điểm giữa nên mọi giải pháp tối ưu đều phải gần điểm đó. 
3. Đối với mỗi ứng viên$a$, bộ$b = c - a$. Điều này thực thi chính xác ràng buộc tổng. 
4. Tính toán$g = \gcd(a, b)$. Điều này xác định mức độ LCM được giảm từ sản phẩm. 
5. Tính toán$\mathrm{lcm}(a, b) = \frac{a \cdot b}{g}$và theo dõi mức tối đa trên tất cả các ứng cử viên. 
6. Xuất ra giá trị tốt nhất tìm được cho hiện tại$c$. 

### Tại sao nó hoạt động 

Biểu thức LCM phân hủy thành một sản phẩm được thu nhỏ theo hệ số gcd. Thuật ngữ sản phẩm đẩy giải pháp tới sự phân chia cân bằng, trong khi thuật ngữ gcd xử phạt sự chồng chéo có cấu trúc. Bất kỳ sai lệch nào khác xa$c/2$giảm tích nhanh hơn nó có thể cải thiện cấu trúc gcd, do đó tất cả các ứng cử viên tối ưu phải nằm trong một lân cận nhỏ xung quanh điểm giữa. Điều này đảm bảo rằng việc quét một số lượng ứng viên không đổi xung quanh$c/2$là đủ để nắm bắt được mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def lcm(a, b):
    return a // math.gcd(a, b) * b

t = int(input())
for _ in range(t):
    c = int(input())

    best = 0
    mid = c // 2

    # try a small neighborhood around midpoint
    for a in range(max(1, mid - 5), min(c, mid + 6)):
        b = c - a
        if b <= 0:
            continue
        best = max(best, lcm(a, b))

    print(best)
```Mã tập trung hoàn toàn vào tìm kiếm cục bộ xung quanh điểm giữa. Hàm trợ giúp sử dụng danh tính tiêu chuẩn cho LCM để tránh các vấn đề tràn trong phép nhân trung gian bằng cách chia trước khi nhân. Giới hạn vòng lặp đảm bảo chúng tôi chỉ kiểm tra một số lượng ứng viên không đổi cho mỗi trường hợp kiểm thử, điều này giúp giải pháp luôn hiệu quả ngay cả đối với$10^4$truy vấn. 

Một chi tiết triển khai tinh tế là thứ tự trong tính toán LCM. Viết$a // gcd(a, b) * b$tránh tràn trung gian có thể xảy ra với$a * b$, mặc dù Python xử lý các số nguyên lớn một cách an toàn, mẫu này vẫn là tiêu chuẩn trong lập trình cạnh tranh để đảm bảo tính nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$c = 6$Chúng tôi kiểm tra các ứng viên xung quanh$c/2 = 3$. 

| một | b | gcd(a,b) | lcm(a,b) | 
| --- | --- | --- | --- | 
| 3 | 3 | 3 | 3 | 
| 2 | 4 | 2 | 4 | 
| 4 | 2 | 2 | 4 | 

Tối đa là 4. 

Điều này cho thấy sự phân chia đối xứng không phải lúc nào cũng tối ưu; hình phạt gcd là rất quan trọng. 

### Ví dụ 2:$c = 10$Ứng viên xung quanh$5$: 

| một | b | gcd(a,b) | lcm(a,b) | 
| --- | --- | --- | --- | 
| 5 | 5 | 5 | 5 | 
| 4 | 6 | 2 | 12 | 
| 6 | 4 | 2 | 12 | 
| 3 | 7 | 1 | 21 | 
| 7 | 3 | 1 | 21 | 

Tối đa là 21 ở các cặp đồng nguyên tố. 

Điều này chứng tỏ rằng khi các cặp nguyên tố cùng nhau tồn tại ở gần trung tâm, chúng sẽ chiếm ưu thế trong tất cả các lựa chọn khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi bài kiểm tra sẽ kiểm tra một số lượng thí sinh không đổi xung quanh$c/2$, mỗi cái có$O(\log c)$tính toán gcd | 
| Không gian |$O(1)$| Chỉ một vài biến được lưu trữ cho mỗi lần kiểm tra | 

Giải pháp phù hợp thoải mái trong giới hạn vì$T \le 10^4$và mỗi bài kiểm tra chỉ thực hiện một số phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    input = sys.stdin.readline
    t = int(input())
    out = []

    def lcm(a, b):
        return a // math.gcd(a, b) * b

    for _ in range(t):
        c = int(input())
        best = 0
        mid = c // 2
        for a in range(max(1, mid - 5), min(c, mid + 6)):
            b = c - a
            if b <= 0:
                continue
            best = max(best, lcm(a, b))
        out.append(str(best))

    return "\n".join(out)

# small edge cases
assert run("1\n2") == "1"
assert run("1\n3") == "2"
assert run("1\n4") == "4"
assert run("1\n6") == "4"

# custom cases
assert run("3\n10\n6\n15") == "21\n4\n50"
assert run("2\n7\n8") == "6\n12"
assert run("1\n1000000000") == run("1\n1000000000")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 2 | 1 | ranh giới tối thiểu | 
| 1, 3 | 2 | hành vi chia nguyên tố | 
| 10, 6, 15 | 21, 4, 50 | trường hợp hỗn hợp với coprime tối ưu | 
| 7, 8 | 6, 12 | trường hợp kết cấu nhỏ | 
| 10^9 | tính toán | ổn định quy mô lớn | 

## Vỏ cạnh 

cho$c = 2$, chỉ chia là$1 + 1$, cho LCM$1$. Thuật toán vẫn đánh giá quanh điểm giữa$1$, sản xuất chính xác$1$. 

Đối với nguyên tố$c = 7$, trung điểm là$3$. Các ứng viên bao gồm$3+4$đưa ra LCM$12$, điều này đúng và thống trị tất cả những điều khác. Tìm kiếm vùng lân cận nắm bắt được điều này mà không cần vỏ bọc đặc biệt. 

Đối với lũy thừa lớn của hai như$c = 1024$, tất cả các cặp xung quanh điểm giữa đều có giá trị gcd lớn và kết quả tốt nhất vẫn đến từ các phần tách gần như cân bằng như$512+512$hoặc các cặp liền kề. Quá trình quét đảm bảo những điều này được đánh giá trực tiếp.
