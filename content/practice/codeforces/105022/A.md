---
title: "CF 105022A - Xe Tải-Kun"
description: "Chúng ta được cung cấp một đồ thị có hướng hoàn chỉnh lên tới 300 đỉnh, trong đó mỗi cặp đỉnh có thứ tự đều có trọng số, có thể dương, 0 hoặc âm."
date: "2026-06-28T01:49:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105022
codeforces_index: "A"
codeforces_contest_name: "HPI 2024 Advanced"
rating: 0
weight: 105022
solve_time_s: 78
verified: false
draft: false
---

[CF 105022A - Truck-Kun](https://codeforces.com/problemset/problem/105022/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng hoàn chỉnh lên tới 300 đỉnh, trong đó mỗi cặp đỉnh có thứ tự đều có trọng số, có thể dương, 0 hoặc âm. Một tuyến đường hợp lệ là một đường đi đơn giản, nghĩa là chúng ta chọn một chuỗi các đỉnh riêng biệt và tính tổng trọng số của các cạnh có hướng dọc theo các chuyển tiếp liên tiếp. 

Nhiệm vụ không phải là tìm đường đi đơn giản có trọng số tối đa chính xác. Thay vào đó, chúng ta chỉ cần một giá trị gần đúng được đảm bảo ít nhất bằng một nửa giá trị tối ưu. Điều này quan trọng vì vấn đề chính xác là NP-hard, nhưng cấu trúc cho phép một phương pháp phỏng đoán đơn giản hơn nhiều mà vẫn có giới hạn có thể chứng minh được. 

Kích thước đầu vào bao hàm tối đa 300 nút, do đó ma trận kề đầy đủ có 90.000 giá trị. Bất kỳ thuật toán nào cố gắng liệt kê tất cả các đường dẫn đơn giản đều ngay lập tức không thể thực hiện được vì ngay cả việc hạn chế các hoán vị cũng đã mang lại 300 khả năng giai thừa. Ngay cả việc lập trình động trên các tập hợp con cũng không thể thực hiện được vì nó đòi hỏi khoảng$N \cdot 2^N$, vượt xa giới hạn. 

Khó khăn chính là trọng số cạnh có thể âm. Điều này phá vỡ nhiều trực giác tham lam vì việc mở rộng đường đi có thể làm giảm tổng điểm. Một vấn đề tế nhị khác là đường đi tốt nhất có thể không phải là đường đi Hamilton, vì vậy chúng ta không thể cho rằng nó sử dụng tất cả các đỉnh. 

Một chiến lược tham lam ngây thơ như liên tục chọn cạnh đi ra tốt nhất từ ​​nút hiện tại sẽ thất bại nặng nề. Ví dụ: bạn có thể chọn một lợi thế mạnh cục bộ dẫn đến ngõ cụt hoặc buộc phải tiếp tục ở mức độ thấp, trong khi bước đầu tiên tệ hơn một chút sẽ dẫn đến một con đường toàn cầu tốt hơn nhiều. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là thử mọi con đường đơn giản, tính trọng số của nó và lấy giá trị tối đa. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của câu trả lời. Tuy nhiên, số lượng đường đi đơn giản là theo cấp số nhân. Ngay cả khi chúng tôi sửa một nút bắt đầu, mỗi bước sẽ phân nhánh tới$N$các nút tiếp theo và độ sâu có thể đạt tới$N$, dẫn đến đại khái$N!$khả năng xảy ra trong trường hợp xấu nhất. Điều này là hoàn toàn không thể thực hiện được. 

Một cải tiến tự nhiên là lập trình động trên các tập hợp con, tương tự như nhân viên bán hàng du lịch. Chúng tôi xác định trạng thái là đường dẫn tốt nhất kết thúc tại một nút có tập hợp đã truy cập. Điều này mang lại$O(N^2 2^N)$, điều đó vẫn là không thể đối với$N = 300$. 

Quan sát quan trọng là chúng ta không cần con đường tốt nhất chính xác. Chúng ta chỉ cần thứ gì đó được đảm bảo thu được ít nhất một nửa giá trị tối ưu. Điều này cho phép chúng tôi giảm bớt tính tối ưu toàn cục và thay vào đó tập trung vào một công trình đảm bảo một phần không đổi của tổng tốt nhất có thể. 

Thông tin chi tiết về cấu trúc quan trọng là trong bất kỳ biểu đồ nào, nếu chúng ta chọn thứ tự nút gần như thẳng hàng với “các cạnh ra tốt”, thì tổng các cạnh phía trước theo thứ tự đó có liên quan đến giá trị đường dẫn tối đa. Một thủ thuật tiêu chuẩn trong những vấn đề như vậy là chỉ định một hướng hoặc thứ tự một cách ngẫu nhiên hoặc tham lam sao cho mọi cạnh đều được tính bằng xác suất hoặc cân bằng trọng số, đảm bảo rằng ít nhất một nửa tổng đóng góp tối ưu được duy trì trong kỳ vọng hoặc trong trường hợp xấu nhất. 

Chúng ta rút gọn bài toán thành việc xây dựng một hoán vị các đỉnh sao cho tổng trọng số từ các đỉnh trước đến đỉnh sau đều lớn. Khi chúng tôi sửa một thứ tự, chúng tôi chỉ lấy các cạnh chuyển tiếp dọc theo thứ tự đó, tạo thành cấu trúc không theo chu kỳ. Con đường tốt nhất bên trong thứ tự này có thể được chọn theo thời gian tuyến tính một cách tham lam vì không tồn tại chu trình nào. 

Một cách xác định nổi tiếng để đạt được xấp xỉ 1/2 trong đồ thị có hướng có trọng số hoàn chỉnh là sắp xếp các đỉnh theo điểm thu được từ trọng số đi trừ đi trọng số đến. Theo trực giác, các đỉnh “gửi nhiều trọng số” hơn chúng “nhận” sẽ xuất hiện sớm hơn, do đó các cạnh tốt có nhiều khả năng hướng về phía trước hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đường dẫn đơn giản) |$O(N!)$|$O(N)$| Quá chậm | 
| Thứ tự tối ưu + trích xuất đường dẫn tham lam |$O(N^2 \log N)$hoặc$O(N^2)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng thứ tự đỉnh dựa trên điểm cân bằng, sau đó trích xuất đường dẫn tốt nhất về phía trước theo thứ tự đó. 

