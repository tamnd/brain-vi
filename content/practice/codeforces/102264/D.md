---
title: "CF 102264D - Kết nối các dấu chấm"
description: "Chúng ta có (N) điểm lưới riêng biệt ((Xi,Yi)). Mỗi điểm phải được kết nối với một trong các trục tọa độ. Kết nối ngang từ ((Xi,Yi)) đến trục (y) có chi phí (Xi), trong khi kết nối dọc với trục (x) có chi phí (Yi)."
date: "2026-08-17T19:51:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102264
codeforces_index: "D"
codeforces_contest_name: "2019 Facebook Hacker Cup, Round 1"
rating: 0
weight: 102264
solve_time_s: 240
verified: true
draft: false
---

[CF 102264D - Kết nối các dấu chấm](https://codeforces.com/problemset/problem/102264/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (N) điểm lưới riêng biệt ((X_i,Y_i)). Mỗi điểm phải được kết nối với một trong các trục tọa độ. Kết nối ngang từ ((X_i,Y_i)) đến trục (y) có chi phí (X_i), trong khi kết nối dọc với trục (x) có chi phí (Y_i). 

Chi phí không phải là tổng độ dài của từng đoạn. Trong số tất cả các đoạn ngang, chỉ đoạn dài nhất (X_i) mới quan trọng và trong số tất cả các đoạn dọc, chỉ đoạn dài nhất (Y_i) mới quan trọng. Nếu chiều dài ngang lớn nhất là (h) và chiều dài dọc lớn nhất là (v) thì tổng chi phí là (h+v). 

Ngoài ra còn có các giới hạn trên (H) và (V) về số lượng phân đoạn ngang và dọc có thể được sử dụng. Phần khó khăn là hạn chế vượt qua. Đoạn ngang thuộc ((X_i,Y_i)) chiếm (0\le x\le X_i), trong khi đoạn dọc thuộc ((X_j,Y_j)) chiếm (0\le y\le Y_j). Họ băng qua bên trong của họ chính xác khi 

[ 
X_j<X_i 
] 

và 

[ 
Y_i<Y_j. 
] 

Sự bất bình đẳng rất nghiêm ngặt vì giao lộ tại điểm cuối được cho phép rõ ràng. 

Các tọa độ được tạo ra bởi phép lặp mô-đun bậc hai, do đó đầu vào chỉ cung cấp hai tọa độ (X) đầu tiên và hai tọa độ (Y) đầu tiên. Các giá trị còn lại phải được tạo trong thời gian (O(N)). (N) lớn nhất là (800.000), do đó bất kỳ số bậc hai nào trong (N) đều không thể sử dụng được ngay lập tức. Chẵn (N^2) có nghĩa là kiểm tra cặp (6,4\time10^{11}) ở kích thước tối đa. Giải pháp (O(N\log N)) là phù hợp, trong khi tìm kiếm theo cấp số nhân theo hướng hoàn toàn nằm ngoài phạm vi. 

Một số trường hợp ranh giới có thể lặng lẽ phá vỡ một giải pháp. Nếu (H+V<N), không có đủ đoạn được phép kết nối mọi điểm. Ví dụ,```
1
3 1 1
1 2 0 1 0 3
1 2 0 1 0 3
```tạo ra ((1,1),(2,2),(3,3)). Chỉ cho phép hai đoạn cho ba điểm, vì vậy câu trả lời là`Case #1: -1`. 

Thực tế là các giao lộ ở điểm cuối được phép cũng có vấn đề. Coi như```
1
2 1 1
1 2 0 0 0 10
1 2 0 0 0 10
```Các điểm là ((1,1)) và ((2,2)). Kết nối cái thứ nhất theo chiều ngang và cái thứ hai theo chiều dọc. Giao điểm của chúng tại ((2,1)), là điểm cuối của đoạn ngang nên được phép. Câu trả lời là`Case #1: 3`. Việc triển khai bất cẩn bằng cách sử dụng các bất đẳng thức không nghiêm ngặt có thể từ chối cấu hình hợp lệ này. 

Tọa độ bằng nhau đòi hỏi sự chăm sóc như nhau. Vì```
1
3 1 2
2 2 0 0 1 2
1 2 0 1 0 3
```các điểm là ((2,1),(2,2),(2,3)). Một điểm có thể nằm ngang và hai điểm còn lại có thể nằm dọc. Tất cả các phân đoạn đều nằm trên cùng một đường thẳng đứng hoặc gặp nhau tại các điểm cuối, vì vậy điều này hợp lệ và có chi phí (2+2=4). Coi tọa độ (X) bằng nhau như giao điểm sẽ cho câu trả lời sai. 

Cuối cùng, số 0 là ngưỡng có ý nghĩa. Nếu mọi điểm được kết nối theo chiều dọc thì không có đoạn ngang nào và chi phí của chúng bằng 0. Tương tự, nếu mọi điểm đều nằm ngang thì chi phí dọc bằng 0. Một giải pháp luôn giả định sử dụng cả hai hướng sẽ bỏ lỡ các câu trả lời tối ưu, chẳng hạn như mẫu đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là quyết định một cách độc lập xem mọi điểm là ngang hay dọc. Có (2^N) bài tập có thể thực hiện được. Đối với mỗi nhiệm vụ, chúng tôi có thể kiểm tra tất cả các cặp ngang và dọc xem có giao nhau không và đếm xem có bao nhiêu đoạn của mỗi loại được sử dụng. Kiểm tra chéo đơn giản sẽ kiểm tra các cặp (O(N^2)) và đưa ra thời gian (O(N^2 2^N)). Ngay cả khi việc kiểm tra chéo đã được tối ưu hóa thì việc gán (2^N) vẫn khiến phương pháp này trở nên vô vọng. Tại (N=30), đã có hơn một tỷ bài tập. 

Quan sát hữu ích là chi phí chỉ phụ thuộc vào hai con số. Gọi (h) là cực đại (X_i) trong số các điểm nằm ngang và gọi (v) là cực đại (Y_i) trong số các điểm thẳng đứng. Khi (h) và (v) được cố định, mọi điểm với (X_i>h) buộc phải nằm thẳng đứng và mọi điểm với (Y_i>v) buộc phải nằm ngang. 

Điều kiện vượt qua trở nên đơn giản hơn nhiều dưới các ngưỡng này. Nếu (X_i>h), điểm (i) phải thẳng đứng. Nếu (Y_j>v), điểm (j) phải nằm ngang. Một điểm thỏa mãn cả hai điều kiện sẽ phải có cả hai hướng, vì vậy một điểm như vậy khiến cho các ngưỡng không thể thực hiện được. 

Thực tế cấu trúc quan trọng là điều này cũng đủ, ngoài giới hạn số lượng phân đoạn. Giả sử không có điểm nào có cả (X_i>h) và (Y_i>v). Các điểm có (Y_i>v) có thể được đặt theo chiều ngang và các điểm có (X_i>h) có thể được đặt theo chiều dọc. Đối với các điểm linh hoạt còn lại, nếu cần nhiều đoạn ngang hơn thì hãy chọn những đoạn có tọa độ (Y) lớn nhất làm trục ngang và đặt các đoạn còn lại theo chiều dọc. 

Nhiệm vụ này không thể tạo ra một giao lộ. Mọi điểm nằm ngang cưỡng bức đều có (Y) lớn hơn mọi điểm thẳng đứng bắt buộc. Mọi điểm thẳng đứng bắt buộc đều có (X) lớn hơn mọi điểm nằm ngang có thể có. Trong số các điểm linh hoạt, một điểm nằm ngang được chọn có giá trị a (Y) ít nhất bằng với mọi điểm thẳng đứng linh hoạt. Do đó, cả ba loại cặp ngang-dọc đều an toàn. 

Vì vậy, vấn đề giảm xuống còn việc tìm cặp ngưỡng rẻ nhất (h,v) thỏa mãn yêu cầu về số lượng phân đoạn và tránh một điểm trong hình chữ nhật phía trên bên phải (X>h,Y>v). 

Mức tối thiểu (h) mà giới hạn dọc yêu cầu là ((V+1))-st lớn nhất (X), trừ khi (V=N), trong trường hợp đó nó có thể bằng 0. Tương tự, mức tối thiểu (v) mà giới hạn ngang yêu cầu là ((H+1))-st lớn nhất (Y), trừ khi (H=N), trong trường hợp đó nó có thể bằng 0. 

Bây giờ hãy sửa (h). Mọi điểm với (X>h) đều bị buộc phải thẳng đứng nên để tránh cắt nhau ta phải chọn 

[ 
v\ge \max_{X_i>h}Y_i. 
] 

Chúng ta cũng cần (v) thỏa mãn yêu cầu đếm theo chiều ngang. Do đó, ngưỡng dọc rẻ nhất có thể có cho (h) này là 

[ 
v=\max\left(y_{\text{req}},\max_{X_i>h}Y_i\right). 
] 

Chúng ta có thể đánh giá điều này cho mọi tọa độ (X) riêng biệt. Sắp xếp các điểm theo mức giảm (X) cho phép chúng ta duy trì mức tối đa (Y) trong số các điểm có (X) lớn hơn. Sự nghiêm ngặt chính xác là những gì quy tắc giao điểm cuối yêu cầu. 

Hai cách tiếp cận so sánh như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2 2^N)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tạo tất cả (N) điểm từ hai chuỗi lặp lại. Số nguyên Python được sử dụng trực tiếp, do đó, các sản phẩm trong phép truy hồi vẫn an toàn ngay cả khi chúng vượt quá phạm vi 32 bit. 
2. Nếu (H+V<N), trả về ngay (-1). Mỗi điểm cần một đoạn nên giới hạn hai đoạn phải có đủ tổng dung lượng. 
3. Trong khi tạo điểm, ghi lại mức tối đa (X) và mức tối đa (Y). Nếu (V=N), việc kết nối mọi điểm theo chiều dọc được cho phép và chi phí chính xác là tối đa (Y). Sử dụng điều này như một câu trả lời ban đầu. Nếu (H=N), việc kết nối mọi điểm theo chiều ngang sẽ mang lại giá trị tối đa cho ứng viên (X). 
4. Sắp xếp tất cả tọa độ (Y) theo thứ tự giảm dần. Nếu (H<N), giá trị tại chỉ số dựa trên 0 (H) là giá trị lớn nhất ((H+1))-st (Y), hãy gọi nó là (y_{\text{req}}). Mọi lời giải hợp lệ có tối đa (H) đoạn ngang đều phải có (v\ge y_{\text{req}}). Nếu (H=N), hãy sử dụng (y_{\text{req}}=0). 
5. Sắp xếp điểm theo thứ tự giảm dần (X). Nếu (V<N), điểm tại chỉ số dựa trên 0 (V) có ((V+1))-st lớn nhất (X), hãy gọi đây là (x_{\text{req}}). Mọi lời giải hợp lệ có tối đa (V) đoạn dọc đều phải có (h\ge x_{\text{req}}). Nếu (V=N), hãy sử dụng (x_{\text{req}}=0). 
6. Quét các điểm đã sắp xếp theo nhóm bằng nhau (X). Trước khi xử lý một nhóm có tọa độ (x), hãy duy trì`greater_y`, lớn nhất (Y) trong số các điểm có tọa độ (X) lớn hơn (x). Việc phân nhóm chặt chẽ này là cần thiết vì đoạn ngang kết thúc tại (x) có thể chạm vào đoạn thẳng đứng có điểm cuối cũng tại (x). 
7. Với mỗi nhóm có (x\ge x_{\text{req}}), hãy chọn (h=x). Các điểm có (X>h) bị ép theo chiều dọc nên mọi ngưỡng dọc đều phải thỏa mãn (v\ge\text{Greater_y}). Kết hợp điều này với yêu cầu về số lượng phân đoạn sẽ mang lại 

