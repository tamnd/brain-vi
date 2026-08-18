---
title: "CF 102222F - Tiếp tục"
description: "Chúng ta có một biểu đồ có trọng số vô hướng hoàn chỉnh của các thành phố. Thành phố i có giá trị rủi ro r[i] và d[i][j] là khoảng cách di chuyển trực tiếp giữa thành phố i và j. Một truy vấn đưa ra hai điểm cuối u và v, cùng với ngưỡng rủi ro w."
date: "2026-08-17T22:07:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "F"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 88
verified: true
draft: false
---

[CF 102222F - Tiếp tục](https://codeforces.com/problemset/problem/102222/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ có trọng số vô hướng hoàn chỉnh của các thành phố. Thành phố`i`có giá trị rủi ro`r[i]`, Và`d[i][j]`là khoảng cách di chuyển trực tiếp giữa các thành phố`i`Và`j`. Một truy vấn đưa ra hai điểm cuối`u`Và`v`, cùng với ngưỡng rủi ro`w`. 

Đối với truy vấn đó, chúng tôi muốn con đường ngắn nhất từ`u`ĐẾN`v`theo một hạn chế: mọi thành phố trung gian trên tuyến đường tối đa phải có rủi ro`w`. Bản thân các điểm cuối đều được phép bất kể rủi ro của chúng là gì, vì hạn chế áp dụng cho các thành phố không phải là hai điểm cuối. Cạnh trực tiếp từ`u`ĐẾN`v`do đó luôn là một ứng cử viên hợp lệ khi`u != v`. 

Đầu vào chứa tới 50 trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm có tối đa 200 thành phố và tối đa 20.000 truy vấn. Ma trận khoảng cách đã đưa ra một cạnh giữa mỗi cặp thành phố, do đó biểu đồ dày đặc. Từ`n`chỉ có 200, một`O(n^3)`thuật toán tiền xử lý là thực tế. Số lượng truy vấn lớn sẽ thay đổi điều chúng ta nên tránh: việc chạy tính toán đường đi ngắn nhất đầy đủ một cách độc lập cho mỗi truy vấn sẽ quá tốn kém. 

Một cách hữu ích để ước tính chi phí cưỡng bức là lấy các giá trị lớn nhất,`n = 200`Và`q = 20,000`. Một phép tính Floyd-Warshall cần khoảng`200^3 = 8,000,000`lặp lại thư giãn cho một truy vấn. Việc lặp lại điều đó cho tất cả các truy vấn sẽ mang lại khoảng`1.6 * 10^11`lặp lại trong trường hợp xấu nhất, trước khi tính đến chi phí Python. Chúng ta cần thực hiện tính toán đồ thị đắt tiền không phụ thuộc vào số lượng truy vấn. 

Có một số trường hợp khó xử lý. Đầu tiên, các điểm cuối không phải thỏa mãn ngưỡng. Ví dụ, hãy xem xét```
1
2 1
10 1
0 5
5 0
1 2 1
```Không có thành phố trung gian nào cả nên đường đi thẳng từ thành phố 1 đến thành phố 2 là hợp lệ và đáp án là`5`. Một giải pháp loại bỏ mọi thành phố có nguy cơ vượt quá`w`, bao gồm cả các điểm cuối, sẽ khai báo không chính xác tuyến đường không thể thực hiện được. 

Thứ hai, điểm cuối có thể là cùng một thành phố. Ví dụ,```
1
1 1
100
0
1 1 1
```Câu trả lời là`0`. Thành phố không cần phải đáp ứng ngưỡng vì chúng ta đã đến đích và không có thành phố trung gian nào được sử dụng. 

Thứ ba, một đường dẫn có thể sử dụng một số thành phố trung gian được phép. Ví dụ,```
1
3 1
1 1 2
0 10 100
10 0 10
100 10 0
1 3 1
```Có ngưỡng`1`, thành phố 2 được phép là thành phố trung gian nên tuyến đường`1 -> 2 -> 3`chi phí`20`, tốt hơn chi phí trực tiếp`100`. Một giải pháp chỉ kiểm tra các cạnh trực tiếp hoặc chỉ một thành phố trung gian mà không cho phép lặp lại việc nới lỏng Floyd-Warshall, sẽ bỏ lỡ cải tiến này. 

Cụm từ "thành phố khác" cũng có nghĩa là ngưỡng là hạn chế đối với các đỉnh trung gian, không phải trên toàn bộ tập đỉnh của đường đi. Sự khác biệt này là chi tiết trung tâm đằng sau giải pháp. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xử lý mọi truy vấn một cách độc lập. Đối với một truy vấn`(u, v, w)`, chúng ta có thể bắt đầu với ma trận khoảng cách ban đầu và chạy Floyd-Warshall trong khi chỉ cho phép các thành phố có`r[i] <= w`như các đỉnh trung gian. Floyd-Warshall ở đây đúng vì sau khi xử lý một tập hợp các thành phố trung gian được phép,`dist[i][j]`đại diện cho con đường ngắn nhất từ`i`ĐẾN`j`các đỉnh trong của nó thuộc tập hợp đó. 

Vấn đề là công việc lặp đi lặp lại. Với`n = 200`, chi phí cho một lần chạy Floyd-Warshall`O(n^3) = O(8 * 10^6)`. Với`q = 20,000`truy vấn, trường hợp xấu nhất là`O(qn^3)`, xung quanh`1.6 * 10^11`hoạt động thư giãn. Bản thân biểu đồ không thay đổi giữa các truy vấn, chỉ có rủi ro tối đa được phép mới thay đổi, vì vậy việc tính toán lại các khoảng thời gian giãn nở giống nhau là lãng phí. 

Quan sát quan trọng là tập hợp các thành phố trung gian được phép phát triển đơn điệu như`w`tăng lên. Giả sử chúng ta sắp xếp tất cả các thành phố theo mức độ rủi ro của chúng. Đối với một ngưỡng nhỏ, có lẽ chỉ có thành phố an toàn nhất mới có thể ở mức trung gian. Khi ngưỡng tăng đủ để bao gồm thành phố tiếp theo, chúng ta không cần tính toán lại tất cả công việc trước đó. Chúng ta có thể lấy ma trận đường đi ngắn nhất hiện tại và thực hiện chính xác một giai đoạn Floyd-Warshall bằng cách sử dụng thành phố mới được kích hoạt. 

Đây chính xác là cấu trúc của Floyd-Warshall. Của nó`k`-giai đoạn thứ thêm đỉnh`k`tới tập các đỉnh có thể được sử dụng nội bộ. Ở đây, chúng tôi chỉ đơn giản chọn thứ tự của các giai đoạn đó theo rủi ro của thành phố hơn là chỉ số thành phố. 

Chúng ta có thể xử lý các truy vấn theo thứ tự tăng dần`w`. Trong khi di chuyển qua các truy vấn đã được sắp xếp, chúng tôi dần dần kích hoạt mọi thành phố có mức độ rủi ro cao nhất là ngưỡng của truy vấn hiện tại. Bất cứ khi nào một thành phố bắt đầu hoạt động, chúng tôi sẽ thực hiện một giai đoạn thư giãn Floyd-Warshall xuyên suốt thành phố đó. Sau tất cả các thành phố có`r[i] <= w`đã được kích hoạt, ma trận chứa các đường đi ngắn nhất sử dụng chính xác các thành phố trung gian được truy vấn đó cho phép. 

Các điểm cuối cần được xem xét đặc biệt về mặt khái niệm nhưng không cần sửa đổi đặc biệt trong ma trận. Kể cả nếu`u`hoặc`v`có rủi ro lớn hơn`w`, cạnh trực tiếp`u -> v`vẫn hiện diện và ma trận Floyd-Warshall được phép sử dụng điểm cuối làm nguồn hoặc đích. Chúng tôi chỉ kiểm soát những đỉnh nào được sử dụng làm đỉnh trung gian. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(qn^3)`|`O(n^2)`| Quá chậm | 
| Tối ưu |`O(n^3 + q log q)`|`O(n^2 + q)`| Đã chấp nhận | 

các`q log q`thuật ngữ xuất phát từ việc sắp xếp các truy vấn. Các giai đoạn Floyd-Warshall góp phần`O(n^3)`bởi vì mỗi trong số`n`thành phố được kích hoạt đúng một lần. 

## Hướng dẫn thuật toán 

1. Đọc rủi ro của mọi thành phố và ma trận khoảng cách hoàn chỉnh. Giữ ma trận làm ma trận đường đi ngắn nhất hiện tại, ban đầu chỉ chứa khoảng cách di chuyển trực tiếp. 
2. Sắp xếp các chỉ số thành phố theo giá trị rủi ro của chúng. Thứ tự sắp xếp xác định thời điểm mỗi thành phố trở thành đỉnh trung gian. 
3. Đọc tất cả các truy vấn và lưu trữ từng truy vấn cùng với vị trí ban đầu của nó. Sắp xếp các truy vấn theo ngưỡng của chúng`w`, bởi vì việc xử lý chúng theo thứ tự này có nghĩa là tập hợp các thành phố trung gian được phép chỉ tăng lên. 
4. Duy trì một con trỏ vào danh sách thành phố đã được sắp xếp. Đối với ngưỡng truy vấn hiện tại`w`, kích hoạt mọi thành phố có rủi ro cao nhất`w`. Đối với mỗi thành phố mới được kích hoạt`k`, thực hiện động tác thư giãn Floyd-Warshall```
dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```cho mỗi cặp`i, j`. 

Thứ tự quan trọng ở đây. Một thành phố được kích hoạt một lần và việc nới lỏng nó được thực hiện sau khi tất cả các thành phố có rủi ro nhỏ hơn đã được kích hoạt. Điều này phù hợp với bất biến Floyd-Warshall tiêu chuẩn, với thứ tự rủi ro thay thế thứ tự đỉnh bằng số. 
5. Sau tất cả các thành phố có mức độ rủi ro cao nhất`w`đã được kích hoạt,`dist[u][v]`là câu trả lời cho truy vấn. Lưu trữ nó ở vị trí ban đầu của truy vấn để đầu ra cuối cùng vẫn theo thứ tự đầu vào. 
6. In câu trả lời theo thứ tự truy vấn ban đầu, trước câu hỏi bắt buộc`Case #x:`tiêu đề. 

### Tại sao nó hoạt động 

Hãy xem xét khoảnh khắc ngay sau một thành phố`k`đã được kích hoạt. Tất cả các thành phố có rủi ro nhỏ hơn hoặc bằng nhau đã được kích hoạt trước đó đều đã có sẵn dưới dạng các đỉnh trung gian. Trước khi xử lý`k`,`dist[i][j]`là con đường ngắn nhất mà các thành phố trung gian thuộc về tập hợp đã được kích hoạt. Bất kỳ đường dẫn mới được cải tiến nào có thể thực hiện được sau khi thêm`k`có hình thức`i -> ... -> k -> ... -> j`, trong đó cả hai bên chỉ sử dụng các thành phố trung gian đã kích hoạt trước đó. Việc nới lỏng Floyd-Warshall kiểm tra chính xác khả năng đó thông qua`dist[i][k] + dist[k][j]`. 

Bằng cách quy nạp theo thứ tự kích hoạt, sau khi kích hoạt mọi thành phố có rủi ro cao nhất`w`,`dist[i][j]`là con đường ngắn nhất mà các thành phố trung gian đều gặp rủi ro nhiều nhất`w`. Vì các điểm cuối không bị hạn chế nên đây chính xác là tập hợp các tuyến đường được truy vấn cho phép. Như vậy`dist[u][v]`là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    output = []

    for case_id in range(1, t + 1):
        n, q = map(int, input().split())
        risk = list(map(int, input().split()))

        dist = [list(map(int, input().split())) for _ in range(n)]

        cities = sorted(range(n), key=risk.__getitem__)

        queries = []
        for idx in range(q):
            u, v, w = map(int, input().split())
            queries.append((w, u - 1, v - 1, idx))

        queries.sort()

        ans = [0] * q
        city_ptr = 0

        for w, u, v, idx in queries:
            while city_ptr < n and risk[cities[city_ptr]] <= w:
                k = cities[city_ptr]

                dk = dist[k]

                for i in range(n):
                    di = dist[i]
                    dik = di[k]

                    for j in range(n):
                        candidate = dik + dk[j]
                        if candidate < di[j]:
                            di[j] = candidate

                city_ptr += 1

            ans[idx] = dist[u][v]

        output.append(f"Case #{case_id}:")
        output.extend(map(str, ans))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Phần chính đầu tiên của quá trình triển khai sẽ đọc ma trận trực tiếp vào`dist`. Chúng tôi thay đổi ma trận này trong suốt thuật toán, vì vậy sau khi một thành phố được kích hoạt,`dist`chứa khoảng cách tốt nhất bằng cách sử dụng tất cả các thành phố trung gian hiện có. 

các`cities`mảng lưu trữ các chỉ số thành phố thay vì bản thân các giá trị rủi ro. Sắp xếp nó theo`risk.__getitem__`đưa ra thứ tự chính xác mà các đỉnh sẽ được đưa vào Floyd-Warshall. 

Truy vấn được lưu trữ dưới dạng`(w, u, v, original_index)`và sắp xếp theo`w`. Chỉ mục ban đầu là cần thiết vì việc sắp xếp sẽ thay đổi thứ tự tính toán các câu trả lời, trong khi đầu ra phải tuân theo thứ tự của đầu vào. 

các`while`vòng lặp kích hoạt tất cả các thành phố thỏa mãn`risk[city] <= w`. Sự so sánh là có chủ ý`<=`, không`<`. Một thành phố có rủi ro bằng ngưỡng truy vấn được cho phép. 

Đối với mỗi thành phố được kích hoạt`k`, các vòng lặp lồng nhau thực hiện một giai đoạn Floyd-Warshall hoàn chỉnh. Các biến`dk`,`di`, Và`dik`là các tham chiếu cục bộ được sử dụng để giảm chi phí lập chỉ mục Python. Vì giới hạn thời gian tương đối rộng rãi đối với C++ nhưng Python có chi phí cho mỗi hoạt động cao hơn đáng kể, nên những tối ưu hóa nhỏ này sẽ quan trọng khi ma trận đạt kích thước tối đa. 

Việc thư giãn sử dụng số nguyên Python, do đó không có vấn đề tràn số nguyên. Một đường đi đơn giản ngắn nhất sử dụng tối đa`n - 1`các cạnh và với mỗi cạnh nhiều nhất`10^5`, dù sao thì câu trả lời cũng nằm trong phạm vi số nguyên của Python. 

Các điểm cuối không bao giờ bị loại bỏ dựa trên rủi ro của chúng. Ma trận luôn chứa mọi cạnh trực tiếp và thuật toán chỉ giới hạn các đỉnh được chọn làm điểm trung gian Floyd-Warshall. Do đó, một truy vấn như`u = 1`,`v = 2`,`w = 1`vẫn có hiệu lực ngay cả khi thành phố 1 gặp rủi ro`100`. 

Trường hợp tự truy vấn cũng hoạt động mà không cần xử lý đặc biệt vì`dist[u][u]`bắt đầu từ số 0 và không thể trở thành dương thông qua việc nới lỏng đường đi ngắn nhất. 

## Ví dụ đã hoạt động 

Chỉ có một mẫu được cung cấp trong câu lệnh, vì vậy dấu vết thứ hai bên dưới sử dụng đầu vào được xây dựng nhỏ để hiển thị hành vi ngưỡng điểm cuối. 

### Mẫu 1 

Những rủi ro là`[1, 2, 3]`, và khoảng cách trực tiếp là:```
0 1 3
1 0 1
3 1 0
```Các truy vấn được xử lý theo thứ tự ngưỡng. Trạng thái liên quan là: 

| Ngưỡng truy vấn | Thành phố mới được kích hoạt | Khoảng cách liên quan | Trả lời | 
| --- | --- | --- | --- | 
|`1`| thành phố 1 |`dist[1][1] = 0`|`0`| 
|`1`| không |`dist[1][2] = 1`|`1`| 
|`1`| không |`dist[1][3] = 3`|`3`| 
|`2`| thành phố 2 |`dist[1][2] = 1`|`1`| 
|`2`| không |`dist[1][3] = 2`|`2`| 
|`2`| không |`dist[1][1] = 0`|`0`| 

Khi chỉ cho phép thành phố 1 làm trung gian, đường đi từ thành phố 1 đến thành phố 3 vẫn tốn phí`3`. Khi thành phố 2 có sẵn, việc thư giãn qua thành phố 2 sẽ thay đổi khoảng cách đó thành`1 + 1 = 2`. 

Thứ tự truy vấn trong đầu vào được giữ nguyên bằng cách sử dụng các chỉ mục gốc được lưu trữ, do đó kết quả đầu ra là:```
Case #1:
0
1
3
0
1
2
```### Ví dụ được xây dựng 

Hãy xem xét:```
1
3 3
5 1 1
0 10 100
10 0 10
100 10 0
1 3 1
1 3 5
1 1 1
```Thứ tự thành phố được sắp xếp theo rủi ro là thành phố 2, thành phố 3, thành phố 1, vì rủi ro của chúng là`1, 1, 5`. 

Trạng thái xử lý là: 

| Ngưỡng truy vấn | Thành phố mới được kích hoạt |`dist[1][3]`| Câu trả lời truy vấn | 
| --- | --- | --- | --- | 
|`1`| thành phố 2, thành phố 3 |`20`|`20`| 
|`1`| không |`20`|`0`vì`1 -> 1`| 
|`5`| thành phố 1 |`20`|`20`| 

Đối với ngưỡng`1`, thành phố 1 không phải là trung gian được phép, nhưng thành phố 2 thì có. Tuyến đường`1 -> 2 -> 3`chi phí`20`, vì vậy truy vấn đầu tiên được trả lời bằng`20`. 

Đối với việc tự truy vấn`1 -> 1`, nguy cơ thành phố 1 bị`5`là không liên quan. Không yêu cầu thành phố trung gian và vẫn giữ nguyên mục nhập chéo`0`. 

Khi ngưỡng trở thành`5`, thành phố 1 cũng được kích hoạt. Nó không thể cải thiện tuyến đường đã tối ưu trong ví dụ này, vì vậy khoảng cách vẫn còn`20`. 

Dấu vết này thể hiện cả quá trình kích hoạt đơn điệu và thực tế là rủi ro điểm cuối không hạn chế truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^3 + q log q)`| Mỗi thành phố gây ra một`O(n^2)`Giai đoạn Floyd-Warshall và tất cả các truy vấn được sắp xếp một lần | 
| Không gian |`O(n^2 + q)`| Ma trận khoảng cách lấy`O(n^2)`, trong khi các truy vấn và câu trả lời được lưu trữ mất`O(q)`| 

Vì`n <= 200`, quá trình tiền xử lý chỉ yêu cầu`n`Các giai đoạn của Floyd-Warshall, hoặc về`8 * 10^6`cặp thư giãn trong trường hợp thử nghiệm lớn nhất. Các truy vấn chỉ thêm công việc sắp xếp và tra cứu theo thời gian liên tục sau khi xử lý trước. Việc sử dụng bộ nhớ cũng khiêm tốn vì thuật toán chỉ giữ lại một`n x n`ma trận thay vì lưu trữ ma trận đường đi ngắn nhất riêng biệt cho mọi ngưỡng có thể. 

## Trường hợp thử nghiệm 

Bộ khai thác kiểm tra sau đây sử dụng phiên bản chức năng của cùng một thuật toán để có thể kiểm tra từng đầu vào một cách độc lập.```python
import sys
import io

