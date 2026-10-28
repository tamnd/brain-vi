---
title: "CF 106159C - Tạo danh sách phát"
description: "Chúng ta được cung cấp một chuỗi các bài hát, mỗi bài hát gắn liền với một giá trị có thể dương hoặc âm. Chúng tôi muốn chọn một tập hợp con của các bài hát này để tối đa hóa tổng giá trị đã chọn, nhưng có một hạn chế về khoảng cách: nếu chúng tôi chọn một bài hát ở vị trí i thì chúng tôi bị cấm…"
date: "2026-06-20T22:12:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106159
codeforces_index: "C"
codeforces_contest_name: "XIII UnB Contest Mirror"
rating: 0
weight: 106159
solve_time_s: 41
verified: true
draft: false
---

[CF 106159C - Tạo danh sách phát](https://codeforces.com/problemset/problem/106159/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các bài hát, mỗi bài hát gắn liền với một giá trị có thể dương hoặc âm. Chúng tôi muốn chọn một tập hợp con của các bài hát này để tối đa hóa tổng giá trị đã chọn, nhưng có một hạn chế về khoảng cách: nếu chúng tôi chọn một bài hát ở vị trí`i`, sau đó chúng tôi bị cấm chọn bất kỳ bài hát nào tiếp theo`K - 1`vị trí, nghĩa là chỉ số nghiêm ngặt giữa`i`Và`i + K`không thể được chọn. 

Nhiệm vụ là tính tổng số tốt nhất có thể theo quy tắc này, bao gồm cả khả năng không chọn bài hát nào, dẫn đến tổng bằng 0. 

Các ràng buộc cho phép lên đến`N = 10^5`các phần tử. Một giải pháp ngây thơ thử tất cả các tập hợp con hoặc thậm chí tất cả các kết hợp hợp lệ sẽ bùng nổ theo cấp số nhân và thậm chí lập trình động kiểm tra tất cả các ứng cử viên trước đó cho mỗi vị trí sẽ có nguy cơ xảy ra hành vi bậc hai. Với giới hạn 1 giây, chúng ta nên hướng tới thời gian gần như tuyến tính hoặc gần tuyến tính, chắc chắn không tệ hơn`O(N log N)`. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều âm. Cách giải thích tham lam hoặc lựa chọn bắt buộc có thể chọn sai một số bài hát, nhưng chiến lược tối ưu là không chọn gì và đưa ra số 0. Một trường hợp cạnh khác là khi`K = 1`, điều này loại bỏ tất cả các hạn chế và giảm vấn đề xuống mức đơn giản là nhận mọi đóng góp tích cực. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của bài hát và kiểm tra xem nó có tuân thủ quy tắc giãn cách hay không. Điều này đúng nhưng không khả thi vì có`2^N`các tập hợp con và thậm chí việc xác thực từng tập hợp con sẽ tiêu tốn thời gian tuyến tính, dẫn đến thời gian chạy lớn đến mức khủng khiếp. 

Một lực lượng vũ phu có cấu trúc hơn sẽ cải thiện đôi chút bằng cách đưa ra quyết định ở mỗi chỉ số: hoặc lấy bài hát`i`và nhảy tới`i + K`, hoặc bỏ qua và chuyển đến`i + 1`. Phép đệ quy này khám phá chính xác tất cả các giải pháp hợp lệ nhưng vẫn phân nhánh nhiều, dẫn đến hành vi hàm mũ trong trường hợp xấu nhất vì các trạng thái hậu tố giống nhau được tính toán lại nhiều lần. 

Quan sát quan trọng là vấn đề có cấu trúc con tối ưu trên các vị trí. Nếu chúng tôi xác định câu trả lời tốt nhất bắt đầu từ chỉ mục`i`, nó chỉ phụ thuộc vào những lựa chọn trong tương lai và quyết định tại`i`là cục bộ: bao gồm nó và bỏ qua`K`, hoặc loại trừ nó và tiếp tục tại`i + 1`. Điều này tự nhiên dẫn đến lập trình động trên các chỉ số. 

Chúng tôi xác định`dp[i]`là số tiền tối đa chúng ta có thể nhận được bắt đầu từ vị trí`i`. Sau đó, chúng tôi tính toán các chuyển đổi theo thứ tự ngược lại để biết trước các trạng thái trong tương lai. Điều này làm giảm vấn đề thành DP tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| DP trên chỉ số | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ta xử lý mảng từ phải qua trái sao cho khi ở vị trí`i`, chúng tôi đã biết câu trả lời tốt nhất cho các vị trí ngoài nó. 

1. Xác định mảng DP`dp`kích thước`N + K + 5`, được khởi tạo bằng số không. Khoảng trống thừa sẽ tránh được việc kiểm tra giới hạn khi nhảy về phía trước. 
2. Lặp lại`i`từ`N`xuống tới`1`. Tại mỗi vị trí, chúng tôi tính toán hai khả năng. 
3. Tùy chọn đầu tiên là bỏ qua bài hát hiện tại, điều này mang lại giá trị`dp[i + 1]`. Điều này thể hiện việc không sử dụng bài hát`i`không hề. 
4. Lựa chọn thứ hai là lấy bài hát`i`, mang lại giá trị`A[i] + dp[i + K]`. Điều này hoạt động vì sau khi chọn`i`, vị trí hợp lệ tiếp theo là`i + K`. 
5. Đặt`dp[i]`đạt giá trị lớn nhất của hai giá trị này. 
6. Sau khi điền vào bảng câu trả lời là`dp[1]`, nhưng chúng tôi cũng so sánh nó với 0 để cho phép lựa chọn trống. 

### Tại sao nó hoạt động 

Tại mọi chỉ số`i`, tiểu bang`dp[i]`nắm bắt số tiền tốt nhất có thể đạt được chỉ bằng cách sử dụng các chỉ số`i`và hơn thế nữa. Bất kỳ giải pháp hợp lệ nào cũng phải bao gồm`i`hoặc loại trừ nó. Nếu loại trừ`i`, chính xác là vậy`dp[i + 1]`. Nếu nó bao gồm`i`, hạn chế buộc lựa chọn tiếp theo bắt đầu tại`i + K`, và sự tiếp nối tốt nhất từ ​​đó là`dp[i + K]`. Vì cả hai lựa chọn đều liệt kê tất cả các khả năng hợp lệ mà không trùng lặp, nên quá trình lặp lại hoàn tất và lấy mức tối ưu bảo toàn tối đa theo cách quy nạp từ cuối mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())
    A = list(map(int, input().split()))
    
    # 1-index the array for clarity
    A = [0] + A
    
    dp = [0] * (N + K + 5)
    
    for i in range(N, 0, -1):
        take = A[i] + dp[i + K]
        skip = dp[i + 1]
        dp[i] = max(take, skip)
    
    print(max(0, dp[1]))

