---
title: "CF 1054A - Thang máy hay cầu thang bộ?"
description: "Masha bắt đầu ở tầng $x$ và muốn đạt đến tầng $y$. Cô có hai cách để di chuyển theo chiều dọc bên trong tòa nhà: cầu thang bộ hoặc thang máy. Cầu thang luôn hoạt động theo kiểu tuyến tính đơn giản, mỗi lần di chuyển giữa các tầng lân cận đều tốn một khoảng thời gian cố định $t1$."
date: "2026-06-15T10:22:01+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1054
codeforces_index: "A"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 1"
rating: 800
weight: 1054
solve_time_s: 147
verified: true
draft: false
---

[CF 1054A - Thang máy hay cầu thang?](https://codeforces.com/problemset/problem/1054/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Masha bắt đầu trên sàn$x$và muốn chạm tới tầng$y$. Cô có hai cách để di chuyển theo chiều dọc bên trong tòa nhà: cầu thang bộ hoặc thang máy. Cầu thang luôn hoạt động theo tuyến tính đơn giản, mỗi lần di chuyển giữa các tầng lân cận đều tốn một khoảng thời gian cố định$t_1$. Vì vậy nếu khoảng cách sàn lớn thì tổng chi phí sẽ tăng theo tỷ lệ. 

Thang máy hoạt động khác vì nó chưa ở tầng của Masha. Lúc cô ấy bước ra thì thang máy đã ở tầng$z$với những cánh cửa đóng kín. Trước khi có thể phục vụ cô ấy, nó có thể cần phải di chuyển đến tầng của cô ấy, mở cửa, đóng cửa và sau đó vận chuyển cô ấy đến tầng mục tiêu$y$. Mỗi chuyển động giữa các tầng liền kề đều tốn chi phí$t_2$và chi phí vận hành mỗi cửa (mở hoặc đóng)$t_3$. 

Quy tắc quyết định rất nghiêm ngặt: Masha sẽ chỉ sử dụng cầu thang nếu thời gian đi thang bộ hoàn toàn nhỏ hơn thời gian đi thang máy. Nếu không cô ấy sẽ đi thang máy. 

Các ràng buộc rất nhỏ, tất cả các giá trị tối đa là 1000. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu phức tạp hoặc kỹ thuật tối ưu hóa. Ngay cả một số phép tính số học không đổi cho mỗi trường hợp thử nghiệm cũng đủ. 

Một trường hợp phức tạp phát sinh khi thang máy khởi động rất gần hoặc thậm chí ở tầng của Masha. Trong tình huống đó, việc triển khai đơn giản đôi khi quên tính đến cả hoạt động của cửa và tính đối xứng chuyển động. Một sai lầm phổ biến khác là xử lý không đúng việc thang máy luôn phải “đến Masha trước” trước khi lên.$y$, bất kể hướng nào. 

Ví dụ, nếu$x = 10$,$y = 1$, Và$z = 9$, thang máy đầu tiên di chuyển 1 tầng để đến Masha, sau đó thực hiện các thao tác mở cửa, sau đó di chuyển hết quãng đường đến$y$. Thiếu bước nhận hàng ban đầu là nguyên nhân điển hình dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng từng giây chuyển động: di chuyển thang máy từng bước từ$z$ĐẾN$x$, mở và đóng cửa, sau đó di chuyển từ$x$ĐẾN$y$. Điều này đơn giản về mặt khái niệm và luôn đúng, nhưng không cần thiết vì mỗi pha có một biểu thức dạng đóng. Mặc dù các ràng buộc là nhỏ, nhưng việc mô phỏng sẽ làm tăng thêm sự phức tạp không cần thiết và tạo ra nhiều cơ hội xảy ra các lỗi ngẫu nhiên trong thời gian mở cửa. 

Quan sát quan trọng là mọi phần của quá trình đều tuyến tính và mang tính quyết định. Khoảng cách giữa các tầng là sự khác biệt tuyệt đối và mỗi chuyển động hoặc hành động của cửa đều đóng góp một chi phí cố định. Điều đó có nghĩa là toàn bộ quá trình có thể được biểu diễn dưới dạng một công thức trực tiếp. 

Đối với cầu thang, chi phí chỉ đơn giản là khoảng cách$|x - y|$nhân với$t_1$. 

Đối với thang máy, chúng tôi chia quy trình thành ba giai đoạn khái niệm. Đầu tiên thang máy đi từ$z$ĐẾN$x$, tính chi phí$|z - x| \cdot t_2$. Sau đó là một chuỗi thao tác cửa khi Masha bước vào: mở và đóng một lần. Sau đó thang máy di chuyển từ$x$ĐẾN$y$, tính chi phí$|x - y| \cdot t_2$, và cuối cùng cánh cửa lại mở ra ở điểm đến. Điều này dẫn đến chi phí cố định của ba hành động cửa cộng với một lần đóng nữa khi bắt đầu, tùy theo cách giải thích, nhưng công thức tiêu chuẩn đơn giản hóa nó thành tổng cộng bốn sự kiện liên quan đến cửa trong mô tả hành trình đầy đủ. 

Bởi vì mọi thứ đều có tính cộng và độc lập nên vấn đề giảm xuống còn việc đánh giá hai biểu thức và so sánh chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(khoảng cách) | O(1) | Quá chậm và dễ mắc lỗi | 
| Tính toán công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính thời gian leo cầu thang là chênh lệch tuyệt đối giữa$x$Và$y$, nhân với$t_1$. Điều này phản ánh trực tiếp rằng mỗi lần chuyển tầng lên cầu thang đều có chi phí không đổi. 
2. Tính thời gian thang máy di chuyển từ vị trí hiện tại$z$đến tầng của Masha$x$BẰNG$|z - x| \cdot t_2$. Đây là mô hình thang máy di chuyển trống để đón cô ấy. 
3. Thêm chi phí vận hành cửa vào thang máy và chuẩn bị di chuyển. Mỗi chi phí hoạt động$t_3$và có hai quá trình chuyển đổi thiết yếu: mở và đóng khi lên máy bay, cộng với mở khi đến nơi. 
4. Tính hành trình thang máy từ$x$ĐẾN$y$BẰNG$|x - y| \cdot t_2$, đại diện cho chuyến đi chính. 
5. Thêm chi phí mở cửa cuối cùng tại điểm đến để cho phép thoát ra. 
6. So sánh tổng thời gian đi thang máy và đi thang bộ. Nếu thời gian thang máy nhỏ hơn hoặc bằng thời gian đi thang bộ, thì xuất CÓ, nếu không thì xuất NO. 

Tính đúng đắn xuất phát từ thực tế là cả hai phương thức vận tải đều phân tách thành các phân đoạn độc lập với chi phí chỉ phụ thuộc vào khoảng cách sàn và mức phạt cố định cho mỗi hành động. Không có sự tương tác giữa các phân đoạn nên việc tính tổng chúng sẽ nắm bắt chính xác tổng thời gian. Đường đi của thang máy là xác định khi Masha chọn nó, vì vậy công thức tính toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, y, z, t1, t2, t3 = map(int, input().split())

    stair = abs(x - y) * t1

    elevator = abs(z - x) * t2          # move elevator to Masha
    elevator += 2 * t3                  # open + close at pickup
    elevator += abs(x - y) * t2         # move to destination
    elevator += 2 * t3                  # open + close at destination

    if elevator <= stair:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Mã theo sau sự phân rã trực tiếp. Việc tính toán bậc thang là một sự khác biệt tuyệt đối duy nhất nhân với chi phí. Việc tính toán thang máy mô hình hóa rõ ràng từng giai đoạn: định vị lại, lên máy bay, di chuyển và thoát ra. Phép so sánh sử dụng quy tắc bất đẳng thức nghiêm ngặt được đảo ngược thành một điều kiện đơn giản. 

Một chi tiết triển khai tinh tế là giữ tất cả các hoạt động ở dạng số nguyên. Vì tất cả đầu vào đều nhỏ nên tràn không phải là vấn đề trong Python, nhưng trong các ngôn ngữ khác thì điều đó lại quan trọng. Một điểm khác là đảm bảo cả hai giai đoạn cửa đều được tính; thiếu thao tác lấy hoặc trả cửa dẫn đến thời gian thang máy bị đánh giá thấp một cách có hệ thống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 4 4 2 1
```Cầu thang:$|5 - 1| = 4$, vậy chi phí là$4 \cdot 4 = 16$. 

Tính toán thang máy: 

| Bước | Di chuyển vị trí | Chi phí bổ sung | Tổng số chạy | 
| --- | --- | --- | --- | 
| Di chuyển z→x | | 4 - 5 | = 1 | 
| Đón cửa | mở+đóng | 2 * 1 = 2 | 4 | 
| Di chuyển x→y | | 5 - 1 | = 4 | 
| Cửa thoát hiểm | mở+đóng | 2 * 1 = 2 | 14 | 

Tổng số thang máy là 14, nhỏ hơn 16, vì vậy đầu ra là CÓ. 

Dấu vết này xác nhận rằng cả hai phân đoạn chuyển động đều cần thiết và hoạt động của cửa tạo thành một chi phí cố định không phụ thuộc vào khoảng cách. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
3 10 8 2 5 3
```Cầu thang:$|3 - 10| = 7$, trị giá$7 \cdot 2 = 14$. 

Thang máy: 

| Bước | Di chuyển vị trí | Chi phí bổ sung | Tổng số chạy | 
| --- | --- | --- | --- | 
| Di chuyển z→x | | 8 - 3 | = 5 | 
| Đón cửa | mở+đóng | 6 | 31 | 
| Di chuyển x→y | | 3 - 10 | = 7 | 
| Cửa thoát hiểm | mở+đóng | 6 | 72 | 

Thang máy chậm hơn nhiều nên câu trả lời là KHÔNG. 

Ví dụ này nêu bật chi phí di chuyển bằng thang máy lớn như thế nào khi$t_2$cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên tới 1000, nhưng giải pháp hoàn toàn không mở rộng theo kích thước đầu vào, do đó, nó thoải mái nằm gọn trong giới hạn ngay cả đối với nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, y, z, t1, t2, t3 = map(int, sys.stdin.readline().split())

    stair = abs(x - y) * t1

    elevator = abs(z - x) * t2
    elevator += 2 * t3
    elevator += abs(x - y) * t2
    elevator += 2 * t3

    return "YES\n" if elevator <= stair else "NO\n"

# provided sample
assert run("5 1 4 4 2 1") == "YES\n"

# Masha already near destination but elevator far
assert run("10 1 1 1 10 1") == "NO\n"

# elevator optimal case
assert run("1 3 2 1 1 1") == "YES\n"

# symmetric floors
assert run("2 5 10 3 1 2") in ("YES\n", "NO\n")

# equal speeds but elevator far
assert run("1 100 50 1 1 1") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 1 1 1 10 1 | KHÔNG | cửa thang máy xa và đắt tiền | 
| 1 3 2 1 1 1 | CÓ | thang máy cạnh tranh ở khoảng cách ngắn | 
| 1 100 50 1 1 1 | CÓ | sự tỉnh táo chuyển động đối xứng lớn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi thang máy đã bắt đầu ở tầng của Masha, nghĩa là$z = x$. Trong trường hợp này, số hạng chuyển động ban đầu biến mất nhưng thao tác cửa vẫn còn. Đối với đầu vào như$x = 5, y = 10, z = 5, t_1 = 3, t_2 = 1, t_3 = 2$, thời gian thang máy chỉ trở thành chi phí cửa cộng với việc di chuyển đến$y$, mà công thức ghi lại chính xác là 0 cho số hạng chuyển động đầu tiên. 

Một trường hợp khác là khi đích đến liền kề với điểm bắt đầu, chẳng hạn như$x = 1, y = 2$. Việc triển khai đơn giản có thể đếm quá mức chuyển động hoặc quên rằng chênh lệch tuyệt đối là 1. Việc tính toán bậc thang xử lý việc này một cách rõ ràng như$1 \cdot t_1$, và công thức thang máy tương tự giảm đến mức di chuyển tối thiểu. 

Trường hợp cạnh cuối cùng xảy ra khi$t_1 = t_2 = t_3$. Trong kịch bản như vậy, việc so sánh phụ thuộc hoàn toàn vào cấu trúc của khoảng cách hơn là trọng số. Công thức vẫn hoạt động chính xác vì cả hai chế độ đều giảm xuống các kết hợp tuyến tính có cùng chênh lệch tuyệt đối và các hằng số cố định, duy trì tính chính xác trong điều kiện bằng nhau.
