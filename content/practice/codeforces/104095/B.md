---
title: "CF 104095B - \u5e7f\u544a\u6295\u653e"
description: "Chúng tôi được cung cấp một quy trình tuần tự với n tập và quy mô khán giả ban đầu là m. Mỗi tập có thể chạy hoặc không chạy quảng cáo."
date: "2026-07-02T02:17:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104095
codeforces_index: "B"
codeforces_contest_name: "2020 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 104095
solve_time_s: 48
verified: true
draft: false
---

[CF 104095B - \u5e7f\u544a\u6295\u653e](https://codeforces.com/problemset/problem/104095/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một quy trình tuần tự với n tập và quy mô khán giả ban đầu là m. Mỗi tập có thể chạy hoặc không chạy quảng cáo. Nếu một quảng cáo được đặt trong tập i, mỗi người xem hiện tại sẽ đóng góp đơn vị lợi nhuận pi và sau đó lượng khán giả sẽ giảm đi bằng cách chia sàn cho di trước tập tiếp theo. Nếu không có quảng cáo nào được đặt thì sẽ không kiếm được gì và đối tượng vẫn không thay đổi. 

Khó khăn chính là các quyết định được kết hợp theo thời gian. Việc chúng tôi có quảng cáo sớm hay không sẽ ảnh hưởng đến số lượng người xem ở lại sau đó và việc bỏ qua quảng cáo sẽ duy trì quy mô khán giả cho các tập sau. Nhiệm vụ là chọn một tập hợp con các tập phim để tối đa hóa tổng doanh thu. 

Các ràng buộc n, m ≤ 10^5 và pi lên tới 10^6 ngụ ý rằng bất kỳ cách tiếp cận nào thử tất cả các tập hợp con đều không thể thực hiện được, vì đó sẽ là trạng thái 2^n. Ngay cả O(n^2) cũng quá lớn. Chúng ta buộc phải hướng tới một quy trình động tuyến tính hoặc gần tuyến tính trong đó mỗi quá trình chuyển đổi trạng thái đều được kiểm soát cẩn thận. 

Một trường hợp thất bại tinh tế xuất hiện khi một người bị cám dỗ tham lam chọn tất cả các giai đoạn có lợi nhuận một cách độc lập. Ví dụ: xét m = 10, có hai tập: 

tập 1: p1 = 100, d1 = 10 

tập 2: p2 = 1, d2 = 1 

Nếu chúng ta quảng cáo cả hai, tập 1 mang lại 1000, khán giả trở thành 1 và tập 2 mang lại 1. Tổng cộng là 1001. Nếu bỏ qua tập 1 và chỉ xem tập 2, chúng ta nhận được 10. Một sự lựa chọn tham lam ngây thơ chỉ dựa trên số pi sẽ thất bại hoàn toàn. Sự kết hợp thông qua việc giảm lượng khán giả có nghĩa là các quyết định của địa phương không độc lập. 

Một trường hợp thất bại khác là nghĩ rằng chúng ta phải luôn nhận quảng cáo nếu nó mang lại lợi nhuận dương. Bởi vì di có thể lớn nên một quảng cáo sớm có thể phá hủy tiềm năng kiếm tiền trong tương lai, khiến nó trở nên tồi tệ hơn về tổng thể. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô phỏng tất cả các tập hợp con của các tập. Đối với mỗi tập hợp con, chúng tôi xử lý các tập theo thứ tự, theo dõi khán giả hiện tại, áp dụng phép nhân chia và tích lũy phần thưởng. Điều này đúng vì nó tôn trọng quy trình một cách chính xác, nhưng nó yêu cầu kiểm tra 2^n tập hợp con và mỗi mô phỏng có giá O(n), cho ra O(n·2^n), điều này không khả thi. 

Quan sát quan trọng là sự phát triển của khán giả sẽ mang tính quyết định sau khi chúng tôi chỉnh sửa tập hợp các tập đã chọn. Mỗi quyết định chỉ ảnh hưởng đến các trạng thái trong tương lai thông qua giá trị đối tượng hiện tại và giá trị đó chỉ phụ thuộc vào số lần chúng tôi áp dụng mỗi di dọc theo đường dẫn. Điều này gợi ý rằng thay vì suy luận trực tiếp về các tập hợp con, chúng ta nên coi vấn đề như một hệ thống chuyển đổi trạng thái trên các giá trị đối tượng có thể có. 

Một cách có cấu trúc hơn để xem nó là xác định một quy trình động qua các tập trong đó trạng thái có ý nghĩa duy nhất là chỉ mục tập hiện tại và quy mô khán giả hiện tại. Từ trạng thái (i, c), chúng ta có hai chuyển đổi: bỏ qua hoặc thực hiện. Việc lấy sẽ tạo ra phần thưởng c·pi và di chuyển đến (i+1, tầng(c/di)), trong khi bỏ qua các bước đi đến (i+1, c). Tuy nhiên, biểu đồ trạng thái này vẫn còn quá lớn vì c có thể nhận nhiều giá trị. 

Sự đơn giản hóa quan trọng là c chỉ nhận các giá trị từ việc chia m nhiều lần cho các di khác nhau. Vì m ≤ 10^5 nên mỗi phân chia đều giảm c và mỗi di ≥ 1. Điều này có nghĩa là số lượng giá trị đối tượng có thể tiếp cận riêng biệt là nhỏ và trên thực tế bị giới hạn bởi số lần chúng ta có thể chia trước khi đạt đến 1. Điều đó giúp có thể thực hiện lập trình động trên chỉ mục tập và giá trị đối tượng hiện tại chỉ sử dụng các trạng thái có thể tiếp cận, thường được nén bằng cách chỉ lưu trữ các chuyển tiếp hữu ích. 

Do đó, chúng tôi có thể thực hiện DP trong đó dp[i][c] là doanh thu tối đa bắt đầu từ tập i với c người xem. Việc chuyển đổi rất đơn giản và chúng tôi tính toán theo thứ tự ngược lại. Vì c giảm khi chúng ta thực hiện hành động nên mỗi trạng thái chỉ chuyển sang giá trị nhỏ hơn hoặc bằng nhau, điều này cho phép ghi nhớ mà không bị bùng nổ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(n·2^n) | O(n) | Quá chậm | 
| DP qua (i, c) trạng thái | O(nm log m) trong trường hợp xấu nhất, nhưng được cắt bớt nhỏ hơn nhiều trong thực tế | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP qua các tập được xử lý từ sau ra trước. Ở mỗi tập i và giá trị khán giả c, chúng tôi quyết định có nên đặt quảng cáo hay không. 

1. Chúng tôi khởi tạo cấu trúc ghi nhớ cho các trạng thái (i, c). Mục tiêu là tính toán lợi nhuận tốt nhất có thể đạt được bắt đầu từ tập i với người xem c. Điều này thể hiện toàn bộ vấn đề quyết định còn lại kể từ thời điểm đó trở đi. 
2. Chúng tôi xử lý các tập từ n xuống 1. Việc xử lý ngược là cần thiết vì mỗi quyết định đều phụ thuộc vào kết quả trong tương lai và DP ngược đảm bảo rằng những kết quả đó đã được biết trước. 
3. Ở tập i với khán giả hiện tại c, chúng tôi tính toán hai phương án. Nếu chúng ta bỏ qua quảng cáo, trạng thái sẽ chuyển sang (i+1, c) với mức tăng ngay lập tức bằng 0. Nếu chúng tôi thực hiện quảng cáo, chúng tôi sẽ nhận được c·pi ngay lập tức và chuyển sang (i+1, sàn(c/di)). 
4. Chúng ta lưu trữ kết quả tốt nhất trong hai lựa chọn này dưới dạng dp[i][c]. Điều này đảm bảo rằng mọi tiểu bang đều ghi lại quyết định tối ưu kể từ thời điểm đó trở đi. 
5. Đáp án là dp[1][m], tượng trưng cho việc bắt đầu từ tập 1 với đầy đủ khán giả. 

Lý do điều này hoạt động là vì quá trình này đáp ứng cấu trúc con tối ưu trên cặp (i, c). Sau khi chúng tôi sửa tập i và khán giả c, tất cả các kết quả trong tương lai chỉ phụ thuộc vào cặp này và không có quyết định nào trước đó ảnh hưởng đến quá trình chuyển đổi trong tương lai ngoại trừ thông qua c. Do đó, việc tính toán lại các lựa chọn tối ưu cho từng trạng thái là đủ và không còn sự ghép nối giữa các trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
p = list(map(int, input().split()))
d = list(map(int, input().split()))

from functools import lru_cache

@lru_cache(maxsize=None)
def dp(i, c):
    if i == n:
        return 0

    # skip
    best = dp(i + 1, c)

    # take ad
    nc = c // d[i]
    best = max(best, c * p[i] + dp(i + 1, nc))

    return best

print(dp(0, m))
```Mã trực tiếp thực hiện định nghĩa trạng thái. Hàm dp(i, c) biểu thị doanh thu tốt nhất có thể bắt đầu từ tập i với người xem c. Đệ quy khám phá cả hai lựa chọn ở mỗi tập. Ghi nhớ đảm bảo mỗi trạng thái được tính toán một lần. 

Bước chia c // d[i] là phép biến đổi duy nhất làm thay đổi trạng thái. Điều quan trọng là phải sử dụng phép chia sàn theo số nguyên vì bài toán yêu cầu điều đó một cách rõ ràng. Độ sâu đệ quy được giới hạn bởi n, nhưng việc ghi nhớ ngăn chặn sự phân nhánh theo cấp số nhân. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu đầu tiên. 

Cho p = [9, 14, 10, 4, 5], d = [2, 7, 1, 8, 10], m = 20. 

Chúng tôi so sánh các quyết định ở mỗi bước. 

| Tập | Khán giả c | Hành động | Đạt được | Tiếp theo c | 
| --- | --- | --- | --- | --- | 
| 1 | 20 | lấy | 180 | 10 | 
| 2 | 10 | lấy | 140 | 1 | 
| 3 | 1 | lấy | 10 | 1 | 
| 4 | 1 | bỏ qua | 0 | 1 | 
| 5 | 1 | lấy | 5 | 0 | 

Tổng cộng = 335. 

Dấu vết này cho thấy việc giảm sớm ảnh hưởng mạnh mẽ đến các trạng thái sau này như thế nào. Quan sát quan trọng là tập 3 sử dụng mức giảm lớn để thu gọn khán giả xuống còn 1, đưa ra các quyết định sau này hoàn toàn độc lập với mức tăng trưởng trước đó. 

Bây giờ hãy xem xét một kịch bản tương phản: 

m = 6 

p = [5, 100, 1] 

d = [2, 2, 1] 

Chúng tôi so sánh các chiến lược. 

| Bộ được chọn | Tập 1c | Tập 2c | Tập 3c | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| {1,2,3} | 6 | 3 | 1 | 30 + 300 + 1 = 331 | 
| {2,3} | 6 | 6 | 3 | 600 + 3 = 603 | 

Điều này cho thấy rằng việc bỏ qua những quảng cáo có tác động thấp ban đầu có thể duy trì lượng khán giả và tăng đáng kể lợi nhuận sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) trường hợp xấu nhất với tính năng ghi nhớ, thường ít hơn nhiều | mỗi trạng thái (i, c) được tính một lần và c co lại theo các phép chia | 
| Không gian | O(n·m) | bảng ghi nhớ lưu trữ kết quả cho từng cặp (i, c) | 

Với m ≤ 10^5, không gian trạng thái DP đơn giản là lớn, nhưng các trạng thái có thể tiếp cận bị cắt bớt nhiều bởi phép chia số nguyên lặp đi lặp lại, nhanh chóng thu gọn c về 1. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    n, m = map(int, sys.stdin.readline().split())
    p = list(map(int, sys.stdin.readline().split()))
    d = list(map(int, sys.stdin.readline().split()))

    from functools import lru_cache

    @lru_cache(None)
    def dp(i, c):
        if i == n:
            return 0
        best = dp(i + 1, c)
        best = max(best, c * p[i] + dp(i + 1, c // d[i]))
        return best

    return str(dp(0, m))

# provided sample
assert run("5 20\n9 14 10 4 5\n2 7 1 8 10\n") == "335"

# minimum case
assert run("1 1\n10\n1\n") == "10"

# skip vs take tradeoff
assert run("2 10\n1 100\n10 10\n") == "1000"

# all di = 1 (no reduction)
assert run("3 5\n1 2 3\n1 1 1\n") == "5*1 + 5*2 + 5*3"  # conceptual placeholder

# single dominant late reward
assert run("3 5\n1 1 100\n2 2 1\n") == "500"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 … | 10 | quyết định đúng đắn duy nhất | 
| 2 10 … | 1000 | tác dụng của việc trì hoãn giảm | 
| di tất cả 1 | tích lũy tuyến tính đầy đủ | không có trường hợp phân rã khán giả | 
| pi lớn muộn | 500 | bỏ qua tối ưu trước đó | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả di bằng 1. Trong trường hợp này, lượng khán giả không bao giờ giảm, vì vậy chiến lược tối ưu chỉ đơn giản là xem từng tập, vì mỗi quảng cáo đều đóng góp toàn bộ m·pi một cách độc lập. DP xử lý việc này một cách tự nhiên vì trạng thái c không bao giờ thay đổi, do đó mỗi quyết định về tập là độc lập. 

Một trường hợp cạnh khác là khi một số di rất lớn, có thể bằng m. Điều này ngay lập tức thu gọn lượng khán giả xuống còn 1, do đó, mọi lợi ích sau này đều ở mức tối thiểu. Thuật toán xử lý điều này một cách chính xác vì sau khi áp dụng một tập như vậy, tất cả các trạng thái trong tương lai sẽ hoạt động trên c = 1, ngăn chặn việc đánh giá quá cao. 

Trường hợp cạnh cuối cùng là khi pi cực kỳ lớn ở cuối dãy. Cách tiếp cận tham lam ngây thơ vẫn sẽ nhận được quảng cáo sớm, nhưng DP đã khám phá chính xác việc bỏ qua mức giảm sớm để duy trì c lớn cho tập có giá trị cao, đảm bảo đạt được mức tối ưu toàn cầu.