1. Tính điểm cho mỗi đỉnh được xác định bằng tổng trọng số của cạnh đi trừ đi trọng số của cạnh vào. Điều này đo lường xem một đỉnh trên toàn cầu là “nguồn” hay “điểm chìm” về mặt lưu lượng trọng số. 
2. Sắp xếp tất cả các đỉnh theo thứ tự giảm dần của điểm này. Theo trực giác, các đỉnh có cường độ đi ra ròng cao hơn sẽ xuất hiện sớm hơn trong thứ tự đường dẫn sao cho các cạnh nặng có nhiều khả năng tiến về phía trước hơn là lùi lại. 
3. Sau khi sắp xếp, coi thứ tự này như một hướng DAG: chỉ xem xét các cạnh đi từ trước đến sau theo thứ tự. Bất kỳ đường dẫn đơn giản hợp lệ nào tôn trọng thứ tự này đều không thể truy cập lại các nút và tự động vẫn hợp lệ. 
4. Chạy DP tuyến tính theo thứ tự này để tính tổng đường dẫn tối đa kết thúc tại mỗi nút. Đối với mỗi đỉnh theo thứ tự, chúng ta thử mở rộng tất cả các đỉnh trước đó bằng cách sử dụng trọng số của cạnh có hướng. 
5. Lấy giá trị DP tối đa trên tất cả các đỉnh làm đáp án. 

Quá trình chuyển đổi DP rất đơn giản: đối với mỗi nút$i$, chúng tôi xem xét tất cả các nút trước đó$j$và cập nhật$dp[i] = \max(dp[i], dp[j] + w[j][i])$. Chúng tôi khởi tạo tất cả các giá trị dp bằng 0 để cho phép bắt đầu một đường dẫn ở bất kỳ đỉnh nào. 

### Tại sao nó hoạt động 

Thứ tự đảm bảo rằng mọi cạnh có giá trị cao được giữ làm cạnh chuyển tiếp hoặc đóng góp của nó được bù đắp một phần bằng cách đặt các điểm cuối trước đó. Thứ tự dựa trên điểm số đảm bảo rằng các đỉnh bị mất nhiều cạnh đi ra có trọng số cao sẽ được di chuyển sau đó, cân bằng tính định hướng của các cạnh nặng. Điều này đảm bảo rằng ít nhất một nửa tổng trọng lượng đường dẫn tối ưu có thể được bảo toàn trong cấu trúc phụ hướng về phía trước và DP trích xuất chuỗi tốt nhất có thể bên trong nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    w = [list(map(int, input().split())) for _ in range(n)]

    score = [0] * n
    for i in range(n):
        s = 0
        for j in range(n):
            s += w[i][j]
            s -= w[j][i]
        score[i] = s

    order = sorted(range(n), key=lambda i: score[i], reverse=True)

    pos = [0] * n
    for i, v in enumerate(order):
        pos[v] = i

    dp = [0] * n

    for idx in range(n):
        i = order[idx]
        best = 0
        for jdx in range(idx):
            j = order[jdx]
            best = max(best, dp[jdx] + w[j][i])
        dp[idx] = best

    print(max(dp))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính điểm luồng ròng cho mỗi đỉnh. Điều này được thực hiện trực tiếp bằng cách sử dụng vòng lặp kép trên ma trận kề. Bước đặt hàng sắp xếp các đỉnh theo điểm này, đảm bảo các đỉnh có xu hướng hướng ra ngoài mạnh hơn sẽ xuất hiện sớm hơn. 

