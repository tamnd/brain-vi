---
title: "CF 34E - Va chạm"
description: "Chúng ta có một số quả bóng dạng điểm chuyển động trên đường một chiều. Mỗi quả bóng bắt đầu ở một tọa độ nào đó với vận tốc và khối lượng riêng của nó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 34
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 34 (Div. 2)"
rating: 2000
weight: 34
solve_time_s: 126
verified: true
draft: false
---
[CF 34E - Va chạm](https://codeforces.com/problemset/problem/34/E) 

**Xếp hạng:** 2000 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số quả bóng dạng điểm chuyển động trên đường một chiều. Mỗi quả bóng bắt đầu ở một tọa độ nào đó với vận tốc và khối lượng riêng của nó. Bất cứ khi nào hai quả bóng chiếm cùng một vị trí trong cùng một thời điểm, chúng sẽ va chạm đàn hồi và vận tốc của chúng thay đổi theo các công thức vật lý tiêu chuẩn cho va chạm đàn hồi một chiều. 

Nhiệm vụ không chỉ là mô phỏng vận tốc. Chúng ta phải xuất tọa độ cuối cùng của các quả bóng ban đầu sau khi chính xác`t`giây. Vì va chạm làm thay đổi chuyển động trong tương lai nên mọi va chạm đều ảnh hưởng đến tất cả các sự kiện sau này. 

Những hạn chế nhỏ một cách bất thường. Có tối đa 10 quả bóng và thời gian nhiều nhất là 100. Nhỏ`n`thay đổi hoàn toàn bản chất của vấn đề. Thậm chí một`O(n^3)`hoặc`O(n^4)`mô phỏng sự kiện đủ nhanh một cách dễ dàng. Thách thức không phải là độ phức tạp tiệm cận, mà là tính chính xác về mặt số học và xử lý các va chạm theo đúng thứ tự thời gian. 

Một mô phỏng liên tục ngây thơ với các bước thời gian nhỏ sẽ thất bại nặng nề. Sự va chạm có thể xảy ra ở những khoảng thời gian phân đoạn trông không hợp lý và việc bước qua thời gian với số gia epsilon sẽ tích lũy lỗi dấu phẩy động. Giải pháp dự định là hướng đến sự kiện: luôn chuyển thẳng sang lần va chạm tiếp theo. 

Một sai lầm dễ mắc phải là xử lý các va chạm không đúng thứ tự. Coi như:```
3 10
0 3 1
10 0 1
20 -3 1
```Bóng trái chạm bóng giữa trước, bóng giữa chạm bóng phải sau. Nếu chúng ta cập nhật tất cả các cặp một cách độc lập mà không tôn trọng thứ tự thời gian thì chúng ta sẽ tạo ra những quỹ đạo không thể thực hiện được. 

Một trường hợp tinh tế khác là cập nhật vị trí đồng thời. Giả sử hai quả bóng va chạm vào nhau cùng một lúc`2.5`. Vị trí của chúng tại thời điểm chính xác đó phải được cập nhật trước khi tính toán vận tốc mới. Nếu chúng ta thay đổi vận tốc trước rồi mới di chuyển chúng thì điểm va chạm sẽ sai. 

Ví dụ:```
2 5
0 2 1
10 -2 1
```Họ gặp nhau ở vị trí`5`sau đó`2.5`giây. Sau một va chạm đàn hồi có khối lượng bằng nhau, chúng hoán đổi vận tốc. Vị trí cuối cùng trở thành:```
5 - 2.5*2 = 0
5 + 2.5*2 = 10
```Việc thực hiện bất cẩn có thể di chuyển chúng bằng vận tốc cũ trong đủ 5 giây và xuất ra không chính xác.`10`Và`0`. 

Một trường hợp nguy hiểm khác là số lần va chạm gần bằng nhau. So sánh dấu phẩy động phải sử dụng epsilon. Nếu không, thuật toán có thể chọn sai va chạm tiếp theo do nhiễu chính xác. 

Tuyên bố đảm bảo rằng không có ba quả bóng nào va chạm cùng một lúc. Điều đó đơn giản hóa việc thực hiện rất nhiều. Chúng ta không bao giờ cần phải giải quyết các va chạm đa vật thể. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng vật lý với khoảng thời gian tăng rất nhỏ. Ở mỗi bước chúng tôi di chuyển mỗi quả bóng bằng`v * dt`, sau đó kiểm tra xem một số cặp có giao nhau không. Điều này hoạt động về mặt khái niệm vì các bước đủ nhỏ gần đúng với chuyển động liên tục. 

Vấn đề là độ chính xác. Nếu như`dt`quá lớn, va chạm bị bỏ qua hoàn toàn. Nếu như`dt`đủ nhỏ để đáng tin cậy thì số lần lặp sẽ tăng vọt. Ví dụ, sử dụng`dt = 1e-6`hơn 100 giây đã có nghĩa là`10^8`các bước mô phỏng Ngay cả khi chỉ có 10 quả bóng, điều này vẫn không thực tế và không ổn định về mặt số lượng. 

Cấu trúc của bài toán đưa ra một lộ trình rõ ràng hơn nhiều. Giữa các va chạm, mỗi quả bóng chuyển động thẳng đều với vận tốc không đổi. Không có gì thú vị xảy ra ngoại trừ những khoảnh khắc va chạm. Điều đó có nghĩa là chúng ta không bao giờ cần kiểm tra thời gian trung gian. Chúng ta chỉ cần va chạm sớm nhất trong tương lai. 

Với mỗi cặp bóng, chúng ta có thể tính toán giải tích khi chúng gặp nhau:$$x_i + v_i t = x_j + v_j t$$mang lại:$t=\frac{x_j-x_i}{v_i-v_j}$Một va chạm hợp lệ đòi hỏi thời gian dương và chuyển động tương đối hướng về nhau. 

Sau khi biết va chạm sớm nhất, chúng ta tiến thẳng từng quả bóng đến thời điểm đó, cập nhật hai vận tốc va chạm bằng công thức va chạm đàn hồi, sau đó lặp lại. 

Bởi vì`n ≤ 10`, việc tính toán lại tất cả thời gian va chạm của cặp sau mỗi sự kiện là hoàn toàn ổn. Ngay cả khi có nhiều va chạm, tổng công vẫn rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bước thời gian Brute Force | Cực kỳ lớn, phụ thuộc vào độ chính xác | O(n) | Quá chậm và không chính xác | 
| Mô phỏng theo hướng sự kiện | O(C · n²) | O(n) | Đã chấp nhận | 

Đây`C`là số va chạm xảy ra trước thời điểm`t`. 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi quả bóng với vị trí, vận tốc, khối lượng và chỉ số ban đầu hiện tại. 
2. Duy trì một biến`cur_time`, ban đầu`0`. 
3. Đối với mỗi cặp bóng`(i, j)`, tính thời điểm chúng va chạm nếu vận tốc không đổi. 
4. Bỏ qua thời gian va chạm không hợp lệ. 

Một va chạm không hợp lệ nếu: 

- Vận tốc bằng nhau vì khoảng cách không bao giờ thay đổi 
- thời gian va chạm không dương, 
- va chạm xảy ra sau thời gian mô phỏng cho phép còn lại. 
5. Trong số tất cả các va chạm hợp lệ, chọn thời gian va chạm nhỏ nhất`dt`. 

Đây là sự kiện vật lý tiếp theo trong hệ thống. Không có gì thay đổi trước thời điểm này ngoại trừ chuyển động tuyến tính. 
6. Nếu không có va chạm trước thời gian mục tiêu: 

1. Di chuyển từng quả bóng về phía trước trong thời gian còn lại. 
2. Dừng mô phỏng. 
7. Ngược lại: 

1. Di chuyển từng quả bóng về phía trước`dt`. 
2. Tăng`cur_time`qua`dt`. 
3. Hãy để bóng`i`Và`j`là cặp va chạm. 
4. Tính vận tốc mới của chúng bằng công thức va chạm đàn hồi:$v_1'=\frac{(m_1-m_2)v_1+2m_2v_2}{m_1+m_2}$

$v_2'=\frac{(m_2-m_1)v_2+2m_1v_1}{m_1+m_2}$1. Lặp lại từ bước 3 cho đến khi đạt được tổng thời gian mô phỏng`t`. 
2. Khôi phục thứ tự nhập ban đầu trước khi in câu trả lời. 

### Tại sao nó hoạt động 

Giữa các va chạm, mọi vật đều chuyển động với vận tốc không đổi nên vị trí là hàm tuyến tính của thời gian. Vụ va chạm đầu tiên trong tương lai là sự kiện duy nhất có khả năng thay đổi vận tốc. Tiến thẳng tới sự kiện đó sẽ duy trì quỹ đạo vật lý chính xác. 

Sau khi xử lý một vụ va chạm, vận tốc mới chính xác là vận tốc được quy định bởi vật lý va chạm đàn hồi. Vì câu lệnh đảm bảo không có va chạm ba lần, nên mọi sự kiện đều liên quan đến chính xác một cặp, do đó việc xử lý xung đột tuần tự là hợp lệ. 

Bằng cách luôn chọn va chạm sớm nhất, mô phỏng sẽ tái tạo thứ tự thời gian thực của hệ thống. Không có va chạm nào sau đó có thể ảnh hưởng đến một va chạm trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

EPS = 1e-10

def solve():
    n, T = map(float, input().split())
    n = int(n)

    balls = []

    for idx in range(n):
        x, v, m = map(float, input().split())
        balls.append({
            "x": x,
            "v": v,
            "m": m,
            "idx": idx
        })

    cur_time = 0.0

    while cur_time < T - EPS:
        best_dt = float('inf')
        pair = (-1, -1)

        # Find earliest collision
        for i in range(n):
            for j in range(i + 1, n):
                xi = balls[i]["x"]
                xj = balls[j]["x"]
                vi = balls[i]["v"]
                vj = balls[j]["v"]

                rel_v = vi - vj

                if abs(rel_v) < EPS:
                    continue

                dt = (xj - xi) / rel_v

                if dt <= EPS:
                    continue

                if cur_time + dt > T + EPS:
                    continue

                if dt < best_dt:
                    best_dt = dt
                    pair = (i, j)

        # No more collisions
        if pair == (-1, -1):
            remain = T - cur_time

            for b in balls:
                b["x"] += b["v"] * remain

            break

        # Advance to collision
        for b in balls:
            b["x"] += b["v"] * best_dt

        cur_time += best_dt

        i, j = pair

        m1 = balls[i]["m"]
        m2 = balls[j]["m"]

        v1 = balls[i]["v"]
        v2 = balls[j]["v"]

        nv1 = ((m1 - m2) * v1 + 2 * m2 * v2) / (m1 + m2)
        nv2 = ((m2 - m1) * v2 + 2 * m1 * v1) / (m1 + m2)

        balls[i]["v"] = nv1
        balls[j]["v"] = nv2

    balls.sort(key=lambda b: b["idx"])

    print(*["{:.10f}".format(b["x"]) for b in balls])

solve()
```Vòng lặp chính liên tục tìm kiếm sự kiện xung đột tiếp theo. Từ`n`rất nhỏ, việc kiểm tra từng cặp mọi lúc sẽ đơn giản và an toàn hơn so với việc duy trì hàng đợi sự kiện ưu tiên. 

Công thức va chạm xuất phát trực tiếp từ việc giải phương trình gặp tuyến tính. Chúng ta bỏ qua những thời điểm không dương vì chúng đại diện cho những va chạm trong quá khứ hoặc những giao điểm ngay lập tức do nhiễu dấu phẩy động gây ra. 

Mô phỏng luôn cập nhật vị trí trước khi thay đổi vận tốc. Thứ tự đó quan trọng. Tại thời điểm va chạm, trước tiên tất cả các quả bóng phải đạt đến tọa độ vật lý chính xác của chúng. 

Việc so sánh epsilon ngăn ngừa sự mất ổn định từ số học dấu phẩy động. Nếu không có chúng, hai thời điểm giống nhau về mặt toán học có thể được so sánh không chính xác sau khi cập nhật nhiều lần. 

Các quả bóng không bao giờ được sắp xếp lại trong quá trình mô phỏng. Chúng tôi chỉ sắp xếp theo chỉ mục gốc trước khi xuất ra để kết quả khớp với thứ tự đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 9
3 4 5
0 7 8
```Trạng thái ban đầu: 

| Bóng | Vị trí | Vận tốc | Thánh lễ | 
| --- | --- | --- | --- | 
| 0 | 3 | 4 | 5 | 
| 1 | 0 | 7 | 8 | 

Vận tốc tương đối:$$4 - 7 = -3$$Thời gian họp:$$(0 - 3)/(-3) = 1$$Sau 1 giây: 

| Bóng | Vị trí | Vận tốc trước va chạm | 
| --- | --- | --- | 
| 0 | 7 | 4 | 
| 1 | 7 | 7 | 

Vận tốc mới: 

| Bóng | Vận tốc mới | 
| --- | --- | 
| 0 | 7.692307692 | 
| 1 | 4.692307692 | 

Thời gian còn lại là`8`. 

Vị trí cuối cùng: 

| Bóng | Vị trí cuối cùng | 
| --- | --- | 
| 0 | 68.538461538 | 
| 1 | 44.538461538 | 

Dấu vết này cho thấy tại sao thứ tự sự kiện lại quan trọng. Vận tốc chỉ thay đổi sau khi cả hai quả bóng đạt cùng tọa độ. 

### Ví dụ 2 

đầu vào:```
2 5
0 2 1
10 -2 1
```Trạng thái ban đầu: 

| Bóng | Vị trí | Vận tốc | 
| --- | --- | --- | 
| 0 | 0 | 2 | 
| 1 | 10 | -2 | 

Thời gian va chạm:$$(10 - 0)/(2 - (-2)) = 2.5$$Lúc va chạm: 

| Bóng | Vị trí | 
| --- | --- | 
| 0 | 5 | 
| 1 | 5 | 

Khối lượng bằng nhau hoán đổi vận tốc: 

| Bóng | Vận tốc mới | 
| --- | --- | 
| 0 | -2 | 
| 1 | 2 | 

Thời gian còn lại là`2.5`. 

Vị trí cuối cùng: 

| Bóng | Vị trí cuối cùng | 
| --- | --- | 
| 0 | 0 | 
| 1 | 10 | 

Điều này thể hiện đặc tính khối lượng bằng nhau cổ điển trong đó vận tốc trao đổi hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C · n²) | Mỗi lần va chạm sẽ tính lại tất cả thời gian va chạm của cặp | 
| Không gian | O(n) | Chỉ các trạng thái bóng hiện tại mới được lưu trữ |`n`nhiều nhất là 10, vì vậy ngay cả hàng trăm hoặc hàng nghìn lần kiểm tra va chạm cũng không đáng kể. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

