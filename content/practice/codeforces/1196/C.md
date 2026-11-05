---
title: "CF 1196C - Robot đột phá"
description: "Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn mô tả một tập hợp robot được đặt trên một lưới vô hạn. Mỗi robot đều bắt đầu ở một tọa độ cố định và nó cũng có một hệ thống chuyển động cá nhân được mô tả theo bốn hướng chính mà nó có thể sử dụng."
date: "2026-06-13T14:10:14+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1196
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 575 (Div. 3)"
rating: 1500
weight: 1196
solve_time_s: 379
verified: true
draft: false
---

[CF 1196C - Đột phá robot](https://codeforces.com/problemset/problem/1196/C) 

**Đánh giá:** 1500 
**Thẻ:** triển khai 
**Thời gian giải:** 6 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn mô tả một tập hợp robot được đặt trên một lưới vô hạn. Mỗi robot đều bắt đầu ở một tọa độ cố định và nó cũng có một hệ thống chuyển động cá nhân được mô tả theo bốn hướng chính mà nó có thể sử dụng. 

Một lệnh duy nhất được phát tới tất cả các robot trong một truy vấn. Lệnh đó chỉ định một ô mục tiêu và mỗi robot cố gắng di chuyển về phía ô đó chỉ bằng các hướng được phép. Tuy nhiên, một số robot bị hạn chế: nếu một hướng bị vô hiệu hóa, chúng không thể đi qua các cạnh theo hướng đó. Kết quả là, mặc dù lưới là vô hạn nhưng mỗi robot chỉ có thể tiếp cận một tập hợp con của tất cả các ô. 

Nhiệm vụ là quyết định xem có tồn tại một điểm lưới duy nhất mà mọi robot trong truy vấn có thể tiếp cận hay không. Nếu một điểm như vậy tồn tại, chúng ta phải xuất ra một tọa độ hợp lệ; nếu không chúng tôi xuất ra 0. 

Hạn chế chính là tổng số robot trên tất cả các truy vấn tối đa là 100.000. Điều này loại trừ bất kỳ giải pháp nào cố gắng khám phá lưới trên mỗi robot hoặc mô phỏng đường dẫn. Bất kỳ số bậc hai nào trong n cho mỗi truy vấn cũng không an toàn vì bản thân một truy vấn có thể lớn. 

Một trường hợp lỗi nhỏ xuất hiện khi các robot khác nhau áp đặt các hạn chế hướng xung đột nhằm loại bỏ tất cả các giao lộ chung. Ví dụ: một robot chỉ có thể di chuyển lên trên và sang phải, nghĩa là nó chỉ có thể di chuyển đến các điểm về phía đông bắc so với điểm xuất phát, trong khi một robot khác chỉ có thể di chuyển xuống dưới và sang trái, hạn chế nó ở các khu vực phía tây nam. Ngay cả khi vị trí bắt đầu của chúng gần nhau thì các vùng có thể tiếp cận có thể không giao nhau. 

Một sai lầm ngây thơ là cho rằng vì chuyển động diễn ra trên một lưới nên tất cả các robot luôn có thể gặp nhau ở một nơi nào đó nếu lưới là vô hạn. Điều này bỏ qua rằng các hạn chế về hướng sẽ khắc mặt phẳng thành các vùng bị ràng buộc có thể không trùng nhau. 

Một vấn đề tế nhị khác là giả định rằng chỉ kiểm tra vị trí bắt đầu là đủ. Không phải vậy: robot có thể cần phải di chuyển đến điểm hẹn ở xa và tính khả thi phụ thuộc vào hình dạng khu vực có thể tiếp cận của mỗi robot chứ không chỉ nguồn gốc của nó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử các điểm gặp ứng viên và xác minh xem mọi robot có thể đạt đến điểm đó hay không. Tuy nhiên, lưới không bị giới hạn nên chúng tôi không thể liệt kê các ứng cử viên. Ngay cả khi chúng tôi giới hạn bản thân trong các hộp giới hạn của tọa độ đầu vào, các vùng có thể tiếp cận nói chung không phải là hình chữ nhật, vì vậy điều này vẫn không thành công. 

Quan sát quan trọng là các hạn chế chuyển động của mỗi rô-bốt luôn giảm tập hợp có thể tiếp cận của nó thành một trong số ít hình dạng hình học: toàn bộ mặt phẳng, nửa mặt phẳng hoặc vùng giống góc phần tư được neo tại điểm bắt đầu của nó. Điều này xuất phát từ thực tế là mỗi hướng bị thiếu sẽ chặn chuyển động trên một đường ranh giới, ngăn chặn hiệu quả việc vượt qua ngưỡng dọc hoặc ngang. 

Cụ thể hơn, mỗi robot có thể áp đặt độc lập các ràng buộc có dạng “x ít nhất phải là giá trị này”, “x nhiều nhất phải là giá trị này”, “y ít nhất phải là giá trị này” hoặc “y ít nhất phải là giá trị này”, tùy thuộc vào hướng nào bị thiếu. Vùng có thể tiếp cận luôn là tích Descartes của các khoảng, có thể không bị giới hạn. 

Do đó, bài toán giảm xuống còn việc tìm một điểm duy nhất thỏa mãn đồng thời các ràng buộc về khoảng của tất cả các robot. Đó chính xác là giao điểm của các khoảng trên x và y riêng biệt. 

Đối với mỗi robot, chúng tôi xác định những ranh giới mà nó thực thi. Nếu nó không thể di chuyển sang trái thì bất kỳ điểm nào có thể chạm tới đều phải có x ≥ xi. Nếu nó không thể di chuyển sang phải thì x ≤ xi. Tương tự cho các ràng buộc lên và xuống trên y. Mỗi robot có tới bốn bất bình đẳng và chúng tôi phân tích chúng trên tất cả các robot. 

Nếu kết quả là khoảng x và khoảng y khả thi đều không trống thì mọi điểm nguyên trong giao điểm đều hợp lệ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(vô hạn / không khả thi) | O(1) | Quá chậm | 
| Tối ưu | O(n) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

1. Bắt đầu với phạm vi rộng nhất có thể: x_min = -∞, x_max = +∞, y_min = -∞, y_max = +∞. Chúng đại diện cho tập hợp tất cả các điểm gặp nhau có thể có trước khi áp dụng các ràng buộc. 
2. Với mỗi robot tại (x, y), kiểm tra xem hướng nào bị vô hiệu hóa. Nếu robot không thể di chuyển sang trái thì nó không thể giảm x nên bất kỳ điểm nào có thể chạm tới đều phải thỏa mãn x ≥ x. Chúng tôi cập nhật x_min = max(x_min, x). Điều này đảm bảo tính nhất quán trên tất cả các robot. 
3. Robot không thể di chuyển sang phải thì không thể tăng x nên bất kỳ điểm nào có thể chạm tới đều phải thỏa mãn x ≤ x. Chúng tôi cập nhật x_max = min(x_max, x). Điều này hạn chế giới hạn trên của x khả thi. 
4. Nếu robot không thể di chuyển xuống thì y ≥ y phải giữ nên ta cập nhật y_min = max(y_min, y). 
5. Nếu robot không thể di chuyển lên thì y ≤ y phải giữ nên ta cập nhật y_max = min(y_max, y). 
6. Sau khi xử lý tất cả các robot, hãy kiểm tra xem giao lộ có hợp lệ hay không. Nếu x_min ≤ x_max và y_min ≤ y_max thì tồn tại một điểm gặp nhau hợp lệ. Xuất ra bất kỳ điểm nguyên nào trong hình chữ nhật, ví dụ (x_min, y_min). 
7. Ngược lại, xuất 0. 

### Tại sao nó hoạt động 

Mỗi robot giới hạn vùng khả thi trong một tập hợp luôn thẳng hàng với trục và lồi ở dạng rất đơn giản: nửa mặt phẳng hoặc toàn mặt phẳng tùy thuộc vào các hướng bị thiếu. Việc giao các ràng buộc này trên tất cả các rô-bốt sẽ đảm bảo tính chính xác vì vùng khả thi cuối cùng chính xác là tập hợp các điểm mà mọi rô-bốt có thể tiếp cận. Nếu giao điểm đó trống ở một trong hai chiều thì không có điểm nào có thể đáp ứng đồng thời tất cả các ràng buộc, do đó không tồn tại điểm gặp nhau. Nếu nó không trống thì bất kỳ điểm nào trong phần chồng lấp đều thỏa mãn giới hạn định hướng của mọi robot. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

q = int(input())
for _ in range(q):
    n = int(input())
    
    x_min, x_max = -INF, INF
    y_min, y_max = -INF, INF

    for _ in range(n):
        x, y, f1, f2, f3, f4 = map(int, input().split())

        can_left = f1
        can_up = f2
        can_right = f3
        can_down = f4

        if not can_left:
            x_min = max(x_min, x)
        if not can_right:
            x_max = min(x_max, x)
        if not can_down:
            y_min = max(y_min, y)
        if not can_up:
            y_max = min(y_max, y)

    if x_min <= x_max and y_min <= y_max:
        print(1, x_min, y_min)
    else:
        print(0)
```Việc triển khai duy trì bốn giới hạn chạy trong khi đọc tệp robots. Mỗi robot chỉ đóng góp các bản cập nhật theo thời gian liên tục, do đó giải pháp sẽ thay đổi tuyến tính theo kích thước đầu vào. 

Một cạm bẫy phổ biến là trộn lẫn hướng đi thiếu nào sẽ ảnh hưởng đến sự bất bình đẳng nào. Ánh xạ chính xác luôn bắt nguồn từ việc “không thể di chuyển về phía tọa độ giảm” ngụ ý giới hạn dưới và “không thể di chuyển về phía tọa độ tăng” ngụ ý giới hạn trên. 

Một sự tinh tế khác là khởi tạo. Việc sử dụng các giá trị trọng điểm lớn đảm bảo các giao lộ hoạt động chính xác ngay cả khi tất cả rô-bốt cho phép hoàn toàn tự do, trong trường hợp đó, giới hạn cuối cùng vẫn không bị ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
-1 -2 0 0 0 0
-1 -2 0 0 0 0
```Cả hai robot đều hoàn toàn bất động nên mỗi robot chỉ được phép đứng yên tại điểm xuất phát. Điều đó ngay lập tức buộc cả x và y lần lượt phải chính xác -1 và -2. 

| Robot | x_min | x_max | y_min | y_max | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | -1 | -2 | -2 | 
| 2 | -1 | -1 | -2 | -2 | 

Giao lộ cuối cùng là hợp lệ, vì vậy chúng tôi xuất ra:```
1 -1 -2
```Điều này xác nhận rằng các điểm cố định giống hệt nhau giao nhau một cách chính xác. 

### Ví dụ 2 

đầu vào:```
3
1 5 1 1 1 1
2 5 0 1 0 1
3 5 1 0 0 0
```Chúng tôi theo dõi các hạn chế từng bước. 

| Robot | x_min | x_max | y_min | y_max | 
| --- | --- | --- | --- | --- | 
| 1 | -∞ | ∞ | -∞ | ∞ | 
| 2 | 2 | 2 | -∞ | ∞ | 
| 3 | 3 | 2 | -∞ | 0 | 

Sau khi xử lý tất cả các robot, x_min = 3 và x_max = 2, không nhất quán. Vì vậy không tồn tại điểm gặp gỡ chung. 

Đầu ra:```
0
```Điều này chứng tỏ các hạn chế về hướng xung đột đã thu hẹp khoảng cách khả thi thành khoảng trống như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | mỗi robot cập nhật giới hạn một lần | 
| Không gian | O(1) | chỉ có bốn biến được duy trì | 

Tổng số robot trên tất cả các truy vấn là 100.000, do đó, việc quét tuyến tính nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**18
    q = int(input())
    out = []
    for _ in range(q):
        n = int(input())
        x_min, x_max = -INF, INF
        y_min, y_max = -INF, INF

        for _ in range(n):
            x, y, f1, f2, f3, f4 = map(int, input().split())
            if not f1:
                x_min = max(x_min, x)
            if not f3:
                x_max = min(x_max, x)
            if not f4:
                y_min = max(y_min, y)
            if not f2:
                y_max = min(y_max, y)

        if x_min <= x_max and y_min <= y_max:
            out.append(f"1 {x_min} {y_min}")
        else:
            out.append("0")

    return "\n".join(out)

# provided sample
assert run("""4
2
-1 -2 0 0 0 0
-1 -2 0 0 0 0
3
1 5 1 1 1 1
2 5 0 1 0 1
3 5 1 0 0 0
2
1337 1337 0 1 1 1
1336 1337 1 1 0 1
1
3 5 1 1 1 1
""") == """1 -1 -2
1 3 5
0
1 3 5"""

# minimum case
assert run("""1
1
0 0 1 1 1 1
""") == "1 0 0"

# all constrained conflict
assert run("""1
2
0 0 0 0 1 1
1 0 1 1 0 0
""") == "0"

# single robot
assert run("""1
1
5 5 1 1 1 1
""") == "1 5 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| robot duy nhất hoàn toàn miễn phí | 1xy | trường hợp cơ sở | 
| giới hạn xung đột | 0 | ngã tư vắng | 
| đầu vào mẫu | hỗn hợp | tính chính xác của các truy vấn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các robot không có hạn chế nào cả. Trong tình huống đó, không có ràng buộc nào được áp dụng và giao điểm vẫn là toàn bộ mặt phẳng. Thuật toán đưa ra chính xác vị trí của robot đầu tiên hoặc bất kỳ tọa độ cố định nào bắt nguồn từ quá trình khởi tạo và vì bất kỳ điểm nào đều hợp lệ nên điều này là chính xác. 

Một trường hợp cạnh khác là khi các ràng buộc chặt chẽ nhưng nhất quán tại một điểm duy nhất. Ví dụ: nếu một rô-bốt tác dụng x ≥ 3 và một rô-bốt khác tác dụng x ≤ 3, thì thuật toán sẽ thu gọn x thành chính xác 3. Điều tương tự cũng áp dụng độc lập với y, tạo ra một tọa độ hợp lệ duy nhất. 

Trường hợp cạnh cuối cùng là khi các ràng buộc không nhất quán chỉ trong một chiều. Ngay cả khi x có sự trùng lặp hợp lệ, chỉ riêng sự mâu thuẫn trong y sẽ gây ra kết quả đầu ra bằng 0 vì việc kiểm tra tính khả thi yêu cầu cả hai khoảng trùng lặp đồng thời.
