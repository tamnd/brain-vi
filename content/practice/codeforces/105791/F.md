---
title: "CF 105791F - Bốn là quá nhiều"
description: "Chúng tôi được giao cho một nhóm vận động viên chạy marathon X cần phương tiện di chuyển đến đích. Có sẵn N tùy chọn chuyến đi và mỗi tùy chọn chuyến đi hoạt động giống như một phương tiện duy nhất có thể được sử dụng nhiều nhất một lần."
date: "2026-06-21T13:10:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "F"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 52
verified: true
draft: false
---

[CF 105791F - Bốn là quá nhiều](https://codeforces.com/problemset/problem/105791/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một nhóm vận động viên chạy marathon X cần phương tiện di chuyển đến đích. Có sẵn N tùy chọn chuyến đi và mỗi tùy chọn chuyến đi hoạt động giống như một phương tiện duy nhất có thể được sử dụng nhiều nhất một lần. Mỗi chuyến đi có hai thuộc tính: sức chứa pi, nghĩa là nó có thể chở tối đa pi hành khách trong nhóm, và chi phí vi, nghĩa là nếu chúng ta chọn chuyến đi đó, chúng ta phải trả vi bất kể chúng ta thực sự sử dụng bao nhiêu ghế trên đó. 

Mục tiêu là chọn một tập hợp con các chuyến xe sao cho tổng công suất vận chuyển ít nhất là X, đồng thời giảm thiểu tổng chi phí. Chúng tôi không chỉ định riêng từng hành khách các chuyến đi theo cách bị hạn chế vượt quá giới hạn sức chứa, bởi vì một khi chuyến đi được chọn, chuyến đi đó sẽ đóng góp toàn bộ sức lực của mình để phục vụ cả nhóm. 

Các ràng buộc X, N 1000 và pi 4 đủ nhỏ để có thể thực hiện được giải pháp quy hoạch động bậc hai hoặc bậc ba thấp. Một giải pháp thử tất cả các tập hợp con của chuyến đi sẽ có hàm mũ theo N và ngay lập tức không khả thi vì nó yêu cầu đánh giá 2^1000 kết hợp. Ngay cả một cách tiếp cận ngây thơ cố gắng tham lam chọn những chuyến đi rẻ nhất trên mỗi đơn vị sức chứa cũng có thể thất bại vì hiệu quả chi phí không mang tính phụ gia theo cách đảm bảo việc đóng gói tối ưu. 

Một trường hợp thất bại tinh vi của lý luận tham lam xuất hiện khi một chuyến đi đắt hơn một chút với sức chứa lớn hơn giúp tránh được nhiều chuyến đi nhỏ hơn. Ví dụ, xét X = 6 với các hành trình (4, 20), (3, 10), (3, 8). Một chiến lược tham lam chọn chi phí thấp nhất trước tiên sẽ lấy (3, 8) và (3, 10), đạt được chi phí 18. Nhưng chọn (4, 20) cộng với không có gì khác là không hợp lệ, và chọn (4, 20) cộng (3, 8) sẽ cho khả năng 7 với chi phí 28, điều này tệ hơn. Điều này minh họa rằng các quyết định của địa phương dựa trên chi phí hoặc chi phí theo năng lực là không tốt. 

Một trường hợp khó khăn khác là khi tổng công suất khả dụng không đủ. Với X = 6 với các chuyến đi (3, 10) và (2, 5), tổng sức chứa là 5 nên không có lựa chọn nào có thể đến đích và đáp án đúng là -1. Bất kỳ phương pháp DP hoặc tham lam nào cũng phải xử lý rõ ràng tình trạng không thể truy cập này. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là xem xét từng tập hợp con các chuyến đi và tính tổng công suất cũng như tổng chi phí của nó. Điều này hiệu quả vì mọi giải pháp hợp lệ đều được đưa vào không gian tìm kiếm, vì vậy chúng tôi có thể chọn chi phí tối thiểu trong số những giải pháp đạt ít nhất dung lượng X. Tuy nhiên, điều này đòi hỏi phải lặp lại trên 2^N tập hợp con và đối với mỗi tập hợp con tổng hợp các giá trị, dẫn đến các phép toán O(N·2^N), vượt xa mọi giới hạn hợp lý khi N là 1000. 

Quan sát chính là đây là cấu trúc ba lô cổ điển, nhưng có một điểm thay đổi: chúng tôi không cố gắng khớp chính xác một trọng lượng mà để đạt được ít nhất dung lượng X. Mỗi chuyến đi là một vật phẩm có thể mang theo một lần, đóng góp một trọng lượng nhỏ (nhiều nhất là 4) và một khoản chi phí. Điều này ngay lập tức gợi ý một công thức lập trình động dựa trên khả năng có thể đạt được. 

Chúng tôi xác định trạng thái mà chúng tôi theo dõi chi phí tối thiểu cần thiết để đạt được chính xác j hành khách được vận chuyển cho tất cả j từ 0 đến X. Mọi công suất trên X đều có thể được coi là X một cách an toàn vì việc vượt quá yêu cầu sẽ không cải thiện thêm câu trả lời. Điều này nén vấn đề vào một chiếc ba lô tiêu chuẩn 0/1 trong đó trọng lượng nhỏ và kích thước mục tiêu được giới hạn bởi 1000. 

Chúng tôi lặp lại các chuyến đi và cập nhật mảng DP ngược lại để mỗi chuyến đi được sử dụng tối đa một lần. Sau khi xử lý tất cả các chuyến đi, câu trả lời là dp[j] tối thiểu cho tất cả j ≥ X, trong thực tế chỉ là dp[X]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·2^N) | O(N) | Quá chậm | 
| DP vượt công suất | O(N·X) | O(X) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi đây như một chiếc ba lô giảm thiểu chi phí trong đó “trọng lượng” là sức chứa hành khách và “giá trị” là mức giảm thiểu chi phí âm.