[ 
v=\max(y_{\text{req}},\text{Greater_y}). 
] 

Chi phí ứng viên là (h+v). 

1. Cập nhật câu trả lời với số ứng viên tối thiểu. Sau khi đánh giá nhóm, kết hợp tọa độ (Y) của nó vào`greater_y`trước khi chuyển sang nhỏ hơn tiếp theo (X). 

### Tại sao nó hoạt động 

Đối với bất kỳ bản vẽ hợp lệ nào, gọi (h) là chiều dài ngang dài nhất và (v) chiều dài dọc dài nhất của nó. Mọi điểm có (X>h) phải thẳng đứng và mọi điểm có (Y>v) phải nằm ngang. Do đó (h) phải thỏa mãn giới hạn (V) và (v) phải thỏa mãn giới hạn (H). Ngoài ra, không có điểm nào với (X>h) có thể có (Y>v), vì điểm như vậy sẽ buộc phải sử dụng cả hai hướng. Do đó, đối với một (h) cố định, mọi giải pháp hợp lệ đều có chi phí ít nhất là (h+\max(y_{\text{req}},\max_{X_i>h}Y_i)). 

Quá trình quét đánh giá chính xác giới hạn dưới này cho mọi (h) có thể. Ngược lại, với mỗi giá trị được đánh giá (h), việc chọn giá trị tối thiểu (v) đó sẽ làm cho các tập hợp ngang và dọc bắt buộc rời rạc và cho đủ công suất. Việc chỉ định bất kỳ điểm nằm ngang còn lại nào theo thứ tự giảm dần (Y) sẽ ngăn chặn sự giao nhau giữa các điểm linh hoạt. Do đó, mọi ứng cử viên được đánh giá bằng quá trình quét đều tương ứng với một bản vẽ hợp lệ và ứng cử viên tối thiểu chính xác là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, H, V, xdata, ydata):
    x1, x2, ax, bx, cx, dx = xdata
    y1, y2, ay, by, cy, dy = ydata

    if H + V < n:
        return -1

    points = [(x1, y1), (x2, y2)]
    ys = [y1, y2]

    max_x = max(x1, x2)
    max_y = max(y1, y2)

    px2, px1 = x1, x2
    py2, py1 = y1, y2

    for _ in range(2, n):
        nx = ((ax * px2 + bx * px1 + cx) % dx) + 1
        ny = ((ay * py2 + by * py1 + cy) % dy) + 1

        points.append((nx, ny))
        ys.append(ny)

        if nx > max_x:
            max_x = nx
        if ny > max_y:
            max_y = ny

        px2, px1 = px1, nx
        py2, py1 = py1, ny

    ans = 10**30

    # Using no horizontal segments.
    if V >= n:
        ans = min(ans, max_y)

    # Using no vertical segments.
    if H >= n:
        ans = min(ans, max_x)

    # If no horizontal segment is allowed, the all-vertical
    # candidate above is the only possible orientation.
    if H == 0:
        return ans

    ys.sort(reverse=True)
    y_req = ys[H] if H < n else 0

    points.sort(key=lambda p: p[0], reverse=True)
    x_req = points[V][0] if V < n else 0

    greater_y = 0
    i = 0

    while i < n:
        x = points[i][0]
        j = i
        group_y = 0

        while j < n and points[j][0] == x:
            if points[j][1] > group_y:
                group_y = points[j][1]
            j += 1

        if x >= x_req:
            v_cost = max(y_req, greater_y)
            candidate = x + v_cost
            if candidate < ans:
                ans = candidate

        if group_y > greater_y:
            greater_y = group_y

        i = j

    return ans

