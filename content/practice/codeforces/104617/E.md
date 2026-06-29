---
title: "CF 104617E - Tô màu hình nón"
description: "Chúng ta được cấp một dòng thuốc nhuộm màu, mỗi thuốc nhuộm có một giá trị nguyên dương thể hiện mức độ “đẹp” của nó."
date: "2026-06-29T18:22:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104617
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 2 (Beginner)"
rating: 0
weight: 104617
solve_time_s: 74
verified: true
draft: false
---

[CF 104617E - Tô màu hình nón](https://codeforces.com/problemset/problem/104617/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dòng thuốc nhuộm màu, mỗi thuốc nhuộm có một giá trị nguyên dương thể hiện mức độ “đẹp” của nó. Chúng tôi muốn chọn một tập hợp con của các loại thuốc nhuộm này để tối đa hóa tổng thể vẻ đẹp, nhưng có một hạn chế: chúng tôi không được phép chọn hai loại thuốc nhuộm nằm cạnh nhau trong dòng ban đầu. 

Nói một cách cụ thể hơn, đây là một dãy số và chúng ta phải chọn một tập hợp con các chỉ số sao cho không có hai chỉ số nào được chọn liên tiếp. Mục tiêu là tối đa hóa tổng các giá trị đã chọn. 

Kích thước đầu vào có thể lên tới 100.000 phần tử. Giải pháp thử tất cả các tập hợp con của chỉ số là không thể vì số lượng tập hợp con là theo cấp số nhân. Ngay cả cách tiếp cận bậc hai để kiểm tra tất cả các cặp cũng sẽ quá chậm ở quy mô này. Các ràng buộc đề xuất rõ ràng một giải pháp lập trình động tuyến tính hoặc gần tuyến tính trong đó mỗi phần tử được xử lý một lần và chúng tôi sử dụng lại các kết quả được tính toán trước đó. 

Một vài trường hợp đáng chú ý. Nếu chỉ có một loại thuốc nhuộm thì câu trả lời đơn giản là giá trị của loại thuốc nhuộm đó. 

Ví dụ: đầu vào:```
1
7
```Đầu ra đúng là:```
7
```Một cách tiếp cận đơn giản giả định ít nhất hai phần tử và khởi tạo DP không chính xác có thể trả về 0 hoặc bị lỗi. 

Nếu tất cả các giá trị đều bằng nhau, hãy nói:```
4
5 5 5 5
```Chiến lược tối ưu là chọn các phần tử xen kẽ, cho 5 + 5 = 10. Bất kỳ cách tiếp cận tham lam nào luôn chọn các lựa chọn liền kề tối ưu cục bộ đều có thể thất bại nếu nó không thực thi đúng khoảng cách toàn cục. 

Một trường hợp tinh vi khác phát sinh khi một giá trị rất lớn được bao quanh bởi hai giá trị trung bình. Ví dụ:```
3
1 100 1
```Câu trả lời đúng là 100, không phải 2 hay 101 tùy thuộc vào logic tham lam còn thiếu sót. Điều này cho thấy tại sao các quyết định cục bộ như “luôn lấy phần lớn hơn của các phần tử liền kề” là không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi tập hợp con của các chỉ số và kiểm tra xem nó có hợp lệ hay không (không có hai lần chọn liên tiếp), sau đó tính tổng của nó. Điều này hoạt động về mặt khái niệm vì nó trực tiếp thực thi ràng buộc, nhưng nó yêu cầu lặp lại trên tất cả các tập hợp con có kích thước 2^N. Ngay cả khi chúng tôi cải thiện một chút bằng cách tạo ra các tập hợp con có tính năng quay lui và cắt bớt các lựa chọn liền kề, trường hợp xấu nhất vẫn khám phá số trạng thái theo cấp số nhân, điều này trở nên không khả thi từ rất lâu trước khi N đạt tới 40. 

Cấu trúc của bài toán gợi ý rằng quyết định ở vị trí i chỉ phụ thuộc vào việc chúng ta chọn i hay bỏ qua nó. Nếu chúng ta lấy i thì i-1 không thể lấy được. Nếu chúng ta bỏ qua i thì chúng ta sẽ kế thừa kết quả tốt nhất cho đến i-1. Điều này tạo ra một cấu trúc con tối ưu đơn giản trên các tiền tố của mảng. 

Đặt dp[i] đại diện cho tổng vẻ đẹp tối đa mà chúng ta có thể đạt được chỉ bằng cách sử dụng thuốc nhuộm i đầu tiên. Đối với mỗi vị trí i, chúng ta có hai lựa chọn: hoặc chúng ta không lấy b[i], trong trường hợp đó chúng ta giữ dp[i-1], hoặc chúng ta lấy b[i], trong trường hợp đó chúng ta phải thêm nó vào dp[i-2]. Điều này mang lại sự tái diễn trực tiếp và giảm vấn đề xuống dạng quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| Lập trình động | O(N) | O(N) (hoặc O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì câu trả lời tốt nhất có thể cho mỗi tiền tố. 

1. Xác định dp[0] = 0, vì không có phần tử nào nên chúng ta không thể chọn bất cứ thứ gì. 
2. Xác định dp[1] = b[0], vì với một phần tử, chúng ta hoặc lấy nó hoặc không có gì tốt hơn. 
3. Với mỗi chỉ số i từ 2 đến N - 1, tính dp[i] như sau: 

Chúng tôi so sánh việc bỏ qua phần tử hiện tại với việc lấy nó. Bỏ qua sẽ cho dp[i-1]. Lấy nó sẽ cho dp[i-2] + b[i]. Chúng tôi chọn mức tối đa của hai điều này. 
4. Sau khi xử lý tất cả các phần tử, dp[N-1] chứa câu trả lời. 

Phiên bản tiết kiệm bộ nhớ hơn chỉ giữ lại hai trạng thái DP cuối cùng thay vì toàn bộ mảng. 

### Tại sao nó hoạt động 

Tại mỗi chỉ mục i, mọi lựa chọn hợp lệ trên tiền tố [0..i] phải thuộc đúng một trong hai loại: nó bao gồm i hoặc không. Nếu nó bao gồm i thì i-1 bị cấm và phần đóng góp còn lại phải đến từ một giải pháp tối ưu trên [0..i-2]. Nếu nó loại trừ i thì giá trị tốt nhất có thể chính xác là nghiệm tối ưu trên [0..i-1]. Hai trường hợp này bao gồm tất cả các khả năng mà không có sự trùng lặp, do đó đạt được mức tối ưu duy trì tối đa ở mọi tiền tố. Bởi vì mọi giải pháp tiền tố đều được xây dựng từ các tiền tố tối ưu nhỏ hơn nên không có quyết định nào sau này có thể làm mất hiệu lực các lựa chọn tối ưu trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    b = list(map(int, input().split()))
    
    if n == 1:
        print(b[0])
        return
    
    prev2 = 0
    prev1 = b[0]
    
    for i in range(1, n):
        cur = max(prev1, prev2 + b[i])
        prev2 = prev1
        prev1 = cur
    
    print(prev1)

if __name__ == "__main__":
    solve()
```Việc triển khai nén mảng DP thành hai biến.`prev1`đại diện cho dp[i-1] và`prev2`đại diện cho dp[i-2]. Ở mỗi bước, chúng tôi tính toán xem việc lấy giá trị hiện tại cộng với`prev2`nhịp bỏ qua nó và giữ`prev1`. 

Trường hợp một phần tử được xử lý rõ ràng để tránh tham chiếu trạng thái dp chưa được khởi tạo. Không có điều này,`prev2 + b[i]`sẽ không hợp lệ nếu i = 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 10 9 10 7
```| tôi | b[i] | bỏ qua (dp[i-1]) | lấy (dp[i-2] + b[i]) | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | - | 5 | 5 | 
| 1 | 10 | 5 | 10 | 10 | 
| 2 | 9 | 10 | 14 | 14 | 
| 3 | 10 | 14 | 20 | 20 | 
| 4 | 7 | 20 | 21 | 21 | 

Bảng này cho thấy quyết định ở mỗi chỉ số cân bằng lợi ích trước mắt với việc bỏ qua vì lợi ích trong tương lai như thế nào. Câu trả lời cuối cùng là 21, đạt được bằng cách chọn chỉ số 1, 3 và 4? Trên thực tế chỉ số 1 và 3 cho 20, nhưng tối ưu là chỉ số 0, 1, 3? Điều đó vi phạm tính liền kề, vì vậy lựa chọn đúng là 0, 1 không hợp lệ. Cấu trúc tối ưu thực tế là 5 + 9 + 7 không hợp lệ do các ràng buộc kề cận. DP giải quyết chính xác vấn đề này bằng cách thực thi tính tối ưu của tiền tố thay vì xây dựng tập hợp con rõ ràng. 

### Ví dụ 2 

đầu vào:```
3
1 100 1
```| tôi | b[i] | bỏ qua | lấy | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | 1 | 1 | 
| 1 | 100 | 1 | 100 | 100 | 
| 2 | 1 | 100 | 2 | 100 | 

Chiến lược tối ưu chỉ chọn phần tử ở giữa. DP tránh được sự cám dỗ để kết hợp cả hai đầu một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được xử lý một lần với chuyển tiếp O(1) | 
| Không gian | O(1) | Chỉ có hai biến DP cuộn được lưu trữ | 

Quét tuyến tính dễ dàng phù hợp với các giới hạn cho N lên tới 100.000 và việc sử dụng bộ nhớ liên tục đảm bảo không có chi phí từ các mảng lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp)).strip()

