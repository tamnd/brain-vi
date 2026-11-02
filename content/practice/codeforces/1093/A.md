---
title: "CF 1093A - Xúc Xắc"
description: "Chúng ta được cho một con xúc xắc sáu mặt tiêu chuẩn, nhưng thay vì các giá trị thông thường từ 1 đến 6, các mặt của nó chứa các số nguyên 2, 3, 4, 5, 6 và 7, tất cả đều khác biệt. Mỗi cuộn tạo ra một trong những số này và điểm cho một chuỗi cuộn là tổng của các mặt nhìn thấy được."
date: "2026-06-15T14:57:46+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1093
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 56 (Rated for Div. 2)"
rating: 800
weight: 1093
solve_time_s: 388
verified: false
draft: false
---

[CF 1093A - Tung xúc xắc](https://codeforces.com/problemset/problem/1093/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 6 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một con xúc xắc sáu mặt tiêu chuẩn, nhưng thay vì các giá trị thông thường từ 1 đến 6, các mặt của nó chứa các số nguyên 2, 3, 4, 5, 6 và 7, tất cả đều khác biệt. Mỗi cuộn tạo ra một trong những số này và điểm cho một chuỗi cuộn là tổng của các mặt nhìn thấy được. 

Đối với mỗi giá trị truy vấn$x$, chúng ta được yêu cầu xác định số cuộn bất kỳ$k$sao cho có thể thu được tổng số tiền chính xác$x$sử dụng chính xác$k$cuộn. Chúng tôi không bắt buộc phải xây dựng chuỗi cuộn, chỉ xuất ra một giá trị hợp lệ$k$. 

Giải thích chính là khi chúng ta sửa$k$, câu hỏi đặt ra là liệu chúng ta có thể đại diện cho$x$như một tổng của$k$số nguyên, mỗi số được chọn từ$\{2,3,4,5,6,7\}$. 

Các hạn chế là nhỏ:$t \le 100$Và$x \le 100$. Điều này ngay lập tức loại trừ mọi nhu cầu về tổ hợp hoặc tìm kiếm nặng. Bất kỳ giải pháp nào tính toán từng câu trả lời trong thời gian không đổi cho mỗi truy vấn đều đủ. 

Một điểm tinh tế là nhiều giá trị khác nhau của$k$có thể làm việc tương tự$x$. Bài toán không yêu cầu giá trị tối thiểu hoặc tối đa mà chỉ yêu cầu giá trị hợp lệ. Sự linh hoạt đó là điều làm cho việc xây dựng trở nên đơn giản. 

Không có trường hợp thực tế nào liên quan đến tính không thể xảy ra, vì câu lệnh đảm bảo luôn có giải pháp cho mọi truy vấn. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử mọi số cuộn có thể$k$, và với mỗi$k$, kiểm tra xem$x$có thể được hình thành bằng cách sử dụng$k$các số từ 2 đến 7. Để cố định$k$, điều này tương đương với việc hỏi liệu$x$nằm trong phạm vi$[2k, 7k]$, vì 2 là mức đóng góp tối thiểu của một lần đổ và 7 là mức tối đa. 

Nếu chúng ta dùng bạo lực trên tất cả$k$lên đến, nói,$x$, và với mỗi$k$thực hiện kiểm tra, độ phức tạp vẫn còn nhỏ với các ràng buộc nhất định, nhưng điều đó là không cần thiết. Cấu trúc của bài toán gợi ý một cách xây dựng trực tiếp. 

Quan sát quan trọng là đối với bất kỳ lựa chọn nào$k$, số tiền có thể đạt được tạo thành một khoảng liên tục từ$2k$ĐẾN$7k$. Khoảng này không có khoảng trống vì chúng ta có thể điều chỉnh từng cuộn riêng lẻ theo bước 1 trong khi vẫn nằm trong mệnh giá cho phép. Vì vậy, thay vì tìm kiếm theo dãy, chúng ta chỉ cần tìm số nguyên bất kỳ$k$như vậy$2k \le x \le 7k$. 

Sắp xếp lại, chúng tôi muốn:$$\frac{x}{7} \le k \le \frac{x}{2}$$Từ$k$phải là số nguyên, mọi số nguyên trong phạm vi này đều hoạt động. Một lựa chọn mang tính xây dựng đơn giản là lấy giá trị nhỏ nhất$k$điều đó vẫn cho phép tiếp cận$x$, đó là:$$k = \left\lceil \frac{x}{7} \right\rceil$$Một khi chúng tôi sửa lỗi này$k$, chênh lệch còn lại$x - 2k$luôn có thể được phân phối bằng cách tăng một số cuộn từ 2 lên 7. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên k và trình tự | O(x · 6^k) | O(1) | Quá chậm | 
| Khoảng thời gian xây dựng | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng truy vấn$t$. Mỗi truy vấn đưa ra một tổng mục tiêu$x$. 
2. Đối với mỗi$x$, tính số nguyên nhỏ nhất$k$như vậy$7k \ge x$. Điều này đảm bảo rằng ngay cả khi tất cả các cuộn đều là 7, chúng ta vẫn có thể đạt hoặc vượt$x$. 
3. Xuất cái này$k$như câu trả lời cho truy vấn. 

Lý do chúng tôi chọn trần nhà$x/7$là nó giảm thiểu số lượng cuộn trong khi vẫn đảm bảo đủ tổng công suất để đạt được$x$. Bất kỳ nhỏ hơn$k$sẽ giới hạn số tiền tối đa có thể đạt được dưới đây$x$, làm cho nó không thể. 

### Tại sao nó hoạt động 

Đối với bất kỳ số lượng cuộn cố định$k$, tập hợp các tổng có thể đạt được chính xác là khoảng$[2k, 7k]$. Điều này là do mỗi cuộn độc lập đóng góp bất kỳ giá trị nào từ 2 đến 7 và việc điều chỉnh một cuộn theo ±1 sẽ thay đổi tổng số chính xác bằng 1 trong khi vẫn hợp lệ. Do đó, tất cả các số nguyên trong phạm vi đều có thể truy cập được. 

Lựa chọn$k = \lceil x/7 \rceil$đảm bảo$x \le 7k$. Đồng thời, kể từ$k \ge 1$, chúng ta luôn có đủ cấu trúc để biểu diễn$x$bằng cách bắt đầu từ tất cả 2 giây và tăng dần một số cuộn. Như vậy$x \ge 2k$cũng tự động giữ phạm vi xây dựng này trong các ràng buộc của bài toán và mọi khoảng trống còn lại có thể được lấp đầy bằng cách tăng dần các cuộn đã chọn lên đến 7. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x = int(input())
        k = (x + 6) // 7
        print(k)

if __name__ == "__main__":
    solve()
```Mã xử lý từng truy vấn một cách độc lập. biểu thức`(x + 6) // 7`là số chia trần số nguyên tiêu chuẩn cho$x/7$, đảm bảo chúng tôi luôn chọn cái nhỏ nhất$k$như vậy$7k \ge x$. 

Việc thực hiện tránh mọi mô phỏng của việc tung xúc xắc. Logic trực tiếp tính toán số cuộn khả thi tối thiểu, đủ vì bài toán cho phép mọi cấu hình hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$x = 13$Chúng tôi tính toán$k = \lceil 13/7 \rceil = 2$| Bước | x | k = trần(x/7) | 7k | Khả thi? | 
| --- | --- | --- | --- | --- | 
| 1 | 13 | 2 | 14 | Có | 

Với 2 cuộn, chúng ta có thể đạt được tổng từ 4 đến 14, vì vậy có thể đạt được 13. Ví dụ: 6 + 7 có tác dụng. 

Điều này khẳng định rằng việc xây dựng là đủ ngay cả khi mục tiêu ở gần ranh giới trên. 

### Ví dụ 2 

đầu vào:$x = 37$Chúng tôi tính toán$k = \lceil 37/7 \rceil = 6$| Bước | x | k | 7k | Khả thi? | 
| --- | --- | --- | --- | --- | 
| 1 | 37 | 6 | 42 | Có | 

Với 6 cuộn, số tiền có thể đạt được nằm trong khoảng từ 12 đến 42. Mục tiêu 37 nằm trong khoảng này. 

Điều này chứng tỏ rằng ngay cả khi$x$không phải là bội số của 7, độ chùng được tạo ra bởi nhiều cuộn cho phép điều chỉnh tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi truy vấn được xử lý bằng một phép toán số học duy nhất | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ | 

Các ràng buộc cho phép tối đa 100 truy vấn, do đó việc quét tuyến tính là chuyện nhỏ. Giải pháp chạy trong thời gian không đổi cho mỗi lần kiểm tra và thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    for _ in range(t):
        x = int(sys.stdin.readline())
        k = (x + 6) // 7
        print(k)

    return output.getvalue().strip()

# provided samples
assert run("4\n2\n13\n37\n100\n") == "1\n2\n6\n15"

# custom cases
assert run("1\n7\n") == "1"
assert run("1\n8\n") == "2"
assert run("1\n14\n") == "2"
assert run("1\n1\n") == "1"
assert run("1\n100\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 | 1 | ranh giới cuộn đơn chính xác | 
| 8 | 2 | vượt qua khoảng cách khả thi đầu tiên | 
| 14 | 2 | ranh giới trên của k nhỏ | 
| 1 | 1 | xử lý ràng buộc tối thiểu | 
| 100 | 15 | nhân rộng giá trị lớn | 

## Vỏ cạnh 

Một ranh giới quan trọng là khi$x$chính xác là chia hết cho 7. Ví dụ:$x = 14$. Thuật toán đưa ra$k = 2$. Với hai cuộn, tối đa là$14$, đạt được bởi$7 + 7$, xác nhận tính đúng đắn ở ranh giới trên. 

Một trường hợp khác là khi$x$chỉ ở trên bội số của 7, chẳng hạn như$x = 8$. Đây$k = 2$. Phạm vi là$[4, 14]$, vì vậy 8 là có thể đạt được, ví dụ$2 + 6$. Điều này cho thấy rằng ngay cả khi một cuộn không đủ, việc thêm cuộn thứ hai sẽ tạo ra đủ sự linh hoạt để lấp đầy khoảng trống. 

Cuối cùng, đối với các giá trị rất nhỏ như$x = 2$, công thức cho$k = 1$. Tổng duy nhất có thể có chính xác là một cuộn, phải là 2, khớp chính xác với yêu cầu.
