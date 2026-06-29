---
title: "CF 104618E - Tô màu hình nón"
description: "Chúng ta có một dòng thuốc nhuộm $N$, mỗi dòng có giá trị vẻ đẹp nguyên dương. Từ chuỗi này, chúng ta muốn chọn một tập hợp con các vị trí sao cho không có hai vị trí được chọn nào liền kề nhau trên dòng ban đầu."
date: "2026-06-29T17:29:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104618
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 1"
rating: 0
weight: 104618
solve_time_s: 79
verified: true
draft: false
---

[CF 104618E - Tô màu hình nón](https://codeforces.com/problemset/problem/104618/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$N$thuốc nhuộm, mỗi loại có giá trị vẻ đẹp nguyên dương. Từ chuỗi này, chúng ta muốn chọn một tập hợp con các vị trí sao cho không có hai vị trí được chọn nào liền kề nhau trên dòng ban đầu. Điểm của một lựa chọn là tổng các giá trị vẻ đẹp đã chọn và chúng tôi muốn tối đa hóa điểm này. 

Nói một cách quen thuộc hơn, đây là tập hợp độc lập có trọng số tối đa trên biểu đồ đường dẫn: mỗi vị trí là một nút, các cạnh kết nối các chỉ số liên tiếp và chúng ta không thể chọn các nút liền kề. 

Ràng buộc$N \le 10^5$loại trừ mọi phép liệt kê tập hợp con theo cấp số nhân. Một sự ngây thơ$O(2^N)$cách tiếp cận là không thể, và thậm chí$O(N^2)$chuyển đổi qua các mảng con sẽ quá chậm. Chúng ta cần một giải pháp quy hoạch động tuyến tính hoặc gần tuyến tính. 

Một trường hợp phức tạp xuất phát từ các mảng nhỏ trong đó trực giác tham lam không thành công. Ví dụ, hãy xem xét: 

đầu vào:```
3
10 1 10
```Một chiến lược tham lam ngây thơ như chọn các phần tử tối ưu cục bộ có thể chọn cả hai số 10 nếu không cẩn thận, nhưng chúng không liền kề nhau, vì vậy chiến lược đó có hiệu quả ở đây. Tuy nhiên: 

đầu vào:```
4
8 9 8 9
```Việc tham lam chọn liên tục phần tử còn lại tối đa mà không theo dõi trạng thái có thể thất bại tùy theo thứ tự thực hiện, trong khi câu trả lời đúng là 18 (chọn chỉ số 2 và 4). 

Một trường hợp cạnh khác là khi$N=1$, trong đó câu trả lời chỉ là một giá trị duy nhất và mọi công thức DP đều phải khởi tạo chính xác các trạng thái cơ sở. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi tập hợp con của các chỉ mục và kiểm tra xem nó có chứa các cặp liền kề hay không. Đối với mỗi tập hợp con, chúng tôi tính tổng của nó và theo dõi mức tối đa. có$2^N$tập hợp con và kiểm tra chi phí của từng tập hợp con$O(N)$nếu được thực hiện trực tiếp, đưa ra$O(N2^N)$thời gian. Ngay cả khi chúng tôi tối ưu hóa việc kiểm tra tập hợp con bằng các phép toán bit, số lượng tập hợp con theo cấp số nhân vẫn là nút thắt cổ chai. 

Quan sát quan trọng là các quyết định mang tính cục bộ: tại chỉ số$i$, chúng ta chỉ cần biết liệu chúng ta đã lấy$i-1$. Nếu chúng ta bỏ qua chỉ mục$i$, điểm số tốt nhất lên đến$i$giống như cho đến$i-1$. Nếu chúng ta lấy chỉ số$i$, chúng ta phải cộng giá trị của nó vào lời giải tốt nhất lên tới$i-2$. Điều này tạo ra một cấu trúc con tối ưu nơi câu trả lời lên đến$i$chỉ phụ thuộc vào hai trạng thái trước đó. 

Điều này làm giảm vấn đề xuống mức tái diễn đơn giản, tương tự như “tổng tối đa của các phần tử không liền kề” cổ điển. 

Cho phép$dp[i]$là số tiền tối đa có thể đạt được chỉ bằng cách sử dụng số đầu tiên$i$các phần tử. Sau đó:$$dp[i] = \max(dp[i-1], dp[i-2] + b_i)$$Điều này ngay lập tức dẫn đến DP thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N2^N)$|$O(N)$| Quá chậm | 
| Lập trình động |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi chỉ duy trì hai trạng thái DP cuối cùng. 

1. Khởi tạo hai biến để thể hiện câu trả lời tốt nhất cho tiền tố trống và tiền tố có độ dài bằng một. Tiền tố trống có giá trị 0 và phần tử đầu tiên có giá trị$b_1$. 
2. Đối với từng vị trí$i$từ 2 đến$N$, tính toán hai khả năng: bỏ qua phần tử hiện tại, phần tử này giữ phần tử tốt nhất trước đó hoặc lấy phần tử hiện tại, phần tử này bổ sung thêm$b_i$để có kết quả tốt nhất từ ​​hai bước lùi lại. Cái lớn hơn trong hai cái này sẽ trở thành trạng thái mới. 
3. Sau khi tính toán trạng thái mới, hãy dịch chuyển các trạng thái trước đó về phía trước để các giá trị “trước” và “lùi hai bước” luôn được căn chỉnh chính xác cho lần lặp tiếp theo. 
4. Tiếp tục cho đến khi tất cả các phần tử được xử lý. Câu trả lời cuối cùng là giá trị tốt nhất ở vị trí cuối cùng. 

