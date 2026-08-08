---
title: "CF 102550B - \u0410\u0442\u0430\u043a\u0443\u044e\u0449\u0438\u0435 \u043f\u0430\u0440\u044b"
description: "Sự cố mô tả một tập hợp các mặt hàng trong đó mỗi mặt hàng có một mức giá và một số cặp mặt hàng tương thích với nhau. Chúng ta cần tìm bộ ba món đồ rẻ nhất sao cho mỗi cặp trong số ba món đồ đó đều tương thích với nhau."
date: "2026-08-06T20:36:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102550
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2018-2019, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102550
solve_time_s: 63
verified: true
draft: false
---

[CF 102550B - \u0410\u0442\u0430\u043a\u0443\u044e\u0449\u0438\u0435 \u043f\u0430\u0440\u044b](https://codeforces.com/problemset/problem/102550/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả một tập hợp các mặt hàng trong đó mỗi mặt hàng có một mức giá và một số cặp mặt hàng tương thích với nhau. Chúng ta cần tìm bộ ba món đồ rẻ nhất sao cho mỗi cặp trong số ba món đồ đó đều tương thích với nhau. Theo thuật ngữ đồ thị, mọi mục đều là một đỉnh, mọi cặp tương thích là một cạnh và chúng ta cần tổng trọng số tối thiểu của một hình tam giác. Nếu không tồn tại tam giác thì đáp án là`-1`. 

Đầu vào cung cấp số lượng mặt hàng và số lượng cặp tương thích, theo sau là giá của từng mặt hàng và danh sách các cặp tương thích. Sản lượng là tổng giá tối thiểu có thể có của tất cả các bộ ba mặt hàng có mối liên hệ lẫn nhau. 

Các ràng buộc được thiết kế xoay quanh thực tế là việc kiểm tra trực tiếp từng bộ ba thường quá tốn kém. Nếu có`n`các mục, số lượng bộ ba có thể là`n * (n - 1) * (n - 2) / 6`, phát triển theo khối. Ngay cả đối với vài nghìn mục, số lượng này vẫn trở nên quá lớn, vì vậy chúng ta cần tránh liệt kê tất cả các nhóm gồm ba mục. 

Các trường hợp chính xảy ra do nhầm lẫn một đường đi với một hình tam giác hoặc do quên rằng cả ba cặp đều phải tồn tại. Ví dụ:```
3 2
2 3 4
1 2
2 3
```Câu trả lời là`-1`. Mặt hàng`1, 2, 3`được kết nối thành một chuỗi, nhưng các vật phẩm`1`Và`3`không tương thích. Một giải pháp bất cẩn chỉ kiểm tra xem một mục có hai hàng xóm hay không sẽ chấp nhận trường hợp này một cách sai lầm. 

Một trường hợp khác là khi tồn tại một tam giác hợp lệ nhưng có những cặp khác trông rẻ hơn nhưng không tạo thành tam giác.```
4 4
10 1 2 100
1 2
2 3
3 1
1 4
```Câu trả lời là`13`, sử dụng vật phẩm`1, 2, 3`. Mục thứ tư có các kết nối rẻ tiền tới một số đỉnh nhưng không thể thay thế thành viên tam giác. 

Trường hợp biên cuối cùng là khi đồ thị không có chu trình nào có độ dài bằng ba.```
4 3
5 5 5 5
1 2
2 3
3 4
```Câu trả lời là`-1`. Một chuỗi có thể chứa nhiều cạnh nhưng không có cặp cặp tấn công nào khép lại thành hình tam giác. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ kiểm tra mọi bộ ba mục có thể có. Đối với mỗi bộ ba`(i, j, k)`, chúng tôi kiểm tra xem ba cạnh tương thích bắt buộc có tồn tại hay không và nếu có, hãy cập nhật tổng giá tối thiểu của chúng. Điều này đúng vì mọi câu trả lời có thể đều được xem xét. Tuy nhiên, số lượng bộ ba là`O(n^3)`và mỗi bộ ba yêu cầu kiểm tra cạnh theo thời gian không đổi. Đối với các đồ thị lớn, điều này có nghĩa là phải thực hiện hàng tỷ thao tác, điều này là không khả thi. 

Quan sát hữu ích là một tam giác phải có ba đỉnh mà mỗi cặp được nối với nhau. Thay vì tạo ra tất cả các bộ ba, chúng ta có thể lặp qua các cạnh hiện có. Khi chúng ta nhìn vào một cạnh`(u, v)`, bất kỳ tam giác nào chứa cạnh này đều phải sử dụng một lân cận chung của`u`Và`v`. Giao điểm của danh sách kề của chúng cho ta chính xác các đỉnh thứ ba có thể có. 

Biểu đồ có thể có nhiều cạnh, vì vậy chúng ta cần thực hiện việc kiểm tra giao lộ này một cách hiệu quả. Chúng tôi lưu trữ các tập hợp kề, cho phép kiểm tra tư cách thành viên theo thời gian trung bình liên tục. Đối với mỗi cạnh, chúng tôi lặp qua tập kề cận nhỏ hơn và kiểm tra xem mỗi ứng cử viên có được kết nối với điểm cuối khác hay không. Mỗi người hàng xóm chung được phát hiện sẽ tạo thành một hình tam giác và chúng tôi cập nhật câu trả lời. 

Lực lượng vũ phu hoạt động vì mọi tam giác có thể đều được thử nghiệm, nhưng thất bại vì nó tạo ra quá nhiều bộ ba không cần thiết. Phương pháp giao kề chỉ kiểm tra các bộ ba gần giống với các tam giác, sử dụng cấu trúc đồ thị để tránh các kết hợp không thể xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm | 
| Nút giao liền kề | O(m * sqrt(m)) trung bình cho đồ thị thưa thớt | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ giá của mọi mặt hàng và xây dựng tập kề cho mỗi đỉnh. Mỗi bộ chứa các mục được kết nối trực tiếp với đỉnh đó, vì vậy chúng ta có thể nhanh chóng kiểm tra xem một cặp có tương thích hay không. 
2. Đi qua mọi cạnh`(u, v)`trong biểu đồ. Tam giác chứa cạnh này phải có đỉnh thứ ba nối với cả hai cạnh này`u`Và`v`. 
3. Lặp lại tập nhỏ hơn trong hai tập kề của`u`Và`v`. Với mọi đỉnh ứng cử viên`x`, kiểm tra xem`x`cũng thuộc tập kề cận của điểm cuối khác. 
4. Khi có một đỉnh như vậy thì ba đỉnh đó tạo thành một tam giác hợp lệ. Tính toán`price[u] + price[v] + price[x]`và giảm thiểu câu trả lời. 
5. Sau khi tất cả các cạnh được xử lý, in giá trị nhỏ nhất tìm được. Nếu không tìm thấy hình tam giác nào, hãy in`-1`. 

Lý do phải lặp qua danh sách kề nhỏ hơn là vì mọi ứng cử viên từ cạnh đó đều có thể là đỉnh thứ ba. Việc kiểm tra phía nhỏ hơn giúp giảm tổng số lần kiểm tra tư cách thành viên không cần thiết. 

Tại sao nó hoạt động: 

Mọi nghiệm hợp lệ đều là một tam giác và mọi tam giác đều có ba cạnh. Trong quá trình truyền tải, thuật toán xử lý ít nhất một trong các cạnh đó. Khi nó xử lý cạnh đó, đỉnh thứ ba của tam giác xuất hiện trong giao điểm của tập kề, do đó tam giác được tìm thấy. Vì mọi tam giác tìm được đều được đánh giá và tổng tối thiểu được giữ nguyên nên câu trả lời cuối cùng chính xác là tam giác rẻ nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    price = list(map(int, input().split()))

    adj = [set() for _ in range(n)]
    edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].add(v)
        adj[v].add(u)
        edges.append((u, v))

    ans = float("inf")

    for u, v in edges:
        if len(adj[u]) > len(adj[v]):
            u, v = v, u

        for x in adj[u]:
            if x in adj[v]:
                ans = min(ans, price[u] + price[v] + price[x])

    print(-1 if ans == float("inf") else ans)

