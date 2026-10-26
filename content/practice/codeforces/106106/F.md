---
title: "CF 106106F - \u0417\u0430\u0431\u043e\u0440 \u0414\u0436\u0438\u043d\u0430"
description: "Chúng ta có một hàng rào làm bằng n tấm ván thẳng đứng đặt cạnh nhau. Bảng i có chiều cao h[i] nên nếu nó nhận được một màu thì nó sẽ tiêu tốn chính xác h[i] cm vuông sơn. Mỗi bảng phải được sơn màu đỏ hoặc xanh lá cây."
date: "2026-06-25T11:41:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106106
codeforces_index: "F"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u042e\u043d\u0438\u043e\u0440\u044b 2024"
rating: 0
weight: 106106
solve_time_s: 33
verified: true
draft: false
---

[CF 106106F - \u0417\u0430\u0431\u043e\u0440 \u0414\u0436\u0438\u043d\u0430](https://codeforces.com/problemset/problem/106106/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng rào làm bằng`n`các tấm ván dọc đặt cạnh nhau. Bảng`i`có chiều cao`h[i]`, vì vậy nếu nó nhận được một màu, nó sẽ tiêu thụ chính xác`h[i]`cm vuông sơn. Mỗi bảng phải được sơn màu đỏ hoặc xanh lá cây. 

Sơn màu đỏ có thể che phủ nhiều nhất`a`cm vuông và sơn màu xanh lá cây có thể che phủ nhiều nhất`b`cm vuông. Trong số tất cả các màu hợp lệ, chúng ta cần một màu mà các bảng lân cận có màu khác nhau có tổng chiều dài tiếp xúc nhỏ nhất có thể. Đối với hai bảng liền kề, chiều dài tiếp xúc ngắn hơn so với chiều cao của chúng, vì các bảng chỉ chạm vào đoạn thẳng đứng chồng lên nhau. 

Ví dụ: nếu hai bảng lân cận có chiều cao`5`Và`3`và các màu sắc khác nhau, chúng góp phần`3`đến giá trị kém hấp dẫn. Nếu màu sắc của chúng bằng nhau thì chúng không đóng góp gì cả. 

Kích thước đầu vào đưa ra hướng đi của giải pháp. Số lượng bảng chỉ`200`, nhưng công suất sơn có thể đạt tới`40000`. Một không gian trạng thái liên quan đến số lượng bảng và vùng màu đỏ được sử dụng là thực tế, trong khi thử tất cả`2^n`chất tạo màu là không thể. Ngay cả với`n = 40`, vũ lực sẽ vượt quá giới hạn thực tế, và`n = 200`làm cho nó hoàn toàn không thể thực hiện được. 

Những trường hợp khó khăn không chỉ có đầu vào lớn. Một giải pháp cũng phải xử lý các tình huống trong đó một màu không được sử dụng, trong đó giới hạn sơn chính xác bằng diện tích yêu cầu và khi việc lựa chọn màu rẻ tại địa phương tạo ra những thay đổi tốn kém sau này. 

Hãy xem xét đầu vào này:```
1
0 5
5
```Tấm ván duy nhất có chiều cao`5`. Đầu ra đúng là:```
0
```Một giải pháp bất cẩn có thể cho rằng cả hai màu đều được yêu cầu và từ chối nó, nhưng một màu được phép giữ nguyên. 

Một trường hợp quan trọng khác là:```
3
3 3
2 2 2
```Đầu ra đúng là:```
-1
```Tổng diện tích hàng rào là`6`, vậy cả hai màu với nhau đều có đủ công suất. Tuy nhiên không màu nào có thể chiếm được cả ba bảng vì mỗi màu đều có công suất`3`, trong khi mỗi bảng đóng góp`2`. Tiểu bang phải theo dõi chính xác diện tích sơn có thể đạt được thay vì chỉ tổng công suất sẵn có. 

Trường hợp thứ ba là:```
4
10 10
8 1 8 1
```Màu sắc tối ưu không thể được tìm thấy bằng cách nhóm các tấm ván cao nhất lại với nhau. Chi phí chuyển đổi phụ thuộc vào độ cao lân cận, vì vậy quyết định tốt nhất phụ thuộc vào màu của bảng trước đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi màu sắc có thể có của hàng rào. Đối với mỗi màu, chúng tôi tính toán tổng diện tích màu đỏ, diện tích màu xanh lá cây và chi phí của mỗi đường viền giữa các màu khác nhau. Phương pháp này đúng vì mọi phép gán có thể đều được kiểm tra, nhưng nó cũng kiểm tra`2^n`khả năng. Với`n = 200`, điều này vượt xa bất kỳ số lượng hoạt động khả thi nào. 

Cấu trúc giúp ích cho chúng ta là sự tương tác duy nhất giữa các bảng lân cận là màu sắc của bảng trước đó. Khi chúng ta quyết định màu của bảng tiếp theo, chi phí mới duy nhất chúng ta cần thêm là liệu màu này có khác với màu trước hay không. Lịch sử đầy đủ của các bảng trước đó là không cần thiết. 

Điều này gợi ý lập trình động. Chúng tôi xử lý các bảng từ trái sang phải và lưu trữ mức độ kém hấp dẫn tối thiểu sau khi tô màu tiền tố. Nhà nước cần ghi nhớ lượng sơn đỏ đã sử dụng và màu của bảng sơn cuối cùng. Số lượng màu xanh lá cây không cần phải được lưu trữ vì tổng chiều cao của tiền tố được xử lý đã được biết, do đó có thể tính toán mức sử dụng màu xanh lá cây. 

Lực lượng vũ phu hoạt động vì nó khám phá tất cả các lịch sử màu có thể có trước đó, nhưng hầu hết các lịch sử đó đều tương đương nhau khi chúng có cùng cách sử dụng màu đỏ và màu kết thúc. Lập trình động hợp nhất các tình huống tương đương này thành một trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n * a * 2) | O(a * 2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý các bảng từ trái sang phải. Duy trì hai giá trị lập trình động cho mỗi lần sử dụng sơn đỏ có thể. Một giá trị thể hiện mức độ kém hấp dẫn tối thiểu khi bảng cuối cùng hiện tại có màu đỏ và giá trị còn lại thể hiện tình trạng tương tự khi bảng cuối cùng hiện tại có màu xanh lá cây. 

Trạng thái là đủ vì các quyết định trong tương lai chỉ quan tâm đến màu bảng trước đó và dung lượng màu đỏ đã được tiêu thụ bao nhiêu. 
2. Khởi tạo bảng đầu tiên một cách riêng biệt. Nếu bảng đầu tiên có màu đỏ thì cách sử dụng màu đỏ là`h[0]`và chi phí là`0`. Nếu nó có màu xanh lá cây thì cách sử dụng màu đỏ là`0`và chi phí là`0`. 

Không có đường viền trước cho bảng đầu tiên nên không thể tạo thêm sự kém hấp dẫn. 
3. Đối với mỗi bảng tiếp theo, hãy tạo một mảng lập trình động mới. Hãy thử sơn bảng màu đỏ và thử sơn màu xanh lá cây từ mọi trạng thái trước đó có thể truy cập được. 

Nếu màu sắc giữ nguyên thì giá không thay đổi. Nếu màu thay đổi thì thêm`min(h[i-1], h[i])`, là độ dài của đường viền xấu mới được tạo. 
4. Khi sơn bảng màu đỏ, hãy tăng mức sử dụng màu đỏ được theo dõi theo chiều cao của bảng. Khi sơn màu xanh lá cây, hãy giữ nguyên cách sử dụng màu đỏ. 

Việc kiểm tra công suất cây xanh bị trì hoãn cho đến khi kết thúc vì tổng chiều cao của hàng rào được cố định. Đối với trạng thái cuối cùng có sử dụng màu đỏ`r`, cách sử dụng xanh là`total_height - r`. 
5. Sau khi tất cả các bảng được xử lý, hãy kiểm tra mọi trạng thái. Một trạng thái hợp lệ nếu mức sử dụng màu đỏ nhiều nhất`a`và việc sử dụng xanh nhiều nhất là`b`. Chi phí nhỏ nhất trong số các tiểu bang đó chính là câu trả lời. Nếu không có trạng thái nào hợp lệ, xuất ra`-1`. 

Tại sao nó hoạt động: 

Tính bất biến của quy hoạch động là sau khi xử lý dữ liệu đầu tiên`i`bảng, mọi trạng thái được lưu trữ đều có mức độ kém hấp dẫn tối thiểu có thể đối với việc sử dụng màu đỏ chính xác và màu bảng cuối cùng. Khi thêm bảng tiếp theo, mọi lựa chọn màu sắc có thể có đều được xem xét và đóng góp mới duy nhất vào chi phí là đường viền với bảng trước đó. Vì nhà nước lưu giữ chính xác thông tin cần thiết cho các lựa chọn trong tương lai nên không có giải pháp tối ưu nào bị loại bỏ. Sau bảng cuối cùng, tất cả các màu hoàn chỉnh hợp lệ sẽ được biểu thị bằng một số trạng thái, vì vậy việc lấy mức tối thiểu sẽ mang lại câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a, b = map(int, input().split())
    h = list(map(int, input().split()))

    INF = 10**18

    dp_red = [INF] * (a + 1)
    dp_green = [INF] * (a + 1)

    if h[0] <= a:
        dp_red[h[0]] = 0
    dp_green[0] = 0

    total = h[0]

    for i in range(1, n):
        nh = h[i]
        total += nh
        ndp_red = [INF] * (a + 1)
        ndp_green = [INF] * (a + 1)

        border = min(h[i - 1], h[i])

        for red_used in range(a + 1):
            if dp_red[red_used] != INF:
                if red_used + nh <= a:
                    ndp_red[red_used + nh] = min(
                        ndp_red[red_used + nh],
                        dp_red[red_used]
                    )
                ndp_green[red_used] = min(
                    ndp_green[red_used],
                    dp_red[red_used] + border
                )

            if dp_green[red_used] != INF:
                if red_used + nh <= a:
                    ndp_red[red_used + nh] = min(
                        ndp_red[red_used + nh],
                        dp_green[red_used] + border
                    )
                ndp_green[red_used] = min(
                    ndp_green[red_used],
                    dp_green[red_used]
                )

        dp_red, dp_green = ndp_red, ndp_green

    ans = INF
    for red_used in range(a + 1):
        green_used = total - red_used
        if green_used <= b:
            ans = min(ans, dp_red[red_used], dp_green[red_used])

    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Các mảng`dp_red`Và`dp_green`chỉ lưu trữ số lượng sơn màu đỏ có thể tiếp cận. Các chỉ số của chúng là vùng màu đỏ được sử dụng cho đến nay, đây là thứ nguyên năng lực duy nhất cần thiết. 

Việc khởi tạo xử lý bảng đầu tiên một cách riêng biệt vì không có bảng trước đó để so sánh. Mỗi lần chuyển đổi sau này sẽ xem xét hai màu mới có thể có và chỉ thêm chi phí đường viền khi màu thay đổi. 

Số tiền xanh không bao giờ được lưu trữ. Cuối cùng, tổng diện tích hàng rào trừ đi diện tích màu đỏ được lưu trữ sẽ là diện tích màu xanh lá cây. Điều này tránh tăng gấp đôi kích thước trạng thái bằng cách theo dõi cả hai màu. 

Séc`red_used + nh <= a`ngăn chặn việc tạo ra các trạng thái màu đỏ không hợp lệ. Từ`a`nhiều nhất là`40000`, các giá trị số nguyên vẫn đủ nhỏ đối với số nguyên Python mà không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4
5 7
3 3 4 1
```Tổng chiều cao là`11`. Bảng hiển thị các trạng thái cuối cùng quan trọng. 

| Bước | Chiều cao bảng | Cách sử dụng màu đỏ | Màu cuối cùng | Chi phí tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | Xanh | 0 | 
| 1 | 3 | 3 | Đỏ | 0 | 
| 2 | 3 | 3 | Xanh | 3 | 
| 2 | 3 | 6 | Đỏ | 0 | 
| 3 | 4 | 6 | Đỏ | 0 | 
| 3 | 4 | 3 | Xanh | 3 | 
| 4 | 1 | 6 | Đỏ | 0 | 
| 4 | 1 | 5 | Xanh | 3 | 

Trạng thái hợp lệ tốt nhất có cách sử dụng màu đỏ`5`và sử dụng xanh`6`, chi phí sản xuất`3`. Dấu vết cho thấy rằng chương trình động giữ riêng các cách sử dụng màu đỏ khác nhau vì chúng dẫn đến khả năng phân phối sơn cuối cùng khác nhau. 

Đối với mẫu thứ ba:```
3
3 3
2 2 2
```| Bước | Chiều cao bảng | Cách sử dụng màu đỏ | Màu cuối cùng | Chi phí tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | Xanh | 0 | 
| 1 | 2 | 2 | Đỏ | 0 | 
| 2 | 2 | 0 | Xanh | 0 | 
| 2 | 2 | 2 | Đỏ | 0 | 
| 3 | 2 | 2 | Đỏ | 0 | 
| 3 | 2 | 2 | Xanh | 2 | 

Không có trạng thái cuối cùng nào có cả cách sử dụng màu đỏ nhiều nhất`3`và sử dụng xanh nhiều nhất`3`. Thuật toán báo cáo chính xác rằng hàng rào không thể sơn được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * a) | Mỗi trong số`n`bảng quét tất cả các cách sử dụng màu đỏ có thể có và thực hiện công việc liên tục. | 
| Không gian | O(a) | Chỉ các mảng lập trình động trước đó và hiện tại được lưu trữ. | 

Giá trị tối đa của`a`là`40000`, Và`n`là`200`, mang lại khoảng tám triệu chuyển đổi trạng thái. Điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_instance(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a, b = map(int, input().split())
    h = list(map(int, input().split()))

    INF = 10**18
    dp_red = [INF] * (a + 1)
    dp_green = [INF] * (a + 1)

    if h[0] <= a:
        dp_red[h[0]] = 0
    dp_green[0] = 0

    total = h[0]

    for i in range(1, n):
        total += h[i]
        ndp_red = [INF] * (a + 1)
        ndp_green = [INF] * (a + 1)
        cost = min(h[i - 1], h[i])

        for r in range(a + 1):
            if dp_red[r] < INF:
                if r + h[i] <= a:
                    ndp_red[r + h[i]] = min(ndp_red[r + h[i]], dp_red[r])
                ndp_green[r] = min(ndp_green[r], dp_red[r] + cost)

            if dp_green[r] < INF:
                if r + h[i] <= a:
                    ndp_red[r + h[i]] = min(ndp_red[r + h[i]], dp_green[r] + cost)
                ndp_green[r] = min(ndp_green[r], dp_green[r])

        dp_red, dp_green = ndp_red, ndp_green

    ans = INF
    for r in range(a + 1):
        if total - r <= b:
            ans = min(ans, dp_red[r], dp_green[r])

    return str(-1 if ans == INF else ans) + "\n"

assert solve_instance("""4
5 7
3 3 4 1
""") == "3\n"

assert solve_instance("""3
2 3
1 3 1
""") == "2\n"

assert solve_instance("""3
3 3
2 2 2
""") == "-1\n"

assert solve_instance("""1
0 5
5
""") == "0\n"

assert solve_instance("""4
10 10
8 1 8 1
""") == "1\n"

assert solve_instance("""5
0 15
3 3 3 3 3
""") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 5 / 5`|`0`| Một bảng duy nhất và màu đỏ chưa sử dụng | 
|`4 / 10 10 / 8 1 8 1`|`1`| Quyết định tùy thuộc vào độ cao lân cận | 
|`5 / 0 15 / 3 3 3 3 3`|`0`| Tất cả các bảng buộc thành một màu | 
|`3 / 3 3 / 2 2 2`|`-1`| Phân phối sơn không thể | 

## Vỏ cạnh 

Đối với trường hợp bo mạch đơn:```
1
0 5
5
```Trạng thái ban đầu đưa bảng về trạng thái xanh với cách sử dụng màu đỏ`0`. Kiểm tra cuối cùng cho thấy việc sử dụng xanh là`5`, phù hợp với giới hạn và trả về`0`vì không có cặp lân cận nào tồn tại. 

Đối với trường hợp phân phối không thể:```
3
3 3
2 2 2
```Lập trình động tạo ra các trạng thái sử dụng màu đỏ`0`,`2`,`4`, v.v. chỉ khi chúng thực sự có thể đạt được. Bộ lọc cuối cùng sẽ loại bỏ mọi trạng thái vì trạng thái hợp lệ sẽ cần nhiều nhất cả mức sử dụng màu đỏ và mức sử dụng màu xanh lá cây.`3`, điều đó không thể xảy ra. Câu trả lời vẫn là vô hạn và được chuyển đổi thành`-1`. 

Đối với trường hợp không sử dụng một màu:```
5
0 15
3 3 3 3 3
```Mọi chuyển đổi màu đỏ đều bị từ chối vì dung lượng màu đỏ bằng 0. Các trạng thái duy nhất còn sót lại là trạng thái màu xanh lá cây và vì tất cả các bảng liền kề đều có cùng màu nên tính kém hấp dẫn vẫn còn`0`.