Lý do điều này có hiệu quả là vì mọi giải pháp tối ưu đều phải bao gồm hoặc loại trừ phần tử cuối cùng và cả hai trường hợp đều giảm xuống các bài toán con nhỏ hơn một cách nghiêm ngặt không trùng lặp trong các ràng buộc bắt buộc. 

### Tại sao nó hoạt động 

Tại mọi chỉ số$i$, bất kỳ lựa chọn hợp lệ nào cũng sẽ chia thành hai lớp riêng biệt: những lớp loại trừ$i$, chính xác là tất cả các lựa chọn hợp lệ trong lần đầu tiên$i-1$các phần tử và những phần tử bao gồm$i$, lực lượng loại trừ$i-1$và giảm xuống các lựa chọn hợp lệ trong lần đầu tiên$i-2$các phần tử. Hai trường hợp này bao gồm tất cả các khả năng và bảo toàn cấu trúc con tối ưu, do đó, lấy mức tối đa bảo toàn tính tối ưu toàn cục theo cách quy nạp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
b = list(map(int, input().split()))

if n == 0:
    print(0)
    sys.exit()

if n == 1:
    print(b[0])
    sys.exit()

prev2 = 0
prev1 = b[0]

for i in range(1, n):
    take = prev2 + b[i]
    skip = prev1
    cur = max(skip, take)
    prev2 = prev1
    prev1 = cur

print(prev1)
```Việc triển khai nén mảng DP thành hai biến.`prev1`đại diện cho$dp[i-1]$, trong khi`prev2`đại diện cho$dp[i-2]$. Ở mỗi bước, chúng tôi tính toán trực tiếp sự tái phát. 

Các trường hợp cơ bản xử lý$N=1$rõ ràng để chúng tôi không dựa vào các giá trị DP không xác định. Thứ tự chuyển tiếp quan trọng:`prev2`phải được cập nhật sau khi tính toán trạng thái hiện tại, nếu không chúng ta sẽ ghi đè giá trị cần thiết cho lần lặp tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 10 9 10 7
```| tôi | b[i] | bỏ qua (dp[i-1]) | lấy (dp[i-2]+b[i]) | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 5 | 5 | 
| 1 | 10 | 5 | 10 | 10 | 
| 2 | 9 | 10 | 14 | 14 | 
| 3 | 10 | 14 | 20 | 20 | 
| 4 | 7 | 20 | 21 | 21 | 

Giá trị cuối cùng 21 xuất phát từ việc chọn chỉ số 1, 3 và 5 trong lập chỉ mục dựa trên 1 (10 + 10 + 7). Bảng này cho thấy cách mỗi trạng thái so sánh rõ ràng việc bỏ qua và lấy, đảm bảo các ràng buộc lân cận được tôn trọng. 

### Ví dụ 2 

đầu vào:```
4
8 9 8 9
```| tôi | b[i] | bỏ qua | lấy | dp | 
| --- | --- | --- | --- | --- | 
| 0 | 8 | 0 | 8 | 8 | 
| 1 | 9 | 8 | 9 | 9 | 
| 2 | 8 | 9 | 16 | 16 | 
| 3 | 9 | 16 | 17 | 17 | 

Giải pháp tối ưu xen kẽ các lựa chọn để tránh sự liền kề. DP tích lũy chính xác các lượt chọn xen kẽ, xác nhận rằng các lựa chọn cục bộ tham lam là không đủ nếu không theo dõi các quyết định trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi phần tử được xử lý một lần với quá trình chuyển đổi theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có hai trạng thái DP luân phiên được lưu trữ | 

Quét tuyến tính là tối ưu cho$N = 10^5$và bộ nhớ không đổi đảm bảo không gây áp lực lên giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input().strip())
    b = list(map(int, input().split()))
    
    if n == 0:
        return "0"
    if n == 1:
        return str(b[0])

    prev2 = 0
    prev1 = b[0]

    for i in range(1, n):
        cur = max(prev1, prev2 + b[i])
        prev2 = prev1
        prev1 = cur

    return str(prev1)

# provided sample
assert run("5\n5 10 9 10 7\n") == "21"

# minimum size
assert run("1\n42\n") == "42"

# two elements
assert run("2\n5 100\n") == "100"

# all equal
assert run("5\n7 7 7 7 7\n") == "21"

# alternating high values
assert run("4\n10 1 10 1\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 42 | tính đúng đắn của trường hợp cơ sở | 
| 2 yếu tố | 100 | lựa chọn tối đa chính xác giữa cặp liền kề | 
| tất cả đều bình đẳng | 21 | xử lý nhiều lựa chọn tối ưu | 
| đỉnh xen kẽ | 20 | DP chọn các chỉ số xen kẽ một cách tối ưu | 

## Vỏ cạnh 

cho$N=1$, thuật toán trực tiếp trả về giá trị đơn vì vòng lặp bị bỏ qua hoàn toàn. Điều này tránh việc truy cập các trạng thái DP không hợp lệ. 

Vì$N=2$, quá trình chuyển đổi sẽ so sánh chính xác việc lấy phần tử thứ nhất hoặc thứ hai mà không cần bất kỳ logic đặc biệt nào ngoài việc khởi tạo. 

Ví dụ:```
2
5 100
```Việc khởi tạo mang lại`prev2 = 0`,`prev1 = 5`. Tại chỉ số 2, chúng tôi tính toán`take = 100`,`skip = 5`, do đó kết quả là 100. Điều này phù hợp với lựa chọn tối ưu là chỉ chọn phần tử thứ hai. 

Điều này cho thấy phép truy toán xử lý các ràng buộc lân cận một cách tự nhiên ngay cả trong các cấu hình tối thiểu mà không cần logic phân nhánh bổ sung.
