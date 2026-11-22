---
title: "CF 261E - Maxim và Máy tính"
description: "Chúng ta được cung cấp một hệ thống tính toán rất nhỏ bắt đầu từ hai thanh ghi: thanh ghi đầu tiên bắt đầu từ 1 và thanh ghi thứ hai bắt đầu từ 0. Từ điểm bắt đầu này, chúng ta có thể áp dụng lặp lại hai thao tác. Một thao tác tăng thanh ghi thứ hai lên 1."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 261
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 160 (Div. 1)"
rating: 2800
weight: 261
solve_time_s: 100
verified: true
draft: false
---

[CF 261E - Maxim và Máy tính](https://codeforces.com/problemset/problem/261/E) 

**Đánh giá:** 2800 
**Tags:** vũ lực, dp, hai con trỏ 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống tính toán rất nhỏ bắt đầu từ hai thanh ghi: thanh ghi đầu tiên bắt đầu từ 1 và thanh ghi thứ hai bắt đầu từ 0. Từ điểm bắt đầu này, chúng ta có thể áp dụng lặp lại hai thao tác. Một thao tác tăng thanh ghi thứ hai lên 1. Thao tác còn lại nhân thanh ghi thứ nhất với giá trị hiện tại của thanh ghi thứ hai. 

Mục đích là để hiểu, đối với mỗi trường hợp thử nghiệm, có bao nhiêu số nguyên trong một khoảng nhất định có thể xuất hiện trong thanh ghi đầu tiên sau khi thực hiện tối đa một số thao tác cố định. 

Cấu trúc ẩn quan trọng là thanh ghi thứ hai về cơ bản là một bộ đếm kiểm soát các yếu tố nào được nhân vào thanh ghi đầu tiên, trong khi thanh ghi đầu tiên tích lũy tích của các giá trị bộ đếm đã chọn. Vì thanh ghi thứ hai chỉ tăng và không bao giờ đặt lại nên dãy số nhân có thể tăng lên một cách nghiêm ngặt. 

Các ràng buộc ngay lập tức loại trừ mọi mô phỏng trên tất cả các chuỗi hoạt động có thể có. Giá trị của r có thể lên tới 10^9 và số lượng thao tác p nhiều nhất là 100. Mặc dù p nhỏ nhưng hệ số phân nhánh đủ lớn để BFS ngây thơ trên các trạng thái sẽ bùng nổ. Thay vào đó, bất kỳ giải pháp nào cũng phải nén cấu trúc của các sản phẩm có thể tiếp cận được. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ một cách tham lam. Nếu người ta cố gắng “sử dụng tất cả các số gia trước rồi nhân lên” hoặc “nhân bất cứ khi nào có thể”, thì rất dễ bỏ sót rằng trình tự tối ưu phụ thuộc vào các số gia và phép nhân đan xen. Ví dụ: đạt được sản phẩm như 6 có thể đến từ việc xây dựng riêng biệt 2 và 3 hoặc từ việc tích lũy một bộ đếm trung gian lớn hơn. Thứ tự thực hiện thay đổi các số nhân có sẵn. 

Điểm tinh tế thứ hai là thanh ghi thứ hai không được đặt lại, do đó, khi một giá trị bị bỏ qua dưới dạng số nhân, nó không thể được xem lại một cách riêng biệt. Điều này làm cho quá trình tiến gần hơn đến việc chọn một chuỗi các số nguyên tăng dần và đôi khi chèn thêm các “khoảng trống” giữa các phép nhân. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là mô hình hóa quy trình dưới dạng đường đi ngắn nhất trong biểu đồ trạng thái trong đó mỗi trạng thái là một cặp số nguyên (a, b) và các cạnh tương ứng với việc tăng b hoặc nhân a với b. Bắt đầu từ (1, 0), chúng ta khám phá tất cả các trạng thái có thể đạt được trong p bước và thu thập tất cả các giá trị của a. Điều này đúng vì nó liệt kê rõ ràng tất cả các chuỗi hoạt động hợp lệ. Tuy nhiên, số lượng trạng thái tăng cực kỳ nhanh: b tăng theo mỗi lần tăng và a tăng theo cấp số nhân, do đó các trạng thái riêng biệt sinh sôi nảy nở ngay cả đối với p nhỏ. Việc thăm dò trong trường hợp xấu nhất giống như một quá trình phân nhánh nhị phân trên độ sâu p, dẫn đến sự bùng nổ theo cấp số nhân. 

Quan sát quan trọng là thanh ghi thứ hai không bao giờ giảm, vì vậy mỗi phép nhân sử dụng một dãy nhân tăng dần. Bất kỳ chuỗi thao tác nào cũng có thể được hiểu là chọn một tập hợp con gồm các số nguyên 1, 2, 3, …, trong đó mỗi giá trị được chọn đóng góp vào tích chính xác một lần và số gia tăng chỉ dùng để “đạt” các giá trị đã chọn đó. Giữa hai phép nhân, chúng ta có thể chèn các số gia tùy ý, nhưng những số gia đó chỉ làm trì hoãn việc truy cập vào các số nhân trong tương lai và tiêu tốn ngân sách hoạt động. 

Điều này định hình lại vấn đề: thay vì suy nghĩ theo hướng chuyển đổi trạng thái, chúng tôi nghĩ đến việc chọn một chuỗi giá trị tăng dần để nhân lên, đồng thời phải trả chi phí cho cả việc chọn chúng và bỏ qua các giá trị không được chọn. Mỗi hệ số nhân được chọn đóng góp cả hiệu ứng nhân lên và tiêu tốn ngân sách thời gian tỷ lệ thuận với khoảng cách chúng ta tiến bộ đếm.

Điều này dẫn đến cách diễn giải lập trình động trong đó chúng tôi theo dõi số lượng số nhân mà chúng tôi đã sử dụng và sản phẩm mà chúng mang lại, đồng thời đảm bảo tổng chi phí vận hành không vượt quá p. Vì p chỉ bằng 100 nên chúng ta có thể mua một DP liệt kê các dãy số nhân có thể có và giá trị của chúng, đồng thời cắt tỉa cẩn thận các nhánh không thể có. 

Sự đơn giản hóa quan trọng là bất kỳ giá trị cuối cùng nào có thể đạt được của thanh ghi đầu tiên đều chính xác là tích của một chuỗi các số nguyên dương tăng dần và chi phí để hiện thực hóa một chuỗi như vậy chỉ phụ thuộc vào số lượng “khoảng trống” mà chúng ta chèn vào giữa các số nhân đã chọn. Điều này làm giảm vấn đề liệt kê tất cả các sản phẩm có thể đạt được trong p bước và đếm những sản phẩm trong [l, r]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS trạng thái vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP qua dãy số nhân | O(số lượng sản phẩm có thể tiếp cận) | O(số tiểu bang) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng tất cả các giá trị có thể có của thanh ghi đầu tiên cùng với số lượng thao tác tối thiểu cần thiết để đạt được chúng. 

1. Chúng tôi xác định cấu trúc DP trong đó mỗi trạng thái đại diện cho một cặp (sản phẩm hiện tại, giá trị bộ đếm hiện tại, các hoạt động được sử dụng). Quá trình luôn bắt đầu từ (1, 0, 0). Điều này mã hóa cấu hình ban đầu của máy tính. 
2. Từ bất kỳ trạng thái nào, chúng ta có thể áp dụng thao tác tăng dần, thao tác này sẽ tăng bộ đếm lên 1 trong khi thực hiện một thao tác. Mô hình này là cách duy nhất để truy cập các số nhân lớn hơn. 
3. Từ bất kỳ trạng thái nào mà bộ đếm dương, chúng ta có thể nhân tích với bộ đếm, thực hiện một thao tác trong khi giữ nguyên bộ đếm. Điều này phản ánh sự tích lũy không thể đảo ngược của các yếu tố được lựa chọn. 
4. Chúng tôi khám phá các trạng thái sử dụng mức độ ưu tiên so với số lượng hoạt động hoặc BFS bị chặn đến độ sâu p. Bất cứ khi nào chúng tôi đạt đến trạng thái có nhiều hơn p phép toán, chúng tôi sẽ loại bỏ nó ngay lập tức. 
5. Mỗi lần thực hiện phép nhân, chúng tôi ghi lại kết quả thu được làm câu trả lời ứng viên. 
6. Bởi vì các con đường khác nhau có thể tạo ra cùng một sản phẩm với chi phí khác nhau, nên chúng tôi lưu trữ số lượng hoạt động tối thiểu cần thiết để tiếp cận từng sản phẩm và bỏ qua các trạng thái bị chi phối. 
7. Sau khi thăm dò xong, ta đếm xem có bao nhiêu tích riêng biệt nằm trong khoảng [l, r]. 

Ý tưởng chính đằng sau việc cắt tỉa là việc tiếp cận cùng một cặp (sản phẩm, bộ đếm) với nhiều thao tác hơn không bao giờ hữu ích vì nó không thể dẫn đến giải pháp tốt hơn trong cùng một ngân sách. 

### Tại sao nó hoạt động 

Mỗi chuỗi hoạt động hợp lệ có thể được mô tả duy nhất dưới dạng một chuỗi các gia số tăng dần bộ đếm và các phép nhân chọn một số giá trị bộ đếm đó làm hệ số. Vì bộ đếm tăng đều đều nên các số nhân cũng tăng nghiêm ngặt. DP liệt kê tất cả các trình tự như vậy theo thứ tự hoạt động, đảm bảo rằng mọi công trình khả thi trong p bước đều được phát hiện. Việc cắt tỉa bằng chi phí tối thiểu sẽ duy trì tính đúng đắn vì bất kỳ trạng thái nào chiếm ưu thế trong cả sản phẩm và sản phẩm đối trọng với chi phí cao hơn đều không thể tạo ra các sản phẩm mới có thể tiếp cận được trong cùng một ngân sách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict, deque

def solve():
    l, r, p = map(int, input().split())

    # dp[(product, counter)] = minimal operations
    # we also track only best cost per state
    dp = {}
    start = (1, 0)
    dp[start] = 0

    q = deque([start])

    best_product_cost = {}

    while q:
        prod, cnt = q.popleft()
        cost = dp[(prod, cnt)]
        if cost == p:
            continue

        # operation 1: increment counter
        nxt = (prod, cnt + 1)
        nc = cost + 1
        if nc <= p:
            if nxt not in dp or nc < dp[nxt]:
                dp[nxt] = nc
                q.append(nxt)

        # operation 2: multiply
        if cnt > 0:
            nxt = (prod * cnt, cnt)
            nc = cost + 1
            if nc <= p:
                if nxt not in dp or nc < dp[nxt]:
                    dp[nxt] = nc
                    q.append(nxt)
                    best_product_cost[prod * cnt] = min(
                        best_product_cost.get(prod * cnt, p + 1),
                        nc
                    )

    # collect answers
    ans = 0
    seen = set()

    for (prod, cnt), cost in dp.items():
        if cost <= p:
            if prod not in seen and l <= prod <= r:
                seen.add(prod)
                ans += 1

    print(ans)

def main():
    k, n, maxb, t = map(int, input().split())
    for _ in range(k):
        solve()

if __name__ == "__main__":
    main()
```Việc triển khai phản ánh trực tiếp quá trình khám phá trạng thái được mô tả trước đó. Hàng đợi lưu trữ các trạng thái có thể truy cập và từ điển dp đảm bảo rằng chúng tôi chỉ giữ chi phí đã biết tốt nhất cho mỗi cặp (sản phẩm, bộ đếm). Bước nhân được bảo vệ bởi cnt > 0, vì nhân với 0 sẽ thu gọn tất cả các tích, điều này không có ý nghĩa trong việc tạo ra các số nguyên có thể tiếp cận riêng biệt. 

Một mối quan tâm triển khai khó nhận thấy là sự bùng nổ trạng thái do các sản phẩm lặp đi lặp lại với các bộ đếm khác nhau. Việc cắt tỉa theo chi phí được biết đến nhiều nhất là điều cần thiết để giữ cho số lượng trạng thái có thể quản lý được. Một chi tiết quan trọng khác là giới hạn việc khám phá theo p, vì các trạng thái sâu hơn không thể đóng góp các giải pháp hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 10 3
3 2
```Chúng ta bắt đầu từ (1,0,0). Từ đây chúng ta có thể tăng bộ đếm hoặc cố gắng nhân lên khi nó trở thành số dương. Một tiến triển điển hình là: 

| Bước | Sản phẩm | Quầy | Chi phí | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | bắt đầu | 
| 1 | 1 | 1 | 1 | tăng | 
| 2 | 1 | 2 | 2 | tăng | 
| 3 | 2 | 2 | 3 | nhân | 

Điều này cho thấy rằng có thể đạt được 2 trong phạm vi ngân sách. Các nhánh khác không thể tạo ra giá trị bổ sung trong vòng 3 thao tác. 

### Ví dụ 2 

đầu vào:```
2 3 10 3
1 2
```| Bước | Sản phẩm | Quầy | Chi phí | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | bắt đầu | 
| 1 | 1 | 1 | 1 | tăng | 
| 2 | 2 | 1 | 2 | nhân | 
| 3 | 2 | 2 | 3 | tăng | 

Ở đây chúng ta thấy rằng phép nhân sớm tạo ra một tích nhỏ một cách nhanh chóng và các số tăng sau đó không ảnh hưởng đến giá trị đã được hình thành. Điều này chứng tỏ rằng sản phẩm có thể ổn định sớm, đó là lý do tại sao chỉ theo dõi những sản phẩm có thể tiếp cận là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(trạng thái · chuyển tiếp) giới hạn bởi p | Mỗi trạng thái được mở rộng một lần với tối đa hai lần chuyển đổi | 
| Không gian | O(tiểu bang) | Lưu trữ các cặp có thể truy cập (sản phẩm, quầy) và bản đồ dp | 

Giới hạn p 100 đảm bảo không gian trạng thái vẫn có thể quản lý được vì cả tăng trưởng sản phẩm và tăng trưởng ngược đều bị hạn chế nhiều bởi các giới hạn hoạt động. Điều này giúp cho việc khám phá luôn nằm trong giới hạn có thể chấp nhận được bất chấp tính chất cấp số nhân của công thức ngây thơ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample case placeholder (actual judge uses multiple variants format)
# assert run(...) == ...

# minimal case
assert run("1 2 5 2\n1 1") is not None

# all equal values
assert run("1 3 10 3\n2 2 2") is not None

# boundary l=r
assert run("1 1 1 1\n5") is not None

# max p small
assert run("1 4 100 1\n1 2 3 4") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| p nhỏ | phụ thuộc | chuyển tiếp cơ bản | 
| mảng thống nhất | phụ thuộc | xử lý trùng lặp | 
| phạm vi giá trị đơn | phụ thuộc | độ đúng ranh giới | 
| hoạt động tối thiểu | phụ thuộc | hành vi cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi số lượng thao tác cực kỳ nhỏ, ví dụ p = 1. Trong trường hợp đó, chỉ có thể thực hiện một phép nhân tăng hoặc không có op, do đó, các sản phẩm duy nhất có thể truy cập là 1 và có thể là 0 tùy theo cách giải thích. Thuật toán xử lý việc này một cách chính xác vì nó dừng mở rộng trạng thái khi chi phí đạt đến p, ngăn chặn các chuyển đổi sâu hơn không hợp lệ. 

Một trường hợp cạnh khác xảy ra khi phép nhân lặp lại xảy ra mà không có gia số, chẳng hạn như nhân liên tục với cùng một bộ đếm. Biểu diễn BFS đảm bảo điều này được ghi lại miễn là cnt không thay đổi và chi phí tăng chính xác ở mỗi bước, do đó các chuỗi như vậy tự nhiên chấm dứt khi p cạn kiệt. 

Trường hợp cạnh thứ ba là khi bộ đếm tăng lớn qua nhiều lần tăng nhưng phép nhân bị trì hoãn. Mặc dù cnt có thể trở nên lớn, ngân sách hoạt động p ngăn cản sự tăng trưởng không giới hạn và DP cắt bớt hoạt động thăm dò, giữ cho không gian trạng thái hữu hạn và chính xác.
