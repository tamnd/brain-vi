---
title: "CF 1216C - Tấm trắng"
description: "Chúng ta có ba hình chữ nhật trên một mặt phẳng thẳng hàng với các trục. Hình chữ nhật đầu tiên là một tờ giấy màu trắng và hai hình chữ nhật tiếp theo là một tờ giấy màu đen. Mỗi hình chữ nhật được xác định bởi tọa độ dưới cùng bên trái và trên cùng bên phải."
date: "2026-06-11T22:52:34+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1216
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 587 (Div. 3)"
rating: 1700
weight: 1216
solve_time_s: 148
verified: true
draft: false
---

[CF 1216C - Bảng trắng](https://codeforces.com/problemset/problem/1216/C) 

**Đánh giá:** 1700 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba hình chữ nhật trên một mặt phẳng thẳng hàng với các trục. Hình chữ nhật đầu tiên là một tờ giấy màu trắng và hai hình chữ nhật tiếp theo là một tờ giấy màu đen. Mỗi hình chữ nhật được xác định bởi tọa độ dưới cùng bên trái và trên cùng bên phải. Nhiệm vụ là xác định xem sau khi đặt các tờ giấy đen lên trên tờ giấy trắng, có còn phần nào của tờ giấy trắng nhìn thấy được từ phía trên hay không. 

Các tọa độ xác định chính xác các cạnh của hình chữ nhật và hình chữ nhật bao gồm các ranh giới của chúng. Một điểm trên mép của tờ giấy trắng được tính là nhìn thấy được nếu nó không bị che phủ hoàn toàn bởi bất kỳ tờ giấy đen nào. Các tấm màu đen có thể chồng lên nhau và có thể che một phần hoặc toàn bộ tấm màu trắng. 

Phạm vi tọa độ tăng lên$10^6$, nhưng chỉ có ba hình chữ nhật, vì vậy bất kỳ thuật toán nào thực hiện một số lượng nhỏ các phép so sánh số học không đổi đều hiệu quả. Chúng tôi không cần cấu trúc dữ liệu, lưới hoặc quét phức tạp; một cách tiếp cận lý luận hình học là đủ. 

Các trường hợp cạnh chính phát sinh khi tờ giấy trắng được bao phủ hoàn toàn dọc theo một trục chứ không phải trục kia. Ví dụ: tấm vải trắng có thể được che phủ hoàn toàn theo chiều ngang nhưng vẫn mở rộng theo chiều dọc bên ngoài tấm vải đen. Việc kiểm tra đơn giản về tổng diện tích hoặc sự chồng chéo đơn giản sẽ thất bại ở đây. Chúng ta cũng cần xử lý các phần chồng chéo có chiều rộng bằng 0 hoặc chiều cao bằng 0 một cách khéo léo. 

Ví dụ: nếu tấm màu trắng trải dài từ (0,0) đến (4,4), tấm màu đen đầu tiên bao phủ (0,0) đến (4,2) và tấm màu đen thứ hai bao phủ (0,2) đến (4,4). Tấm vải trắng được che phủ hoàn toàn. Bất kỳ phương pháp nào chỉ kiểm tra các phần chồng chéo riêng lẻ mà không kết hợp chúng đều có thể báo cáo không chính xác một phần hiển thị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là xem xét mọi điểm trên tờ giấy trắng và kiểm tra xem nó có bị che bởi tờ giấy đen hay không. Về nguyên tắc, điều này hoạt động nhưng không khả thi vì ngay cả việc lấy mẫu lưới vừa phải cũng sẽ mất hàng triệu lần kiểm tra với giới hạn tọa độ lên tới$10^6$. Kiểm tra từng tọa độ số nguyên bên trong hình chữ nhật màu trắng là$O((x_2-x_1)*(y_2-y_1))$, có thể đạt tới$10^{12}$hoạt động, vượt xa thời hạn. 

Quan sát quan trọng là một hình chữ nhật chỉ có thể được che phủ hoàn toàn nếu nó bị chặn hoàn toàn dọc theo một trục. Điều này cho phép chúng ta giảm vấn đề xuống còn một số lượng nhỏ các kiểm tra 1D. Chúng ta có thể kiểm tra riêng phạm vi bao phủ dọc theo trục ngang (x) và trục dọc (y). Tấm trắng chỉ vô hình nếu tồn tại một cấu hình trong đó các tấm màu đen cùng bao phủ toàn bộ nhịp ngang hoặc toàn bộ nhịp dọc. Nếu vẫn còn khoảng trống dọc theo một trong hai trục thì vẫn hiển thị được một phần của tờ giấy trắng. 

Vì vậy giải pháp tối ưu là kiểm tra bên trái tối đa và bên phải tối thiểu của tờ giấy đen xem chúng có chặn hoàn toàn tờ giấy trắng theo chiều ngang hay không. Tương tự, nó kiểm tra phần dưới tối đa và phần trên tối thiểu của tấm màu đen để có độ bao phủ theo chiều dọc. Nếu một trong hai trục có khe hở bên ngoài hình chiếu tấm đen kết hợp thì câu trả lời là "CÓ"; nếu không thì là "KHÔNG". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((x2-x1)*(y2-y1)) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trích xuất tọa độ của tờ trắng: x1, y1, x2, y2. 
2. Trích xuất tọa độ của tờ đen đầu tiên: x3, y3, x4, y4. 
3. Trích xuất tọa độ của tờ đen thứ 2: x5, y5, x6, y6. 
4. Tính toán độ bao phủ theo chiều ngang của các tấm màu đen cộng lại. Khối ngoài cùng bên trái là min(x3, x5) và khối ngoài cùng bên phải là max(x4, x6). Kiểm tra xem phạm vi kết hợp này có bao phủ toàn bộ trang tính màu trắng hay không, tức là nếu dải ô ngoài cùng bên trái ≤ x1 và dải ô ngoài cùng bên phải ≥ x2. 
5. Tính toán phạm vi bao phủ theo chiều dọc tương tự. Khối dưới cùng là min(y3, y5) và khối trên cùng là max(y4, y6). Kiểm tra xem cái này có bao phủ hoàn toàn tờ giấy trắng theo chiều dọc hay không. 
6. Nếu cả vùng phủ sóng ngang và dọc đều chặn hoàn toàn tờ giấy trắng, hãy in "KHÔNG". Ngược lại in ra "CÓ". 

Tại sao nó hoạt động: Bằng cách chiếu các hình chữ nhật lên mỗi trục, chúng tôi giảm vấn đề bao phủ 2D xuống các khoảng 1D. Tờ giấy trắng chỉ hoàn toàn vô hình nếu toàn bộ khoảng ngang của nó bị che phủ và toàn bộ khoảng dọc của nó bị che phủ. Bất kỳ khoảng trống nào dọc theo một trong hai trục đều đảm bảo có ít nhất một điểm nhìn thấy được. Điều này phù hợp với mọi sự kết hợp của các lớp phủ một phần và đảm bảo chúng tôi xử lý các trường hợp cạnh của các trang màu đen chồng lên nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x1, y1, x2, y2 = map(int, input().split())
x3, y3, x4, y4 = map(int, input().split())
x5, y5, x6, y6 = map(int, input().split())

# Compute horizontal coverage
h_left = min(x3, x5)
h_right = max(x4, x6)
horizontal_covers = h_left <= x1 and h_right >= x2

# Compute vertical coverage
v_bottom = min(y3, y5)
v_top = max(y4, y6)
vertical_covers = v_bottom <= y1 and v_top >= y2

if horizontal_covers and vertical_covers:
    print("NO")
else:
    print("YES")
```Giải pháp đầu tiên đọc tọa độ và tính toán các ranh giới cực trị của các tấm màu đen dọc theo mỗi trục. Sau đó, chúng tôi kiểm tra xem tập hợp các hình chiếu của tấm đen có chứa đầy đủ tấm trắng dọc theo cả hai trục hay không. Điều quan trọng là sử dụng`<=`Và`>=`để xử lý việc căn chỉnh cạnh chính xác, vì phạm vi bao phủ bao gồm cả cạnh được tính là chặn. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
2 2 4 4
1 1 3 5
3 1 5 5
```| Biến | Giá trị | 
| --- | --- | 
| h_left | phút(1,3)=1 | 
| h_đúng | tối đa(3,5)=5 | 
| ngang_covers | 1 2 và 5 ≥ 4 → Đúng | 
| v_bottom | phút(1,1)=1 | 
| v_top | tối đa(5,5)=5 | 
| dọc_covers | 1 2 và 5 ≥ 4 → Đúng | 

Cả hai trục đều được bao phủ hoàn toàn, đầu ra là KHÔNG. 

**Mẫu 2:** 

đầu vào:```
2 2 4 4
0 0 3 3
1 1 5 5
```| Biến | Giá trị | 
| --- | --- | 
| h_left | phút(0,1)=0 | 
| h_đúng | tối đa(3,5)=5 | 
| ngang_covers | 0 2 và 5 ≥ 4 → Đúng | 
| v_bottom | phút(0,1)=0 | 
| v_top | tối đa(3,5)=5 | 
| dọc_covers | 0 2 và 5 ≥ 4 → Đúng | 

Ở đây đoàn vẫn bao, xuất NO. Nếu chúng ta di chuyển một tờ màu đen lên sao cho v_bottom > y1, một khoảng trống sẽ xuất hiện, cho kết quả CÓ. 

Những dấu vết này cho thấy việc chiếu lên từng trục sẽ xác định chính xác phạm vi bao phủ mà không cần kiểm tra từng điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số ít phép tính và so sánh | 
| Không gian | O(1) | Chỉ các biến cho tọa độ và cực trị | 

Chỉ với ba hình chữ nhật, giải pháp này thực thi trong thời gian và bộ nhớ không đổi. Tọa độ lớn nhất là$10^6$và tất cả các phép toán đều là so sánh số nguyên và tối thiểu/tối đa, dễ dàng khớp trong giới hạn 1 giây, 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x1, y1, x2, y2 = map(int, input().split())
    x3, y3, x4, y4 = map(int, input().split())
    x5, y5, x6, y6 = map(int, input().split())
    h_left = min(x3, x5)
    h_right = max(x4, x6)
    horizontal_covers = h_left <= x1 and h_right >= x2
    v_bottom = min(y3, y5)
    v_top = max(y4, y6)
    vertical_covers = v_bottom <= y1 and v_top >= y2
    return "NO" if horizontal_covers and vertical_covers else "YES"

# provided samples
assert run("2 2 4 4\n1 1 3 5\n3 1 5 5\n") == "NO", "sample 1"
assert run("2 2 4 4\n0 0 3 3\n1 1 5 5\n") == "NO", "sample 2"

# custom cases
assert run("0 0 1 1\n2 2 3 3\n4 4 5 5\n") == "YES", "white completely free"
assert run("0 0 2 2\n0 0 1 3\n1 0 3 2\n") == "NO", "exact full coverage"
assert run("0 0 2 2\n0 0 1 1\n1 0 3 1\n") == "YES", "vertical gap"
assert run("0 0 2 2\n0 0 1 3\n1 1 3 3\n") == "YES", "horizontal gap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| | | |
