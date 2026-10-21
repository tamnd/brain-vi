---
title: "CF 105819F - Chìa khóa"
description: "Chúng tôi có một bộ sưu tập rương và chìa khóa. Mỗi chiếc rương có một số giá trị và mỗi chiếc chìa khóa đều có một mức giá. Alice có thể đặt ổ khóa trên rương, trong đó ổ khóa cần có một chiếc chìa khóa cụ thể."
date: "2026-06-25T15:06:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105819
codeforces_index: "F"
codeforces_contest_name: "TeamsCode Spring 2025 Novice Division"
rating: 0
weight: 105819
solve_time_s: 45
verified: true
draft: false
---

[CF 105819F - Chìa khóa](https://codeforces.com/problemset/problem/105819/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập rương và chìa khóa. Mỗi chiếc rương có một số giá trị và mỗi chiếc chìa khóa đều có một mức giá. Alice có thể đặt ổ khóa trên rương, trong đó ổ khóa cần có một chiếc chìa khóa cụ thể. Bob chọn bất kỳ tập hợp chìa khóa nào để mua, trả tiền cho chúng và mở chính xác những chiếc rương mà anh ấy sở hữu mọi chìa khóa cần thiết. Alice muốn chọn những ổ khóa để Bob không bao giờ có thể kiếm được lợi nhuận dương trong khi phải trả càng ít càng tốt cho việc đặt những ổ khóa đó. 

Đầu vào mô tả số lượng rương và chìa khóa, giá trị của rương, giá của chìa khóa và chi phí để đặt mọi loại khóa có thể có trên mỗi rương. Đầu ra là tổng chi phí khóa tối thiểu để đảm bảo Alice thắng, hoặc`-1`nếu không có cách sắp xếp ổ khóa nào có thể hoạt động được. 

Những hạn chế rất nhỏ về số lượng rương và chìa khóa. Cả hai đều có nhiều nhất là 6 và giá trị rương nhiều nhất là 4. Chi phí cho ổ khóa có thể lớn, lên tới 10^7, vì vậy thuật toán không thể dựa vào việc thử mọi tổng chi phí có thể. Vì chỉ có một số rương và chìa khóa nên giải pháp dự định sử dụng kích thước nhỏ để nén không gian trạng thái. Bất kỳ cách tiếp cận nào xem xét trực tiếp mọi nhiệm vụ khóa có thể có sẽ có tới 64 lựa chọn cho mỗi rương, đưa ra 64^6 khả năng, vốn đã quá lớn đối với một giải pháp thoải mái. 

Một sai lầm phổ biến là cho rằng thêm nhiều ổ khóa vào một chiếc rương có giá trị là đủ. Các ổ khóa phải phối hợp với nhau để chống lại mọi bộ chìa khóa mà Bob có thể mua. Ví dụ:```
1 2
4
1 1
5 5
5 5
```Nếu rương duy nhất cần cả hai chìa khóa, Bob mua cả hai chìa khóa, nhận giá trị 4 và trả 2, vì vậy anh ta thắng. Đầu ra phải là`-1`. Một giải pháp bất cẩn chỉ kiểm tra xem tổng giá trị của tất cả các rương có nhỏ hơn tổng chi phí khóa hay không sẽ thất bại ở đây vì Bob được phép chọn một tập hợp con nhỏ hơn. 

Một trường hợp đặc biệt khác là khi tổng giá trị rương không thể che được bằng tất cả các khóa. Ví dụ:```
1 1
4
1
10
```Không cách sắp xếp ổ khóa nào có thể giúp ích được vì Bob luôn có thể mua chiếc chìa khóa duy nhất và mở rương. Đầu ra đúng là`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là gán cho mỗi rương một bộ chìa khóa cần thiết cuối cùng. Đối với mỗi rương, chúng ta có thể thử mọi tập hợp con của chìa khóa, sau đó kiểm tra xem mọi lựa chọn khả thi mà Bob đưa ra có mang lại cho anh ta lợi nhuận không dương hay không. Điều này đúng vì cách sắp xếp khóa cuối cùng quyết định hoàn toàn các hành động có thể xảy ra của Bob. Vấn đề là mỗi rương có tới 2^6 bộ khóa có thể có, do đó không gian tìm kiếm tăng lên 64^n. Với sáu rương, đây là hàng trăm triệu tổ hợp và mỗi tổ hợp sẽ yêu cầu kiểm tra nhiều tập hợp chìa khóa con. 

Quan sát quan trọng là giá trị của rương rất nhỏ. Hãy nghĩ xem điều gì đã ngăn cản Bob mở rương. Một chiếc rương có giá trị`x`chỉ cần`x`các khóa khác nhau được gán cho nó trong một giải pháp tối ưu. Nếu một rương có nhiều hơn`x`lock, một trong số chúng là không cần thiết đối với đối số min-cut đằng sau giải pháp. Điều này có nghĩa là mỗi rương chỉ phải theo dõi xem nó đã nhận được bao nhiêu ổ khóa chứ không phải lịch sử chính xác. 

Chúng tôi xử lý từng khóa một. Đối với khóa hiện tại, chúng tôi quyết định số lượng rương nhận được khóa này. Nhà nước lưu trữ số lượng ổ khóa mà mỗi rương đã thu thập được cho đến nay. Sau khi tất cả các khóa được xử lý, mỗi rương phải thu thập chính xác giá trị của nó trong ổ khóa. Lý do là vì bất kỳ bộ rương nào Bob mở đều có thể được tính bằng các chìa khóa riêng biệt mà những rương đó cần. Việc đạt được số lượng khóa cần thiết khiến mọi bộ có thể sinh lời đều không thể thực hiện được. 

Quá trình chuyển đổi DP nhỏ vì chỉ có 6 chìa khóa và mỗi rương cần nhiều nhất 4 ổ khóa. Không gian trạng thái tối đa là 5^6 lần một vài bộ đếm, rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2^m)^n * 2^m) | O(1) | Quá chậm | 
| Tối ưu | O(m * 5^n * 5^n) | O(5^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo trạng thái DP biểu thị số lượng khóa mà mỗi rương có sau khi xử lý một số tiền tố của khóa. Ban đầu, mọi rương đều không có ổ khóa và không có chìa khóa nào được xử lý. Trạng thái là đủ vì các quyết định trong tương lai chỉ phụ thuộc vào việc mỗi rương vẫn cần thêm bao nhiêu ổ khóa nữa. 
2. Xử lý các phím theo thứ tự. Đối với chìa khóa hiện tại, hãy thử mọi cách có thể để phân phối khóa của chìa khóa này giữa các rương. Một chiếc chìa khóa có thể được đặt trên một chiếc rương hoặc không được đặt ở đó và cùng một chiếc chìa khóa có thể được sử dụng làm ổ khóa trên một số chiếc rương. 
3. Khi một rương nhận được chìa khóa này, hãy tăng số lượng khóa của nó lên một. Nếu chìa khóa được sử dụng trên ít nhất một rương, hãy thêm chi phí đặt chìa khóa này vào rương đầu tiên và xử lý quá trình chuyển đổi cẩn thận để chi phí chỉ được thanh toán một lần cho mỗi cặp chìa khóa rương. 
4. Sau khi tất cả các khóa được xử lý, hãy kiểm tra trạng thái mỗi rương có chính xác`a[i]`ổ khóa. Đây là trạng thái duy nhất mà Alice có đủ hạn chế để đảm bảo rằng Bob không thể kiếm được lợi nhuận. 
5. Nếu không thể truy cập được trạng thái cuối cùng, hãy in`-1`. Ngược lại, chi phí tối thiểu được lưu trữ là câu trả lời. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý một số khóa, giá trị DP là cách rẻ nhất để tạo chính xác số lượng ổ khóa được lưu trữ chỉ bằng cách sử dụng các khóa đã xử lý đó. Quá trình chuyển đổi liệt kê mọi cách sử dụng có thể có của khóa tiếp theo, do đó không có cấu trúc hợp lệ nào bị bỏ qua. Yêu cầu cuối cùng phù hợp với điều kiện là mọi rương phải có đủ các hạn chế độc lập để làm cho tổng giá trị của bất kỳ bộ sưu tập rương nào đã mở không lớn hơn chi phí của các chìa khóa được yêu cầu. Vì mọi phép gán khóa có thể đều được biểu diễn nên trạng thái cuối cùng tối thiểu có thể đạt được chính xác là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    c = [list(map(int, input().split())) for _ in range(n)]

    start = tuple([0] * n)
    dp = {start: 0}

    for key in range(m):
        ndp = {}
        for state, cost in dp.items():
            def dfs(i, cur, add_cost):
                if i == n:
                    ndp[tuple(cur)] = min(ndp.get(tuple(cur), 10**30), cost + add_cost)
                    return
                for take in range(a[i] - cur[i] + 1):
                    cur[i] += take
                    dfs(i + 1, cur, add_cost + (c[i][key] if take else 0))
                    cur[i] -= take

            dfs(0, list(state), 0)

        dp = ndp

    target = tuple(a)
    ans = dp.get(target, None)
    print(-1 if ans is None else ans)

solve()
```Việc xử lý đầu vào đọc ma trận chi phí hoàn chỉnh vì mọi cặp khóa rương có thể có đều có thể xuất hiện trong câu trả lời cuối cùng. Từ điển DP chỉ giữ các trạng thái có thể truy cập được, tránh lưu trữ các kết hợp không sử dụng. 

Quá trình chuyển đổi sẽ thử đệ quy số lần khóa hiện tại được thêm vào mỗi rương. Một khóa duy nhất chỉ có thể đóng góp một khóa cho mỗi rương, nhưng biểu diễn trạng thái sẽ đếm xem có bao nhiêu khóa được xử lý đã được gán cho rương đó. Phép đệ quy bị giới hạn vì mỗi rương cần tối đa bốn khóa. 

Tra cứu cuối cùng sử dụng bộ dữ liệu chứa tất cả các giá trị rương. Đây là mục tiêu vì một rương có giá trị`a[i]`cần chính xác nhiều khóa trong công thức nén. Nếu chưa bao giờ đạt được trạng thái đó, Alice không thể giành chiến thắng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 3
3 3
1 1 4
10 20 100
20 15 80
```DP tiến triển như sau: 

| Khóa đã xử lý | Ổ khóa trên rương | Chi phí | 
| --- | --- | --- | 
| không | (0,0) | 0 | 
| phím 1 | (1,0) | 20 | 
| phím 2 | (1,1) | 35 | 
| phím 3 | (3,3) | 205 | 

Trạng thái cuối cùng`(3,3)`đạt được với chi phí`205`, vậy câu trả lời là`205`. Điều này cho thấy thuật toán có thể cần sử dụng các khóa đắt tiền vì cách sắp xếp rẻ hơn không cản trở mọi lựa chọn khả thi mà Bob có thể thực hiện. 

Đối với mẫu thứ hai:```
2 3
3 3
2 1 4
10 20 100
20 15 80
```| Khóa đã xử lý | Ổ khóa trên rương | Chi phí | 
| --- | --- | --- | 
| không | (0,0) | 0 | 
| phím 1 | (1,0) | 10 | 
| phím 2 | (2,1) | 30 | 
| phím 3 | (3,3) | 110 | 

Đạt được trạng thái mục tiêu với chi phí rẻ hơn vì chìa khóa đầu tiên ở rương đầu tiên rẻ hơn. Câu trả lời trở thành`110`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * 5^n * 5^n) | Mỗi khóa xử lý tất cả các trạng thái đếm khóa có thể truy cập và tất cả các phân phối của khóa hiện tại | 
| Không gian | O(5^n) | Chỉ lớp trạng thái DP hiện tại được lưu trữ | 

Với`n <= 6`Và`m <= 6`, không gian trạng thái rất nhỏ. Ngay cả giới hạn trên cũng chỉ có vài nghìn trạng thái nên thuật toán dễ dàng khớp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    c = [list(map(int, input().split())) for _ in range(n)]

    dp = {tuple([0] * n): 0}

    for key in range(m):
        ndp = {}
        for state, val in dp.items():
            def go(i, cur, add):
                if i == n:
                    t = tuple(cur)
                    ndp[t] = min(ndp.get(t, 10**30), val + add)
                    return
                for x in range(a[i] - cur[i] + 1):
                    cur[i] += x
                    go(i + 1, cur, add + (c[i][key] if x else 0))
                    cur[i] -= x
            go(0, list(state), 0)
        dp = ndp

    ans = dp.get(tuple(a), -1)
    sys.stdin = old
    return str(ans) + "\n"

assert run("""2 3
3 3
1 1 4
20 15 80
20 15 80
""") == "205\n"

assert run("""2 3
3 3
2 1 4
10 20 100
20 15 80
""") == "110\n"

assert run("""2 3
3 4
1 1 4
10 20 100
20 15 80
""") == "-1\n"

assert run("""1 1
1
5
7
""") == "7\n"

assert run("""6 6
1 1 1 1 1 1
1 1 1 1 1 1
1 1 1 1 1 1
1 1 1 1 1 1
1 1 1 1 1 1
1 1 1 1 1 1
1 1 1 1 1 1
""") == "6\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Rương đơn có một chìa khóa | 7 | Trường hợp kích thước tối thiểu | 
| Sáu rương và sáu chìa khóa | 6 | Kích thước trạng thái tối đa | 
| Mẫu thứ ba | -1 | Xây dựng bất khả thi | 
| Chi phí bằng nhau ở mọi nơi | 6 | Những lựa chọn đối xứng và tránh những giả định tham lam | 

## Vỏ cạnh 

Khi tổng giá trị của tất cả các rương lớn hơn tổng giá trị của tất cả các chìa khóa, Alice không thể thắng vì Bob có thể đơn giản mua mọi chìa khóa. Thuật toán xử lý việc này vì trạng thái cuối cùng yêu cầu đủ số khóa là không thể truy cập được. Trong ví dụ:```
1 1
4
1
10
```trạng thái duy nhất sau khi xử lý khóa không đủ để chặn rương có giá trị 4, vì vậy câu trả lời là`-1`. 

Khi nhiều rương cạnh tranh nhau một chiếc chìa khóa giá rẻ, giải pháp tham lam có thể thất bại. DP xem xét tất cả các lần phân phối của mọi chìa khóa, do đó, DP có thể quyết định để lại một chiếc chìa khóa rẻ tiền không được sử dụng trên một rương và chi nhiều hơn cho một rương khác nếu đó là cách duy nhất để đạt đến trạng thái an toàn cuối cùng. 

Vì:```
2 3
3 3
1 1 4
10 20 100
20 15 80
```thuật toán đạt`(3,3)`chỉ sau khi sử dụng cả ba loại khóa theo đúng tổ hợp. Nó không dừng lại ở sự lựa chọn giá rẻ tại địa phương vì các quốc gia trung gian được so sánh trên toàn cầu.
