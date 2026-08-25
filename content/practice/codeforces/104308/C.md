---
title: "CF 104308C - Ghép nối tối ưu"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, có một mảng có độ dài chẵn. Chúng ta phải phân chia mảng thành các cặp rời nhau để mỗi phần tử thuộc về đúng một cặp. Đối với mỗi cặp, đóng góp của nó cho câu trả lời là giá trị lớn hơn trong hai giá trị bên trong cặp đó."
date: "2026-07-01T20:01:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104308
codeforces_index: "C"
codeforces_contest_name: "Mirror of Independence Day Programming Contest 2023 by MIST Computer Club"
rating: 0
weight: 104308
solve_time_s: 63
verified: true
draft: false
---

[CF 104308C - Ghép nối tối ưu](https://codeforces.com/problemset/problem/104308/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, có một mảng có độ dài chẵn. Chúng ta phải phân chia mảng thành các cặp rời nhau để mỗi phần tử thuộc về đúng một cặp. Đối với mỗi cặp, đóng góp của nó cho câu trả lời là giá trị lớn hơn trong hai giá trị bên trong cặp đó. Mục tiêu là chọn chiến lược ghép đôi sao cho tối thiểu hóa tổng các giá trị cực đại theo cặp này. 

Vì vậy, nhiệm vụ không phải là tìm một kết quả phù hợp tối ưu duy nhất mà là quyết định cách nhóm các giá trị sao cho các phần tử lớn không làm tăng chi phí nhiều cặp một cách không cần thiết. 

Các ràng buộc cho phép lên đến$10^5$tổng số phần tử trên tất cả các trường hợp thử nghiệm, với giá trị lên tới$10^9$. Điều này ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các cặp. Số cách phân vùng$n$các phần tử thành từng cặp sẽ tăng trưởng theo cấp số nhân, do đó ngay cả đối với$n = 20$vũ lực đã là không thể thực hiện được. Một giải pháp hợp lệ phải chạy trong khoảng$O(n \log n)$hoặc tốt hơn cho mỗi trường hợp thử nghiệm, vì sắp xếp$10^5$các phần tử đã gần đến giới hạn tốc độ nhanh thoải mái trong Python. 

Một số tình huống khó khăn đáng lưu ý. 

Nếu tất cả các phần tử đều bằng nhau thì mọi cặp đều có chi phí giống nhau, do đó, bất kỳ thuật toán chính xác nào cũng phải bảo toàn tính bất biến đó. 

Nếu mảng đã được sắp xếp theo thứ tự giảm dần, thì việc tham lam ngây thơ như ghép cặp đầu tiên với thứ hai, thứ ba với thứ tư thực sự là tối ưu, nhưng ghép đầu tiên với cuối cùng là tai hại vì nó buộc phần tử tối đa vào mọi cặp liên quan đến nó một cách gián tiếp thông qua cấu trúc. 

Ví dụ: một trường hợp thất bại tinh tế đối với các chiến lược tham lam không chính xác xuất hiện khi các giá trị xen kẽ nhau`[1, 100, 2, 99]`. Ghép đôi cực đoan`(1,100)`Và`(2,99)`chi phí sản lượng`100 + 99 = 199`, trong khi ghép nối tối ưu`(1,2)`Và`(99,100)`sản lượng`2 + 100 = 102`. Bất kỳ chiến lược nào không kiểm soát rõ ràng việc đặt hàng sẽ có xu hướng bỏ lỡ cấu trúc này. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử mọi cách ghép nối có thể của mảng thành$n/2$cặp, tính tổng cực đại cho mỗi cấu hình và lấy mức tối thiểu. Điều này đúng vì nó đánh giá mục tiêu trực tiếp cho mọi cấu trúc hợp lệ. Vấn đề là số lượng kết hợp hoàn hảo trên$n$phần tử là$(n-1)!!$, vốn đã trở nên to lớn ở mức nhỏ$n$. Vì$n = 20$, chuyện này kết thúc rồi$10^7$khả năng, và mỗi chi phí đánh giá$O(n)$, vì vậy cách tiếp cận này sụp đổ ngay lập tức. 

Quan sát quan trọng là chi phí của một cặp chỉ phụ thuộc vào phần tử lớn hơn. Điều này có nghĩa là mọi phần tử đều đóng góp với tư cách là "người chiến thắng" trong cặp của nó hoặc bị ẩn dưới một đối tác lớn hơn. Để giảm thiểu tổng, chúng tôi muốn các phần tử lớn được sử dụng ít thường xuyên nhất có thể như cực đại. Điều đó gợi ý nên ghép các phần tử lớn với các phần tử nhỏ nhất có thể, nhưng trực giác này phải nhất quán trên toàn cầu. 

Sau khi mảng được sắp xếp, chúng ta có thể suy luận về cấu trúc cục bộ. Xét bốn yếu tố bất kỳ$a \le b \le c \le d$. Nếu chúng ta ghép chúng thành$(a, d)$Và$(b, c)$, chi phí là$d + c$. Thay vào đó, nếu chúng ta ghép chúng thành$(a, b)$Và$(c, d)$, chi phí là$b + d$. Từ$c \ge b$, cấu hình thứ hai không bao giờ tăng chi phí và hoàn toàn tốt hơn trừ khi các giá trị bằng nhau. Đối số trao đổi này cho thấy rằng sự giao thoa giữa các phần tử nhỏ và lớn là có hại và cấu trúc tối ưu sẽ sụp đổ thành cặp liền kề sau khi sắp xếp. 

Do đó, giải pháp trở thành sắp xếp mảng và ghép các phần tử liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + ghép nối tham lam) | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc mảng cho test case hiện tại. Cấu trúc của vấn đề được chứa đầy đủ trong mỗi trường hợp thử nghiệm, do đó không tồn tại sự tương tác giữa các trường hợp. 
2. Sắp xếp mảng theo thứ tự không giảm. Bước này trình bày thứ tự tự nhiên của các phần tử để chúng ta có thể suy luận cục bộ về việc hình thành cặp tối ưu. 
3. Duyệt mảng đã sắp xếp theo từng bước hai, ghép các phần tử liên tiếp nhau. Mỗi cặp được hình thành như`(a[i], a[i+1])`. Việc ghép nối này đảm bảo rằng mọi phần tử nhỏ hơn đều được khớp với phần tử lớn hơn gần nhất có thể, ngăn không cho các phần tử lớn bị lãng phí trên nhiều cặp. 
4. Với mỗi cặp, hãy thêm phần tử thứ hai (phần tử lớn hơn sau khi sắp xếp) vào câu trả lời. Vì mảng đã được sắp xếp,`a[i+1]`được đảm bảo là mức tối đa của cặp. 
5. Xuất tổng tích lũy cho test case. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ cặp nào có cấu trúc "giao nhau", trong đó phần tử nhỏ hơn được ghép với phần tử rất lớn trong khi cặp khác chứa các giá trị trung gian, có thể được cải thiện cục bộ bằng cách hoán đổi điểm cuối. Mỗi lần hoán đổi sẽ giảm hoặc bảo toàn tổng chi phí vì nó ngăn phần tử lớn hơn bị lu mờ một cách không cần thiết. Việc áp dụng lặp đi lặp lại quá trình trao đổi này sẽ loại bỏ tất cả các cặp không liền kề, chỉ để lại các cặp liên tiếp là một cấu hình ổn định. Cấu trúc này đảm bảo rằng mọi phần tử ngoại trừ những phần tử ở vị trí chẵn đều đóng góp tối đa chính xác một lần và các cực đại này là những lựa chọn nhỏ nhất có thể có ở giai đoạn ghép nối của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        ans = 0
        for i in range(1, n, 2):
            ans += a[i]
        out.append(str(ans))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh việc sắp xếp và quét tuyến tính. Chi tiết triển khai chính là chúng tôi tính tổng mọi phần tử thứ hai bắt đầu từ chỉ mục 1 sau khi sắp xếp, vì đó là những phần tử đóng vai trò là cực đại trong mỗi cặp tối ưu. Việc sử dụng I/O nhanh là cần thiết vì tổng kích thước đầu vào có thể đạt tới$10^5$số nguyên trên các trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 100, 2, 99]`Sau khi sắp xếp, mảng trở thành`[1, 2, 99, 100]`. 

| Bước | Mảng | Cặp hình thành | Giá cặp | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 2, 99, 100] | (1,2) | 2 | 2 | 
| 2 | [1, 2, 99, 100] | (99.100) | 100 | 102 | 

Tổng số là 102, cho thấy việc ghép nối liền kề sẽ tránh lãng phí các giá trị lớn trên các cặp khác nhau. 

### Ví dụ 2 

đầu vào:`[5, 5, 5, 5, 1, 1]`Mảng được sắp xếp:`[1, 1, 5, 5, 5, 5]`| Bước | Mảng | Cặp hình thành | Giá cặp | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 1, 5, 5, 5, 5] | (1,1) | 1 | 1 | 
| 2 | [1, 1, 5, 5, 5, 5] | (5,5) | 5 | 6 | 
| 3 | [1, 1, 5, 5, 5, 5] | (5,5) | 5 | 11 | 

Bất kỳ sự sắp xếp lại nào cũng tạo ra tổng bằng nhau hoặc cao hơn vì các cặp hoán đổi không thể giảm đồng thời cả hai phần tử lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế, ghép nối là tuyến tính | 
| Không gian |$O(1)$thêm (không bao gồm đầu vào) | Chỉ sử dụng tích lũy và sắp xếp tại chỗ | 

Các ràng buộc cho phép lên đến$10^5$tổng số phần tử, do đó việc sắp xếp một lần cho mỗi trường hợp thử nghiệm nằm trong giới hạn. Việc quét tuyến tính không đáng kể so với việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        ans = 0
        for i in range(1, n, 2):
            ans += a[i]
        out.append(str(ans))
    print("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        from contextlib import redirect_stdout
        import io as sio
        out = sio.StringIO()
        with redirect_stdout(out):
            solve()
        return out.getvalue().strip()
    finally:
        sys.stdin = old_stdin

# sample-like test
assert run("""1
4
1 100 2 99
""") == "102"

# minimum case
assert run("""1
2
10 1
""") == "10"

# all equal
assert run("""1
6
5 5 5 5 5 5
""") == "15"

# already sorted
assert run("""1
4
1 2 3 4
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 4 1 100 2 99 | 102 | lực lượng xen kẽ chiến lược phân loại đúng đắn | 
| 1 2 10 1 | 10 | trường hợp hợp lệ nhỏ nhất | 
| 1 6 tất cả 5s | 15 | tính đối xứng và độ ổn định có giá trị bằng nhau | 
| 1 4 1 2 3 4 | 6 | đã được sắp xếp tăng độ chính xác thứ tự | 

## Vỏ cạnh 

Đối với các đầu vào xen kẽ như`[1, 100, 2, 99]`, thuật toán sắp xếp thành`[1, 2, 99, 100]`và tạo thành cặp`(1,2)`Và`(99,100)`. Các yếu tố thứ hai`2`Và`100`được tích lũy, tạo ra 102. Bất kỳ nỗ lực ghép nối nào mà không sắp xếp đều tạo ra các cặp chéo làm tăng ít nhất một phần tử lớn lên vị trí có chi phí cao hơn. 

Đối với các đầu vào nặng trùng lặp như`[5, 5, 5, 5, 1, 1]`, việc sắp xếp tạo ra sự kề cận ổn định trong đó việc hoán đổi các cặp không làm thay đổi kết quả. Thuật toán chỉ định một cách nhất quán một bản sao của mỗi cặp là mức tối đa và vì tất cả các phần tử lớn đều bằng nhau nên không có sự sắp xếp lại nào có thể làm giảm tổng số.
