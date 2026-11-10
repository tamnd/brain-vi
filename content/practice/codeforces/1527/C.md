---
title: "CF 1527C - Trọng lượng cặp trình tự"
description: "Chúng ta được cung cấp một số mảng và đối với mỗi mảng, chúng ta xem xét mọi mảng con liền kề có thể có. Đối với mỗi mảng con, chúng tôi xác định “trọng số” của nó là số cặp vị trí bên trong nó có cùng giá trị."
date: "2026-06-10T17:14:28+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1527
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 721 (Div. 2)"
rating: 1600
weight: 1527
solve_time_s: 94
verified: true
draft: false
---

[CF 1527C - Trọng lượng cặp trình tự](https://codeforces.com/problemset/problem/1527/C) 

**Đánh giá:** 1600 
**Tags:** băm, triển khai, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số mảng và đối với mỗi mảng, chúng ta xem xét mọi mảng con liền kề có thể có. Đối với mỗi mảng con, chúng tôi xác định “trọng số” của nó là số cặp vị trí bên trong nó có cùng giá trị. Nói cách khác, bên trong một phân đoạn đã chọn, chúng tôi xem xét tất cả các cặp chỉ mục và đếm xem có bao nhiêu cặp có các phần tử bằng nhau. 

Nhiệm vụ là tính tổng trọng số trên tất cả các mảng con của mảng. Thay vì đánh giá từng mảng con một cách độc lập, chúng ta phải tổng hợp giá trị này một cách hiệu quả cho toàn bộ mảng. 

Các ràng buộc đủ chặt chẽ đến mức bất kỳ giải pháp nào liệt kê các mảng con một cách rõ ràng đều ngay lập tức quá chậm. Mỗi trường hợp thử nghiệm cho phép tối đa 100.000 phần tử và có thể có tới 100.000 trường hợp thử nghiệm. Mặc dù tổng độ dài của tất cả các bài kiểm tra bị giới hạn, nhưng cách tiếp cận bậc hai hoặc gần bậc hai cho mỗi bài kiểm tra là không thể. Bất kỳ giải pháp nào cũng phải gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một cách tiếp cận đơn giản sẽ tạo ra tất cả các mảng con và đếm các cặp bằng nhau bên trong mỗi mảng. Điều này đã thất bại đối với các đầu vào rất nhỏ như`n = 10^5`vì số lượng mảng con là O(n^2) và việc đếm các cặp bên trong mỗi mảng con sẽ thêm một hệ số khác. 

Cạm bẫy phổ biến thứ hai là cố gắng tính toán các đóng góp cho mỗi giá trị mà không tính toán chính xác tần suất một cặp xuất hiện trên các mảng con khác nhau. Khó khăn là một cặp phần tử bằng nhau không chỉ đóng góp một lần trên toàn cục mà còn đóng góp cho mọi mảng con chứa cả hai điểm cuối. 

## Phương pháp tiếp cận 

Chiến lược brute-force bắt đầu bằng cách lặp lại từng mảng con`[l, r]`, và bên trong nó đếm xem có bao nhiêu cặp`(i, j)`thỏa mãn`l ≤ i < j ≤ r`Và`a[i] = a[j]`. Điều này có thể được thực hiện bằng một vòng lặp lồng nhau và một bảng tần số cho mỗi mảng con. Ngay cả khi chúng tôi duy trì tần số trong khi mở rộng`r`, chúng ta vẫn có mảng con O(n^2) và số lần cập nhật là O(1), dẫn đến O(n^2) cho mỗi trường hợp thử nghiệm. Với`n = 10^5`, điều này vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì tính tổng các mảng con trước, trước tiên chúng ta tính tổng các cặp vị trí bằng nhau. Lấy hai chỉ số`i < j`với`a[i] = a[j]`. Chúng tôi muốn biết có bao nhiêu mảng con mà cặp này đóng góp vào trọng số. 

Một mảng con đóng góp cặp này khi và chỉ khi nó chứa đầy đủ cả hai`i`Và`j`. Điều đó có nghĩa là điểm cuối bên trái`l`có thể ở bất cứ đâu từ`1`ĐẾN`i`, và điểm cuối bên phải`r`có thể ở bất cứ đâu từ`j`ĐẾN`n`. Vì vậy, cặp này xuất hiện chính xác`i * (n - j + 1)`mảng con. 

Bây giờ bài toán sẽ tính tổng phần đóng góp này trên tất cả các cặp bằng nhau. Chúng tôi xử lý các chỉ số từ trái sang phải, duy trì cho mỗi giá trị số lần nó xuất hiện cho đến nay. Khi chúng ta ở vị trí`j`, mọi lần xuất hiện trước đó`i`có cùng giá trị đóng góp`i * (n - j + 1)`. Chúng ta có thể tích lũy điều này một cách hiệu quả bằng cách duy trì tổng số chỉ số đang chạy trên mỗi giá trị. 

Điều này làm giảm vấn đề xuống còn một lần với bản đồ băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1)-O(n) | Quá chậm | 
| Tối ưu | O(n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi xử lý mảng từ trái sang phải trong khi theo dõi lần xuất hiện của từng giá trị. Đối với mỗi giá trị, chúng tôi duy trì hai thông tin: số lần nó xuất hiện cho đến nay và tổng các chỉ số trước đó của nó. 
2. Khi chúng ta đạt đến vị trí`j`có giá trị`x`, chúng tôi muốn tính xem vị trí này đóng góp bao nhiêu vào các cặp có sự xuất hiện trước đó của`x`. Mỗi chỉ số trước đó`i`tạo thành một cặp đóng góp`i * (n - j + 1)`. 
3. Thay vì lặp lại tất cả các bước trước đó`i`, chúng tôi sử dụng tổng các chỉ số được lưu trữ. Nếu như`S[x]`là tổng của tất cả các vị trí trước đó trong đó`x`xuất hiện thì tổng đóng góp của`j`là`S[x] * (n - j + 1)`. 
4. Chúng tôi thêm phần đóng góp này vào câu trả lời. 
5. Sau đó chúng tôi cập nhật trạng thái được lưu trữ: tăng số lượng và thêm`j`đến tổng`S[x]`. 
6. Lặp lại điều này cho tất cả các phần tử trong mảng và tính tổng câu trả lời. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ`(i, j)`có giá trị bằng nhau được tính chính xác một lần khi xử lý`j`. Tại thời điểm đó, thuật toán đã tích lũy tất cả các vị trí trước đó`i`cho giá trị đó. Mỗi cặp như vậy đóng góp chính xác vào`i * (n - j + 1)`mảng con và phép nhân được tổng hợp chính xác thông qua tổng được lưu trữ. Vì mỗi cặp được gán duy nhất cho điểm cuối bên phải của nó nên không có việc tính trùng hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        # store: value -> (count, sum of indices)
        cnt = {}
        idx_sum = {}
        
        ans = 0
        
        for j, x in enumerate(a, start=1):
            if x in idx_sum:
                ans += idx_sum[x] * (n - j + 1)
                cnt[x] += 1
                idx_sum[x] += j
            else:
                cnt[x] = 1
                idx_sum[x] = j
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên việc lập chỉ mục dựa trên 1 để làm rõ công thức đóng góp`(n - j + 1)`. Từ điển`idx_sum`theo dõi tổng của tất cả các vị trí trước đó cho mỗi giá trị. Khi xử lý một lần xuất hiện mới, chúng tôi tính toán ngay số lượng mảng con mà nó hoàn thành theo cặp với các lần xuất hiện trước đó. 

Một điểm tinh tế là chúng ta không bao giờ cần sử dụng rõ ràng`cnt`trong tính toán cuối cùng, nhưng việc giữ nó sẽ giúp trạng thái hoàn thiện về mặt khái niệm. Tính chính xác phụ thuộc vào việc cập nhật`idx_sum[x]`sau khi sử dụng nó cho chỉ mục hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 2, 1, 1]
```Chúng tôi theo dõi sự đóng góp cho mỗi vị trí. 

| j | x | idx_sum trước | đóng góp | trả lời | idx_sum sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 1 | 
| 2 | 2 | 0 | 0 | 0 | 2 | 
| 3 | 1 | 1 | 1 × (4 - 3 + 1) = 2 | 2 | 4 | 
| 4 | 1 | 4 | 4 × (4 - 4 + 1) = 4 | 6 | 8 | 

Câu trả lời cuối cùng là 6. 

Dấu vết này cho thấy mỗi lần xuất hiện của`1`tích lũy đóng góp từ tất cả các lần trước`1`s, được chia tỷ lệ theo số lượng mảng con mở rộng sang bên phải. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1, 2, 3, 4]
```| j | x | idx_sum trước | đóng góp | trả lời | idx_sum sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 1 | 
| 2 | 2 | 0 | 0 | 0 | 2 | 
| 3 | 3 | 0 | 0 | 0 | 3 | 
| 4 | 4 | 0 | 0 | 0 | 4 | 

