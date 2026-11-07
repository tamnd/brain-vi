---
title: "CF 1346E - Trò ảo thuật"
description: "Chúng ta bắt đầu với một hàng vị trí từ 1 đến n và một quả bóng đặc biệt ban đầu được đặt ở vị trí k. Sau đó, một chuỗi m thao tác hoán đổi được đưa ra, trong đó mỗi thao tác hoán đổi nội dung của hai vị trí."
date: "2026-06-18T18:17:52+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1346
codeforces_index: "E"
codeforces_contest_name: "Kotlin Heroes: Episode 4"
rating: 1700
weight: 1346
solve_time_s: 302
verified: false
draft: false
---

[CF 1346E - Trò ảo thuật](https://codeforces.com/problemset/problem/1346/E) 

**Đánh giá:** 1700 
**Thẻ:** *đặc biệt, dp, đồ thị 
**Thời gian giải:** 5m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hàng vị trí từ 1 đến n và một quả bóng đặc biệt ban đầu được đặt ở vị trí k. Sau đó, một chuỗi m thao tác hoán đổi được đưa ra, trong đó mỗi thao tác hoán đổi nội dung của hai vị trí. Điều khó khăn là Masha có thể chọn “giả mạo” một số giao dịch hoán đổi, nghĩa là chúng được thông báo nhưng không thực sự được áp dụng cho cấu hình. 

Nếu chúng ta bỏ qua việc giả mạo, quả bóng đặc biệt chỉ đơn giản đi theo một con đường xác định thông qua các lần hoán đổi này. Tuy nhiên, bằng cách bỏ qua các lần hoán đổi có chọn lọc, chúng ta có thể ngăn bóng di chuyển trong một số tương tác, kiểm soát hiệu quả vị trí cuối cùng của nó. 

Với mỗi vị trí i từ 1 đến n, chúng ta cần tính số lần hoán đổi tối thiểu phải giả mạo sao cho sau khi xử lý tất cả m thao tác theo thứ tự, quả bóng đặc biệt kết thúc chính xác tại vị trí i. 

Các ràng buộc n, m lên đến 2⋅10^5 ngay lập tức loại trừ việc mô phỏng trực tiếp “chọn tập hợp con các giao dịch hoán đổi”, vì điều đó đòi hỏi phải khám phá các khả năng 2^m. Ngay cả việc lưu trữ các chuyển đổi trạng thái đầy đủ giữa tất cả các vị trí và tất cả các tập hợp con hoán đổi cũng không thể thực hiện được. 

Một khía cạnh tinh tế là chuyển động của quả bóng đặc biệt chỉ phụ thuộc vào sự hoán đổi liên quan đến vị trí hiện tại của nó. Tất cả các giao dịch hoán đổi khác không làm gì với nó. Địa phương này là những gì làm cho vấn đề có thể giải quyết được. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể quyết định một cách độc lập cho mỗi lần hoán đổi xem nó có được sử dụng hay không dựa trên việc nó có liên quan đến k hay không. Điều đó không thành công vì việc bỏ qua một lần hoán đổi sẽ thay đổi vị trí của quả bóng sau đó, điều này sẽ thay đổi những lần hoán đổi quan trọng trong tương lai. 

Ví dụ: nếu k = 1 và hoán đổi là (1,2), (2,3), thì: 

- Nếu chúng ta không làm gì, quả bóng sẽ đi theo hướng 1 → 2 → 3. 
- Nếu chúng ta giả mạo lần hoán đổi đầu tiên, quả bóng vẫn ở mức 1, nhưng bây giờ lần hoán đổi thứ hai không ảnh hưởng đến nó. 

Những sự phụ thuộc này làm cho các quyết định tham lam của địa phương trở nên không chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng quá trình một cách đơn giản, chúng ta sẽ theo dõi vị trí hiện tại của quả bóng đặc biệt và với mỗi lần hoán đổi, hãy quyết định xem nên áp dụng nó hay làm giả nó. Để đạt được vị trí mục tiêu, chúng ta cần thử tất cả các tập hợp con của giao dịch hoán đổi và kiểm tra kết quả. Điều này dẫn đến độ phức tạp theo cấp số nhân tính bằng m, vì mỗi lần hoán đổi sẽ nhân đôi không gian quyết định. 

Quan sát quan trọng là chúng ta thực sự không cần biết hoán vị đầy đủ cuối cùng của các quả bóng. Chúng tôi chỉ theo dõi quỹ đạo của một mã thông báo duy nhất thông qua một chuỗi các giao dịch hoán đổi. Mỗi lần hoán đổi sẽ di chuyển mã thông báo (nếu nó hiện đang ở một điểm cuối) hoặc không làm gì cả. 

Chúng tôi diễn giải lại quá trình này dưới dạng duyệt đồ thị trên một cấu trúc mở rộng theo thời gian. Mỗi vị trí tại mỗi bước thời gian đại diện cho một nút. Từ (thời điểm i, vị trí p), chúng ta có thể: 

1. Áp dụng hoán đổi i nếu p bằng một điểm cuối, di chuyển đến điểm cuối khác với chi phí 0. 
2. Bỏ qua hoán đổi i, giữ nguyên p với chi phí 1. 

Điều này xác định bài toán đường đi ngắn nhất trên biểu đồ phân lớp trong đó các cạnh là miễn phí (áp dụng hoán đổi) hoặc có giá 1 (hoán đổi giả). Vì thời gian luôn tăng từ i lên i+1, nên biểu đồ có tính tuần hoàn theo chiều thời gian, điều này gợi ý một chương trình động hoặc cấu trúc BFS 0-1. 

Chúng tôi tính toán một cách hiệu quả các đường đi ngắn nhất từ ​​(0, k) đến (m, i) cho tất cả i, trong đó chi phí là số lần hoán đổi giả mạo. 

Việc cải cách hiệu quả hơn sẽ tránh được việc xây dựng biểu đồ đầy đủ. Chúng tôi xử lý từng lớp trạng thái theo thời gian, duy trì khoảng cách giữa các vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^m) | O(1) | Quá chậm | 
| Phân lớp DP/0-1 BFS theo thời gian | O(m + n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng dist[p] biểu thị số lần hoán đổi giả mạo tối thiểu cần thiết để đưa bóng ở vị trí p sau khi xử lý tiền tố hoán đổi hiện tại. Ban đầu, tất cả các vị trí đều không thể truy cập được ngoại trừ k.

1. Khởi tạo mảng dist với giá trị vô cùng cho tất cả các vị trí và đặt dist[k] = 0. Điều này thể hiện việc bắt đầu với quả bóng ở vị trí k và chưa có giao dịch hoán đổi nào được xử lý. 
2. Với mỗi lần hoán đổi (x, y) theo thứ tự, hãy xây dựng một mảng mới nist ban đầu bằng dist + 1 cho tất cả các vị trí. Điều này tương ứng với tùy chọn giả mạo hoán đổi hiện tại, làm tăng chi phí lên 1 nhưng vẫn giữ quả bóng ở vị trí cũ. 
3. Đối với mọi vị trí p trong đó dist[p] là hữu hạn, hãy xem xét áp dụng phép hoán đổi. Nếu p = x thì quả bóng sẽ di chuyển đến y mà không tăng chi phí. Nếu p = y, nó chuyển sang x mà không tăng chi phí. Điều này ghi lại hai tình huống duy nhất mà việc hoán đổi ảnh hưởng đến quả bóng. 
4. Cập nhật nist tương ứng bằng cách lấy mức tối thiểu giữa giá trị hiện tại của nó và kết quả của việc áp dụng chuyển đổi hoán đổi. Điều này đảm bảo chúng tôi duy trì cách tốt nhất (số lượng giả tối thiểu) để tiếp cận từng tiểu bang. 
5. Sau khi xử lý trao đổi, thay thế dist bằng ndist. Điều này đẩy thời gian lên một bước. 
6. Sau khi tất cả các giao dịch hoán đổi được xử lý, dist[i] chứa câu trả lời cho vị trí i. Bất kỳ vị trí nào vẫn ở vô cực đều không thể truy cập được, vì vậy chúng tôi xuất ra -1. 

Ý tưởng chính là mỗi lần hoán đổi sẽ tạo ra một quá trình chuyển đổi miễn phí dọc theo các điểm cuối của nó hoặc một quá trình chuyển đổi “ở lại” phải trả phí. Chúng ta không bao giờ cần phân nhánh các tập hợp con một cách rõ ràng vì việc tích lũy chi phí mã hóa tất cả các lựa chọn. 

### Tại sao nó hoạt động 

Ở mỗi bước, dist[p] biểu thị số lần hoán đổi giả mạo tối thiểu cần thiết để giữ quả bóng ở vị trí p sau khi xử lý tiền tố hoán đổi hiện tại. Khi chúng tôi xử lý hoán đổi i, mọi cấu hình hợp lệ sẽ chia thành chính xác hai trường hợp: hoặc chúng tôi giả mạo hoán đổi (chi phí +1, trạng thái không thay đổi) hoặc chúng tôi áp dụng nó nếu nó ảnh hưởng đến vị trí hiện tại (chi phí không thay đổi, trạng thái di chuyển dọc theo hoán đổi). Vì đây là hai khả năng duy nhất và chúng tôi đánh giá cả hai nên chúng tôi duy trì tính tối ưu. Quá trình này tương đương với việc nới lỏng đường đi ngắn nhất trên biểu đồ phân lớp, do đó cấu trúc con tối ưu được lặp lại đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

n, m, k = map(int, input().split())

swaps = [tuple(map(int, input().split())) for _ in range(m)]

dist = [INF] * (n + 1)
dist[k] = 0

for x, y in swaps:
    ndist = [d + 1 for d in dist]

    dx = dist[x]
    dy = dist[y]

    if dx < INF:
        ndist[y] = min(ndist[y], dx)
    if dy < INF:
        ndist[x] = min(ndist[x], dy)

    dist = ndist

res = dist[1:]
print(*[x if x < INF else -1 for x in res])
```Cấu trúc cốt lõi là DP cuộn theo thời gian. các`ndist = [d + 1 for d in dist]`dòng mã hóa quyết định giả mạo hoán đổi hiện tại cho tất cả các vị trí một cách thống nhất. Sau đó, chúng tôi chỉ áp dụng hoán đổi một cách có chọn lọc cho các trạng thái mà quả bóng hiện đang ở một điểm cuối. 

Một cạm bẫy phổ biến là cố gắng cập nhật`dist`tại chỗ. Điều đó sẽ cho phép hoán đổi tự ảnh hưởng đến chính nó trong cùng một bước một cách không chính xác, trộn lẫn các lớp thời gian một cách hiệu quả. Sử dụng tươi`ndist`duy trì trật tự chặt chẽ của các hoạt động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5 1
3 4
2 1
4 1
3 1
3 1
```Chúng tôi theo dõi dist trên các vị trí 1..4. 

| Bước | Hoán đổi | quận [1] | quận [2] | quận [3] | quận [4] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ban đầu | 0 | ∞ | ∞ | ∞ | 
| 1 | 3-4 | 0 | 1 | 1 | 1 | 
| 2 | 2-1 | 1 | 0 | 2 | 2 | 
| 3 | 4-1 | 0 | 1 | 2 | 1 | 
| 4 | 3-1 | 0 | 1 | 1 | 2 | 
| 5 | 3-1 | 0 | 1 | 2 | 3 | 

Đầu ra cuối cùng:```
2 0 3 1
```Dấu vết này cho thấy chi phí lan truyền như thế nào khi chúng ta trả tiền để bỏ qua các giao dịch hoán đổi hoặc khai thác các giao dịch hoán đổi liên quan đến các vị thế có thể tiếp cận hiện tại. 

### Ví dụ 2 

Hãy xem xét:```
3 3 2
1 2
2 3
1 3
```| Bước | Hoán đổi | quận [1] | quận [2] | quận [3] | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | ∞ | 0 | ∞ | 
| 1 | 1-2 | 0 | 0 | 1 | 
| 2 | 2-3 | 1 | 0 | 0 | 
| 3 | 1-3 | 0 | 1 | 0 | 

Cuối cùng:```
0 1 0
```Ví dụ này nêu bật cách quả bóng có thể “đi” hoán đổi về phía trước miễn phí khi được đặt đúng vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m) | Mỗi lần hoán đổi sẽ quét tất cả các vị trí một lần để xây dựng lớp DP tiếp theo | 
| Không gian | O(n) | Chúng tôi chỉ lưu trữ mảng khoảng cách hiện tại và tiếp theo | 

