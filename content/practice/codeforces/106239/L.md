---
title: "CF 106239L - \u6c14\u7403\u91c7\u8d2d"
description: "Chúng ta được cung cấp một tập hợp $n$ loại vấn đề khác nhau, mỗi loại được liên kết với một tỷ lệ xác suất kỳ vọng $pi / qi$ mô hình hóa số lượng người tham gia dự kiến ​​​​sẽ giải quyết vấn đề đó."
date: "2026-06-19T16:27:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106239
codeforces_index: "L"
codeforces_contest_name: "2025\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66\u65b0\u751f\u8d5b(\u51b3\u8d5b)"
rating: 0
weight: 106239
solve_time_s: 49
verified: true
draft: false
---

[CF 106239L - \u6c14\u7403\u91c7\u8d2d](https://codeforces.com/problemset/problem/106239/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$các loại vấn đề khác nhau, mỗi loại có liên quan đến một tỷ lệ xác suất dự kiến$p_i / q_i$mô hình đó có bao nhiêu người tham gia dự kiến ​​​​sẽ giải quyết vấn đề đó. có$m$tổng số người tham gia, vì vậy đối với mỗi loại vấn đề, chúng tôi ước tính cần bao nhiêu quả bóng bay có màu đó bằng cách chia tỷ lệ xác suất này theo$m$và làm tròn đến số nguyên gần nhất. 

Mỗi loại vấn đề đều đã chuẩn bị sẵn một số quả bóng bay. Nếu số tiền chuẩn bị không đủ theo yêu cầu ước tính, chúng tôi sẽ mua số tiền còn thiếu. Nếu đủ thì chúng tôi không làm gì cho loại đó. Nhiệm vụ là tính tổng số quả bóng bay phải mua thêm cho tất cả các loại bài toán. 

Kích thước đầu vào rất nhỏ, có tối đa 14 loại vấn đề và tối đa 200 người tham gia. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào ngay cả khi thực hiện số học đơn giản cho mỗi mục đều đủ nhanh. Sự tinh tế thực sự duy nhất là tính chính xác của bước làm tròn, vì tính toán dấu phẩy động sẽ có rủi ro và không cần thiết. 

Một cạm bẫy phổ biến là tính toán$m \cdot p_i / q_i$sử dụng số học dấu phẩy động và sau đó áp dụng phép ép kiểu hoặc làm tròn. Điều đó có thể gây ra lỗi chính xác, đặc biệt khi các giá trị gần với ranh giới số nguyên. Một vấn đề tế nhị khác là quên áp dụng mức trần một cách chính xác, ví dụ như sử dụng phép chia số nguyên trực tiếp để cắt bớt thay vì làm tròn. 

Một ví dụ thất bại cụ thể là khi$m = 10$,$p = 1$,$q = 11$. Giá trị thực là$10/11$, sẽ trở thành 1 sau mức trần. Phép chia số nguyên sẽ cho kết quả 0, điều này đánh giá thấp yêu cầu và dẫn đến tính toán thiếu hụt không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu về cơ bản đã là tối ưu. Đối với mỗi loại bài toán, chúng tôi trực tiếp tính toán số lượng bóng bay cần thiết bằng cách sử dụng định nghĩa rồi so sánh với số lượng đã có sẵn. Chi phí cho mỗi mặt hàng là không đổi, do đó tổng thời gian chạy là tuyến tính theo$n$, nhiều nhất là 14. 

Nếu người ta diễn giải phép tính theo nghĩa đen bằng cách sử dụng số học dấu phẩy động, thì việc thực hiện sẽ vẫn tầm thường, nhưng tính chính xác sẽ trở nên mong manh. Cái nhìn sâu sắc quan trọng là giá trị được yêu cầu là mức trần của một số hữu tỷ$m \cdot p_i / q_i$và điều này có thể được tính toán chính xác bằng cách sử dụng số học số nguyên mà không có bất kỳ vấn đề nào về độ chính xác của phép chia. 

Sự chuyển đổi xuất phát từ việc viết lại thao tác trần theo cách tránh hoàn toàn phân số. Thay vì tính số thực và làm tròn, chúng ta tính$\lceil x/y \rceil = (x + y - 1) // y$, đảm bảo tính đúng đắn cho tất cả các số nguyên dương. Điều này loại bỏ hoàn toàn dấu phẩy động và làm cho việc tính toán vừa chính xác vừa an toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán dấu phẩy động trực tiếp | O(n) | O(1) | Rủi ro | 
| Công thức trần số nguyên | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi loại bài toán, hãy tính tổng tử số yêu cầu dự kiến như sau:$m \cdot p_i$. Điều này thể hiện tổng số trường hợp được giải quyết dự kiến ​​được chia theo tỷ lệ của người tham gia. 
2. Chuyển đổi tỷ lệ thành trần số nguyên bằng công thức$(m \cdot p_i + q_i - 1) // q_i$. Điều này đảm bảo chúng ta luôn làm tròn khi có số dư sau khi chia. 
3. So sánh yêu cầu tính toán với các bong bóng đã có sẵn$w_i$. Nếu yêu cầu lớn hơn, hãy tính toán sự khác biệt. 
4. Tích lũy tất cả các chênh lệch dương vào tổng số hiện có. Số tiền này đại diện cho tổng số bóng bay phải mua. 
5. Xuất ra giá trị tích lũy cuối cùng sau khi xử lý tất cả các loại bài toán. 

### Tại sao nó hoạt động 

Mỗi loại vấn đề đều độc lập nên tổng nhu cầu chỉ đơn giản là tổng của những thiếu hụt riêng lẻ. Công thức trần đảm bảo rằng mỗi$need_i$là số nguyên nhỏ nhất không nhỏ hơn kỳ vọng phân số thực. Vì chúng tôi chỉ mua khi nguồn cung không đủ nên mỗi đóng góp cho câu trả lời chính xác là mức thâm hụt của loại đó và không có sự tương tác nào tồn tại giữa các danh mục có thể làm mất hiệu lực của tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    ans = 0

    for _ in range(n):
        p, q, w = map(int, input().split())
        need = (m * p + q - 1) // q
        if need > w:
            ans += need - w

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo công thức dẫn xuất. Dòng chính là tính toán trần số nguyên, tránh hoàn toàn mọi số học dấu phẩy động. phép nhân$m * p$là an toàn vì các ràng buộc nhỏ và Python vẫn xử lý các số nguyên lớn. Bước trừ được bảo vệ nên chúng tôi chỉ tích lũy lượng thiếu hụt, đảm bảo rằng chúng tôi không bao giờ cộng sai số dư dưới dạng giá trị âm. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp có ba loại vấn đề trong đó$m = 10$. Giả sử đầu vào là$(p, q, w)$BẰNG$(1, 1, 0)$,$(50, 100, 10)$, Và$(1, 11, 0)$. 

Đối với loại thứ nhất, yêu cầu là$\lceil 10 \cdot 1 / 1 \rceil = 10$, vậy độ thiếu là$10$. 

Đối với loại thứ hai, yêu cầu là$\lceil 10 \cdot 50 / 100 \rceil = \lceil 5 \rceil = 5$, và kể từ đó$w = 10$, không cần mua hàng. 

Đối với loại thứ ba, yêu cầu là$\lceil 10 \cdot 1 / 11 \rceil = 1$, vậy độ thiếu là$1$. 

| tôi | p | q | m·p | cần | w | thâm hụt | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 10 | 10 | 0 | 10 | 
| 2 | 50 | 100 | 500 | 5 | 10 | 0 | 
| 3 | 1 | 11 | 10 | 1 | 0 | 1 | 

Bảng cho thấy mỗi hàng được xử lý độc lập và câu trả lời cuối cùng là tổng số phần thiếu hụt. 

Ví dụ thứ hai có thể nhấn mạnh hành vi trần. Lấy$m = 7$, và một vấn đề duy nhất với$p = 2$,$q = 3$,$w = 4$. Yêu cầu là$\lceil 14/3 \rceil = 5$, nên chúng ta cần mua thêm 1 quả bóng bay. Điều này xác nhận rằng phép chia dựa trên việc cắt bớt sẽ cho kết quả không chính xác là 4 và thiếu đơn vị bổ sung cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi loại bài toán được xử lý một lần với số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một khoản tiền được duy trì | 

Giới hạn hạn chế$n$ở mức 14, vì vậy những giải pháp thậm chí đắt tiền hơn cũng có thể vượt qua một cách thoải mái. Giải pháp này nằm trong giới hạn và có hiệu quả về thời gian không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import ceil

    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        ans = 0
        for _ in range(n):
            p, q, w = map(int, input().split())
            need = (m * p + q - 1) // q
            if need > w:
                ans += need - w
        print(ans)

    from io import StringIO
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample (first example described)
assert run("3 10\n1 1 0\n50 100 10\n1 11 0\n") == "11"

# minimum case
assert run("1 1\n1 1 0\n") == "1"

# already sufficient inventory
assert run("2 10\n1 1 10\n1 2 10\n") == "0"

# ceiling edge case
assert run("1 7\n2 3 4\n") == "1"

# large ratio check
assert run("1 200\n100 100 0\n") == "200"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dòng chảy đơn | 1 | trần cơ bản và thâm hụt | 
| tất cả đều đủ | 0 | không có đóng góp tiêu cực | 
| làm tròn phân số | 1 | hành vi trần đúng | 
| tỷ lệ tối đa | 200 | độ đúng của phép nhân lớn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi yêu cầu tính toán đã được đáp ứng chính xác bởi các bong bóng hiện có. Đối với một đầu vào như$m = 10$,$p = 50$,$q = 100$,$w = 5$, yêu cầu trở thành$\lceil 5 \rceil = 5$. Thuật toán tính toán mức thâm hụt bằng 0 vì phép so sánh`need > w`thất bại. Điều này khẳng định rằng sự bình đẳng không kích hoạt việc mua sắm không cần thiết. 

Một trường hợp khác là khi giá trị phân số thực nằm ngay trên ranh giới số nguyên. Vì$m = 7$,$p = 2$,$q = 3$, chúng tôi nhận được$14/3$. Công thức$(14 + 2)/3$đảm bảo làm tròn chính xác thành 5, không phải 4. Một phép chia số nguyên đơn giản sẽ tạo ra 4 và âm thầm đếm thiếu một, điều này trực tiếp phá vỡ tính chính xác.
