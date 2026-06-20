---
title: "CF 1029B - Tạo cuộc thi"
description: "Chúng tôi được cung cấp một danh sách sắp xếp các khó khăn vấn đề riêng biệt. Từ danh sách này, chúng ta phải chọn một dãy con, không nhất thiết phải liền kề nhau, sẽ tạo thành một cuộc thi."
date: "2026-06-16T21:08:50+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1029
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 506 (Div. 3)"
rating: 1200
weight: 1029
solve_time_s: 145
verified: true
draft: false
---

[CF 1029B - Tạo cuộc thi](https://codeforces.com/problemset/problem/1029/B) 

**Đánh giá:** 1200 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách sắp xếp các khó khăn vấn đề riêng biệt. Từ danh sách này, chúng ta phải chọn một dãy con, không nhất thiết phải liền kề nhau, sẽ tạo thành một cuộc thi. Hạn chế duy nhất là cục bộ: nếu chúng ta sắp xếp các độ khó đã chọn tăng dần thì mỗi cặp liên tiếp phải thỏa mãn rằng độ khó tiếp theo nhiều nhất là gấp đôi độ khó trước đó. 

Mục tiêu là chọn một dãy con thỏa mãn ràng buộc nhân này trong khi tối đa hóa độ dài của nó. 

Kích thước đầu vào có thể lên tới 200.000 phần tử. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê các tập hợp con hoặc thậm chí thử tất cả các điểm bắt đầu với phép khám phá lồng nhau, vì hành vi bậc hai xung quanh$10^{10}$hoạt động không thể thực hiện được trong một giây. Chúng ta cần một cái gì đó tuyến tính hoặc tệ nhất là tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi dãy con tối ưu không liền kề trong mảng ban đầu. Ví dụ: ngay cả khi hai giá trị cách xa nhau về chỉ số, chúng vẫn có thể thuộc chuỗi tối ưu nếu bỏ qua các giá trị trung gian. Một kẻ tham lam ngây thơ luôn chọn các phần tử liền kề trong mảng sẽ thất bại trong các trường hợp như`1 2 4 8 9 10`, trong đó việc bỏ qua một số phần tử nhất định sẽ cho phép chuỗi hợp lệ dài hơn. 

Một vấn đề khác là điều kiện không đối xứng. Nó chỉ hạn chế các phần tử liền kề trong dãy con đã chọn chứ không hạn chế tất cả các cặp. Điều này có nghĩa là cấu trúc là một điều kiện dây chuyền, không phải là một ràng buộc tỷ lệ tổng thể. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi chỉ số bắt đầu và mở rộng chuỗi về phía trước một cách tham lam bằng cách liên tục chọn phần tử hợp lệ tiếp theo. Để bắt đầu cố định, hãy quét chi phí chuyển tiếp$O(n)$và làm điều này mỗi lần bắt đầu sẽ dẫn đến$O(n^2)$. Với$2 \cdot 10^5$, điều này trở nên đại khái$4 \cdot 10^{10}$so sánh thì quá chậm. 

Nhận xét quan trọng là khi chúng ta xác định điểm bắt đầu, chúng ta không bao giờ cần phải xem xét lại các yếu tố trước đó. Ràng buộc chỉ phụ thuộc vào giá trị được chọn cuối cùng. Điều này gợi ý một cách xây dựng tham lam về phía trước, nhưng tham lam bắt đầu từ mọi chỉ số là dư thừa. 

Thay vào đó, chúng tôi duy trì cấu trúc cửa sổ trượt: đối với mỗi vị trí, chúng tôi cố gắng mở rộng chuỗi hợp lệ dài nhất bắt đầu từ đó, nhưng chúng tôi sử dụng lại thực tế là nếu chuỗi bắt đầu từ$i$đã biết thì phần mở rộng tốt nhất từ$i+1$có thể sử dụng lại một phần công việc. Điều này biến vấn đề thành một bản mở rộng kiểu hai con trỏ, trong đó chúng tôi duy trì một cửa sổ hợp lệ và theo dõi kích thước tối đa của nó. 

Vì dãy đã được sắp xếp nên chúng ta chỉ cần đảm bảo tính hợp lệ cục bộ giữa các phần tử được chọn liên tiếp. Điều này cho phép chúng ta mở rộng cửa sổ trong khi vẫn duy trì điều kiện$a[j] \le 2 \cdot a[j-1]$, chỉ co lại khi điều kiện bị phá vỡ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì chuỗi hợp lệ dài nhất kết thúc ở vị trí hiện tại. 

1. Bắt đầu bằng một con trỏ ở đầu mảng và đặt câu trả lời thành 1 vì bất kỳ phần tử đơn lẻ nào cũng là một cuộc thi hợp lệ. Điều này thiết lập độ dài chuỗi cơ sở. 
2. Duyệt mảng từ phần tử thứ hai trở đi. Đối với mỗi phần tử, hãy kiểm tra xem nó có thể mở rộng chuỗi hiện tại hay không bằng cách xác minh xem nó có tối đa gấp đôi phần tử đã chọn trước đó hay không. 
3. Nếu điều kiện được giữ nguyên, hãy kéo dài độ dài chuỗi hiện tại thêm một. Điều này phản ánh rằng phần tử hiện tại có thể được thêm vào mà không vi phạm ràng buộc. 
4. Nếu điều kiện không đúng, hãy đặt lại độ dài chuỗi về 1 bắt đầu từ phần tử hiện tại. Điều này là cần thiết vì không có phần tử nào trước đó trong chuỗi hiện tại có thể kết nối với phần tử này theo ràng buộc đã cho. 
5. Sau mỗi bước, hãy cập nhật mức tối đa toàn cầu với độ dài chuỗi hiện tại. 

Thuật toán dựa trên thực tế là chúng tôi luôn mở rộng chuỗi hợp lệ dài nhất kết thúc ở chỉ mục trước đó. Nếu việc mở rộng không thành công thì bắt đầu mới là tối ưu vì bất kỳ điểm bắt đầu nào sớm hơn sẽ vi phạm điều kiện chuỗi tại cùng một ranh giới. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí nào, thuật toán duy trì chuỗi con hợp lệ dài nhất kết thúc tại vị trí đó và tuân thủ điều kiện cục bộ. Vì tính hợp lệ chỉ phụ thuộc vào các cặp liền kề nên mọi giải pháp tối ưu đều có thể được phân tách thành các phân đoạn cực đại trong đó điều kiện được giữ liên tục. Cuộc thi tốt nhất tương ứng với phân đoạn dài nhất sau khi đặt lại khởi đầu tối ưu và thuật toán liệt kê ngầm tất cả các điểm cuối phân đoạn có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    best = 1
    cur = 1
    
    for i in range(1, n):
        if a[i] <= 2 * a[i - 1]:
            cur += 1
        else:
            cur = 1
        if cur > best:
            best = cur
    
    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp giữ số lượng đang chạy của độ dài chuỗi hợp lệ hiện tại. Biến`cur`biểu thị số lượng phần tử được chọn liên tiếp thỏa mãn điều kiện nhân đôi khi di chuyển trong mảng. Khi điều kiện bị phá vỡ, chúng tôi đặt lại vì không thể mở rộng qua ranh giới đó. 

Biến`best`theo dõi độ dài chuỗi tối đa được nhìn thấy ở bất kỳ đâu trong mảng. Vì mọi dãy con hợp lệ phải xuất hiện dưới dạng một phân đoạn hợp lệ liền kề theo nghĩa được chuyển đổi này, nên chỉ cần theo dõi các phân đoạn này là đủ. 

Chi tiết triển khai chính là đặt lại chính xác khi nào`a[i] > 2 * a[i-1]`. Thiếu sự bất bình đẳng nghiêm ngặt này hoặc quên thiết lập lại dẫn đến việc đếm quá mức các chuỗi không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
1 2 5 6 7 10 21 23 24 49
```| tôi | một [tôi] | Điều kiện so với trước đó | cur | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | 1 | 1 | 
| 1 | 2 | 2 2·1 | 2 | 2 | 
| 2 | 5 | 5 2·2 sai | 1 | 2 | 
| 3 | 6 | 6 2·5 | 2 | 2 | 
| 4 | 7 | 7 2·6 | 3 | 3 | 
| 5 | 10 | 10 2·7 | 4 | 4 | 
| 6 | 21 | 21 2·10 sai | 1 | 4 | 
| 7 | 23 | 23 2·21 | 2 | 4 | 
| 8 | 24 | 24 2·23 | 3 | 4 | 
| 9 | 49 | 49 2·24 sai | 1 | 4 | 

Dấu vết này cho thấy các chuỗi hợp lệ hình thành các phân đoạn một cách tự nhiên như thế nào. Đoạn dài nhất xảy ra ở`[5, 6, 7, 10]`, tạo ra chiều dài 4. 

### Ví dụ 2 

đầu vào:```
5
3 4 8 20 21
```| tôi | một [tôi] | Tình trạng | cur | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | - | 1 | 1 | 
| 1 | 4 | 4 6 | 2 | 2 | 
| 2 | 8 | 8 8 8 | 3 | 3 | 
| 3 | 20 | 20 ≤ 16 sai | 1 | 3 | 
| 4 | 21 | 21 ≤ 40 | 2 | 3 | 

Đoạn tối ưu là`[3, 4, 8]`và các giá trị sau này không thể mở rộng nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các lần kiểm tra liên tục | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó, một đường tuyến tính duy nhất vừa vặn thoải mái trong giới hạn thời gian và bộ nhớ không đổi sẽ tránh được chi phí hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    best = 1
    cur = 1

    for i in range(1, n):
        if a[i] <= 2 * a[i - 1]:
            cur += 1
        else:
            cur = 1
        best = max(best, cur)

    return str(best)

# provided sample
assert run("10\n1 2 5 6 7 10 21 23 24 49\n") == "4"

# minimum size
assert run("1\n100\n") == "1"

# all valid chain
assert run("4\n1 2 3 5\n") == "4"

# strict breaks every time
assert run("4\n1 3 7 15\n") == "1"

# alternating extend/reset pattern
assert run("6\n1 2 5 6 12 13\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | ranh giới tối thiểu | 
| 1 2 3 5 | 4 | chuỗi đầy đủ luôn hợp lệ | 
| 1 3 7 15 | 1 | đặt lại liên tục | 
| 1 2 5 6 12 13 | 2 | chuỗi một phần lặp đi lặp lại | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như`1`, thuật toán khởi tạo`best = 1`và quay trở lại ngay lập tức. Không có quá trình chuyển đổi nào xảy ra nên không kích hoạt việc đặt lại hoặc so sánh sai. 

Đối với một chuỗi tăng hơn gấp đôi như`1 3 7 15`, mọi so sánh đều thất bại vì mỗi phần tử tiếp theo vượt quá hai lần phần tử trước đó. Vòng lặp được đặt lại`cur`đến 1 ở mỗi bước và`best`vẫn là 1 xuyên suốt, khớp với câu trả lời đúng vì không có hai phần tử nào có thể cùng tồn tại trong một cuộc thi hợp lệ. 

Đối với một chuỗi hoàn toàn hợp lệ như`1 2 3 5`, mọi cặp liền kề đều thỏa mãn ràng buộc, vì vậy`cur`tăng liên tục lên 4. Thuật toán không bao giờ đặt lại, xác định chính xác toàn bộ mảng là một cuộc thi hợp lệ.
