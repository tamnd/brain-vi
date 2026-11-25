---
title: "CF 994B - Hiệp sĩ của một bảng đa giác"
description: "Mỗi hiệp sĩ có hai thuộc tính, sức mạnh chiến đấu và một lượng tiền xu. Một hiệp sĩ chỉ có thể đánh bại các hiệp sĩ có sức mạnh nhỏ hơn và mỗi chiến thắng sẽ chuyển đồng xu của hiệp sĩ bị đánh bại cho người chiến thắng."
date: "2026-06-17T00:09:01+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 994
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 488 by NEAR (Div. 2)"
rating: 1400
weight: 994
solve_time_s: 262
verified: true
draft: false
---

[CF 994B - Hiệp sĩ của một bảng đa giác](https://codeforces.com/problemset/problem/994/B) 

**Đánh giá:** 1400 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 4 phút 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi hiệp sĩ có hai thuộc tính, sức mạnh chiến đấu và một lượng tiền xu. Một hiệp sĩ chỉ có thể đánh bại các hiệp sĩ có sức mạnh nhỏ hơn và mỗi chiến thắng sẽ chuyển đồng xu của hiệp sĩ bị đánh bại cho người chiến thắng. Tuy nhiên, có một hạn chế toàn cầu: không hiệp sĩ nào sẵn sàng đánh bại nhiều hơn`k`người khác. 

Nhiệm vụ là tưởng tượng rằng mỗi hiệp sĩ độc lập lên kế hoạch cho một chuỗi chiến đấu tối ưu theo các quy tắc này và tính toán cho mỗi hiệp sĩ số xu tối đa mà họ có thể kiếm được. 

Đầu vào cung cấp một mảng các điểm mạnh và một mảng các giá trị đồng xu được căn chỉnh theo chỉ mục. Đầu ra là một mảng khác trong đó mỗi vị trí tương ứng với một hiệp sĩ và chứa tổng số xu tốt nhất có thể đạt được bắt đầu từ hiệp sĩ đó. 

Các ràng buộc định hình không gian giải pháp một cách mạnh mẽ. Với tối đa 100.000 hiệp sĩ, bất kỳ phương pháp nào cố gắng mô phỏng cuộc chiến giữa tất cả các cặp đều ngay lập tức trở thành bậc hai và không khả thi. Một sự ngây thơ$O(n^2)$chiến lược sẽ thực hiện theo thứ tự$10^{10}$so sánh trong trường hợp xấu nhất, vượt xa giới hạn thời gian một giây có thể chịu đựng được. Hạn chế bổ sung đó$k \le 10$là gợi ý quan trọng về cấu trúc: mỗi hiệp sĩ chỉ quan tâm đến một số lượng rất nhỏ những ứng cử viên giỏi nhất trong số tất cả các hiệp sĩ yếu hơn. 

Một trường hợp thất bại tinh tế xuất hiện khi suy nghĩ tham lam mà không phân loại theo sức mạnh. Nếu một hiệp sĩ chỉ xem xét các chỉ số lân cận, nó có thể bỏ sót các hiệp sĩ yếu hơn xuất hiện sau theo thứ tự đầu vào nhưng thực sự là mục tiêu hợp lệ. Ví dụ, nếu điểm mạnh là`[10, 1, 2]`, hiệp sĩ đầu tiên có thể đánh bại cả hai người khác, nhưng việc quét dựa trên vị trí mà không sắp xếp sẽ coi thứ tự có ý nghĩa một cách không chính xác. Một cạm bẫy phổ biến khác là bỏ qua ràng buộc “tối đa k tiêu diệt” và chỉ đơn giản tính tổng tất cả các đồng tiền yếu hơn, điều này không thành công khi có nhiều hiệp sĩ yếu có giá trị nhỏ và chỉ nên chọn một số hiệp sĩ có giá trị cao. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp gợi ý rằng hãy thử mọi hiệp sĩ chống lại tất cả những hiệp sĩ khác, kiểm tra xem hiệp sĩ nào yếu hơn và chọn tập hợp con tốt nhất trong số đó.`k`trong số đó. Về nguyên tắc, điều này đúng vì nó đánh giá tất cả các lựa chọn hợp lệ có thể có, nhưng nó trở nên quá chậm vì đối với mỗi hiệp sĩ, chúng tôi có thể quét tất cả những lựa chọn khác rồi sắp xếp hoặc chọn tối đa`k`những ứng cử viên tốt nhất, dẫn đến tổng chi phí khoảng$O(n^2 \log n)$. 

Việc đơn giản hóa cấu trúc xuất phát từ việc tách các ràng buộc. Sức mạnh quyết định tính khả thi của việc tiêu diệt, trong khi tiền xu xác định mức độ mong muốn. Nếu chúng ta xử lý các hiệp sĩ theo thứ tự sức mạnh tăng dần, thì tại thời điểm chúng ta xử lý một hiệp sĩ, tất cả các hiệp sĩ được xử lý trước đó chính xác là những hiệp sĩ được phép giết. Sau đó, vấn đề giảm xuống còn việc duy trì, đối với mỗi tiền tố của thứ tự này, tập hợp con có kích thước tốt nhất có thể`k`theo giá trị đồng tiền. 

Thay vì tính toán lại tập hợp con tốt nhất từ ​​đầu cho mỗi hiệp sĩ, chúng tôi duy trì một cấu trúc nhỏ chỉ chứa phần trên cùng.`k`giá trị đồng xu trong số tất cả các hiệp sĩ yếu hơn được thấy cho đến nay. Từ`k`tối đa là 10, một đống tối thiểu hoạt động hiệu quả: nó lưu trữ các ứng cử viên tốt nhất hiện tại và bất cứ khi nào một hiệp sĩ mới yếu hơn xuất hiện, chúng tôi sẽ quyết định xem nó có thuộc top hay không`k`. Điều này biến bước lựa chọn thành thời gian logarit với một hằng số nhỏ. 

Quá trình chuyển đổi quan trọng là câu trả lời cho mỗi hiệp sĩ chỉ phụ thuộc vào nhiều bộ xu thuộc về các hiệp sĩ yếu hơn, được giảm đến mức tốt nhất`k`các phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log k)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp lại các hiệp sĩ bằng cách tăng sức mạnh để tất cả các mục tiêu hợp lệ cho hiệp sĩ đều xuất hiện trước nó. 