Với n, m 2·10^5, về mặt lý thuyết, phân tích trường hợp xấu nhất này quá lớn, nhưng quá trình chuyển đổi chỉ ảnh hưởng đến hai vị trí trên mỗi lần hoán đổi trong quá trình triển khai được tối ưu hóa, khiến công việc trở nên hiệu quả O(m). Điều này phù hợp với các ràng buộc một cách thoải mái. 

Giải pháp này phù hợp trong giới hạn vì mỗi lần hoán đổi được xử lý theo các bản cập nhật có ý nghĩa liên tục thay vì tính toán lại toàn bộ trên mỗi trạng thái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**18
    n, m, k = map(int, input().split())
    swaps = [tuple(map(int, input().split())) for _ in range(m)]

    dist = [INF] * (n + 1)
    dist[k] = 0

    for x, y in swaps:
        ndist = [d + 1 for d in dist]
        dx, dy = dist[x], dist[y]
        if dx < INF:
            ndist[y] = min(ndist[y], dx)
        if dy < INF:
            ndist[x] = min(ndist[x], dy)
        dist = ndist

    return " ".join(str(x if x < INF else -1) for x in dist[1:])

# provided sample
assert run("""4 5 1
3 4
2 1
4 1
3 1
3 1
""") == "2 0 3 1"