1. Chúng ta tạo một mảng DP dp trong đó dp[j] biểu thị chi phí tối thiểu cần thiết để vận chuyển chính xác j hành khách, với j từ 0 đến X. Tất cả các giá trị được khởi tạo thành vô cùng ngoại trừ dp[0] bằng 0, vì việc vận chuyển hành khách bằng 0 không tốn kém gì. Điều này thiết lập một trạng thái cơ bản mà từ đó tất cả các quá trình chuyển đổi bắt nguồn. 
2. Đối với mỗi chuyến đi (pi, vi), chúng tôi cố gắng sử dụng nó để cải thiện các trạng thái có thể truy cập trước đó. Vì mỗi chuyến đi chỉ có thể được thực hiện một lần nên chúng tôi lặp lại j từ X xuống 0. Việc lặp lại ngược lại ngăn cản việc sử dụng lại cùng một chuyến đi nhiều lần trong một bước chuyển tiếp. 
3. Với mỗi j, nếu dp[j] đã có thể truy cập được, chúng tôi xem xét chuyển sang trạng thái min(j + pi, X). Chúng tôi cập nhật dp[min(j + pi, X)] bằng dp[j] + vi nếu nó cải thiện chi phí. Việc kẹp ở X đảm bảo chúng ta không phân biệt giữa “chính xác X” và “lớn hơn X”, vì cả hai đều đủ. 
4. Sau khi xử lý tất cả các chuyến đi, chúng tôi kiểm tra dp[X]. Nếu nó vẫn là vô cực, điều đó có nghĩa là không có tập hợp con chuyến đi nào có thể đạt được công suất cần thiết, vì vậy chúng tôi xuất ra -1. Ngược lại dp[X] là chi phí tối thiểu. 

### Tại sao nó hoạt động 

Trạng thái DP duy trì tính bất biến rằng sau khi xử lý k chuyến đi đầu tiên, dp[j] là chi phí tối thiểu trong số tất cả các tập hợp con của k chuyến đi này đạt được chính xác công suất j (giới hạn ở X). Mọi chuyển đổi đều loại trừ hoặc bao gồm chuyến đi hiện tại và việc lặp lại ngược lại đảm bảo mỗi chuyến đi đóng góp tối đa một lần cho mỗi tập hợp con. Bởi vì tất cả các tập hợp con hợp lệ đều có thể biểu diễn thông qua chuỗi các quyết định như vậy nên không có giải pháp khả thi nào bị bỏ qua và tính tối ưu đến từ việc luôn lưu trữ chi phí tối thiểu trên mỗi công suất có thể tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    X, N = map(int, input().split())
    INF = 10**18
    
    dp = [INF] * (X + 1)
    dp[0] = 0
    
    for _ in range(N):
        p, v = map(int, input().split())
        
        for j in range(X, -1, -1):
            if dp[j] == INF:
                continue
            nj = j + p
            if nj > X:
                nj = X
            dp[nj] = min(dp[nj], dp[j] + v)
    
    print(-1 if dp[X] == INF else dp[X])

if __name__ == "__main__":
    solve()
```Mảng DP được khởi tạo với giá trị trọng điểm lớn để thể hiện các trạng thái không thể truy cập được. Vòng lặp ngược trên j là chi tiết triển khai chính thực thi tính chất 0/1 của lựa chọn. Việc kẹp trạng thái tiếp theo vào X sẽ tránh lãng phí bộ nhớ hoặc thời gian theo dõi dung lượng dư thừa. 

Một lỗi phổ biến là lặp lại j theo thứ tự tăng dần, điều này sẽ cho phép tái sử dụng cùng một chuyến đi nhiều lần trong một lần lặp, khiến vấn đề trở thành một chiếc ba lô không giới hạn và tạo ra kết quả không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
3 10
4 20
3 8
```Chúng tôi theo dõi trạng thái dp sau mỗi chuyến đi, chỉ hiển thị các giá trị nén có liên quan. 