if __name__ == "__main__":
    solve()
```Các tập kề được sử dụng vì thao tác duy nhất chúng ta cần lặp đi lặp lại là hỏi xem hai mục có được kết nối hay không. Một danh sách sẽ yêu cầu quét tất cả các hàng xóm, trong khi một bộ cung cấp khả năng tra cứu trung bình theo thời gian không đổi. 

Danh sách cạnh được lưu trữ riêng vì việc lặp qua các bộ kề sẽ xử lý mỗi cạnh hai lần và sẽ yêu cầu ghi sổ bổ sung. Việc hoán đổi trước vòng lặp bên trong đảm bảo rằng tập lân cận nhỏ hơn luôn được quét. 

Thuật toán không sửa đổi biểu đồ trong khi xử lý nó, do đó không có vấn đề về thứ tự. Tổng giá được lưu trữ bằng số nguyên Python, tự động xử lý các giá trị lớn hơn giới hạn 32 bit. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 3
1 2 3
1 2
2 3
3 1
```dấu vết là: 

| Cạnh | Đã kiểm tra vùng lân cận nhỏ hơn | Hàng xóm chung | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1-2 | {2,3} | 3 | 6 | 
| 2-3 | {1,3} | 1 | 6 | 
| 3-1 | {1,2} | 2 | 6 | 

