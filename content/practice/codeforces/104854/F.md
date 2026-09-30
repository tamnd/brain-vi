---
title: "CF 104854F - Giai thừa nguyên tố"
description: "Chúng ta được cấp một số nguyên $x$. Nhiệm vụ của chúng ta là tìm số $y$ lớn nhất không vượt quá $x$, có hai thuộc tính cùng một lúc. Đầu tiên, $y$ phải là số nguyên tố."
date: "2026-06-28T11:04:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104854
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC, Swiss Subregional"
rating: 0
weight: 104854
solve_time_s: 46
verified: true
draft: false
---

[CF 104854F - Giai thừa nguyên tố](https://codeforces.com/problemset/problem/104854/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$x$. Nhiệm vụ của chúng ta là tìm số lớn nhất$y$điều đó không vượt quá$x$, với hai thuộc tính cùng một lúc. Đầu tiên,$y$phải là số nguyên tố. Thứ hai,$y$cũng phải được biểu diễn dưới dạng giai thừa của một số nguyên nào đó, nghĩa là tồn tại một số nguyên dương$z$như vậy$y = z!$. 

Vì vậy, chúng tôi đang tìm kiếm các số có dạng$1!, 2!, 3!, \dots$đó cũng là số nguyên tố, và trong số đó chúng ta muốn có số lớn nhất lớn nhất$x$. Nếu không có số đó tồn tại, chúng tôi xuất ra$-1$. 

Ràng buộc$x \le 10^5$là cực kỳ nhỏ đối với các tiêu chuẩn lập trình cạnh tranh, điều này ngay lập tức gợi ý rằng chúng ta không cần bất kỳ cấu trúc dữ liệu nâng cao nào hoặc máy móc lý thuyết số được tối ưu hóa tiệm cận. Ngay cả một phép tính trước có kích thước không đổi cũng sẽ đủ nếu tập hợp các ứng cử viên nhỏ. 

Một sự tinh tế quan trọng nằm ở việc hiểu sự tăng trưởng giai thừa. Giai thừa tăng trưởng cực kỳ nhanh chóng, do đó chỉ có những giá trị rất nhỏ của$z$sẽ tạo ra kết quả trong phạm vi đầu vào. Điều này hạn chế đáng kể không gian tìm kiếm. 

Một sai lầm tiềm ẩn là giải thích vấn đề như tìm kiếm trên các số nguyên tố đến$x$có một số biểu diễn giai thừa ẩn. Điều đó sẽ không chính xác, vì bản thân các giá trị giai thừa rất thưa thớt và chúng tôi không lọc các số nguyên tố nói chung mà chỉ lọc các số giai thừa. 

Để giải quyết vấn đề này bằng các ví dụ, nếu$x = 10$, thì giai thừa là$1, 2, 6, 24, \dots$. Trong đó, số nguyên tố là$2$chỉ một. Vậy câu trả lời là$2$. Nếu như$x = 1$, không có giai thừa nào vừa là số nguyên tố vừa hợp lệ theo nghĩa có ý nghĩa, vì vậy câu trả lời là$-1$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ tạo ra các giai thừa một cách tuần tự: bắt đầu từ$1! = 1$, sau đó tính$2!, 3!, \dots$cho đến khi giá trị vượt quá$x$. Đối với mỗi giá trị giai thừa, chúng tôi kiểm tra xem nó có phải là số nguyên tố hay không. Giá trị lớn nhất không vượt quá$x$trở thành câu trả lời. 

Điều này hiệu quả vì giai thừa phát triển đủ nhanh nên chúng tôi chỉ đánh giá một số ít giá trị ngay cả ở giới hạn trên$10^5$. Trong thực tế,$5! = 120$đã vượt quá giới hạn nên chỉ$1!$bởi vì$5!$luôn có liên quan. 

Điểm nghẽn của một lối suy nghĩ ngây thơ là nghĩ rằng chúng ta cần kiểm tra tính nguyên tố cho tất cả các số lên đến$x$, đó sẽ là$O(x \sqrt{x})$hoặc tệ hơn. Điều đó là không cần thiết vì giá trị giai thừa không dày đặc; chúng tôi chỉ kiểm tra tối đa năm ứng viên. 

Quan sát quan trọng là các giai thừa vượt ra ngoài$5!$không liên quan do kích thước và trong số các giai thừa nhỏ, tính nguyên tố rất dễ được xác minh bằng cách kiểm tra hoặc kiểm tra trực tiếp. 

Bây giờ chúng tôi liệt kê rõ ràng các giá trị giai thừa và tính nguyên tố của chúng:$1! = 1$, không phải số nguyên tố$2! = 2$, xuất sắc$3! = 6$, không phải số nguyên tố$4! = 24$, không phải số nguyên tố$5! = 120$, không phải số nguyên tố 

Ngoài ra, tất cả các giai thừa đều chẵn và lớn hơn 2, vì vậy chúng không thể là số nguyên tố. Điều này giải quyết hoàn toàn vấn đề kiểm tra một tập hợp ứng viên không đổi. 

Vì vậy, giá trị hợp lệ duy nhất có thể là$2$, cung cấp$x \ge 2$. Nếu không thì không có giải pháp nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giai thừa Brute Force + Kiểm tra thủ tướng | O(1) (hằng số hiệu quả) | O(1) | Đã chấp nhận | 
| Quan sát được tính toán trước tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định tất cả các giá trị giai thừa nằm trong hoặc có thể gần phạm vi ràng buộc. Chúng tôi tính giai thừa bắt đầu từ 1 cho đến khi giá trị vượt quá$x$, nhưng chúng tôi nhanh chóng nhận thấy trình tự này trở nên không liên quan sau một vài bước. Bước này đảm bảo chúng ta không giả định quá nhiều về cấu trúc nếu chưa xác minh. 
2. Kiểm tra xem giá trị giai thừa nào là số nguyên tố. Đây là bước lọc trung tâm, nơi chúng tôi thực thi đồng thời cả hai điều kiện của vấn đề. 
3. Theo dõi giá trị giai thừa lớn nhất vừa là số nguyên tố vừa là số nguyên tố$\le x$. Vì giai thừa ngày càng tăng nên đây đương nhiên là giá trị hợp lệ cuối cùng gặp phải. 
4. Xuất giá trị đó nếu nó tồn tại, nếu không thì xuất$-1$. Điều này xử lý trường hợp không có giai thừa nào thỏa mãn tính nguyên tố. 

Một sàng lọc trực tiếp hơn của các bước này là nhận ra rằng chỉ$2! = 2$sống sót qua quá trình lọc. 

### Tại sao nó hoạt động 

Các giai thừa phát triển đơn điệu và tính nguyên tố áp đặt một ràng buộc rất nghiêm ngặt: ngoại trừ 2, mọi giai thừa$n!$vì$n \ge 3$chia hết cho 2 và một số nguyên khác, làm cho nó trở thành hợp số. Điều này có nghĩa là giao của tập hợp giai thừa và tập hợp số nguyên tố chứa nhiều nhất một phần tử. Khi chúng tôi xác định được phần tử đó, không cần tìm kiếm thêm nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(n: int) -> bool:
    if n < 2:
        return False
    if n % 2 == 0:
        return n == 2
    d = 3
    while d * d <= n:
        if n % d == 0:
            return False
        d += 2
    return True

def solve():
    x = int(input().strip())

    fact = 1
    best = -1
    z = 1

    while fact <= x:
        if is_prime(fact):
            best = fact
        z += 1
        fact *= z

    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng các giai thừa tăng dần bằng cách sử dụng một sản phẩm đang chạy. Điều kiện vòng lặp đảm bảo chúng ta không bao giờ xem xét các giá trị vượt quá$x$, giữ cho tính toán bị giới hạn. Mỗi giai thừa đều được kiểm tra tính nguyên tố, mặc dù trong thực tế chỉ$2$vượt qua. 

Việc kiểm tra tính nguyên thủy được viết theo tiêu chuẩn$O(\sqrt{n})$hình thức. Mặc dù nó là quá mức cần thiết đối với các giá trị nhỏ như vậy, nhưng nó vẫn giữ nguyên lý luận chung và tránh các giả định mã hóa cứng. 

Một chi tiết triển khai tinh tế là thứ tự cập nhật: chúng tôi nhân sau khi tăng$z$, đảm bảo chúng tôi tạo chính xác$2!, 3!, 4!$, v.v. mà không bỏ qua các giá trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$x = 10$Chúng tôi tạo giai thừa một cách tuần tự: 

| z | giai thừa | là số nguyên tố | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 1 | Sai | -1 | 
| 2 | 2 | Đúng | 2 | 
| 3 | 6 | Sai | 2 | 
| 4 | 24 | Sai | 2 | 

Tại$z=4$, giai thừa đã vượt quá 10 nên chúng ta dừng lại. Giá trị hợp lệ tốt nhất gặp phải là 2. 

Dấu vết này cho thấy rằng mặc dù chúng tôi tính toán nhiều giai thừa nhưng chỉ có một ứng cử viên quan trọng. 

### Ví dụ 2 

đầu vào:$x = 1$| z | giai thừa | là số nguyên tố | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 1 | Sai | -1 | 

Vòng lặp dừng ngay lập tức vì$1! = 1 \le x$, nhưng không tìm thấy giai thừa nguyên tố hợp lệ. 

Điều này thể hiện trường hợp khó khăn khi không có lời giải nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Nhiều nhất là một vài lần lặp giai thừa và kiểm tra tính nguyên tố theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến số nguyên được lưu trữ | 

Chuỗi giai thừa kết thúc gần như ngay lập tức vì giá trị vượt quá$10^5$rất nhanh và việc kiểm tra tính nguyên tố được áp dụng cho những số cực nhỏ. Điều này đảm bảo giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# re-define solution for testing context
def is_prime(n: int) -> bool:
    if n < 2:
        return False
    if n % 2 == 0:
        return n == 2
    d = 3
    while d * d <= n:
        if n % d == 0:
            return False
        d += 2
    return True

def solve():
    x = int(sys.stdin.readline().strip())
    fact = 1
    best = -1
    z = 1
    while fact <= x:
        if is_prime(fact):
            best = fact
        z += 1
        fact *= z
    print(best)

# provided sample (x=1 style case)
assert run("1\n") == "-1", "sample-like case x=1"

# custom cases
assert run("2\n") == "2", "minimum valid answer"
assert run("10\n") == "2", "small range includes 2 only"
assert run("100000\n") == "2", "upper bound still only 2"
assert run("3\n") == "2", "boundary just above 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | -1 | không có số nguyên tố giai thừa hợp lệ | 
| 2 | 2 | trường hợp hợp lệ nhỏ nhất | 
| 10 | 2 | độ chính xác của phạm vi trung gian | 
| 100000 | 2 | ổn định giới hạn trên | 

## Vỏ cạnh 

### Trường hợp: x = 1 

cho$x = 1$, chúng ta bắt đầu với$1! = 1$. Thuật toán đánh giá nó, thấy nó không phải là số nguyên tố và ngay lập tức kết thúc vòng lặp sau khi xác nhận không có ứng cử viên hợp lệ nào. Đầu ra vẫn còn$-1$, phù hợp với yêu cầu không tồn tại số nguyên tố giai thừa nào nhỏ hơn hoặc bằng 1. 

### Trường hợp: x ≥ 2 

Đối với bất kỳ$x \ge 2$, thuật toán tạo ra$1! = 1$, sau đó$2! = 2$. Tại$2$, tính nguyên tố được xác nhận và tốt nhất được cập nhật thành 2. Mặc dù các giai thừa sau này được tạo ra nhưng chúng nhanh chóng vượt quá 2 và không phải là số nguyên tố. Thuật toán giữ nguyên 2 là câu trả lời cuối cùng, chứng tỏ rằng ứng cử viên duy nhất còn sống được chọn một cách nhất quán bất kể kích thước đầu vào.