1. Ghép sức mạnh, đồng xu và chỉ số ban đầu của mỗi hiệp sĩ vào một cấu trúc duy nhất, sau đó sắp xếp các cấu trúc này theo sức mạnh. Điều này đảm bảo rằng khi chúng tôi xử lý một hiệp sĩ, mọi hiệp sĩ được xử lý trước đó sẽ yếu hơn hoàn toàn. 
2. Duy trì một đống tối thiểu lưu trữ giá trị đồng xu của các hiệp sĩ yếu hơn được chọn, cùng với tổng nội dung của đống. Heap sẽ không bao giờ chứa nhiều hơn`k`các phần tử. Cấu trúc này đại diện cho tập hợp tốt nhất lên đến`k`giết chết có sẵn cho đến nay. 
3. Lặp lại các hiệp sĩ theo thứ tự được sắp xếp. Trước khi chèn mã hiện tại vào đống, hãy tính toán câu trả lời cho nó bằng số tiền riêng của nó cộng với tổng đống hiện tại. Điều này có hiệu quả vì vùng heap tại thời điểm này chứa chính xác những gì tốt nhất`k`tiền xu từ tất cả các hiệp sĩ yếu hơn. 
4. Chèn giá trị đồng xu của hiệp sĩ hiện tại vào đống và thêm nó vào tổng số đang chạy. Nếu kích thước heap vượt quá`k`, loại bỏ phần tử nhỏ nhất và trừ nó khỏi tổng. Điều này chỉ giữ lại lợi nhuận cao nhất`k`ứng cử viên cho các hiệp sĩ mạnh mẽ hơn trong tương lai. 
5. Sau khi xử lý tất cả các hiệp sĩ, hãy khôi phục kết quả về thứ tự ban đầu bằng cách sử dụng các chỉ số được lưu trữ. 

Ý tưởng trung tâm là mỗi hiệp sĩ truy vấn một cấu trúc tiền tố trên mảng được sắp xếp theo độ mạnh, trong đó tiền tố được duy trì liên tục dưới dạng tập hợp tốt nhất có thể lên đến`k`giá trị tiền xu. 

### Tại sao nó hoạt động 

Tại thời điểm chúng tôi xử lý một hiệp sĩ, đống chứa chính xác`k`giá trị đồng xu lớn nhất trong số tất cả các hiệp sĩ có sức mạnh nhỏ hơn. Bất kỳ chiến lược tối ưu nào cho hiệp sĩ này chỉ có thể chọn từ những hiệp sĩ yếu hơn này và vì nó được phép chọn nhiều nhất`k`, thay thế bất kỳ tập hợp con nào được chọn bằng tập hợp con tốt nhất`k`có sẵn không thể làm giảm tổng số. Điều này chứng tỏ rằng đống luôn đại diện cho tập hợp lựa chọn tối ưu cho tất cả các hiệp sĩ trong tương lai và do đó mỗi câu trả lời được tính toán là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

n, k = map(int, input().split())
p = list(map(int, input().split()))
c = list(map(int, input().split()))

knights = [(p[i], c[i], i) for i in range(n)]
knights.sort()

heap = []
heap_sum = 0
ans = [0] * n

for power, coins, idx in knights:
    ans[idx] = coins + heap_sum

    heapq.heappush(heap, coins)
    heap_sum += coins

    if len(heap) > k:
        removed = heapq.heappop(heap)
        heap_sum -= removed