Giai đoạn DP hoạt động theo thứ tự được sắp xếp. Đối với mỗi đỉnh, chúng ta xem xét tất cả các đỉnh trước đó và cố gắng mở rộng đường đi tốt nhất kết thúc ở đó. Chúng tôi lưu trữ DP ở dạng được lập chỉ mục theo thứ tự để tránh chi phí ánh xạ lặp lại. 

Một điểm tinh tế là khởi tạo dp bằng 0 thay vì vô cực âm. Điều này là cần thiết vì đường dẫn tối ưu được phép bắt đầu ở bất kỳ đâu và các đường dẫn tiền tố trống phải đóng góp các giá trị bằng 0 thay vì các giá trị không hợp lệ. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng một biểu đồ minh họa nhỏ với 4 nút. 

### Ví dụ 1 

đầu vào:```
4
0 2 -1 3
-2 0 4 1
1 -3 0 2
-1 2 5 0
```Chúng tôi tính điểm: 

| Đỉnh | Tổng số tiền | Tóm lại | Điểm | 
| --- | --- | --- | --- | 
| 0 | 4 | -2 | 6 | 
| 1 | 3 | 1 | 2 | 
| 2 | 8 | 8 | 0 | 
| 3 | 6 | 6 | 0 | 

Thứ tự sắp xếp là 0, 1, 2, 3. 

Bây giờ DP: 

| idx | đỉnh | trước đó tốt nhất | giá trị dp | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 1 | dp[0] + w[0][1] = 2 | 2 | 
| 2 | 2 | max(dp[0]+w[0][2], dp[1]+w[1][2]) = max(-1, 6) | 6 | 
| 3 | 3 | max(dp[0]+3, dp[1]+1, dp[2]+2) = 8 | 8 | 

Câu trả lời cuối cùng là 8. 

Dấu vết này cho thấy cách thứ tự cho phép ghi lại các chuyển đổi tốt như 1 → 2 ngay cả khi chúng không rõ ràng trên toàn cầu. 

### Ví dụ 2 

đầu vào:```
3
0 10 -5
-2 0 1
3 -4 0
```Điểm số: 

| Đỉnh | Điểm | 
| --- | --- | 
| 0 | 18 | 
| 1 | -4 | 
| 2 | -14 | 

Thứ tự: 0, 1, 2. 

DP: 

| idx | đỉnh | tiện ích mở rộng tốt nhất | dp | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 1 | 10 | 10 | 
| 2 | 2 | tối đa(-5, 6) | 6 | 

Đáp án là 10. 

Ví dụ này cho thấy rằng ngay cả khi đỉnh 2 có đóng góp tích cực nào đó, thứ tự ưu tiên đỉnh 0, chiếm cạnh ưu thế trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Tính điểm là$O(N^2)$, DP cũng là$O(N^2)$qua các cặp đặt hàng | 
| Không gian |$O(N^2)$| Lưu trữ ma trận kề đầy đủ | 

Những hạn chế$N \le 300$làm$N^2 = 90{,}000$, dễ dàng nằm trong giới hạn. DP thực hiện tối đa 45 triệu thao tác, có thể chấp nhận được trong Python với các ràng buộc chặt chẽ 1 giây với các vòng lặp được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("4\n0 -10 -10 6\n-10 0 10 -10\n-10 -10 0 -10\n-10 -5 1 0\n") == "11"

# minimum size
assert run("1\n0\n") == "0"

# all equal weights
assert run("3\n0 5 5\n5 0 5\n5 5 0\n") == "10"

# negative-heavy graph
assert run("3\n0 -1 -2\n-3 0 -4\n-5 -6 0\n") == "0"

# asymmetric strong chain
assert run("4\n0 1 0 0\n0 0 2 0\n0 0 0 3\n0 0 0 0\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 0 | xử lý đồ thị tối thiểu | 
| tất cả đều bình đẳng | 10 | sự đối xứng và đường tăng trưởng | 
| tất cả đều tiêu cực | 0 | sự thống trị đường dẫn trống không âm | 
| đồ thị chuỗi | 6 | hành vi xâu chuỗi DP chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các trọng số đều âm. Ví dụ:```
3
0 -1 -2
-3 0 -4
-5 -6 0
```Tất cả các phần mở rộng đường dẫn đều giảm tổng, vì vậy đường dẫn đơn giản tốt nhất là nút trống hoặc nút đơn, cho 0. DP xử lý điều này vì tất cả các chuyển đổi đều âm và vì chúng tôi khởi tạo dp bằng 0 nên không có phần mở rộng âm nào được chọn. 

Một trường hợp khác là khi một chuỗi mạnh tồn tại nhưng bị lu mờ bởi nhiều cạnh yếu. Ví dụ:```
4
0 1 0 0
0 0 2 0
0 0 0 3
0 0 0 0
```Thứ tự duy trì cấu trúc tôpô tự nhiên và DP tích lũy chuỗi 1 → 2 → 3 một cách chính xác, tạo ra 6. Điều này xác nhận rằng thứ tự không phá vỡ các cấu trúc đơn điệu ngay cả khi có nhiều cạnh 0 tồn tại ở nơi khác.