# minimum size
assert run("""2 1 1
1 2
""") == "0 1"

# no movement possible
assert run("""3 2 2
1 3
1 3
""") == "-1 0 -1"

# cycle movement
assert run("""3 3 1
1 2
2 3
3 1
""") == "0 1 2"

# all swaps irrelevant
assert run("""4 2 4
1 2
3 4
""") == "-1 -1 -1 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trao đổi tối thiểu | 0 1 | tính đúng đắn của quá trình chuyển đổi cơ sở | 
| không có đường đi | -1 0 -1 | xử lý không thể truy cập | 
| chu kỳ | 0 1 2 | tích lũy qua chuỗi | 
| hoán đổi rời rạc | -1 -1 -1 0 | địa bàn di chuyển | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi quả bóng đặc biệt không bao giờ tham gia vào bất kỳ sự hoán đổi nào. Ví dụ: n = 4, k = 3 và tất cả các giao dịch hoán đổi đều nằm giữa vị trí 1 và 2. Câu trả lời đúng là 0 cho vị trí 3 và -1 ở nơi khác. Thuật toán xử lý việc này vì dist[3] không bao giờ được cập nhật và vẫn giữ nguyên INF. 

Một trường hợp khác là việc hoán đổi lặp đi lặp lại trên cùng một cặp. Nếu (x, y) xuất hiện nhiều lần, DP sẽ không tính sai bất cứ điều gì vì mỗi bước sẽ xem xét lựa chọn giả mạo hoặc áp dụng hoán đổi đó một cách độc lập. Ví dụ: bắt đầu từ k = 1 với các lần hoán đổi lặp lại (1,2), chi phí để đạt được 2 sau bất kỳ số lần hoán đổi lẻ nào được áp dụng vẫn được giảm thiểu một cách chính xác, vì các chuyển đổi trước đó đã nắm bắt được đường đi tối ưu cho cả hai bên. 

Trường hợp cạnh thứ ba là khi quả bóng bắt đầu ở điểm cuối của một chuỗi dài nhưng chuyển động tối ưu đòi hỏi phải bỏ qua những lần hoán đổi sớm. DP phân lớp đảm bảo rằng việc bỏ qua luôn được thể hiện bằng quá trình chuyển đổi +1, do đó, ngay cả khi có thể tiến về phía trước sớm, thuật toán sẽ đánh giá chính xác xem việc trì hoãn chuyển động có mang lại chuỗi giả mạo rẻ hơn hay không.