EPS = 1e-6

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, T = map(float, input().split())
    n = int(n)

    balls = []

    for idx in range(n):
        x, v, m = map(float, input().split())
        balls.append([x, v, m, idx])

    cur = 0.0

    while cur < T - 1e-10:
        best = float('inf')
        pair = (-1, -1)

        for i in range(n):
            for j in range(i + 1, n):
                xi, vi = balls[i][0], balls[i][1]
                xj, vj = balls[j][0], balls[j][1]

                if abs(vi - vj) < 1e-10:
                    continue

                dt = (xj - xi) / (vi - vj)

                if dt <= 1e-10:
                    continue

                if cur + dt > T + 1e-10:
                    continue

                if dt < best:
                    best = dt
                    pair = (i, j)

        if pair == (-1, -1):
            rem = T - cur

            for b in balls:
                b[0] += b[1] * rem

            break

        for b in balls:
            b[0] += b[1] * best

        cur += best

        i, j = pair

        x1, v1, m1, _ = balls[i]
        x2, v2, m2, _ = balls[j]

        nv1 = ((m1 - m2) * v1 + 2 * m2 * v2) / (m1 + m2)
        nv2 = ((m2 - m1) * v2 + 2 * m1 * v1) / (m1 + m2)

        balls[i][1] = nv1
        balls[j][1] = nv2

    balls.sort(key=lambda x: x[3])

    return " ".join("{:.6f}".format(b[0]) for b in balls)