def solve_input(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    try:
        input = sys.stdin.readline
        t = int(input())
        output = []

        for case_id in range(1, t + 1):
            n, q = map(int, input().split())
            risk = list(map(int, input().split()))
            dist = [list(map(int, input().split())) for _ in range(n)]

            cities = sorted(range(n), key=risk.__getitem__)

            queries = []
            for idx in range(q):
                u, v, w = map(int, input().split())
                queries.append((w, u - 1, v - 1, idx))

            queries.sort()

            ans = [0] * q
            ptr = 0

            for w, u, v, idx in queries:
                while ptr < n and risk[cities[ptr]] <= w:
                    k = cities[ptr]
                    dk = dist[k]

                    for i in range(n):
                        di = dist[i]
                        dik = di[k]

                        for j in range(n):
                            nd = dik + dk[j]
                            if nd < di[j]:
                                di[j] = nd

                    ptr += 1

                ans[idx] = dist[u][v]

            output.append(f"Case #{case_id}:")
            output.extend(map(str, ans))

        return "\n".join(output)
    finally:
        sys.stdin = old_stdin

# Provided sample.
sample1 = """\
1
3 6
1 2 3
0 1 3
1 0 1
3 1 0
1 1 1
1 2 1
1 3 1
1 1 2
1 2 2
1 3 2
"""

assert solve_input(sample1) == """\
Case #1:
0
1
3
0
1
2
""", "sample 1"

# Minimum-size graph, self query and a direct query.
case2 = """\
1
1 2
7
0
1 1 1
1 1 7
"""

assert solve_input(case2) == """\
Case #1:
0
0
""", "minimum-size graph"

# Endpoint may have risk greater than the threshold.
case3 = """\
1
2 2
100 1
0 5
5 0
1 2 1
1 2 100
"""

assert solve_input(case3) == """\
Case #1:
5
5
""", "endpoint risk must not block direct travel"

# Equality boundary and a useful intermediate city.
case4 = """\
1
3 3
5 2 7
0 10 100
10 0 10
100 10 0
1 3 1
1 3 2
1 3 7
"""

assert solve_input(case4) == """\
Case #1:
100
20
20
""", "risk == w must be allowed"

# All risks equal, so every city is activated for the threshold.
case5 = """\
1
4 3
10 10 10 10
0 8 50 50
8 0 7 50
50 7 0 6
50 50 6 0
1 4 1
1 4 10
2 4 10
"""

assert solve_input(case5) == """\
Case #1:
50
21
13
""", "all equal risks"
```Các trường hợp tùy chỉnh có thể được tóm tắt như sau: 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`case2`|`0, 0`| Đồ thị và khoảng cách đường chéo nhỏ nhất có thể | 
|`case3`|`5, 5`| Điểm cuối có thể vượt quá ngưỡng | 
|`case4`|`100, 20, 20`| Chính xác`risk == w`ranh giới và thành phố trung gian mới được kích hoạt | 
|`case5`|`50, 21, 13`| Mọi rủi ro đều như nhau và được lặp đi lặp lại Cải tiến Floyd-Warshall | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là điểm cuối có rủi ro vượt quá ngưỡng. TRONG`case3`, thành phố 1 có nguy cơ`100`, trong khi ngưỡng truy vấn chỉ là`1`. Biểu đồ có hai thành phố và khoảng cách trực tiếp`5`. Không cần thành phố trung gian nên câu trả lời là`5`. Vòng kích hoạt chỉ kiểm soát thành phố nào có thể đóng vai trò`k`trong giai đoạn Floyd-Warshall. Nó không bao giờ loại bỏ thành phố 1 khỏi ma trận, vì vậy`dist[0][1]`còn lại`5`. 

Trường hợp thứ hai là một truy vấn từ một thành phố đến chính nó. TRONG`case2`, thành phố duy nhất gặp rủi ro`7`, trong khi truy vấn đầu tiên sử dụng ngưỡng`1`. Không cần thành phố nào làm đỉnh trung gian vì thành phố xuất phát đã là đích đến. Mục nhập chéo bắt đầu như`0`và mọi bản cập nhật Floyd-Warshall đều có dạng`dist[i][i] <= dist[i][k] + dist[k][i]`, do đó số 0 vẫn là tối ưu. Câu trả lời là do đó`0`. 

Trường hợp cạnh thứ ba là ranh giới đẳng thức. TRONG`case4`, thành phố 2 chính xác có nguy cơ`2`và ngưỡng truy vấn là`2`. Điều kiện trong vòng kích hoạt là`risk[cities[ptr]] <= w`, vì vậy thành phố 2 sẽ có sẵn. Tuyến đường`1 -> 2 -> 3`chi phí`10 + 10 = 20`, cải thiện khoảng cách trực tiếp`100`. sử dụng`< w`thay vào đó sẽ vô hiệu hóa thành phố 2 và quay trở lại`100`. 

Trường hợp thứ tư là có thể cần nhiều thành phố trung gian. TRONG`case5`, tuyến đường từ thành phố 1 đến thành phố 4 có thể sử dụng cả thành phố 2 và thành phố 3, cho`8 + 7 + 6 = 21`khi tất cả các thành phố đều có sẵn. Đối với truy vấn từ thành phố 2 đến thành phố 4, tuyến đường tốt nhất là`2 -> 3 -> 4`, tính chi phí`7 + 6 = 13`. Các giai đoạn Floyd-Warshall tuần tự khám phá những con đường này vì mỗi giai đoạn cho phép thành phố mới được kích hoạt được kết hợp với những con đường đã được hình thành qua các thành phố trước đó. 

Sự tinh tế cuối cùng là thứ tự truy vấn. Các truy vấn được sắp xếp theo ngưỡng nội bộ, do đó ma trận phát triển theo đúng thứ tự đơn điệu. Các chỉ số ban đầu của chúng được giữ lại và câu trả lời được ghi lại vào các vị trí đó. Nếu không có chỉ mục đó, các giá trị được tính toán sẽ đúng nhưng có thể xuất hiện sai thứ tự ở đầu ra.