def solve_output(inp: str):
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    b = list(map(int, sys.stdin.readline().split()))
    
    if n == 1:
        return b[0]
    
    prev2 = 0
    prev1 = b[0]
    
    for i in range(1, n):
        cur = max(prev1, prev2 + b[i])
        prev2 = prev1
        prev1 = cur
    
    return prev1

# provided sample
assert solve_output("5\n5 10 9 10 7\n") == 21

# minimum size
assert solve_output("1\n7\n") == 7

# all equal
assert solve_output("4\n5 5 5 5\n") == 10

# alternating high values
assert solve_output("5\n10 1 10 1 10\n") == 30

# increasing sequence
assert solve_output("5\n1 2 3 4 5\n") == 9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 7 | xử lý trường hợp cơ bản | 
| tất cả đều bình đẳng | 10 | lựa chọn tối ưu xen kẽ | 
| mức cao xen kẽ | 30 | cấu trúc bỏ qua đúng | 
| ngày càng tăng | 9 | hành vi DP tối ưu không tham lam | 

## Vỏ cạnh 

Đối với mảng một phần tử như:```
1
7
```bộ thuật toán`prev1 = b[0]`và không bao giờ đi vào vòng lặp. Đầu ra trực tiếp là 7, khớp với câu trả lời đúng vì tập hợp con hợp lệ duy nhất chứa phần tử đó. 

Đối với trường hợp có giá trị cao xen kẽ như:```
5
10 1 10 1 10
```DP phát triển như sau: bắt đầu từ 10, rồi 10, rồi 20, rồi 20, rồi 30. Ở mỗi bước, việc bỏ qua hoặc lấy được đánh giá trên toàn cầu, do đó thuật toán chọn chính xác cả ba số 10 trong khi tránh các lượt chọn liền kề.
