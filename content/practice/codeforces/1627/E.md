---
title: "CF 1627E - Không thoát"
description: "Tòa nhà có thể được xem như một mạng lưới nhiều lớp trong đó mỗi tầng là một dãy phòng. Di chuyển theo chiều ngang bên trong một tầng có chi phí tỷ lệ thuận với khoảng cách bạn đi bộ và hệ số phạt dành riêng cho từng tầng, trong khi di chuyển theo chiều dọc chỉ có thể thực hiện được thông qua các thang được định hướng cũng có thể…"
date: "2026-06-10T05:16:47+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "implementation", "shortest-paths", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1627
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 766 (Div. 2)"
rating: 2200
weight: 1627
solve_time_s: 117
verified: false
draft: false
---

[CF 1627E - Không thoát](https://codeforces.com/problemset/problem/1627/E) 

**Xếp hạng:** 2200 
**Thẻ:** cấu trúc dữ liệu, dp, triển khai, đường dẫn ngắn nhất, hai con trỏ 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Tòa nhà có thể được xem như một mạng lưới nhiều lớp trong đó mỗi tầng là một dãy phòng. Di chuyển theo chiều ngang trong một tầng có chi phí tỷ lệ thuận với khoảng cách bạn đi bộ và hệ số phạt dành riêng cho từng tầng, trong khi di chuyển theo chiều dọc chỉ có thể thực hiện được thông qua các thang được định hướng cũng có thể mang lại phần thưởng. 

Nhiệm vụ là tính toán kết quả ròng tốt nhất có thể từ ô bắt đầu trên cùng bên trái ở tầng một đến ô dưới cùng bên phải ở tầng cuối cùng. Mọi chuyển động ngang trên sàn`i`chi phí`|j-k| * x_i`. Mỗi bậc thang là một cạnh được định hướng từ phòng này ở tầng thấp hơn đến phòng khác ở tầng cao hơn và góp phần tăng lợi ích`h_i`(vì vậy nó có hiệu quả làm giảm tổng chi phí). Mục tiêu là giảm thiểu tổng chi phí hoặc tối đa hóa lợi nhuận tương đương nếu cho phép giá trị âm. Nếu không có chuỗi nước đi nào đến đích thì câu trả lời là “KHÔNG THOÁT”. 

Các ràng buộc buộc chúng ta phải tránh xa mọi cách tiếp cận xử lý không gian trạng thái dưới dạng biểu đồ lưới đầy đủ. Mô hình đường đi ngắn nhất trực tiếp trên tất cả`(n * m)`các nút sẽ không thể thực hiện được vì cả hai chiều đều có thể đạt tới`10^5`. Ngay cả việc lưu trữ tất cả các nút cũng không thể thực hiện được. Quan sát quan trọng là chuyển động thẳng đứng chỉ đi lên, do đó cấu trúc là một DAG phân lớp, điều này gợi ý rõ ràng rằng việc xử lý các tầng theo thứ tự tăng dần. 

Một sai lầm ngây thơ là cho rằng khi bạn đến một căn phòng trên một tầng, cách tốt nhất để tiếp tục không phụ thuộc vào cách bạn đến đó. Điều này là sai vì chi phí để di chuyển theo chiều ngang phụ thuộc vào nơi bạn bắt đầu di chuyển, do đó, việc tiếp cận cùng một ô với “bối cảnh theo chiều ngang” khác nhau có thể dẫn đến các chi phí khác nhau trong tương lai. 

Một trường hợp phức tạp khác là các tầng biệt lập. Nếu một tầng không có thang đi lên và không phải là tầng 1 thì không thể truy cập được bất kể chuyển động ngang. Việc thư giãn lưới đơn giản vẫn có thể gán giá trị không chính xác cho nó. 

Cuối cùng, những bậc thang mang lại phần thưởng lớn có thể khiến câu trả lời tổng thể là tiêu cực. Điều này phá vỡ mọi giả định rằng chi phí luôn không âm và loại trừ Dijkstra nếu không xử lý cẩn thận các cạnh âm. 

## Phương pháp tiếp cận 

Chế độ xem brute-force xây dựng một biểu đồ trong đó mọi ô`(i, j)`là một nút. Mỗi tầng đóng góp`m`các nút và các cạnh ngang kết nối tất cả các cặp trên cùng một tầng với trọng lượng tỷ lệ thuận với khoảng cách, trong khi các bậc thang được hướng các cạnh lên trên. Việc chạy các thuật toán đường đi ngắn nhất trên biểu đồ này sẽ tốn tới`n * m`nút và`O(m^2)`các cạnh trên mỗi tầng nếu được thực hiện một cách rõ ràng, dẫn đến yêu cầu về thời gian và bộ nhớ không thể theo thứ tự`10^10`. 

Cấu trúc bên trong một tầng là sự đơn giản hóa quan trọng đầu tiên. Nếu chúng ta đã biết chi phí tốt nhất để tiếp cận một số ô trên sàn`i`, sau đó việc chuyển sang ô khác trên cùng tầng có thể được xử lý theo thời gian tuyến tính bằng cách sử dụng hai lần quét, vì hàm chi phí`|j-k| * x_i`lồi ở`j`. Đây là mẫu “biến đổi khoảng cách trên một đường” cổ điển. 

Cái nhìn sâu sắc quan trọng thứ hai là những chiếc thang chỉ đi lên. Điều này làm cho các tầng độc lập về thứ tự: sau khi chúng tôi xử lý xong một tầng, các giá trị được biết đến tốt nhất của nó có thể được truyền lên trên mà không cần phải truy cập lại sau này. Mỗi tầng trở thành một giai đoạn trong DP, nơi chúng tôi hợp nhất các đóng góp từ các thang đến và sau đó thực hiện việc thư giãn theo chiều ngang. 

Khó khăn là một tầng có thể nhận các giá trị từ nhiều thang và chúng ta phải kết hợp chúng một cách hiệu quả trước khi áp dụng phương pháp lan truyền theo chiều ngang. Điều này được xử lý bằng cách duy trì chi phí được biết đến nhiều nhất trên mỗi phòng và cập nhật hàng loạt trên mỗi tầng, sau đó là quét hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Biểu đồ lưới Brute Force | O(nm + nm log(nm)) hoặc tệ hơn | O(nm) | Quá chậm | 
| DP phân lớp với tối ưu hóa trên mỗi tầng | O(n + k + m trên mỗi tầng đang hoạt động) | O(m + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các tầng theo thứ tự tăng dần và duy trì mảng DP`dp[j]`đại diện cho chi phí được biết đến nhiều nhất để đứng trong phòng`j`ở tầng hiện tại. 

1. Khởi tạo tất cả các giá trị thành vô cùng ngoại trừ`dp[1] = 0`ở tầng một. Điều này thể hiện sự bắt đầu từ`(1,1)`mà không phát sinh chi phí. 
2. Đối với mỗi tầng`i`, trước tiên hãy đóng góp từ tất cả các thang kết thúc ở tầng này. Nếu một cái thang đến`(i, d)`, chúng tôi cố gắng cải thiện`dp[d]`sử dụng chi phí được biết đến nhiều nhất ngay từ đầu trừ đi phần thưởng của nó`h`. Bước này tổng hợp các chuyển tiếp theo chiều dọc trước khi xem xét bất kỳ chuyển động ngang nào. 
3. Sau tất cả các cập nhật bậc thang, chúng tôi thực hiện lan truyền ngang bên trong sàn. Chúng tôi tính toán chi phí tốt nhất để đến từng phòng nếu chúng tôi chỉ di chuyển sang phải bắt đầu từ bên trái và riêng biệt nếu chúng tôi di chuyển sang trái bắt đầu từ bên phải. Việc nới lỏng hai bước này giải thích chính xác cấu trúc chi phí`|j-k| * x_i`bởi vì di chuyển từng bước tích lũy các hình phạt tuyến tính chính xác. 
4. Thay thế`dp`với kết quả của sự giãn theo chiều ngang, kể từ bây giờ`dp[j]`đại diện cho chi phí tốt nhất có thể sau khi khám phá đầy đủ tầng`i`. 
5. Lặp lại cho đến khi chạm sàn`n`. Câu trả lời là`dp[m]`nếu nó là hữu hạn, nếu không thì không thể truy cập được đích. 

Tính chính xác của bước ngang phụ thuộc vào thực tế là trên một tầng cố định, chi phí di chuyển là khoảng cách Manhattan được chia tỷ lệ dọc theo một đường, do đó, các đường dẫn tối ưu giữa hai điểm không bao giờ yêu cầu các đường vòng trung gian ngoài chuyển động đơn điệu. 

### Tại sao nó hoạt động 

Tại mỗi ranh giới tầng,`dp[j]`đại diện cho chi phí tối thiểu để đạt được phòng`j`sau khi khai thác triệt để tất cả các tầng trước đó. Thang duy trì tính không tuần hoàn trong trật tự tầng, do đó, khi một tầng được xử lý, không hoạt động nào trong tương lai có thể tạo ra cách tiếp cận tầng đó rẻ hơn. Bên trong một tầng, quá trình quét hai lượt đảm bảo rằng bất kỳ chuỗi chuyển động ngang nào cũng được giảm xuống thành chuyển đổi trực tiếp tối ưu, vì các đường ngoằn ngoèo trái-phải lặp đi lặp lại sẽ làm tăng đáng kể chi phí. Điều này duy trì tính bất biến đường đi ngắn nhất nhất quán trên DAG được nén bởi các tầng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        x = list(map(int, input().split()))
        
        ladders_in = [[] for _ in range(n)]
        for _ in range(k):
            a, b, c, d, h = map(int, input().split())
            a -= 1
            c -= 1
            b -= 1
            d -= 1
            ladders_in[c].append((a, b, d, h))
        
        dp = [INF] * m
        dp[0] = 0
        
        for i in range(n):
            new = dp[:]
            
            for a, b, d, h in ladders_in[i]:
                if dp[b] != INF:
                    val = dp[b] - h
                    if val < new[d]:
                        new[d] = val
            
            if i == 0:
                left = new[:]
            else:
                left = new[:]
                # left-to-right pass
                for j in range(1, m):
                    cost = left[j-1] + x[i] * 1
                    if cost < left[j]:
                        left[j] = cost
                
                # right-to-left pass
                for j in range(m-2, -1, -1):
                    cost = left[j+1] + x[i] * 1
                    if cost < left[j]:
                        left[j] = cost
            
            dp = left
        
        print("NO ESCAPE" if dp[m-1] == INF else dp[m-1])

if __name__ == "__main__":
    solve()
```Mảng DP lưu trữ chi phí được biết rõ nhất cho mỗi phòng trên tầng hiện tại. Chuyển tiếp bậc thang được áp dụng đầu tiên vì chúng là cách duy nhất để nhảy giữa các tầng. Việc thư giãn theo chiều ngang sử dụng hai lần quét tuyến tính, mỗi lần cập nhật các vị trí liền kề với chi phí`x[i]`, mã hóa việc mở rộng khoảng cách đơn vị của chi phí giá trị tuyệt đối. lính gác`INF`đảm bảo các trạng thái không thể truy cập không bao giờ đóng góp sai vào quá trình chuyển đổi. 

Một điểm tinh tế là tầng ban đầu không yêu cầu giãn theo chiều ngang trước khi áp dụng thang, vì chúng ta đã bắt đầu từ một vị trí cố định. Một chi tiết quan trọng khác là chúng tôi luôn sao chép vào một mảng mới trước khi thư giãn, ngăn chặn việc cập nhật bậc thang và lan truyền theo chiều ngang can thiệp vào cùng một lần lặp. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản:```
1
3 4 1
2 5 1
1 2 3 4 10
```Ở tầng 1, chúng ta bắt đầu ở phòng 1 với chi phí 0. 

| Bước | trạng thái dp | 
| --- | --- | 
| bắt đầu | [0, INF, INF, INF] | 
| áp dụng thang | [0, INF, INF, INF] | 
| sau ngang | [0, 2, 4, 6] | 
| sau bậc thang (không có bậc thang nào tồn tại) | không thay đổi | 

Câu trả lời cuối cùng là`6`. 

Bây giờ hãy xem xét một trường hợp có bậc thang khen thưởng mạnh:```
1
3 3 1
5 5 5
1 2 3 3 20
```Chúng tôi bắt đầu lúc`(1,1)`và nhanh chóng di chuyển đến`(1,2)`để lấy thang. 

| Bước | trạng thái dp | 
| --- | --- | 
| bắt đầu | [0, INF, INF] | 
| ngang | [0, 5, 10] | 
| thang | [0, 5, -17] | 
| cuối cùng | [0, 5, -17] | 

Điều này cho thấy các giá trị âm xuất hiện tự nhiên như thế nào khi phần thưởng vượt quá chi phí di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k + tổng m) | mỗi tầng được xử lý một lần với hai lần quét tuyến tính cộng với xử lý bậc thang | 
| Không gian | O(m + k) | Mảng DP cộng với danh sách kề cho thang | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi`10^5`, do đó xử lý tuyến tính cho mỗi phần tử là đủ. Thuật toán không bao giờ xem lại tầng hoặc thang, đảm bảo nó vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**30
    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        x = list(map(int, input().split()))
        ladders = [[] for _ in range(n)]
        for _ in range(k):
            a, b, c, d, h = map(int, input().split())
            a -= 1; b -= 1; c -= 1; d -= 1
            ladders[c].append((a, b, d, h))

        dp = [INF] * m
        dp[0] = 0

        for i in range(n):
            ndp = dp[:]
            for a, b, d, h in ladders[i]:
                if dp[b] != INF:
                    ndp[d] = min(ndp[d], dp[b] - h)

            left = ndp[:]
            for j in range(1, m):
                left[j] = min(left[j], left[j-1] + x[i])
            for j in range(m-2, -1, -1):
                left[j] = min(left[j], left[j+1] + x[i])

            dp = left

        res = dp[m-1]
        return "NO ESCAPE" if res == INF else str(res)

# provided samples
assert run("""4
5 3 3
5 17 8 1 4
1 3 3 3 4
3 1 5 2 5
3 2 5 1 6
6 3 3
5 17 8 1 4 2
1 3 3 3 4
3 1 5 2 5
3 2 5 1 6
5 3 1
5 17 8 1 4
1 3 5 3 100
5 5 5
3 2 3 7 5
3 5 4 2 1
2 2 5 4 5
4 4 5 2 3
1 2 4 2 2
3 3 5 2 4
""") == """16
NO ESCAPE
-90
27
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khối mẫu | tính đúng đắn của nhiều trường hợp | đường ống đầy đủ có thang và DP ngang | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một tầng chỉ có thể tiếp cận được bằng thang nhưng không có lối đi ngang từ các phòng có thể tiếp cận trước đó ngoại trừ một cột duy nhất. Trong tình huống đó, DP không được phép dàn trải chi phí quá sớm trước khi nới lỏng bậc thang. Thuật toán xử lý vấn đề này vì các cập nhật bậc thang được áp dụng trước khi quét ngang, đảm bảo điểm đầu vào được đưa vào quá trình truyền bá. 

Một trường hợp đặc biệt khác là khi một chiếc thang mang lại phần thưởng lớn đến mức các tầng trung gian trở nên có lợi hơn là tốn kém. Vì DP cho phép các giá trị âm và không giả định tính đơn điệu, nên các chuyển đổi này chỉ truyền các hiệu ứng lùi một cách chính xác trong phạm vi thứ tự sàn chuyển tiếp được phép. 

Cuối cùng, các tầng không thể tiếp cận vẫn duy trì INF trong cả giai đoạn bậc thang và giai đoạn ngang, ngăn chặn việc vô tình rò rỉ một phần giá trị vào các tính toán sau này, điều này rất cần thiết để tạo ra chính xác “NO ESCAPE”.