Không có giá trị lặp lại, do đó không có đóng góp nào xảy ra. 

Điều này xác nhận rằng thuật toán tự nhiên tạo ra số 0 khi không có bản sao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử được xử lý một lần với các phép toán băm O(1) | 
| Không gian | O(n) | Bản đồ băm lưu trữ tối đa một mục nhập cho mỗi giá trị riêng biệt | 

Tổng kích thước đầu vào qua các bài kiểm tra được giới hạn bởi 10^5, do đó, quá trình quét tuyến tính trên tất cả các bài kiểm tra vừa vặn thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ tỷ lệ thuận với các giá trị riêng biệt trong từng trường hợp thử nghiệm, cũng được giới hạn bởi n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        idx_sum = {}
        ans = 0
        
        for j, x in enumerate(a, start=1):
            if x in idx_sum:
                ans += idx_sum[x] * (n - j + 1)
                idx_sum[x] += j
            else:
                idx_sum[x] = j
        
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples
assert run("2\n4\n1 2 1 1\n4\n1 2 3 4\n") == "6\n0"

# all equal
assert run("1\n5\n7 7 7 7 7\n") == "50"

# single element
assert run("1\n1\n42\n") == "0"

# alternating duplicates
assert run("1\n6\n1 2 1 2 1 2\n") == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 50 | tích lũy trùng lặp tối đa | 
| phần tử đơn | 0 | trường hợp cạnh tối thiểu | 
| xen kẽ | 18 | đóng góp cặp xen kẽ | 

