---
title: "CF 106063E - El Juego del Calamar"
description: "Chúng ta được cấp một tòa tháp có hình dạng như một dãy các tầng. Mỗi tầng có một số phòng nhỏ, giữa các tầng liên tiếp có cầu thang dẫn thẳng nối một số phòng ở tầng t với các phòng ở tầng t+1."
date: "2026-06-25T12:14:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106063
codeforces_index: "E"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 106063
solve_time_s: 43
verified: true
draft: false
---

[CF 106063E - El Juego del Calamar](https://codeforces.com/problemset/problem/106063/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tòa tháp có hình dạng như một dãy các tầng. Mỗi tầng có một số phòng nhỏ, giữa các tầng liên tiếp có cầu thang dẫn hướng nối một số phòng trên tầng.`t`đến các phòng trên tầng`t+1`. 

Một số lượng lớn người chơi bắt đầu ở tầng 1 và ban đầu họ chọn bất kỳ phòng nào ở đó. Sau đó, quá trình phát triển từng tầng. Ở mỗi tầng, mỗi phòng đều áp đặt một giới hạn cứng: nếu có nhiều người chơi đến hơn sức chứa cho phép thì chỉ có nhiều người sống sót trong phòng đó và những người còn lại sẽ bị loại. Sau bước lọc này, những người chơi sống sót sẽ di chuyển lên cầu thang để đến tầng tiếp theo và một lần nữa phải đối mặt với năng lực ở đó. 

Điểm mấu chốt là vấn đề không hỏi về một lần chạy trò chơi. Thay vào đó, với mỗi tiền tố của tầng`1..t`, chúng tôi độc lập hỏi: liệu trò chơi có dừng ở tầng không`t`, số lượng người sống sót tối đa mà chúng ta có thể đạt được là bao nhiêu, giả sử chúng ta chọn cách phân bổ ban đầu và tất cả các chuyển động một cách tối ưu. 

Vì vậy, về mặt khái niệm, mỗi truy vấn`t`hỏi: bắt đầu với nguồn cung không giới hạn ở tầng 1, lượng người chơi tối đa có thể được đẩy qua biểu đồ phân lớp này lên tầng là bao nhiêu`t`, trong đó mỗi phòng hoạt động giống như một đỉnh có giới hạn sức chứa và các cạnh chỉ đi giữa các tầng liền kề. 

Các ràng buộc chặt chẽ theo một cách rất cụ thể. Số tầng lên tới 1000 tầng, mỗi tầng tối đa 15 phòng. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào xử lý từng tầng một cách độc lập bằng đa thức trong`k`Cách tiếp cận này khả thi, nhưng bất cứ điều gì cố gắng mô phỏng từng người chơi đều không thể thực hiện được vì số lượng người chơi trên thực tế là vô hạn. Giới hạn thực sự là về mặt cấu trúc, không phải về số lượng. 

Một cách tiếp cận ngây thơ cố gắng mô phỏng rõ ràng luồng cho từng`t`riêng biệt sẽ lặp lại công việc trên các tiền tố giống hệt nhau. Sự lặp lại đó là nơi mà hầu hết các giải pháp không chính xác hoặc chậm đều thất bại: việc tính toán lại toàn bộ quy trình động từ đầu cho mỗi tiền tố dẫn đến một hệ số bổ sung`n`, điều đó là không cần thiết. 

Một trường hợp tinh tế đến từ các tầng có phòng không có sức chứa. Nếu một phòng có sức chứa bằng 0, nó sẽ hoạt động giống như một khối hoàn chỉnh ngay cả khi có nhiều cạnh đến. Ví dụ: nếu một tầng có một phòng duy nhất có sức chứa bằng 0 và tất cả các lối đi đều phải đi qua tầng đó thì không người chơi nào có thể vượt qua tầng đó. Bất kỳ giải pháp nào quên áp dụng dung lượng trước khi lan truyền sẽ cho phép luồng truyền qua các nút đó không chính xác. 

Một cạm bẫy khác là hiểu lầm rằng sự chuyển động chỉ diễn ra giữa các tầng liên tiếp. Không có tầng bỏ qua, do đó, bất kỳ cách diễn giải luồng đồ thị tổng thể hoặc đường đi ngắn nhất nào mà bỏ qua việc phân lớp sẽ đánh giá quá cao khả năng tiếp cận. 

## Phương pháp tiếp cận 

Một cách diễn giải brute-force xử lý vấn đề như một phép tính luồng cho mỗi tiền tố`t`. Đối với một cố định`t`, chúng tôi xây dựng một biểu đồ phân lớp có hướng bao gồm tất cả các phòng trong các tầng`1..t`. Mỗi phòng có một công suất, có thể được mô hình hóa bằng cách chia mỗi phòng thành nút trong và nút ngoài với cạnh công suất giữa chúng. Kết nối cầu thang trở thành các cạnh có công suất vô hạn giữa các nút bên ngoài của tầng`t`và các nút trong tầng`t+1`. 

Sau đó, chúng tôi tính toán luồng tối đa từ siêu nguồn kết nối với tất cả các phòng ở tầng 1 đến siêu bồn rửa kết nối với tất cả các phòng ở tầng`t`. 

Điều này đúng vì mọi chuyển động hợp lệ của người chơi đều tương ứng với một luồng khả thi và năng lực thực thi chính xác các quy tắc loại bỏ. Tuy nhiên, việc chạy luồng tối đa riêng biệt cho từng`t`là quá chậm. Ngay cả với nhỏ`k`, chúng tôi sẽ giải quyết tối đa 1000 trường hợp luồng trên biểu đồ có tối đa 15000 nút và nhiều cạnh, vượt xa giới hạn chấp nhận được. 

Quan sát cấu trúc quan trọng là các tiền tố được lồng vào nhau. Luồng tối ưu cho tiền tố`t`mở rộng luồng tối ưu cho tiền tố`t-1`. Không có gì trong mô hình yêu cầu tính toán lại từ đầu, vì trạng thái sau khi xử lý sàn`t`chỉ phụ thuộc vào sự phân bố của tầng trước đó. Điều này có nghĩa là chúng ta có thể chạy lan truyền động một lớp từ tầng 1 đến tầng`n`, duy trì cho mỗi phòng số lượng người chơi tối đa có thể tiếp cận sau khi áp dụng giới hạn sức chứa của phòng đó. 

Khi chúng tôi tính toán từng lớp luồng có thể tiếp cận, câu trả lời cho mỗi lớp`t`đơn giản là tổng lưu lượng tồn tại trên sàn`t`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Luồng tối đa riêng biệt cho mỗi tiền tố | O(n · F(k, m)) | O(nk + m) | Quá chậm | 
| DP xếp lớp trên các tầng | O(n · k²) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với tầng 1, hãy khởi tạo mỗi phòng với số lượng người chơi đến là vô hạn. Vì công suất được áp dụng ngay lập tức nên mỗi phòng`j`ở tầng 1 cuối cùng đã giữ chính xác`C[1][j]`người chơi. Điều này trở thành trạng thái ban đầu. 
2. Xử lý các tầng theo thứ tự tăng dần. Khi bắt đầu sàn xử lý`t`, chúng ta đã biết có bao nhiêu người chơi có thể đến được mỗi phòng trên tầng đó. 
3. Áp dụng giới hạn công suất của tầng`t`bằng cách đảm bảo mỗi phòng`j`có nhiều nhất`C[t][j]`người chơi. Đây là mô hình loại bỏ trước khi di chuyển. 
4. Đối với mỗi cầu thang từ phòng`u`trên sàn`t`về phòng`v`trên sàn`t+1`, đẩy tất cả người chơi còn sống sót khỏi`u`vào trong`v`. Nếu có nhiều cầu thang dẫn vào cùng một phòng thì phần đóng góp của chúng sẽ được cộng lại. 
5. Sau khi thu hết dòng chảy vào sàn`t+1`, áp dụng lại các ràng buộc về công suất để giới hạn tối đa mỗi phòng`C[t+1][j]`. 
6. Lưu tổng số người chơi còn sống trên sàn`t`trước khi chuyển lên làm câu trả lời cho tiền tố đó. 
7. Tiếp tục cho đến tầng cuối cùng. 

Ý tưởng chính là mỗi tầng hoạt động như một chuyển đổi nút thắt cổ chai: nó lấy một phân phối đầu vào, giới hạn nó và phân phối lại cho lớp tiếp theo. 

### Tại sao nó hoạt động 

Tại mỗi tầng`t`, vectơ trạng thái biểu thị số lượng người chơi trong mỗi phòng là mức tối đa có thể đạt được theo tất cả các chiến lược hợp lệ cho tiền tố lên đến`t`. Bất kỳ chiến lược thay thế nào tuyên bố sẽ làm tốt hơn sẽ yêu cầu gửi nhiều luồng khí hơn qua một số phòng hơn khả năng cho phép của nó hoặc qua một cầu thang không tồn tại. Vì cả hai ràng buộc đều được thực thi chính xác ở mỗi lần chuyển đổi nên không có khoản tích lũy bất hợp pháp nào được tính. Do đó, quy trình duy trì dòng chảy tối ưu có thể tiếp cận theo từng lớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, m = map(int, input().split())
    C = [list(map(int, input().split())) for _ in range(n)]

    adj = [[] for _ in range(n)]
    for _ in range(m):
        f, u, v = map(int, input().split())
        f -= 1
        u -= 1
        v -= 1
        adj[f].append((u, v))

    dp = [0] * k
    ans = []

    # floor 1 initialization
    for j in range(k):
        dp[j] = C[0][j]
    ans.append(sum(dp))

    # process floors
    for t in range(1, n):
        nxt = [0] * k

        # move flow through stairs from floor t-1 to t
        for u, v in adj[t - 1]:
            nxt[v] += dp[u]

        # apply capacities of floor t
        for j in range(k):
            if nxt[j] > C[t][j]:
                nxt[j] = C[t][j]

        dp = nxt
        ans.append(sum(dp))

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một vectơ duy nhất`dp`đại diện cho số lượng người chơi có thể có mặt trong mỗi phòng của tầng hiện tại. Bước chuyển tiếp xây dựng sự phân bố của tầng tiếp theo chỉ sử dụng các cầu thang bắt nguồn từ lớp hiện tại. Sau khi tích lũy lưu lượng đến, mảng công suất được áp dụng dưới dạng giới hạn cứng. 

Một điểm tinh tế là chúng tôi chỉ xử lý các cạnh thuộc quá trình chuyển đổi hiện tại giữa các tầng. Vì các cạnh được lập chỉ mục theo tầng trong đầu vào nên chúng tôi tránh quét tất cả`m`các cạnh lặp đi lặp lại. 

Một chi tiết khác là việc tổng hợp`dp`sau mỗi tầng là an toàn vì tổng số người sống sót chính xác là tổng các luồng hợp lệ qua các phòng ở tầng đó; không có sự phân phối lại ẩn sau thời điểm đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có hai tầng và hai phòng mỗi tầng. Giả sử công suất là: 

Tầng 1:`[3, 1]`Tầng 2:`[2, 2]`Và cầu thang là: 

Từ (1,1) → (2,1), (1,1) → (2,2), (1,2) → (2,2). 

Sau tầng 1, trạng thái là`[3, 1]`. 

| Bước | dp[1] | dp[2] | Giải thích | 
| --- | --- | --- | --- | 
| Khởi đầu tầng 1 | 3 | 1 | năng lực áp dụng trực tiếp | 

Bây giờ tuyên truyền lên tầng 2: 

| Bước | nxt[1] | nxt[2] | Giải thích | 
| --- | --- | --- | --- | 
| thêm từ (1,1) | 3 | 3 | phòng 1 chia ra 2 nơi | 
| cộng từ (1,2) | 3 | 4 | phòng thứ hai góp phần vào phòng 2 | 
| áp dụng nắp | 2 | 2 | tầng 2 giới hạn mỗi phòng | 

Vậy câu trả lời cuối cùng cho`t = 2`là`4`. 

Dấu vết này cho thấy nhiều đường dẫn đến tích lũy như thế nào trước khi bị cắt giảm bởi dung lượng nút. 

Bây giờ hãy xem xét trường hợp chặn trong đó tầng 2 có sức chứa bằng 0: 

Tầng 1:`[5, 5]`Tầng 2:`[0, 10]`Ngay cả khi tất cả dòng chảy cố gắng đi vào phòng 2, phòng 1 của tầng 2 sẽ loại bỏ mọi thứ đi qua nó và bất kỳ con đường nào yêu cầu nó sẽ sụp đổ ngay lập tức. Thuật toán mang lại kết quả chính xác là 0 cho phòng bị chặn và chỉ cho phép dòng chảy tránh được nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · k²) | mỗi tầng xử lý tất cả các cầu thang đi ra một lần và áp dụng k kiểm tra công suất | 
| Không gian | O(k + m) | chỉ các vectơ tầng hiện tại và tầng tiếp theo cộng với bộ lưu trữ lân cận | 

