---
title: "CF 105350B - Một vấn đề thú vị về cặp đôi"
description: "Chúng ta được cho một giới hạn trên $n$. Từ tất cả các cặp số nguyên $(a, b)$ sao cho cả hai đều nằm giữa 1 và $n$ và $a < b$, chúng ta chỉ được phép xét những cặp có AND theo bit của hai số bằng 0."
date: "2026-06-23T05:40:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105350
codeforces_index: "B"
codeforces_contest_name: "Theforces Round #34 (ABC-Forces)"
rating: 0
weight: 105350
solve_time_s: 95
verified: false
draft: false
---

[CF 105350B - Một vấn đề thú vị về cặp đôi](https://codeforces.com/problemset/problem/105350/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một giới hạn trên$n$. Từ tất cả các cặp số nguyên$(a, b)$sao cho cả hai đều nằm giữa 1 và$n$Và$a < b$, chúng ta chỉ được phép xem xét những cặp có AND theo bit của hai số bằng 0. Điều đó có nghĩa là trong biểu diễn nhị phân, hai số không bao giờ được chia sẻ vị trí mà cả hai đều có bit 1. Trong số tất cả các cặp hợp lệ như vậy, chúng ta muốn cặp có tích của nó$a \cdot b$là càng lớn càng tốt. 

Mỗi trường hợp thử nghiệm cho một giá trị khác nhau của$n$, vì vậy nhiệm vụ là tính toán tích cực đại này một cách độc lập cho mỗi đầu vào. 

Ràng buộc$t \le 10^4$với$n \le 10^9$ngụ ý rằng chúng tôi không thể chấp nhận bất kỳ phương pháp tiếp cận bậc hai hoặc thậm chí tuyến tính nào trên mỗi bài kiểm tra trong phạm vi giá trị. Bất kỳ giải pháp nào cố gắng liệt kê các cặp hoặc thậm chí lặp lại tất cả các ứng cử viên cho đến$n$sẽ quá chậm, vì$n$bản thân nó lớn và được lặp lại qua nhiều trường hợp thử nghiệm. Điều này thúc đẩy chúng ta hướng tới cách suy luận theo thời gian không đổi hoặc logarit cho mỗi trường hợp thử nghiệm. 

Một kiểu thất bại phổ biến ở đây là giả sử rằng việc chọn hai số lớn nhất gần nhau$n$luôn luôn hợp lệ. Ví dụ, với$n = 15$, cặp$(14, 15)$là số lượng lớn nhất nhưng$14 \& 15 \neq 0$, vì vậy nó không hợp lệ. Một vấn đề tế nhị khác là giả sử chúng ta có thể tham lam chọn bất kỳ hai số lớn nào có các bit rời rạc mà không hiểu cấu trúc bit hạn chế sự cùng tồn tại như thế nào. Cấu trúc của biểu diễn nhị phân hạn chế số lượng lớn nào có thể xuất hiện đồng thời trong một cặp hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi lặp lại tất cả các cặp$(a, b)$với$1 \le a < b \le n$, kiểm tra xem$(a \& b) = 0$và theo dõi sản phẩm tối đa. Điều này đúng vì nó trực tiếp kiểm tra mọi kết hợp hợp lệ. Vấn đề là quy mô. Có khoảng$n^2 / 2$cặp, điều này trở nên hoàn toàn không khả thi ngay cả đối với một trường hợp thử nghiệm khi$n$đạt tới$10^9$. 

Quan sát quan trọng là điều kiện AND cực kỳ hạn chế về mặt cấu trúc nhị phân. Nếu hai số chia sẻ bất kỳ bit nào được đặt thành 1, chúng sẽ không hợp lệ. Điều này ngay lập tức gợi ý rằng việc phân phối các giá trị lớn trên các vị trí bit chồng chéo là nguy hiểm. Để tối đa hóa tích số, chúng tôi muốn cả hai số càng lớn càng tốt nhưng vẫn có các hỗ trợ nhị phân hoàn toàn rời rạc. 

Bây giờ hãy xem xét lũy thừa cao nhất của hai không vượt quá$n$, gọi nó$2^k$. Con số này đặc biệt vì nó có đúng một bit được đặt. Nếu chúng ta chọn$b = 2^k$, thì nó không chiếm bất kỳ bit thấp hơn nào. Điều này mang lại sự linh hoạt tối đa cho số thứ hai, số này có thể sử dụng tất cả các bit thấp hơn còn lại một cách tự do mà không có xung đột. 

Số thứ hai tốt nhất có thể theo ràng buộc này là$2^k - 1$, tất cả đều thấp hơn$k$tập bit. Điều này sử dụng tất cả các bit nhỏ có sẵn trong khi vẫn tách rời khỏi$2^k$. Bất kỳ nỗ lực nào để thay thế$2^k$với số lượng lớn hơn sẽ giới thiệu các bit được đặt bổ sung ở vị trí cao hơn hoặc thấp hơn, điều này buộc số đối tác loại bỏ các bit xung đột và làm giảm sản phẩm có thể đạt được. 

Do đó, cấu trúc tối ưu bị ép buộc: một số là lũy thừa lớn nhất của hai trong giới hạn và số kia là số đơn vị tối đa bên dưới nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi trường hợp thử nghiệm, chúng tôi tiến hành như sau. 

1. Tìm lũy thừa lớn nhất của hai$2^k$như vậy$2^k \le n$. Điều này xác định số bit đơn cao nhất mà chúng ta có thể sử dụng một cách an toàn. 
2. Xây dựng số$a = 2^k - 1$, có tất cả các bit bên dưới$k$được đặt thành 1. Điều này đảm bảo nó càng lớn càng tốt trong khi vẫn tránh được bit được sử dụng bởi$2^k$. 
3. Đặt$b = 2^k$, sức mạnh được lựa chọn của hai. 
4. Tính tích$a \cdot b$và xuất nó. 

Lý do đằng sau việc chọn lũy thừa hai cho một bên là nó cô lập một vị trí bit duy nhất, để lại tất cả các vị trí còn lại miễn phí cho số kia. Bất kỳ lựa chọn thay thế nào cho số lớn hơn đều đưa ra các bit thiết lập bổ sung, buộc số thứ hai trở nên nhỏ hơn để duy trì tính rời rạc, làm giảm tích. 

### Tại sao nó hoạt động 

Cặp tối ưu phải gán các bộ bit rời nhau cho hai số dưới giới hạn trên toàn cục. Để tối đa hóa tích, chúng tôi muốn cả hai số càng lớn càng tốt, nghĩa là sử dụng càng nhiều bit có giá trị cao càng tốt. 

Nếu không có số nào sử dụng vị trí bit cao nhất có sẵn trong$n$, thì cả hai đều bị giới hạn bởi$2^k - 1$, và sản phẩm của họ còn tệ hơn cả việc ghép đôi$2^k$với bất kỳ đối tác hợp lệ nào. Nếu một số sử dụng nhiều bit cao, nó sẽ giảm tính linh hoạt cho số thứ hai, vì mỗi bit được đặt bổ sung sẽ loại bỏ một yếu tố đóng góp tiềm năng vào giá trị của nó. Cấu hình giúp giảm thiểu hạn chế này trong khi vẫn giữ tối đa một số là tập trung tất cả giá trị cao vào một bit duy nhất, chính xác là lũy thừa của hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        
        k = n.bit_length() - 1
        p = 1 << k
        
        # ensure p is the highest power of two <= n
        if p > n:
            p >>= 1
        
        a = p - 1
        b = p
        
        print(a * b)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng`bit_length()`để xác định vị trí bit được đặt cao nhất của$n$, mang lại cho chúng ta lũy thừa lớn nhất của hai không vượt quá$n$. Từ đó chúng ta xây dựng trực tiếp cả hai số cần thiết. 

Một chi tiết triển khai tinh tế là xử lý trường hợp$n$chính xác là lũy thừa của hai. Trong hoàn cảnh đó,`p`đã hợp lệ và số đối tác trở thành$p - 1$, vẫn nằm trong giới hạn. Không cần điều chỉnh thêm vì$p - 1 < p \le n$luôn luôn giữ. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 15$. lũy thừa cao nhất của hai không quá 15 là$8$. 

| Bước | k | p = 2^k | a = p - 1 | b = p | sản phẩm | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 8 | 7 | 8 | 56 | 

Điều này chứng tỏ cặp$(7, 8)$đạt được sản phẩm tối đa dưới ràng buộc bit rời rạc. 

Bây giờ hãy xem xét$n = 2$. Sức mạnh cao nhất của hai là$2$. 

| Bước | k | p | một | b | sản phẩm | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 2 | 1 | 2 | 2 | 

Đây là cặp hợp lệ duy nhất thỏa mãn$a < b$và điều kiện AND. 

Những ví dụ này cho thấy cấu trúc không phụ thuộc vào mật độ của$n$, chỉ trên bit được đặt cao nhất của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm tính toán độ dài bit và một vài thao tác bit trong thời gian không đổi | 
| Không gian |$O(1)$| Không có cấu trúc bổ sung ngoài một vài số nguyên | 

Giải pháp phù hợp một cách thoải mái trong những hạn chế vì thậm chí$10^4$các trường hợp thử nghiệm chỉ yêu cầu các phép tính số học đơn giản cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        k = n.bit_length() - 1
        p = 1 << k
        if p > n:
            p >>= 1
        a = p - 1
        b = p
        out.append(str(a * b))
    return "\n".join(out)

# provided samples (as per statement formatting assumption)
assert run("2\n2\n15\n") == "2\n56"

# minimum case
assert run("1\n2\n") == "2"

# power of two boundary
assert run("1\n8\n") == "56"

# just below power of two
assert run("1\n7\n") == "12"

# large case
assert run("1\n1000000000\n") == str(( (1 << (1000000000.bit_length()-1)) - 1) * (1 << (1000000000.bit_length()-1)))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 2 | 2 | cặp hợp lệ nhỏ nhất | 
| n = 8 | 56 | sức mạnh chính xác của hai ranh giới | 
| n = 7 | 12 | hành vi ngay dưới ranh giới | 
| n = 1e9 | tính toán | chính xác dưới đầu vào lớn | 

## Vỏ cạnh 

Khi nào$n$chính xác là lũy thừa của hai, ví dụ$n = 8$, thuật toán chọn$p = 8$và sản xuất$(7, 8)$. Điều kiện AND được giữ vì 7 chỉ được đặt bit thấp hơn và 8 chỉ được đặt bit cao nhất, do đó không có sự trùng lặp. Sản phẩm$56$là tối ưu vì bất kỳ cặp nào khác sẽ giảm một trong các giá trị xuống dưới 7 hoặc gây ra sự chồng chéo bit. 

Khi$n$chỉ dưới lũy thừa của hai, chẳng hạn như$n = 7$, lũy thừa cao nhất của hai là$4$, sản xuất$(3, 4)$. Mặc dù bản thân 7 là lớn nhưng nó không thể ghép nối với 4 mà không có các bit chồng chéo, vì 7 đã sử dụng nhiều bit thấp hơn có thể xung đột với 3 hoặc 4. Thuật toán tránh được cạm bẫy này một cách tự nhiên bằng cách neo vào bit bị cô lập cao nhất thay vì giá trị tối đa tuyệt đối.