Đồ thị là một hình tam giác hoàn chỉnh. Mỗi cạnh đều tìm thấy đỉnh còn lại và tổng tối thiểu có thể là cả ba mức giá. 

Đối với đầu vào:```
4 4
1 1 1 1
1 2
2 3
3 4
4 1
```dấu vết là: 

| Cạnh | Đã kiểm tra vùng lân cận nhỏ hơn | Hàng xóm chung | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1-2 | {2,4} | không | vô cực | 
| 2-3 | {1,3} | không | vô cực | 
| 3-4 | {2,4} | không | vô cực | 
| 4-1 | {3,1} | không | vô cực | 

Chu kỳ có chiều dài bốn, không phải ba. Không có cặp đỉnh liền kề nào có chung đỉnh lân cận, do đó thuật toán đưa ra kết quả chính xác`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * sqrt(m)) trung bình | Mỗi cạnh kiểm tra cạnh nhỏ hơn của mối quan hệ kề cận, điều này hiệu quả đối với các giới hạn đồ thị dự định. | 
| Không gian | O(n + m) | Các tập kề và danh sách cạnh lưu trữ đồ thị. | 

Giải pháp tránh được số khối gấp ba có thể có và chỉ khám phá các kết nối đồ thị hiện có. Điều này làm cho nó phù hợp với các biểu đồ thưa thớt lớn, nơi việc kiểm tra mọi nhóm ba có thể là không thể. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    data = sys.stdin.readline
    n, m = map(int, data().split())
    price = list(map(int, data().split()))

    adj = [set() for _ in range(n)]
    edges = []

    for _ in range(m):
        u, v = map(int, data().split())
        u -= 1
        v -= 1
        adj[u].add(v)
        adj[v].add(u)
        edges.append((u, v))

    ans = float("inf")

    for u, v in edges:
        if len(adj[u]) > len(adj[v]):
            u, v = v, u
        for x in adj[u]:
            if x in adj[v]:
                ans = min(ans, price[u] + price[v] + price[x])

    sys.stdin = old_stdin
    return str(-1 if ans == float("inf") else ans)

assert run("""3 3
1 2 3
1 2
2 3
3 1
""") == "6"

assert run("""3 2
2 3 4
1 2
2 3
""") == "-1"

assert run("""4 4
1 1 1 1
1 2
2 3
3 4
4 1
""") == "-1"

assert run("""4 4
10 1 2 100
1 2
2 3
3 1
1 4
""") == "13"

assert run("""3 3
1000000 1000000 1000000
1 2
2 3
3 1
""") == "3000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba mục được kết nối đầy đủ | 6 | Phát hiện tam giác cơ bản | 
| Ba món trong một chuỗi | -1 | Ngăn chặn việc nhầm đường dẫn với hình tam giác | 
| Đồ thị bốn chu kỳ | -1 | Kiểm tra xem chu kỳ dài hơn có bị bỏ qua không | 
| Tam giác có thêm một đỉnh gây mất tập trung | 13 | Kiểm tra lựa chọn tam giác tối thiểu | 
| Giá trị giá tối đa | 3000000 | Kiểm tra tổng số nguyên lớn | 

## Vỏ cạnh 

Đối với trường hợp dây chuyền:```
3 2
2 3 4
1 2
2 3
```thuật toán kiểm tra cả hai cạnh. Đối với cạnh`(1,2)`, lân cận của đỉnh`1`chỉ chứa`2`, và đỉnh`2`có hàng xóm`1`Và`3`. Không có đỉnh thứ ba chung. Điều tương tự cũng xảy ra đối với`(2,3)`, vì vậy câu trả lời vẫn chưa được đặt và`-1`được in. 

Đối với biểu đồ chứa kết nối không phải tam giác trông rẻ hơn:```
4 4
10 1 2 100
1 2
2 3
3 1
1 4
```rìa`(1,2)`tìm thấy đỉnh`3`là hàng xóm chung và tạo ra tam giác có tổng`13`. Cạnh`(1,4)`không có hàng xóm chung, vì vậy nó không thể tạo ra câu trả lời thấp hơn không hợp lệ. 

Đối với đồ thị không có hình tam giác:```
4 3
5 5 5 5
1 2
2 3
3 4
```mọi cạnh chỉ thuộc về một đường đi. Vì không có cạnh nào có điểm cuối với hàng xóm dùng chung nên thuật toán không bao giờ cập nhật câu trả lời và trả về`-1`.