if __name__ == "__main__":
    solve()
```Mảng DP có kích thước được đệm thêm để`i + K`Và`i + 1`luôn là các truy cập hợp lệ, tránh việc kiểm tra có điều kiện bên trong vòng lặp. Quá trình chuyển đổi mã hóa trực tiếp hai quyết định có thể có ở mỗi chỉ mục. 

trận chung kết`max(0, dp[1])`là cần thiết vì phép truy toán giả định chúng ta phải bắt đầu từ vị trí`1`, nhưng chiến lược tối ưu có thể không liên quan đến việc chọn gì nếu tất cả các đóng góp đều âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`N = 3, K = 1, A = [1, 2, 3]`Đây`K = 1`có nghĩa là thực sự không có hạn chế nào giữa các lần chọn. 

| tôi | A[i] | dp[i+1] | A[i] + dp[i+K] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 3 | 3 | 0 | 3 | 3 | 
| 2 | 2 | 3 | 5 | 5 | 
| 1 | 1 | 5 | 6 | 6 | 

Kết quả cuối cùng là`6`, xác nhận rằng tất cả các yếu tố có thể được thực hiện. 

### Ví dụ 2 

đầu vào:`N = 4, K = 4, A = [2, 3, 0, 2]`Ở đây việc chọn bất kỳ chỉ mục nào sẽ chặn tất cả những chỉ mục khác. 

| tôi | A[i] | dp[i+1] | A[i] + dp[i+K] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 4 | 2 | 0 | 2 | 2 | 
| 3 | 0 | 2 | 0 | 2 | 
| 2 | 3 | 2 | 3 | 3 | 
| 1 | 2 | 3 | 2 | 3 | 

Câu trả lời tối ưu là`3`, tương ứng với việc chọn bài hát thứ hai. 

Những dấu vết này cho thấy DP cân bằng một cách tự nhiên lợi ích địa phương với khả năng sẵn có trong tương lai như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi vị trí được xử lý một lần với chuyển tiếp O(1) | 
| Không gian | O(N) | Mảng DP lưu trữ các giá trị cho từng vị trí cộng với phần đệm | 

Độ phức tạp tuyến tính dễ dàng phù hợp với các ràng buộc đối với`N ≤ 10^5`và mức sử dụng bộ nhớ vẫn nằm trong giới hạn thông thường vì chúng tôi chỉ lưu trữ một mảng có kích thước tỷ lệ thuận với đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else __import__("builtins").print  # placeholder

# Note: In actual CF usage, replace run with proper capture logic

# Sample-style checks (conceptual)
# assert run("3 1\n1 2 3\n") == "6\n"

# custom cases
# all negative
# assert run("3 2\n-1 -2 -3\n") == "0\n"

# K = N case
# assert run("4 4\n2 3 0 2\n") == "3\n"

# alternating positives
# assert run("5 2\n5 1 5 1 5\n") == "15\n"

# minimum case
# assert run("1 1\n-5\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 / -1 -2 -3`|`0`| xử lý hoàn toàn tiêu cực | 
|`4 4 / 2 3 0 2`|`3`| ràng buộc toàn khối | 
|`5 2 / 5 1 5 1 5`|`15`| mẫu bỏ qua tối ưu | 
|`1 1 / -5`|`0`| trường hợp cạnh đơn phần tử | 

