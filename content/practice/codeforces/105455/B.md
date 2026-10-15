---
title: "CF 105455B - Trò chơi quan liêu"
description: "Chúng ta được cung cấp một chuỗi tuyến tính các cửa sổ dịch vụ và tại mỗi cửa sổ có chính xác một khả năng chuyển đổi giữa các tài liệu: nếu bạn hiện đang giữ một tài liệu cụ thể, bạn có thể chọn thực hiện quy trình chuyển đổi nó thành một tài liệu khác."
date: "2026-06-23T02:51:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105455
codeforces_index: "B"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105455
solve_time_s: 103
verified: true
draft: false
---

[CF 105455B - Trò chơi quan liêu](https://codeforces.com/problemset/problem/105455/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi tuyến tính các cửa sổ dịch vụ và tại mỗi cửa sổ có chính xác một khả năng chuyển đổi giữa các tài liệu: nếu bạn hiện đang giữ một tài liệu cụ thể, bạn có thể chọn thực hiện quy trình chuyển đổi nó thành một tài liệu khác. Việc thực hiện quy trình không xóa tài liệu gốc nên bộ sưu tập tài liệu của bạn chỉ tăng lên theo thời gian. 

Bạn bắt đầu với tài liệu 0 và mục tiêu của bạn là đạt được tài liệu m−1. Hạn chế chính là các cửa sổ được sắp xếp theo thứ tự nghiêm ngặt: một khi bạn di chuyển qua một cửa sổ, bạn không bao giờ có thể quay lại, do đó, phép chuyển đổi do mỗi cửa sổ cung cấp chỉ có thể được sử dụng nhiều nhất một lần và chỉ khi bạn hiện đang ở hoặc trước cửa sổ đó. 

Nhiệm vụ là xác định số lượng thủ tục tối thiểu được thực hiện cần thiết để có được tài liệu m-1 hoặc báo cáo rằng điều đó là không thể. 

Các ràng buộc gợi ý hai chế độ khác nhau. Số lượng cửa sổ và tài liệu có thể lên tới một triệu trong trường hợp khó nhất, do đó, bất kỳ giải pháp nào cố gắng mô phỏng rõ ràng tất cả các bộ tài liệu có thể truy cập hoặc tính toán lại khả năng truy cập nhiều lần trên mỗi cửa sổ sẽ quá chậm. Ngay cả lý luận O(nm) cũng hoàn toàn không khả thi và thậm chí O(n log n) với cấu trúc nặng trên mỗi tài liệu cũng có rủi ro nếu nó yêu cầu các bản cập nhật phức tạp. 

Ràng buộc cấu trúc quan trọng là mỗi cửa sổ đóng góp chính xác một cạnh được định hướng giữa các tài liệu và các cạnh phải được xử lý theo thứ tự. Điều này gợi ý một giải pháp truyền tuyến tính duy nhất trong đó mỗi cạnh được xem xét một lần. 

Một trường hợp thất bại tinh vi xuất hiện khi chỉ nghĩ về “đường dẫn ngắn nhất trong biểu đồ tài liệu”. Ví dụ: nếu bỏ qua việc đặt hàng: 

đầu vào:```
3 4
0 2
2 1
1 3
```Cách giải thích đường đi ngắn nhất ngây thơ ngay lập tức mang lại 0 → 2 → 1 → 3 với chi phí 3, điều này đúng ở đây. Tuy nhiên, nếu chúng ta sắp xếp lại các cạnh:```
3 4
2 1
0 2
1 3
```Một chế độ xem biểu đồ đơn giản vẫn nhìn thấy các cạnh giống nhau, nhưng thứ tự thực hiện có vấn đề: cạnh 2 → 1 xuất hiện trước khi chúng ta có được 2, vì vậy nó không thể được sử dụng. Bất kỳ cách tiếp cận nào bỏ qua thứ tự cửa sổ đều đánh giá quá cao khả năng tiếp cận và có thể xác nhận không chính xác các bước không thể thực hiện được hoặc đánh giá thấp các bước. 

Khó khăn cốt lõi là khả năng tiếp cận phụ thuộc vào cả trạng thái tài liệu và tiền tố của các cửa sổ được xử lý. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng từng cửa sổ quy trình trong khi vẫn duy trì bộ tài liệu đầy đủ mà chúng tôi hiện đang lưu giữ. Tại mỗi cửa sổ i, nếu chúng ta có tài liệu a_i, chúng ta có thể chọn áp dụng phép chuyển đổi và thêm b_i vào tập hợp của mình. Điều này trực tiếp phù hợp với các quy tắc và đúng vì nó tôn trọng trật tự và ràng buộc một cách rõ ràng. 

Tuy nhiên, mô phỏng đơn giản này trở nên tốn kém vì tập tài liệu có thể tăng đến kích thước O(m) và mỗi cửa sổ có khả năng kích hoạt các kiểm tra trên tập tài liệu đang phát triển này. Trong trường hợp xấu nhất, chúng tôi thực hiện kiểm tra hoặc cập nhật tư cách thành viên O(nm), vượt xa giới hạn chấp nhận được khi cả n và m đều đạt một triệu. 

Nhận xét quan trọng là chúng ta không thực sự quan tâm đến bộ tài liệu đầy đủ; chúng tôi chỉ quan tâm đến số lượng thủ tục tối thiểu cần thiết để tiếp cận từng tài liệu. Khi tài liệu có thể truy cập được sau k bước, tài liệu đó vẫn có thể truy cập được mãi mãi vì tài liệu không bao giờ bị mất và các hoạt động trong tương lai chỉ phụ thuộc vào sự tồn tại chứ không phải thời gian. 

Điều này cho phép chúng ta nén toàn bộ trạng thái phát triển thành một mảng dist duy nhất, trong đó dist[x] biểu thị số lượng thủ tục tối thiểu cần thiết để có được tài liệu x. Khi chúng ta ở cửa sổ i với quá trình chuyển đổi a_i → b_i, nếu a_i đã có thể truy cập được bằng dist[a_i] thì chúng ta có thể cải thiện dist[b_i] thành dist[a_i] + 1. Ràng buộc thứ tự được tôn trọng một cách tự nhiên vì bất kỳ đường dẫn nào sử dụng cửa sổ i chỉ có thể dựa vào các cửa sổ trước đó đã được xử lý khi tính toán dist[a_i]. 

Do đó, vấn đề giảm xuống còn một sự nới lỏng từ trái sang phải trên các cạnh, giống hệt như đường đi ngắn nhất trong DAG trong đó các cạnh được đưa ra theo thứ tự tôpô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng thô bạo các bộ tài liệu | O(nm) | O(m) | Quá chậm | 
| Ra lệnh thư giãn DP | O(n) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cửa sổ theo thứ tự nhất định trong khi vẫn duy trì số lượng thủ tục tốt nhất cần thiết để có được từng tài liệu. 

1. Khởi tạo một mảng dist có kích thước m với tất cả các giá trị được đặt thành vô cùng, ngoại trừ dist[0] = 0 vì chúng ta bắt đầu với tài liệu 0 đã có sẵn. 
2. Lặp lại các cửa sổ từ 1 đến n theo thứ tự. Tại cửa sổ i, đọc cặp (a_i, b_i), nghĩa là chúng ta có thể thực hiện một thủ tục để chuyển đổi a_i thành b_i. 
3. Nếu dist[a_i] vẫn là vô cùng, điều đó có nghĩa là tài liệu a_i chưa thể đạt được bằng cách sử dụng bất kỳ chuỗi cửa sổ nào trước đó, vì vậy cửa sổ này không thể được sử dụng một cách có ý nghĩa và chúng tôi bỏ qua nó. 
4. Mặt khác, chúng ta cố gắng nới lỏng trạng thái của b_i bằng cách đặt dist[b_i] = min(dist[b_i], dist[a_i] + 1). Điều này tương ứng với việc sử dụng cửa sổ này làm bước cuối cùng của trình tự thủ tục hợp lệ đạt đến b_i. 
5. Sau khi xử lý tất cả các cửa sổ, câu trả lời là dist[m−1]. Nếu nó vẫn là vô cùng, thì xuất ra điều đó là không thể. 

Ý tưởng quan trọng là mọi chuỗi thủ tục hợp lệ đều tương ứng với việc chọn một chuỗi cửa sổ theo thứ tự chỉ mục tăng dần và DP này đảm bảo chúng tôi xem xét tất cả các chuỗi con đó trong khi luôn chỉ lưu trữ chi phí tốt nhất cho mỗi tài liệu. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý i cửa sổ đầu tiên, dist[x] lưu trữ số lượng thủ tục tối thiểu cần thiết để lấy tài liệu x chỉ sử dụng các cửa sổ cho đến i. Bất kỳ chuỗi hợp lệ nào tạo ra x và kết thúc trong i cửa sổ đầu tiên phải kết thúc bằng một số cửa sổ j ≤ i tạo ra x và việc nới lỏng DP đảm bảo rằng quá trình chuyển đổi được áp dụng chính xác khi cửa sổ j được xử lý. Vì các cửa sổ trước đó đã được giải quyết hoàn toàn trước khi các cửa sổ sau được xem xét nên không có thông tin nào trong tương lai có thể cải thiện các chuyển đổi trước đó theo cách vi phạm trật tự.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    INF = 10**18
    
    for _ in range(t):
        n, m = map(int, input().split())
        dist = [INF] * m
        dist[0] = 0
        
        for _ in range(n):
            a, b = map(int, input().split())
            if dist[a] != INF:
                nd = dist[a] + 1
                if nd < dist[b]:
                    dist[b] = nd
        
        if dist[m - 1] == INF:
            print("Imposible")
        else:
            print(dist[m - 1])

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo ý tưởng thư giãn. Mảng dist nén tất cả các trạng thái có thể truy cập, tránh mọi hoạt động theo dõi rõ ràng đối với các bộ tài liệu. Việc kiểm tra dist[a] != INF là cần thiết vì nó đảm bảo chúng ta chỉ sử dụng một cửa sổ khi tài liệu tiên quyết của nó đã đạt được bằng cách sử dụng các cửa sổ trước đó. 

Bước cập nhật dist[b] = min(dist[b], dist[a] + 1) thực thi tính tối ưu theo nhiều cách có thể để tiếp cận cùng một tài liệu. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên: 

đầu vào:```
2 3
0 1
1 2
```Chúng tôi theo dõi dist theo thời gian. 

| Cửa sổ | a → b | quận [0] | quận [1] | quận [2] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | 0 | ∞ | ∞ | bắt đầu | 
| 1 | 0 → 1 | 0 | 1 | ∞ | thư giãn 1 | 
| 2 | 1 → 2 | 0 | 1 | 2 | thư giãn 2 | 

Cuối cùng, dist[2] = 2, nghĩa là cần có hai thủ tục. 

Bây giờ hãy xem xét trường hợp sử dụng khối đặt hàng: 

đầu vào:```
2 3
1 2
0 1
```| Cửa sổ | a → b | quận [0] | quận [1] | quận [2] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | 0 | ∞ | ∞ | bắt đầu | 
| 1 | 1 → 2 | 0 | ∞ | ∞ | bỏ qua | 
| 2 | 0 → 1 | 0 | 1 | ∞ | thư giãn 1 | 

Ở đây, mặc dù có đường đi 0 → 1 → 2 về mặt cạnh, thứ tự ngăn cản việc sử dụng 1 → 2 vì nó xuất hiện quá sớm. DP phản ánh chính xác điều này và khiến 2 không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cửa sổ được xử lý một lần với thời gian thư giãn liên tục | 
| Không gian | O(m) | Mảng khoảng cách lưu trữ chi phí tốt nhất cho mỗi tài liệu | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì cả n và m đều lên tới một triệu, nhưng chỉ thực hiện các bước tuyến tính và các phép toán mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    INF = 10**18

    for _ in range(T):
        n, m = map(int, input().split())
        dist = [INF] * m
        dist[0] = 0

        for _ in range(n):
            a, b = map(int, input().split())
            if dist[a] != INF:
                dist[b] = min(dist[b], dist[a] + 1)

        out.append("Imposible" if dist[m-1] == INF else str(dist[m-1]))

    return "\n".join(out)

# provided samples
assert run("""4

2 3
0 1
1 2

2 3
0 2
1 0

2 4
1 3
0 1

6 7
0 3
3 2
4 6
3 5
2 5
5 6
""") == """2
1
Imposible
3"""

# custom cases
assert run("""1
1 2
0 1
""") == "1"

assert run("""1
1 3
1 2
""") == "Imposible"

assert run("""1
3 4
0 2
2 3
0 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 1 | trường hợp tối thiểu | 
| điều kiện tiên quyết không thể truy cập | Không thể | thứ tự phụ thuộc | 
| nhiều đường dẫn | 2 | lựa chọn tối ưu giữa các lựa chọn | 

## Vỏ cạnh 

Trường hợp đặc biệt phổ biến là khi đường dẫn khả dụng duy nhất yêu cầu sử dụng một cửa sổ xuất hiện trước khi có thể truy cập được tài liệu tiên quyết của nó. Ví dụ: 

đầu vào:```
3 3
1 2
0 1
```Quá trình xử lý cho thấy cửa sổ 1 → 2 ban đầu không thể được sử dụng vì dist[1] là vô hạn khi nó xuất hiện. Sau đó, khi 0 → 1 được xử lý, chúng tôi cập nhật dist[1], nhưng chúng tôi không truy cập lại các cửa sổ trước đó. Điều này đúng vì bất kỳ chuỗi hợp lệ nào cũng phải tôn trọng thứ tự cửa sổ, vì vậy việc sử dụng 1 → 2 trước 0 → 1 về bản chất là không thể. Thuật toán thực thi ràng buộc này một cách tự nhiên bằng cách chỉ cho phép xử lý chuyển tiếp, tạo ra kết quả chính xác.