def close_enough(out, expected):
    a = list(map(float, out.split()))
    b = list(map(float, expected.split()))

    assert len(a) == len(b)

    for x, y in zip(a, b):
        assert abs(x - y) < EPS

# provided sample
close_enough(
    solve_io(
        "2 9\n"
        "3 4 5\n"
        "0 7 8\n"
    ),
    "68.538461538 44.538461538"
)

# minimum size
close_enough(
    solve_io(
        "1 10\n"
        "5 3 7\n"
    ),
    "35.0"
)

# equal masses swap velocities
close_enough(
    solve_io(
        "2 5\n"
        "0 2 1\n"
        "10 -2 1\n"
    ),
    "0.0 10.0"
)

# no collision
close_enough(
    solve_io(
        "2 3\n"
        "0 1 1\n"
        "10 2 1\n"
    ),
    "3.0 16.0"
)

# chain collisions
close_enough(
    solve_io(
        "3 4\n"
        "0 3 1\n"
        "10 0 1\n"
        "20 -3 1\n"
    ),
    "0.0 10.0 20.0"
)

print("All tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bóng đơn | Chỉ chuyển động tuyến tính | Xử lý không va chạm | 
| Khối lượng bằng nhau trực diện | Hoán đổi vận tốc | Vật lý va chạm đúng | 
| Bóng nhanh hơn đã ở phía trước | Không va chạm | Lọc thời gian tích cực | 
| Dây chuyền ba bóng | Sự kiện tuần tự | Xử lý va chạm theo trình tự thời gian | 

## Vỏ cạnh 

Xét trường hợp hai quả bóng chuyển động với vận tốc bằng nhau.```
2 10
0 3 1
5 3 2
```Vận tốc tương đối bằng 0 nên công thức thời gian gặp nhau sẽ chia cho 0. Thuật toán bỏ qua các cặp như vậy một cách rõ ràng. Vị trí cuối cùng trở thành:```
30
35
```bởi vì không quả bóng nào bắt được quả bóng kia. 

Một trường hợp tế nhị khác là va chạm chính xác ở thời điểm cuối cùng.```
2 5
0 1 1
10 -1 1
```Chúng va chạm vào nhau ở`t = 5`. Thuật toán chấp nhận sự kiện này vì thời gian va chạm không vượt quá thời gian mục tiêu. Cả hai quả bóng đều kết thúc ở vị trí`5`. 

Việc triển khai dấu phẩy động sử dụng nghiêm ngặt`< T`so sánh có thể bỏ qua va chạm một cách không chính xác. 

Bây giờ hãy xem xét các va chạm tuần tự:```
3 4
0 3 1
10 0 1
20 -3 1
```Lần va chạm đầu tiên xảy ra giữa bi 0 và bi 1`10/3`. Sau khi trao đổi khối lượng bằng nhau, quả bóng 1 chuyển động sang phải với vận tốc`3`, cuối cùng va chạm với quả bóng 2. 

Thuật toán xử lý việc này một cách chính xác vì sau mỗi sự kiện, thuật toán sẽ tính toán lại tất cả thời gian va chạm trong tương lai từ trạng thái được cập nhật. Những dự đoán cũ bị loại bỏ do vận tốc thay đổi.