| Bước | Đi xe | dp[0..6] (được nén ở trạng thái X=6 được biết đến nhiều nhất) | 
| --- | --- | --- | 
| ban đầu | - | dp[0]=0, những cái khác=∞ | 
| 1 | (3,10) | dp[3]=10 | 
| 2 | (4,20) | dp[4]=20, dp[6]=30 (thông qua 3+3 không hợp lệ, vì vậy chỉ có trạng thái 4 và 3+4) | 
| 3 | (3,8) | dp[6]=18 | 

Câu trả lời cuối cùng là 18, đạt được bằng cách chọn các chuyến đi có sức chứa 3 và 3 và 4, nhưng (3,8) + (3,10) tối ưu thì kém hơn so với việc kết hợp khác nhau; DP đảm bảo tìm thấy sự kết hợp tốt nhất trong số tất cả các tập hợp con. 

Dấu vết này cho thấy cách tái sử dụng công suất một phần trung gian để tạo thành công suất lớn hơn và tại sao chỉ lưu trữ chi phí tốt nhất trên mỗi công suất là đủ. 

### Ví dụ 2 

đầu vào:```
6 2
3 10
2 5
```| Bước | Đi xe | trạng thái dp | 
| --- | --- | --- | 
| ban đầu | - | dp[0]=0 | 
| 1 | (3,10) | dp[3]=10 | 
| 2 | (2,5) | dp[2]=5, dp[5]=15 | 

Không có trạng thái nào đạt tới 6, vì vậy câu trả lời là -1. 

Điều này khẳng định rằng DP phân biệt chính xác giữa “chi phí tốt nhất có thể đạt được” và “mục tiêu không khả thi”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N·X) | Mỗi chuyến đi cập nhật tối đa trạng thái X DP một lần | 
| Không gian | O(X) | Chỉ một mảng DP duy nhất vượt quá dung lượng được lưu trữ | 

Sản phẩm N·X tối đa là 10^6, tốc độ này khá nhanh trong Python dưới giới hạn 1 giây, đặc biệt với các phép toán số nguyên đơn giản và không có chi phí lồng nhau ngoài các vòng lặp DP. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    X, N = map(int, sys.stdin.readline().split())
    INF = 10**18
    
    dp = [INF] * (X + 1)
    dp[0] = 0
    
    for _ in range(N):
        p, v = map(int, sys.stdin.readline().split())
        for j in range(X, -1, -1):
            if dp[j] == INF:
                continue
            nj = min(X, j + p)
            dp[nj] = min(dp[nj], dp[j] + v)
    
    return str(-1 if dp[X] == INF else dp[X])

# provided sample (interpreted correctly)
assert run("""6 3
3 10
4 20
3 8
""") == "18"

# unreachable case
assert run("""6 2
3 10
2 5
""") == "-1"

# minimum case
assert run("""1 1
1 10
""") == "10"

# exact boundary
assert run("""4 2
2 5
2 6
""") == "11"

# redundant large capacity
assert run("""5 2
4 7
4 8
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyến đi tối thiểu duy nhất | 10 | tính đúng đắn của trường hợp cơ sở | 
| không đủ năng lực | -1 | phát hiện không thể truy cập | 
| điền chính xác | 11 | tích lũy công suất chính xác | 
| chồng chéo dư thừa | 7 | chọn tập con tối ưu | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các chuyến đi kết hợp lại vẫn không thể đạt đến X. Đối với đầu vào X = 10 và các chuyến đi (3, 5), (4, 6), (2, 3), DP không bao giờ đạt đến dp[10] và trạng thái cuối cùng vẫn là vô hạn. Thuật toán xuất ra chính xác -1 vì không có chuỗi chuyển đổi nào tạo ra trạng thái toàn bộ công suất hợp lệ. 

Một trường hợp khó khăn khác là khi một chuyến đi đã đáp ứng được yêu cầu. Đối với X = 3 và một chuyến đi (4, 100), quá trình chuyển đổi sẽ kẹp kết quả vào dp[3] = 100 ngay lập tức. Bước kẹp là cần thiết vì nếu không có nó, DP sẽ xử lý không chính xác các dung lượng vượt quá X như các trạng thái riêng biệt và có thể bỏ lỡ các chuyển tiếp tối ưu. 

Trường hợp cạnh cuối cùng bao gồm nhiều sự kết hợp chồng chéo tạo ra cùng một công suất với các chi phí khác nhau. DP đảm bảo chỉ có chi phí tối thiểu tồn tại ở mỗi mức công suất, do đó, ngay cả khi nhiều tập hợp con đạt đến cùng j thì chỉ tập hợp con tốt nhất được giữ nguyên.