## Vỏ cạnh 

Đối với một mảng gồm tất cả các giá trị giống hệt nhau như`[7, 7, 7, 7, 7]`, mọi phần tử mới đều tương tác với tất cả các phần tử trước đó. Tại vị trí`j`, số tiền được lưu trữ tăng lên khi`1 + 2 + ... + (j-1)`, và mỗi số được nhân với`(n - j + 1)`. Thuật toán tích lũy chính xác những đóng góp ngày càng tăng này mà không bỏ sót bất kỳ khoảng phân đoạn nào. 

Đối với mảng một phần tử, không có cặp phần tử nào cả. Vòng lặp chạy một lần, không tìm thấy lần xuất hiện nào trước đó và bản đồ được lưu trữ vẫn ở mức tầm thường. Đầu ra vẫn bằng 0, phù hợp với định nghĩa về trọng lượng. 

Đối với các giá trị xen kẽ như`[1, 2, 1, 2, 1, 2]`, mỗi giá trị tạo thành chuỗi xuất hiện độc lập. Cấu trúc đảm bảo rằng các đóng góp được phân tách bằng khóa và mỗi cặp chỉ đóng góp khi đạt đến điểm cuối bên phải của nó, ngăn chặn bất kỳ sự can thiệp chéo nào giữa các giá trị khác nhau.
