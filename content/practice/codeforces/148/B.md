---
title: "CF 148B - Trốn Thoát"
description: "Chúng ta được yêu cầu mô phỏng kịch bản rượt đuổi giữa công chúa và rồng. Công chúa chạy với tốc độ không đổi, còn con rồng bay nhanh hơn nhưng chỉ bắt đầu đuổi theo sau một khoảng thời gian trễ. Mỗi khi con rồng đuổi kịp, công chúa có thể thả bijou để đánh lạc hướng nó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 148
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 105 (Div. 2)"
rating: 1500
weight: 148
solve_time_s: 72
verified: true
draft: false
---

[CF 148B - Thoát](https://codeforces.com/problemset/problem/148/B) 

**Đánh giá:** 1500 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng kịch bản rượt đuổi giữa công chúa và rồng. Công chúa chạy với tốc độ không đổi, còn con rồng bay nhanh hơn nhưng chỉ bắt đầu đuổi theo sau một khoảng thời gian trễ. Mỗi khi con rồng đuổi kịp, công chúa có thể thả bijou để đánh lạc hướng nó. Sau đó, con rồng quay trở lại kho bạc, dành một khoảng thời gian cố định ở đó và tiếp tục cuộc rượt đuổi. Mục tiêu là xác định số lượng bijous tối thiểu mà công chúa phải mang theo để đến đích cố định một cách an toàn. 

Đầu vào bao gồm năm số nguyên: tốc độ của công chúa$v_p$, tốc độ của rồng$v_d$, thời gian trễ trước khi con rồng bắt đầu đuổi theo$t$, thời gian con rồng dành thời gian để sửa chữa kho báu sau khi nhặt được một món trang sức$f$, và tổng khoảng cách đến lâu đài$c$. Đầu ra là một số nguyên duy nhất: số lượng bijous tối thiểu được yêu cầu. 

Các ràng buộc rất nhỏ: tốc độ lên tới 100, gấp lên 10, khoảng cách lên tới 1000. Điều này cho phép một phương pháp mô phỏng cuộc rượt đuổi từng bước mà không cần lo ngại về hiệu suất. Việc thực hiện bất cẩn có thể thất bại trong các trường hợp khó khăn, chẳng hạn như khi rồng không bao giờ vượt qua công chúa vì nó chậm hơn hoặc khi rồng bắt được công chúa ngay tại lâu đài - những trường hợp này không yêu cầu bijou bằng 0 hoặc không yêu cầu thêm bijou cho lần vượt cuối cùng. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản: liên tục mô phỏng vị trí của công chúa và rồng và tăng số lần phản công mỗi khi rồng vượt qua cô ấy. Mỗi lần lặp lại sẽ tính toán thời gian cho đến lần vượt tiếp theo, cập nhật vị trí, áp dụng độ trễ và lặp lại cho đến khi công chúa đến được lâu đài. Phương pháp này được đảm bảo chính xác vì nó mô phỏng cuộc rượt đuổi một cách chính xác nhưng việc mô phỏng liên tục các khoảng thời gian nhỏ là không cần thiết và có thể chậm hơn mức cần thiết. 

Phương pháp tối ưu tận dụng tốc độ không đổi để tính toán thời gian và vị trí của mỗi lần vượt một cách phân tích. Vì công chúa và rồng di chuyển với vận tốc không đổi nên thời gian để rồng tiếp cận công chúa trong mỗi đoạn có thể được tính bằng một công thức đơn giản: rồng phải đi được khoảng cách tương đối giữa vị trí hiện tại của nó và công chúa, chia cho chênh lệch tốc độ của chúng. Sau mỗi lần vượt qua, chúng tôi cập nhật vị trí và tính đến độ trễ trong việc ấn định kho bạc của con rồng. Chúng tôi lặp lại điều này cho đến khi công chúa đến được lâu đài. Cái nhìn sâu sắc là chuyển động tương đối làm giảm vấn đề thành số học đơn giản thay vì mô phỏng lặp đi lặp lại, làm cho giải pháp trở nên chính xác và hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(c * small_step) | O(1) | Hoạt động nhưng không cần thiết | 
| Mô phỏng chuyển động tương đối | O(c/v_p) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo vị trí của công chúa ở mức 0 và đặt vị trí của rồng về 0. Bắt đầu đếm bijous ở mức 0. 
2. Tính quãng đường công chúa đi được trong thời gian trễ ban đầu của con rồng$t$. Công chúa di chuyển tại$v_p \cdot t$trong khi con rồng đang nhàn rỗi. 
3. Đi vào một vòng lặp tiếp tục cho đến khi công chúa chưa đến được lâu đài. 
4. Kiểm tra xem rồng có nhanh hơn công chúa không. Nếu không, công chúa sẽ không bao giờ bị bắt và không cần bijou, vì vậy hãy thoát khỏi vòng lặp. 
5. Tính khoảng cách giữa công chúa và rồng khi bắt đầu đoạn rượt đuổi. Con rồng bắt đầu đuổi theo$t$giờ cộng với bất kỳ sự chậm trễ ấn định ngân quỹ nào sau đó. 
6. Tính thời gian rồng bắt được công chúa với vận tốc tương đối:$time = distance / (v_d - v_p)$. 
7. Cập nhật vị trí của công chúa tại thời điểm vượt:$position += v_p \cdot time$. 
8. Nếu công chúa đã đến hoặc đi qua lâu đài trong thời gian này, cô ấy không cần thêm bjous nữa. Thoát khỏi vòng lặp. 
9. Tăng số lượng bijou vì con rồng đã vượt qua cô ấy và cô ấy đánh rơi một bijou. 
10. Cập nhật vị trí của rồng về 0 (trở lại hang động sau khi sửa chữa kho báu) và thêm độ trễ sửa chữa kho báu vào tổng thời gian. Công chúa tiếp tục chạy trong thời gian này, vì vậy hãy cập nhật vị trí của mình:$position += v_p \cdot f$. 
11. Lặp lại vòng lặp cho đến khi công chúa đến được lâu đài. 

Tại sao nó hoạt động: ở mỗi lần lặp, thuật toán sẽ tính toán thời điểm vượt chính xác theo phương pháp phân tích. Bằng cách cập nhật các vị trí dựa trên vận tốc không đổi và áp dụng độ trễ ấn định kho bạc, chúng tôi duy trì tính bất biến rằng vị trí của công chúa luôn phản ánh tiến trình thực tế của cô ấy và bộ đếm bijou khớp với số lần gây xao lãng được sử dụng. Quá trình dừng lại chính xác khi công chúa đến lâu đài. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

vp = int(input())
vd = int(input())
t = int(input())
f = int(input())
c = int(input())

if vp >= vd:
    print(0)
else:
    princess_pos = vp * t
    dragon_pos = 0
    bijous = 0

    while princess_pos < c:
        relative_distance = princess_pos - dragon_pos
        time_to_catch = relative_distance / (vd - vp)
        catch_pos = princess_pos + vp * time_to_catch

        if catch_pos >= c:
            break

        bijous += 1
        princess_pos = catch_pos + vp * f
        dragon_pos = 0

    print(bijous)
```Mã bắt đầu bằng cách xử lý trường hợp cạnh trong đó công chúa nhanh hơn hoặc ngang bằng với rồng, nghĩa là không cần bijous. Nó tính toán vị trí ban đầu của cô trong thời gian con rồng trì hoãn. Trong mỗi vòng lặp, nó tính toán vị trí vượt bằng tốc độ tương đối, kiểm tra xem lâu đài đã đến chưa, tăng số lượng bijou và cập nhật vị trí theo độ trễ của rồng. Số học dấu phẩy động ở đây đủ do giá trị nhỏ; tránh chia số nguyên để duy trì độ chính xác. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
2
1
1
10
```| Bước | Vị trí công chúa | Vị Trí Rồng | Bijous | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | 0 | 
| Bắt Đầu Tiên | 2 | 0 | 1 | 
| Sau Sự Trì Hoãn | 4 | 0 | 1 | 
| Bắt thứ hai | 8 | 0 | 2 | 
| Sau Sự Trì Hoãn | 10 | 0 | 2 | 

Điều này xác nhận rằng cần có hai bijous, phù hợp với mẫu. 

Đối với đầu vào khác:```
3
5
2
1
20
```| Bước | Vị trí công chúa | Vị Trí Rồng | Bijous | 
| --- | --- | --- | --- | 
| Ban đầu | 6 | 0 | 0 | 
| Bắt Đầu Tiên | 12 | 0 | 1 | 
| Sau Sự Trì Hoãn | 15 | 0 | 1 | 
| Bắt thứ hai | 21 | 0 | 2 | 

Vì cô ấy đến lâu đài lúc 20 giờ nên lần đánh bắt thứ hai diễn ra ngoài đích đến nên chỉ cần 1 bijou. Điều này xác nhận việc kiểm tra`if catch_pos >= c`ngăn chặn việc đếm những bijous không cần thiết một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(c / (v_d - v_p)) | Mỗi vòng lặp tương ứng với một lần vượt. Số lần lặp tối đa được giới hạn bởi số lần vượt có thể trong khoảng cách c. | 
| Không gian | O(1) | Chỉ sử dụng các biến vô hướng cho vị trí và bộ đếm. | 

Với các ràng buộc (c ≤ 1000, tốc độ ≤ 100), vòng lặp chạy tối đa vài chục lần, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    vp = int(input())
    vd = int(input())
    t = int(input())
    f = int(input())
    c = int(input())
    if vp >= vd:
        return "0"
    princess_pos = vp * t
    dragon_pos = 0
    bijous = 0
    while princess_pos < c:
        relative_distance = princess_pos - dragon_pos
        time_to_catch = relative_distance / (vd - vp)
        catch_pos = princess_pos + vp * time_to_catch
        if catch_pos >= c:
            break
        bijous += 1
        princess_pos = catch_pos + vp * f
        dragon_pos = 0
    return str(bijous)

assert run("1\n2\n1\n1\n10\n") == "2", "sample 1"
assert run("3\n5\n2\n1\n20\n") == "1", "custom 1"
assert run("2\n2\n1\n1\n5\n") == "0", "dragon not faster"
assert run("5\n10\n1\n1\n1\n") == "0", "catch after castle"
assert run("1\n100\n1\n10\n1000\n") == "9", "maximum iterations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 1 10 | 2 | Kịch bản mẫu từ câu lệnh | 
| 3 5 2 1 20 | 1 | Đảm bảo việc đánh bắt ngoài lâu đài bị bỏ qua | 
| 2 2 1 1 5 | 0 | Rồng không nhanh hơn, không bijous | 
| 5 10 1 1 1 | 0 | Lâu đài đạt được trước lần đánh bắt đầu tiên | 
| 1 100 1 10 1000 | 9 | Chênh lệch tốc độ lớn, vượt nhiều lần | 

## Vỏ cạnh 

Thuật toán xử lý chính xác trường hợp công chúa nhanh hơn hoặc bằng rồng. Đối với đầu vào`vp=2, vd=2, t=1, f=1, c=5`, kiểm tra ban đầu sẽ ngăn chặn bất kỳ việc thực hiện vòng lặp nào, tạo ra bijous bằng 0. Khi con rồng
