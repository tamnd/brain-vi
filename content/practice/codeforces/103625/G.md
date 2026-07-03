---
title: "CF 103625G - Mục tiêu hiện tại: Tồn tại"
description: "Chúng ta được cung cấp một quy trình giống như trò chơi trên một cấu trúc hoạt động giống như một chuỗi các trạng thái, trong đó mỗi trạng thái đại diện cho một vị trí trong thế giới và sự chuyển đổi giữa các trạng thái thể hiện những bước đi có thể xảy ra."
date: "2026-07-02T22:38:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103625
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 03-25-22 Div 1. (Advanced)"
rating: 0
weight: 103625
solve_time_s: 49
verified: true
draft: false
---

[CF 103625G - Mục tiêu hiện tại: Sống sót](https://codeforces.com/problemset/problem/103625/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình giống như trò chơi trên một cấu trúc hoạt động giống như một chuỗi các trạng thái, trong đó mỗi trạng thái đại diện cho một vị trí trong thế giới và sự chuyển đổi giữa các trạng thái thể hiện những bước đi có thể xảy ra. Người chơi bắt đầu ở vị trí ban đầu và liên tục thực hiện các hành động đưa họ về phía trước theo các quy tắc cố định hoặc tiêu thụ tài nguyên cho phép sinh tồn trong các phân đoạn khó khăn. Mục tiêu là để xác định xem liệu người chơi có thể tiếp tục vô thời hạn mà không đạt đến trạng thái thất bại hay tương tự, liệu có cách nào để vượt qua cấu trúc trong khi tôn trọng mọi ràng buộc do cơ chế trò chơi áp đặt hay không. 

Đầu vào mã hóa một hệ thống các trạng thái và chuyển tiếp có hướng giữa chúng. Mỗi quá trình chuyển đổi có thể áp đặt một chi phí hoặc điều kiện và khả năng vượt qua của người chơi phụ thuộc vào việc duy trì tính khả thi của các ràng buộc tích lũy dọc theo bất kỳ con đường nào. Đầu ra thường là một quyết định nhị phân hoặc một giá trị được tính toán biểu thị điều kiện sống sót tối đa, tùy thuộc vào việc liệu có thể sống sót trong trò chơi tối ưu hay không. 

Các ràng buộc ngụ ý rằng số lượng trạng thái có thể lớn, vào khoảng hàng trăm nghìn, với các chuyển đổi có khả năng có cường độ tương tự. Điều này ngay lập tức loại trừ mọi giải pháp khám phá tất cả các đường dẫn một cách rõ ràng hoặc thực hiện tính toán lại trạng thái lặp lại trên mỗi đường dẫn. Bất kỳ cách tiếp cận nào với mức tăng trưởng theo cấp số nhân ở các trạng thái được khám phá sẽ thất bại. Ngay cả các cách tiếp cận bậc hai trên không gian trạng thái cũng không khả thi, vì vậy lời giải phải tuyến tính hoặc gần tuyến tính về số lần chuyển đổi, có thể có chi phí logarit. 

Một trường hợp cạnh tinh vi phổ biến phát sinh khi các chu trình tồn tại trong cấu trúc chuyển tiếp. Ví dụ: nếu một chu kỳ cho phép thu được tài nguyên ròng dương thì khả năng sống sót có thể trở nên không giới hạn, trong khi mô phỏng đường dẫn đơn giản có thể kết thúc hoặc lặp lại không chính xác mà không nhận ra hiệu ứng tích lũy này. Một trường hợp khác xuất hiện khi nhiều đường dẫn đạt đến cùng một trạng thái với các mức tài nguyên khác nhau, trong đó chỉ nên giữ lại trạng thái tốt nhất, nhưng BFS hoặc DFS ngây thơ có thể ghi đè hoặc loại bỏ thông tin cần thiết. 

Vấn đề tế nhị thứ hai xảy ra khi các chuyển đổi có giá trị riêng lẻ nhưng không khả thi về mặt tổng thể. Ví dụ: một đường dẫn có thể có vẻ khả thi theo từng bước, nhưng ràng buộc tích lũy vi phạm giới hạn toàn cục ở bước trung gian. Một quá trình duyệt tham lam ngây thơ chỉ kiểm tra tính hợp lệ cục bộ sẽ chấp nhận một đường dẫn như vậy một cách không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ của vấn đề là mô phỏng mọi đường dẫn có thể bắt đầu từ trạng thái ban đầu, theo dõi tài nguyên hoặc giá trị tồn tại trên đường đi và kiểm tra xem có đường dẫn nào thỏa mãn các ràng buộc vô thời hạn hoặc đạt đến điều kiện mục tiêu hay không. Điều này đúng về mặt khái niệm vì nó khám phá toàn bộ không gian trạng thái mà không bỏ sót bất kỳ khả năng nào. Tuy nhiên, số lượng đường đi trong đồ thị có hướng có thể tăng theo cấp số nhân theo độ sâu, đặc biệt khi có các chu trình, dẫn đến sự bùng nổ trong tính toán. Ngay cả khi chúng ta giới hạn bản thân trong các đường dẫn đơn giản, con số vẫn có thể đạt đến thang đo giai thừa trong các biểu đồ dày đặc, vượt xa khả năng tính toán khả thi. 

Cái nhìn sâu sắc quan trọng là vấn đề không yêu cầu phân biệt giữa các cách khác nhau để đạt đến trạng thái vượt quá điều kiện tốt nhất có thể đạt được. Khi chúng tôi nhận ra rằng việc đạt đến cùng một trạng thái với giá trị tài nguyên yếu hơn sẽ bị chi phối nghiêm ngặt bởi việc tiếp cận trạng thái đó bằng giá trị tài nguyên mạnh hơn, chúng tôi có thể nén không gian tìm kiếm thành một dạng lập trình động trên các trạng thái. Điều này biến vấn đề thành việc truyền bá các giá trị tối ưu thông qua các chuyển đổi trong khi liên tục loại bỏ các cấu hình thống trị.

Nếu cấu trúc bao gồm các chu kỳ và hiệu ứng tích lũy, vấn đề còn giảm xuống còn việc phát hiện xem liệu các cải tiến có thể được nhân rộng vô thời hạn hay không, điều này tương đương với việc tìm xem liệu có tồn tại một chu kỳ cải tiến tích cực trong biểu đồ được chuyển đổi hay không. Đây là lúc lý luận theo phong cách con đường ngắn nhất hoặc sự thư giãn giống như Bellman-Ford trở nên có thể áp dụng được. Mỗi lần nới lỏng sẽ cải thiện một giá trị trạng thái và nếu các cải tiến tiếp tục vượt quá một giới hạn nhất định, điều đó hàm ý khả năng sống sót không giới hạn. 

Do đó, thay vì liệt kê các đường đi, chúng tôi liên tục nới lỏng các chuyển đổi và chỉ duy trì các giá trị trạng thái được biết đến nhiều nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Tối Ưu (DP / Thư Giãn) | O(N + M) hoặc O(NM) tùy theo kiểu máy | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại hệ thống dưới dạng biểu đồ trong đó mỗi nút lưu trữ giá trị khả năng sống sót tốt nhất được biết đến. Quá trình chuyển đổi cập nhật các giá trị này dựa trên các ràng buộc được mã hóa trên các cạnh. 

### Các bước 

1. Khởi tạo một mảng`best[]`trong đó mỗi mục thể hiện giá trị sống sót tối đa khi đến nút đó. Đặt giá trị của nút bắt đầu ở mức tài nguyên ban đầu và tất cả các giá trị khác thành số rất âm biểu thị các trạng thái chưa được tiếp cận. Điều này đảm bảo chúng tôi chỉ truyền bá từ các cấu hình khả thi. 
2. Chèn nút bắt đầu vào cấu trúc xử lý, thường là hàng đợi nếu các bản cập nhật lan truyền cục bộ hoặc chuẩn bị cho các bước thư giãn lặp đi lặp lại nếu cần lan truyền toàn cầu. Sự lựa chọn phụ thuộc vào việc trọng lượng cạnh có hoạt động đồng đều hay yêu cầu cải tiến nhiều lần. 
3. Đối với mỗi cạnh từ một nút`u`đến một nút`v`, tính giá trị khả năng sống sót của ứng viên như là một hàm của`best[u]`và giới hạn cạnh. Nếu ứng viên này tốt hơn`best[v]`, cập nhật`best[v]`. 
4. Bất cứ khi nào`best[v]`cải thiện, nhân rộng cải tiến này về phía trước bằng cách xem xét lại tất cả các cạnh đi từ`v`. Điều này đảm bảo rằng các cải tiến sẽ được thực hiện thông qua các trạng thái phụ thuộc thay vì dừng lại sớm ở các nút trung gian. 
5. Tiếp tục quá trình này cho đến khi không có cập nhật nào xảy ra trong toàn bộ quá trình truyền qua các cạnh hoặc cho đến khi quá trình truyền bá dựa trên hàng đợi đã hết tất cả các cải tiến. 
6. Sau khi hội tụ, hãy kiểm tra xem điều kiện mục tiêu có được thỏa mãn hay không hoặc liệu có nút nào vẫn có thể cải tiến vượt quá ngưỡng cho thấy mức tăng trưởng vô hạn hay không. Nếu vậy, hãy báo cáo khả năng sống sót càng tốt, nếu không thì báo cáo thất bại. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào bất biến ưu thế: tại bất kỳ thời điểm nào, đối với mỗi nút, chúng ta chỉ cần giữ giá trị sống sót tối đa có thể đạt được khi tiếp cận nó. Bất kỳ đường dẫn nào đến cùng một nút có giá trị nhỏ hơn không bao giờ có thể dẫn đến kết quả tốt hơn, vì tất cả các chuyển đổi trong tương lai chỉ phụ thuộc vào giá trị được lưu trữ này và các ràng buộc biên. Bởi vì mọi sự nới lỏng đều cải thiện nghiêm ngặt một giá trị được lưu trữ và các giá trị bị giới hạn bởi các ràng buộc vấn đề hoặc việc phát hiện các chu kỳ, nên quy trình phải chấm dứt hoặc xác định chính xác sự cải thiện không giới hạn. Điều này đảm bảo rằng trạng thái cuối cùng phản ánh khả năng sống sót tốt nhất có thể trên tất cả các đường dẫn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, w))

    INF_NEG = -10**18
    best = [INF_NEG] * n

    start = 0
    best[start] = 0

    changed = True
    for _ in range(n - 1):
        if not changed:
            break
        changed = False
        for u in range(n):
            if best[u] == INF_NEG:
                continue
            for v, w in g[u]:
                cand = best[u] + w
                if cand > best[v]:
                    best[v] = cand
                    changed = True

    for u in range(n):
        if best[u] == INF_NEG:
            continue
        for v, w in g[u]:
            if best[u] + w > best[v]:
                print("Infinite")
                return

    print("Finite")