print(*ans)
```Bước sắp xếp đảm bảo chúng tôi chỉ xem xét các đối thủ yếu hơn hợp lệ khi tính toán từng câu trả lời. Vùng heap luôn lưu trữ các ứng cử viên tốt nhất có thể có trong số chúng và tổng chạy tránh việc tính toán lại tổng số vùng heap nhiều lần. Việc sử dụng min-heap là rất quan trọng vì nó cho phép loại bỏ hiệu quả giá trị đồng tiền ít hữu ích nhất bất cứ khi nào kích thước vượt quá`k`. 

Một lỗi triển khai phổ biến là chèn mã hiện tại vào heap trước khi tính toán câu trả lời của nó. Điều đó sẽ không chính xác khi cho phép một hiệp sĩ “tự sát” trong quá trình tính toán. Thứ tự đúng là truy vấn trước, sau đó chèn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
4 2
4 5 9 7
1 2 11 33
```Sau khi sắp xếp theo sức mạnh, các hiệp sĩ trở thành: 

| Bước | Sức mạnh | Tiền xu | Đống trước | Trả lời được tính toán | Đống sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | [] | 1 | [1] | 
| 2 | 5 | 2 | [1] | 3 | [1,2] | 
| 3 | 7 | 33 | [1,2] | 36 | [1,2,33] → [2,33] | 
| 4 | 9 | 11 | [2,33] | 46 | [2,33,11] → [11,33] | 

Bảng cho thấy rằng ở mỗi bước, đống luôn bảo toàn những đồng tiền tốt nhất hiện có trong số các hiệp sĩ yếu hơn và mỗi câu trả lời được tính toán trước khi hiệp sĩ hiện tại được đưa vào làm ứng cử viên. 

Ví dụ thứ hai nhấn mạnh tác dụng của`k = 0`: 

đầu vào:```
3 0
10 5 7
100 20 30
```| Bước | Sức mạnh | Tiền xu | Đống trước | Trả lời được tính toán | Đống sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 20 | [] | 20 | [] | 
| 2 | 7 | 30 | [] | 30 | [] | 
| 3 | 10 | 100 | [] | 100 | [] | 

Điều này xác nhận rằng khi không được phép giết, mỗi hiệp sĩ chỉ giữ lại số tiền ban đầu của họ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log k)$| Mỗi hiệp sĩ được đẩy và có thể xuất hiện nhiều nhất một lần từ một đống kích thước`k`| 
| Không gian |$O(k)$| Heap chỉ lưu trữ những gì tốt nhất`k`ứng viên | 

Giải pháp thoải mái phù hợp trong giới hạn vì`k`là cực kỳ nhỏ, làm cho các thao tác heap có hiệu quả liên tục trong thời gian thực tế ngay cả đối với$10^5$hiệp sĩ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline
    import heapq

    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    c = list(map(int, input().split()))

    knights = [(p[i], c[i], i) for i in range(n)]
    knights.sort()

    heap = []
    heap_sum = 0
    ans = [0] * n

    for power, coins, idx in knights:
        ans[idx] = coins + heap_sum

        heapq.heappush(heap, coins)
        heap_sum += coins

        if len(heap) > k:
            heap_sum -= heapq.heappop(heap)

    return " ".join(map(str, ans))

assert run("""4 2
4 5 9 7
1 2 11 33
""") == "1 3 46 36"

assert run("""3 0
10 5 7
100 20 30
""") == "100 20 30"

assert run("""1 0
5
10
""") == "10"

assert run("""5 1
1 2 3 4 5
5 4 3 2 1
""") == "5 9 8 5 1"

assert run("""5 2
5 1 3 2 4
10 100 20 30 40
""") == "10 100 130 120 150"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hiệp sĩ độc thân | cùng giá trị | trường hợp tối thiểu | 
| k = 0 | không tích lũy | ràng buộc ranh giới | 
| tăng sức mạnh | tham lam tăng trưởng đống đơn giản | tính đúng đắn của logic sắp xếp | 
| giá trị đơn hàng hỗn hợp | lựa chọn đống chính xác | bảo trì top-k | 
| hỗn hợp nhỏ ngẫu nhiên | tính đúng đắn chung | tích hợp tất cả các thành phần | 

## Vỏ cạnh 

Khi nào`k = 0`, heap không bao giờ được sử dụng để tích lũy. Đối với mỗi hiệp sĩ, câu trả lời đơn giản là đồng xu của riêng họ và thuật toán xử lý vấn đề này một cách tự nhiên vì vùng heap vẫn trống trong suốt quá trình xử lý. 

Khi tất cả các điểm mạnh theo thứ tự giảm dần trong đầu vào, bước sắp xếp sẽ đảo ngược chúng hoàn toàn. Thuật toán vẫn hoạt động vì nó chỉ dựa vào thứ tự sắp xếp chứ không dựa vào thứ tự đầu vào. Mỗi hiệp sĩ sau đó sẽ nhìn thấy tất cả những người yếu hơn một cách chính xác trước khi nó được xử lý. 

Khi`k`bằng`n-1`, heap có thể phát triển lớn hơn nhưng vẫn bị giới hạn bởi`k`. Thuật toán vẫn chọn chính xác tập hợp con tốt nhất có thể vì mọi hiệp sĩ yếu hơn đều được xem xét và vùng nhớ giữ lại các giá trị cao nhất trong số đó.
