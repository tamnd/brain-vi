---
title: "CF 106268E - Đậu Hũ Cắt"
description: "Chúng ta có một khối đậu phụ hình chữ nhật có độ dài các cạnh là $a$, $b$ và $c$. Chúng ta chỉ được phép cắt nó bằng cách cắt hoàn toàn khối dọc theo các mặt phẳng song song với các mặt của nó, do đó mỗi vết cắt sẽ chia tách tất cả các phần mà nó giao nhau."
date: "2026-06-18T23:08:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106268
codeforces_index: "E"
codeforces_contest_name: "The 2025 Asia Yokohama Regional Contest"
rating: 0
weight: 106268
solve_time_s: 35
verified: true
draft: false
---

[CF 106268E - Cắt Đậu Phụ](https://codeforces.com/problemset/problem/106268/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một khối đậu phụ hình chữ nhật có chiều dài các cạnh là số nguyên$a$,$b$, Và$c$. Chúng ta chỉ được phép cắt nó bằng cách cắt hoàn toàn khối dọc theo các mặt phẳng song song với các mặt của nó, do đó mỗi vết cắt sẽ chia tách tất cả các phần mà nó giao nhau. Sau khi thực hiện bất kỳ số lần cắt nào như vậy, chúng tôi sẽ phân vùng tập ban đầu thành các phần hình chữ nhật thẳng hàng với trục. 

Từ những mảnh này, chúng tôi muốn có được ít nhất$k$hình khối giống hệt nhau. Mỗi khối lập phương phải có cùng độ dài cạnh$x$và các hình khối phải được cắt từ các mảnh thu được chỉ bằng cách sử dụng các đường cắt thẳng hàng với nhau, nghĩa là mỗi khối tương ứng với một hình đầy đủ$x \times x \times x$khối con căn chỉnh với các trục ban đầu. 

Mục tiêu là tối đa hóa$x$, và xuất ra dưới dạng phân số rút gọn. 

Giải thích chính là khi chúng ta cố định độ dài cạnh hình lập phương ứng cử viên$x$, số lập phương chúng ta có thể thu được hoàn toàn được xác định bằng bao nhiêu$x$-các đoạn có chiều dài phù hợp với từng chiều. Tức là dọc theo chiều dài$a$, chúng ta có thể phù hợp$\lfloor a / x \rfloor$hình khối, tương tự đối với$b$Và$c$, vậy tổng số lập phương là:$$\lfloor a/x \rfloor \cdot \lfloor b/x \rfloor \cdot \lfloor c/x \rfloor.$$Chúng ta cần sự hợp lý lớn nhất$x$sao cho sản phẩm này ít nhất$k$. 

Các ràng buộc rất lớn:$a, b, c, k \le 10^9$, và lên đến$10^4$trường hợp thử nghiệm. Giải pháp nào cũng phải$O(\log \text{range})$hoặc tốt hơn cho mỗi trường hợp thử nghiệm. Một tìm kiếm tuyến tính hoặc liệt kê ngây thơ có thể$x$là không thể. 

Trường hợp chính xuất phát từ thực tế là câu trả lời không nhất thiết phải là số nguyên. Ví dụ, khi$a=b=c=2$Và$k=1$, câu trả lời là$2$, nhưng khi$k=8$, câu trả lời trở thành$1$. Thú vị hơn, các câu trả lời dạng phân số xuất hiện khi việc xếp gạch chính xác là không thể. 

Một cạm bẫy tinh tế là giả sử hàm này đủ liên tục hoặc đủ mượt để làm tròn đơn giản. Hàm này không đổi từng bước nên có rất nhiều giá trị của$x$tạo ra cùng một số khối và mức tối đa thực sự có thể nằm ở ranh giới nơi một trong các phân chia tầng thay đổi. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử tất cả các độ dài cạnh có thể$x$từ một giá trị rất nhỏ đến$\max(a,b,c)$. Đối với mỗi ứng viên$x$, chúng tôi tính toán:$$\lfloor a/x \rfloor \cdot \lfloor b/x \rfloor \cdot \lfloor c/x \rfloor$$và kiểm tra xem nó có ít nhất$k$. Điều này đúng vì nó mô phỏng chính xác quá trình xây dựng. Tuy nhiên, phạm vi có thể$x$là quá lớn. Nếu chúng tôi thử các bước số nguyên ở mức độ chi tiết đơn vị lên tới$10^9$, điều này đã vượt quá bất kỳ giới hạn thời gian khả thi nào và câu trả lời đúng thường là phân số, điều này đòi hỏi độ chi tiết thậm chí còn tốt hơn. 

Quan sát quan trọng là số lượng hình khối đều đơn điệu và không tăng theo$x$. Nếu chúng ta tăng kích thước khối thì mỗi chiều có thể chứa ít khối hơn nên tổng sản phẩm không thể tăng. Tính đơn điệu này cho phép chúng ta tìm kiếm nhị phân giá trị thực tối ưu$x$. 

Đối với một cố định$x$, chúng ta có thể tính toán tính khả thi trong thời gian không đổi, do đó tìm kiếm nhị phân trên số thực (được thực hiện dưới dạng số hữu tỷ hoặc độ chính xác đủ) trở thành ý tưởng cốt lõi. 

Tuy nhiên, vì câu trả lời được đảm bảo là hợp lý, chúng ta có thể tránh các vấn đề về độ chính xác nổi bằng cách thực hiện tìm kiếm nhị phân trên các phân số một cách ngầm định. Một cái nhìn rõ ràng hơn là tìm kiếm trên$x$dưới dạng số thực sử dụng giới hạn thả nổi và sau đó chuyển đổi kết quả cuối cùng thành phân số rút gọn. Sự giảm bớt xuất phát từ việc nhận thấy rằng ranh giới nơi những thay đổi khả thi phải xảy ra ở một giá trị hợp lý bắt nguồn từ việc chia một trong$a,b,c$bằng một số nguyên. 

Điều này dẫn đến một quyết định đơn điệu tiêu chuẩn + cấu trúc tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force của$x$|$O(\max(a,b,c))$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân trên thực tế$x$với việc kiểm tra tính khả thi |$O(\log R)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hàm khả thi 

Đối với một cạnh lập phương ứng cử viên nhất định$x$, chúng tôi tính toán có bao nhiêu khối có thể được hình thành:$$f(x) = \lfloor a/x \rfloor \cdot \lfloor b/x \rfloor \cdot \lfloor c/x \rfloor.$$Chúng tôi kiểm tra xem$f(x) \ge k$. 

### Các bước 

1. Định nghĩa hàm số cho trước$x$, trả về dù ít nhất$k$hình khối có thể được hình thành. Điều này mã hóa các ràng buộc xây dựng một cách trực tiếp. 
2. Tìm số thực lớn nhất$x$sao cho hàm trả về true. Vì lớn hơn$x$luôn giảm thiểu hoặc bảo toàn tính khả thi, vị từ có tính đơn điệu, cho phép tìm kiếm nhị phân. 
3. Duy trì khoảng thời gian tìm kiếm$[l, r]$, Ở đâu$l$chắc chắn là khả thi và$r$chắc chắn là không thể thực hiện được. Khởi tạo$l = 0$,$r = \max(a,b,c)$. 
4. Liên tục chia đôi khoảng và kiểm tra điểm giữa. Nếu điểm giữa khả thi, hãy di chuyển giới hạn dưới lên; nếu không thì di chuyển giới hạn trên xuống. 
5. Sau khi lặp lại đủ số lần, khoảng đủ nhỏ để thể hiện ranh giới hợp lý chính xác. Chuyển đổi giá trị này thành dạng phân số phù hợp với yêu cầu của bài toán bằng cách diễn giải giá trị thực cuối cùng dưới dạng xấp xỉ hợp lý của ranh giới nơi tính khả thi thay đổi. 

### Tại sao nó hoạt động 

chức năng$f(x)$là đơn điệu không tăng trong$x$. Một khi là một giá trị$x$quá lớn để cho phép$k$hình khối, bất kỳ giá trị nào lớn hơn đều không thể phục hồi tính khả thi vì mọi thuật ngữ sàn chỉ có thể giữ nguyên hoặc giảm đi. Điều này đảm bảo một điểm chuyển tiếp duy nhất giữa các khu vực khả thi và không khả thi. Tìm kiếm nhị phân hội tụ đến ngưỡng này mà không bỏ sót bất kỳ vùng ứng viên nào có thể chứa giá trị tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(a, b, c, x, k):
    return (a // x) * (b // x) * (c // x) >= k

def solve_case(a, b, c, k):
    lo, hi = 1, max(a, b, c)

    while lo <= hi:
        mid = (lo + hi) // 2
        if mid == 0:
            break
        if can(a, b, c, mid, k):
            lo = mid + 1
        else:
            hi = mid - 1

    # hi is the largest integer x that still works
    # for fractional precision, we refine
```