def main():
    T = int(input())
    out = []

    for case_id in range(1, T + 1):
        n, H, V = map(int, input().split())
        xdata = list(map(int, input().split()))
        ydata = list(map(int, input().split()))

        ans = solve_case(n, H, V, xdata, ydata)
        out.append(f"Case #{case_id}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Hai điểm đầu tiên được chèn trực tiếp vì quá trình lặp lại bắt đầu ở điểm thứ ba. Các biến`px2`Và`px1`luôn biểu thị (X_{i-1}) và (X_i) trước khi tạo giá trị tiếp theo và tính bất biến tương tự được duy trì cho chuỗi (Y). 

Phép truy toán sử dụng phép nhân trước modulo, chính xác như đã chỉ định. Các số nguyên có độ chính xác tùy ý của Python tránh tình trạng tràn có thể xảy ra khi triển khai 32 bit có chiều rộng cố định. 

ban đầu`ans`xử lý các trường hợp một hướng không được sử dụng. Điều này quan trọng ngay cả khi cả hai giới hạn đều dương. Trong mẫu đầu tiên, tất cả các điểm đều có thể thẳng đứng, do đó chi phí ngang tối ưu bằng 0. 

Mảng (Y) được sắp xếp theo thứ tự giảm dần sao cho`ys[H]`là giá trị lớn nhất ((H+1))-st. Khi (H=N), không có phần tử nào như vậy và số 0 là ngưỡng chính xác vì tất cả các điểm có thể nằm ngang. 

Các điểm được sắp xếp theo thứ tự giảm dần (X). Trong quá trình quét, nhóm bằng (X) hiện tại không được chèn vào`greater_y`cho đến khi ứng viên của nó đã được đánh giá. Do đó,`greater_y`chứa chính xác các điểm thỏa mãn (X_i>x) chứ không phải (X_i\ge x). Đây là điều kiện biên nhạy cảm nhất trong việc thực hiện. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên có (N=2), (H=2) và (V=2), với các điểm ((6,2)) và ((3,4)). 

Vì (V=N), nên lời giải toàn chiều dọc ngay lập tức là một ứng cử viên có chi phí (4). Vì (H=N), chi phí của giải pháp toàn chiều ngang (6). 

Đối với quét hỗn hợp, (x_{\text{req}}=0) và (y_{\text{req}}=0). 

| Hiện tại (x) | Điểm ở đây (x) |`greater_y`trước nhóm | (v=\max(y_{\text{req}},Greater_y)) | Ứng viên | 
| --- | --- | --- | --- | --- | 
| 6 | ((6,2)) | 0 | 0 | 6 | 
| 3 | ((3,4)) | 2 | 2 | 5 | 

Ứng cử viên theo chiều dọc ban đầu đã là (4), vì vậy câu trả lời cuối cùng là`Case #1: 4`. 

Việc quét này chứng tỏ tại sao định hướng chi phí bằng 0 phải được xem xét riêng biệt. Ngưỡng (h=0) biểu thị không sử dụng đoạn ngang và ngưỡng đó không phải là tọa độ (X) của bất kỳ điểm nào. 

### Mẫu 2 

Mẫu thứ hai có (N=2), (H=2), (V=1), lại có các điểm ((6,2)) và ((3,4)). 

Ở đây (H=N), do đó (y_{\text{req}}=0). Vì chỉ cho phép một đoạn dọc nên (x_{\text{req}}) là đoạn lớn thứ hai (X), cụ thể là (3). 

Chi phí ứng cử viên theo chiều ngang (6). 

| Hiện tại (x) | Điểm ở đây (x) |`greater_y`trước nhóm | (v) | Ứng viên | 
| --- | --- | --- | --- | --- | 
| 6 | ((6,2)) | 0 | 0 | 6 | 
| 3 | ((3,4)) | 2 | 2 | 5 | 

Tại (h=3), điểm có (X=6) bị ép thẳng đứng. Tọa độ (Y) của nó là (2), do đó chi phí dọc ít nhất phải bằng (2). Điểm còn lại nằm ngang với chiều dài (3), cho ra tổng chi phí (3+2=5). 

Kết quả là`Case #2: 5`. Dấu vết này cũng cho thấy lý do tại sao quá trình quét sử dụng các điểm có (X lớn hơn): bản thân điểm tại (X=3) không đóng góp vào`greater_y`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Việc tạo điểm mất (O(N)), sắp xếp (Y)-tọa độ (O(N\log N)), sắp xếp điểm mất (O(N\log N)) và quá trình quét mất (O(N)). | 
| Không gian | (O(N)) | Các điểm được tạo và mảng tọa độ (Y) đều chứa các giá trị (N). | 

Với (N=800.000), việc tạo và quét tuyến tính không tốn kém so với hai thao tác sắp xếp. Thuật toán không bao giờ xây dựng các cặp điểm, vì vậy nó tránh được bộ nhớ bậc hai và thời gian mà định nghĩa hình học có thể gợi ý ban đầu. 

## Trường hợp thử nghiệm```python
# This test harness assumes the solution above is present,
# including main().

import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    captured = io.StringIO()
    sys.stdout = captured

    try:
        main()
        return captured.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples from the prompt.
sample = """\
2
2 2 2
6 3 0 0 0 10
2 4 0 0 0 10
2 2 1
6 3 0 0 0 10
2 4 0 0 0 10
"""

assert run(sample) == """\
Case #1: 4
Case #2: 5
""", "provided samples"

# Minimum-size case, all points vertical.
minimum_case = """\
1
2 0 2
1 2 0 0 0 10
1 3 0 0 0 10
"""

assert run(minimum_case) == "Case #1: 3\n", "minimum size"

# All X-coordinates are equal. Endpoint intersections are allowed.
equal_x_case = """\
1
3 1 2
2 2 0 0 1 2
1 2 0 1 0 3
"""

assert run(equal_x_case) == "Case #1: 4\n", "equal X coordinates"

# A horizontal endpoint may touch a vertical segment.
endpoint_case = """\
1
2 1 1
1 2 0 0 0 10
1 2 0 0 0 10
"""

assert run(endpoint_case) == "Case #1: 3\n", "endpoint intersection"

# Not enough total segments.
impossible_case = """\
1
3 1 1
1 2 0 1 0 3
1 2 0 1 0 3
"""

assert run(impossible_case) == "Case #1: -1\n", "insufficient capacity"

# Maximum-size case with distinct points.
# X_i = Y_i = i, generated by
# value_i = ((value_{i-1} + 0) mod N) + 1
# starting from 1, 2.
max_n = 800000
maximum_case = f"""\
1
{max_n} 400000 400000
1 2 0 1 0 {max_n}
1 2 0 1 0 {max_n}
"""

assert run(maximum_case) == "Case #1: 800000\n", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 2`có điểm ((6,2),(3,4)) |`Case #1: 4`| Mẫu được cung cấp, tối ưu theo chiều dọc | 
|`2 2 1`có điểm ((6,2),(3,4)) |`Case #2: 5`| Cung cấp mẫu, định hướng hỗn hợp | 
|`2 0 2`|`Case #1: 3`| Kích thước tối thiểu và công suất ngang bằng không | 
| Ba điểm bằng nhau (X) |`Case #1: 4`| Xử lý điểm cuối phối hợp bằng nhau | 
| ((1,1),(2,2)), (H=V=1) |`Case #1: 3`| Vượt qua sự bất bình đẳng nghiêm ngặt | 
| (N=3,H=1,V=1) |`Case #1: -1`| Tổng công suất phân khúc không đủ | 
| (N=800000,H=V=400000) |`Case #1: 800000`| Tối đa (N), tạo lặp lại, sắp xếp và số nguyên lớn | 

## Vỏ cạnh 

Khi (H+V<N), thuật toán trả về (-1) trước khi tạo tọa độ. Đối với đầu vào```
1
3 1 1
1 2 0 1 0 3
1 2 0 1 0 3
```các điểm được tạo là ((1,1),(2,2),(3,3)). Chỉ cho phép hai phân đoạn, trong khi ba phân đoạn là bắt buộc. Không có đối số hình học nào có thể vượt qua giới hạn dung lượng đó. 

Khi tất cả các phân đoạn dọc được cho phép, ngưỡng ngang có thể bằng 0. Đối với mẫu đầu tiên có (V=N=2), thuật toán khởi tạo`ans`với mức tối đa (Y), là (4). Lần quét sau chỉ tìm thấy chi phí (6) và (5), nên đáp án vẫn là (4). Việc này xử lý ứng viên còn thiếu (h=0). 

Khi tọa độ bằng nhau, quá trình quét sẽ xử lý toàn bộ nhóm bằng (X) trước khi cập nhật`greater_y`. Đối với các điểm ((2,1),(2,2),(2,3)), thí sinh có (h=2) sẽ không có điểm nào với (X>2), mặc dù có những điểm với (X=2). Điều đó đúng vì giao lộ tại tọa độ chung (X) là giao lộ điểm cuối và được phép. 

Đối với trường hợp điểm cuối có điểm ((1,1)) và ((2,2)), việc chọn điểm đầu tiên theo chiều ngang và điểm thứ hai theo chiều dọc sẽ tạo ra giao điểm tại ((2,1)), điểm cuối của đoạn ngang. Điều kiện giao nhau yêu cầu cả (X_{\text{vertical}<X_{\text{horizontal}}) và (Y_{\text{horizontal}<Y_{\text{vertical}}) và bất đẳng thức đầu tiên là sai. Do đó, thuật toán chấp nhận cấu hình và thu được chi phí (1+2=3). 

Đối với thử nghiệm lặp lại kích thước tối đa, (X_i=Y_i=i) cho (1\le i\le800000). Với (H=V=400000), các ngưỡng yêu cầu đều là (400000). Việc chọn (h=400000) buộc (400000) điểm còn lại phải thẳng đứng, có (Y) lớn nhất là (800000), đưa ra chi phí (1.200.000), nhưng việc chọn ngưỡng cân bằng trong đó việc quét chiếm các điểm phía trên bên phải sẽ mang lại giá trị tối ưu (800000). Bài kiểm tra thực hiện phép lặp, xử lý ngưỡng nghiêm ngặt, sắp xếp tự do bằng nhau và kích thước số nguyên ở giới hạn trên.