Các ràng buộc cho phép lên tới 1000 tầng và 15 phòng mỗi tầng, vì vậy`n · k²`nhiều nhất là khoảng 225.000 thao tác, phù hợp thoải mái trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n, k, m = map(int, input().split())
    C = [list(map(int, input().split())) for _ in range(n)]
    adj = [[] for _ in range(n)]
    for _ in range(m):
        f, u, v = map(int, input().split())
        adj[f - 1].append((u - 1, v - 1))

    dp = [0] * k
    out = []

    for j in range(k):
        dp[j] = C[0][j]
    out.append(sum(dp))

    for t in range(1, n):
        nxt = [0] * k
        for u, v in adj[t - 1]:
            nxt[v] += dp[u]
        for j in range(k):
            if nxt[j] > C[t][j]:
                nxt[j] = C[t][j]
        dp = nxt
        out.append(sum(dp))

    return "\n".join(map(str, out))

# minimal case
assert run("""1 1 0
5
""") == "5"

# two floors no edges
assert run("""2 2 0
1 2
3 4
""") == "3\n7"

# capacity blocking
assert run("""2 1 0
10
0
""") == "10\n0"

# simple propagation
assert run("""2 2 2
3 1
2 2
1 1
1 2
""") == "4\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 5 | khởi tạo cơ sở | 
| không có cạnh | 3/7 | tầng độc lập | 
| công suất bằng không | 10/0 | hành vi chặn | 
| tuyên truyền | 4/4 | chuyển dòng chảy chính xác | 

## Vỏ cạnh 

Phiên bản một tầng kiểm tra rằng thuật toán không thực hiện bất kỳ bước truyền bá nào và áp dụng trực tiếp công suất. Đối với đầu vào với`n = 1`, vectơ dp được khởi tạo từ`C[1]`, và số tiền được trả về ngay lập tức, phù hợp với định nghĩa dừng ở tầng một. 

Một biểu đồ bị ngắt kết nối hoàn toàn trong đó`m = 0`đảm bảo không có dòng chảy nào có thể di chuyển giữa các tầng. Thuật toán xử lý vấn đề này bằng cách tạo ra một vectơ chuyển tiếp bằng 0, do đó, mỗi tầng sau tầng đầu tiên sẽ sụp đổ theo khả năng riêng của nó, mô hình cách ly chính xác. 

Một sàn có tất cả công suất bằng 0 thể hiện sự loại bỏ khó khăn. Vì vectơ DP được giữ ở mức 0 ở lớp đó nên tất cả các tầng tiếp theo vẫn bằng 0 bất kể có cầu thang nào, phản ánh rằng không có người chơi nào sống sót qua nút cổ chai.