## Vỏ cạnh 

Khi tất cả các giá trị đều âm, DP vẫn sẽ tính tổng hậu tố tốt nhất có thể đạt được, nhưng mọi chuyển đổi sẽ truyền các giá trị âm trừ khi chúng tôi kẹp kết quả một cách rõ ràng. trận chung kết`max(0, dp[1])`đảm bảo rằng ngay cả khi mọi lựa chọn đều làm giảm tổng, chúng tôi vẫn ưu tiên lựa chọn trống. 

Đối với đầu vào`N = 3, K = 2, A = [-1, -2, -3]`, DP phát triển như sau. Tại`i = 3`,`dp[3] = max(-3, 0) = 0`. Tại`i = 2`, nhận cho`-2 + dp[4] = -2`, bỏ qua mang lại`0`, Vì thế`dp[2] = 0`. Tại`i = 1`, nhận cho`-1 + dp[3] = -1`, bỏ qua mang lại`0`, Vì thế`dp[1] = 0`. Câu trả lời cuối cùng là`0`, khớp với danh sách phát trống. 

Khi`K = 1`, mọi chỉ số đều độc lập. Vì`A = [1, 2, 3]`, mỗi bang so sánh việc lấy`A[i] + dp[i+1]`chống bỏ qua`dp[i+1]`, điều này đơn giản chỉ là tích lũy tất cả các mặt tích cực và DP tính tổng chính xác mọi thứ vì mọi đóng góp đều có lợi.