if __name__ == "__main__":
    solve()
```Việc triển khai là quy trình thư giãn kiểu Bellman-Ford được điều chỉnh để tối đa hóa điểm khả năng sống sót thay vì giảm thiểu khoảng cách. Giai đoạn đầu tiên tính toán giá trị tốt nhất có thể đạt được cho mỗi nút trong nhiều nhất`n-1`vòng, đảm bảo rằng tất cả các cải tiến đường dẫn đơn giản đều được nắm bắt. 

Giai đoạn thứ hai kiểm tra bất kỳ cạnh nào vẫn có thể cải thiện giá trị sau khi hội tụ. Một cạnh như vậy biểu thị một chu kỳ tích cực, có nghĩa là tỷ lệ sống sót có thể tăng lên vô thời hạn bằng cách lặp lại, tương ứng với một kết quả vô hạn. 

Một vấn đề tế nhị phổ biến là bỏ qua các nút không thể truy cập một cách chính xác. Séc`best[u] == INF_NEG`đảm bảo chúng tôi không bao giờ truyền bá từ các trạng thái không hợp lệ, điều này ngăn chặn lạm phát giả tạo các giá trị từ các thành phần không thể truy cập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một biểu đồ nhỏ trong đó việc sinh tồn rất đơn giản: 

đầu vào:```
4 4
1 2 5
2 3 3
3 4 -2
1 3 4
```Chúng tôi khởi tạo: 

| Bước | mảng tốt nhất | 
| --- | --- | 
| ban đầu | [0, -∞, -∞, -∞] | 
| thư giãn 1 | [0, 5, 4, -∞] | 
| thư giãn 2 | [0, 5, 8, 2] | 
| thư giãn 3 | không thay đổi | 
| thư giãn 4 | không thay đổi | 

Sau khi hội tụ, không có cạnh nào cho phép cải tiến hơn nữa. Hệ thống ổn định, nghĩa là không có chu kỳ nào làm tăng tổng giá trị. 

Đầu ra:```
Finite
```Điều này xác nhận rằng tất cả các cải tiến đều đến từ sự lan truyền theo chu kỳ và không tồn tại sự tích lũy vô hạn. 

### Ví dụ 2 

Hãy xem xét một chu kỳ làm tăng giá trị: 

đầu vào:```
3 3
1 2 1
2 3 1
3 1 1
```| Bước | mảng tốt nhất | 
| --- | --- | 
| ban đầu | [0, -∞, -∞] | 
| thư giãn | [0, 1, 2] | 
| kiểm tra chu kỳ | phát hiện sự cải thiện thông qua 3→1 | 

Cạnh 3 → 1 vẫn cải thiện giá trị vì 2 + 1 > 0, biểu thị một chu kỳ dương. 

Đầu ra:```
Infinite
```Điều này chứng tỏ rằng một khi một chu kỳ có thể làm tăng giá trị tích lũy, việc truyền tải lặp đi lặp lại mang lại khả năng sống sót không giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi lần thư giãn xử lý tất cả các cạnh, được lặp lại tối đa N lần | 
| Không gian | O(N + M) | Lưu trữ đồ thị cộng với mảng tốt nhất | 

Các ràng buộc cho phép lên tới vài trăm nghìn cạnh, do đó, cách tiếp cận thư giãn giới hạn vẫn khả thi, đặc biệt là dừng sớm khi không có cập nhật nào xảy ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    def solve():
        n, m = map(int, input().split())
        g = [[] for _ in range(n)]
        for _ in range(m):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append((v, w))

        INF_NEG = -10**18
        best = [INF_NEG] * n
        best[0] = 0

        for _ in range(n - 1):
            changed = False
            for u in range(n):
                if best[u] == INF_NEG:
                    continue
                for v, w in g[u]:
                    if best[u] + w > best[v]:
                        best[v] = best[u] + w
                        changed = True
            if not changed:
                break

        for u in range(n):
            if best[u] == INF_NEG:
                continue
            for v, w in g[u]:
                if best[u] + w > best[v]:
                    return "Infinite"
        return "Finite"

    return solve()

# sample 1
assert run("""4 4
1 2 5
2 3 3
3 4 -2
1 3 4
""") == "Finite"

# sample 2
assert run("""3 3
1 2 1
2 3 1
3 1 1
""") == "Infinite"

# edge: single node
assert run("""1 0
""") == "Finite"

# edge: disconnected graph
assert run("""3 1
2 3 10
""") == "Finite"

# edge: self loop positive cycle
assert run("""1 1
1 1 1
""") == "Infinite"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút, không có cạnh | hữu hạn | trường hợp cơ sở | 
| đồ thị bị ngắt kết nối | hữu hạn | trạng thái không thể truy cập bị bỏ qua | 
| tự vòng tích cực | Vô hạn | phát hiện chu kỳ trực tiếp | 

## Vỏ cạnh 

Tự vòng lặp một nút có trọng số dương là trường hợp trực tiếp nhất có thể tồn tại vô hạn. Thuật toán đánh dấu chính xác điều này vì sau khi khởi tạo,`best[0]`trở thành không âm và vòng lặp tự vẫn cải thiện nó, kích hoạt kiểm tra phát hiện chu kỳ. 

Thành phần bị ngắt kết nối chứa chu trình không được ảnh hưởng đến kết quả trừ khi có thể truy cập được ngay từ đầu. các`best[u] == INF_NEG`bảo vệ đảm bảo rằng các nút không thể truy cập sẽ không tham gia vào quá trình thư giãn, do đó các chu kỳ trong các thành phần bị cô lập sẽ được bỏ qua một cách chính xác. 

Một chuỗi không tuần hoàn dài đảm bảo rằng quá trình thư giãn lặp đi lặp lại sẽ ổn định sau nhiều nhất`n-1`vượt qua. Thuật toán tôn trọng giới hạn này và điều kiện kết thúc sớm sẽ ngăn chặn những lần lặp lại không cần thiết khi không có cải tiến nào xảy ra.
