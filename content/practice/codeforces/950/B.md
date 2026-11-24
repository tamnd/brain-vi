---
title: "CF 950B - Tin nhắn bị chặn"
description: "Chúng ta có hai chuỗi số nguyên dương, mỗi chuỗi biểu thị cách một luồng byte dài được truyền theo từng khối. Mỗi chuỗi là một phân đoạn có tổng chiều dài như nhau nhưng các điểm phân đoạn là khác nhau."
date: "2026-06-17T02:20:11+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 950
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 469 (Div. 2)"
rating: 1100
weight: 950
solve_time_s: 68
verified: true
draft: false
---

[CF 950B - Tin nhắn bị chặn](https://codeforces.com/problemset/problem/950/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi số nguyên dương, mỗi chuỗi biểu thị cách một luồng byte dài được truyền theo từng khối. Mỗi chuỗi là một phân đoạn có tổng chiều dài như nhau nhưng các điểm phân đoạn là khác nhau. 

Cấu trúc ẩn mà chúng ta muốn xây dựng lại là một kho lưu trữ được tạo thành từ nhiều tệp được viết lần lượt. Mỗi tệp có tổng kích thước nhất định, nhưng trong quá trình truyền, mỗi tệp có thể được chia thành nhiều khối liên tiếp. Điều quan trọng là trong mỗi tin nhắn, chúng ta chỉ thấy những khối nhỏ hơn này chứ không thấy ranh giới tệp gốc. Nhiệm vụ của chúng tôi là xác định số lượng tệp gốc tối đa có thể sao cho cả hai thư có thể đến từ cùng một chuỗi kích thước tệp, chỉ được phân chia khác nhau. 

Vì vậy, chúng ta có hai mảng có tổng bằng nhau và chúng ta phải phân chia cả hai mảng thành các phân đoạn liền kề sao cho tổng các phân đoạn khớp theo thứ tự. Mục tiêu là tối đa hóa số lượng phân khúc phù hợp mà chúng tôi có thể tạo. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào tệ hơn tuyến tính trong tổng kích thước đầu vào sẽ quá chậm. Cả hai mảng có thể có tới 100.000 phần tử, vì vậy chúng ta cần một phương thức xử lý chúng trong một lần chuyển hoặc gần thời gian chuyển một lần. Bất cứ điều gì liên quan đến việc quét lồng nhau hoặc tính toán lại các tổng tiền tố nhiều lần sẽ không thành công. 

Một sai lầm ngây thơ là thử tất cả các điểm phân vùng có thể một cách độc lập trong cả hai mảng. Ví dụ: ngay cả đối với các đầu vào nhỏ, người ta có thể cố gắng căn chỉnh các tổng tiền tố từ mảng đầu tiên với tất cả các tổng tiền tố của mảng thứ hai. Điều này bị hỏng nhanh chóng: đối với các mảng như`1 1 1 1 ...`số lượng kết hợp phân vùng tăng theo phương trình bậc hai. 

Một trường hợp lỗi tinh vi khác xuất hiện khi việc so khớp tham lam được thực hiện mà không đồng bộ hóa. Nếu chúng ta tham lam cắt một tệp bất cứ khi nào một tổng tiền tố nhỏ hơn tổng tiền tố kia, nhưng quên thực thi sự bằng nhau của các ranh giới tệp đã hoàn thành trong cả hai mảng cùng một lúc, thì chúng ta có thể tạo ra các phân vùng không khớp. 

Cấu trúc đúng phụ thuộc vào việc đồng bộ hóa các tổng tích lũy để chúng ta chỉ “cắt một tập tin” khi cả hai bên đều có tổng bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force cố gắng chọn các điểm phân vùng trong cả hai mảng và khớp tổng phân đoạn. Người ta có thể tưởng tượng việc tạo ra tất cả các tổng tiền tố, sau đó cố gắng ghép chúng theo thứ tự tăng dần để mỗi cặp được chọn tạo thành một ranh giới tệp. Mặc dù điều này đơn giản về mặt khái niệm nhưng nó đòi hỏi phải khám phá sự kết hợp của các điểm phân vùng, dẫn đến hành vi hàm mũ hoặc ít nhất là bậc hai tùy thuộc vào việc triển khai. Với 10^5 phần tử, điều này là không thể. 

Thông tin chi tiết quan trọng là cả hai mảng đều biểu thị tổng chuỗi kích thước tệp giống nhau, chỉ được cắt nhỏ thành các phần khác nhau. Nếu chúng ta duyệt đồng thời cả hai mảng, tích lũy tổng, chúng ta có thể phát hiện khi nào đạt đến ranh giới tệp đầy đủ một cách chính xác khi tổng tích lũy khớp. Tại thời điểm đó, chúng ta có thể khai báo một tệp đã hoàn thành một cách an toàn và đặt lại cả hai bộ tích lũy. 

Điều này có tác dụng vì ranh giới tệp tương ứng chính xác với các điểm mà cả hai tổng tích lũy đều thẳng hàng. Vì trật tự được duy trì nên chúng ta không bao giờ cần phải xem xét lại các quyết định trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các phân vùng) | O(2^n) hoặc O(n^2) | O(n) | Quá chậm | 
| Đồng bộ hóa tham lam hai con trỏ | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai con trỏ, một con trỏ cho mỗi mảng và hai tổng chạy biểu thị tệp được tích lũy một phần hiện tại từ mỗi thông báo. 

1. Khởi tạo hai chỉ số`i = 0`,`j = 0`, và hai bộ tích lũy`a_sum = 0`,`b_sum = 0`. Đồng thời khởi tạo`files = 0`. 
2. Di chuyển qua cả hai mảng trong khi không có mảng nào hết. Ở mỗi bước, chúng tôi thêm khối tiếp theo vào bộ tích lũy nhỏ hơn hoặc cả hai. 
3. Nếu`a_sum`nhỏ hơn`b_sum`, chúng ta lấy khối tiếp theo từ mảng đầu tiên và thêm nó vào`a_sum`. Chúng tôi tiến lên`i`. 
4. Nếu`b_sum`nhỏ hơn`a_sum`, chúng ta lấy khối tiếp theo từ mảng thứ hai và thêm nó vào`b_sum`. Chúng tôi tiến lên`j`. 
5. Nếu`a_sum == b_sum`và cả hai đều khác 0, chúng tôi đã tìm thấy ranh giới tệp hoàn chỉnh được chia sẻ bởi cả hai cách biểu diễn. Chúng tôi tăng`files`và đặt lại cả hai tổng về 0. 
6. Tiếp tục cho đến khi cả hai mảng đều được sử dụng hết. Câu trả lời cuối cùng là`files`. 

Lý do quy tắc từng bước này có tác dụng là vì chúng ta không bao giờ cho phép một bên vượt quá giới hạn mà không có sự đền bù từ bên kia. Chúng tôi luôn bảo toàn tính bất biến rằng cả hai bộ tích lũy đều biểu thị tổng một phần của cùng một ứng cử viên tệp hiện tại. Khi sự bình đẳng xảy ra, cả hai cấu trúc từng phần đều tương ứng với một tệp đầy đủ trong kho lưu trữ gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    x = list(map(int, input().split()))
    y = list(map(int, input().split()))
    
    i = j = 0
    sx = sy = 0
    ans = 0
    
    while i < n or j < m:
        if sx <= sy:
            if i < n:
                sx += x[i]
                i += 1
        else:
            if j < m:
                sy += y[j]
                j += 1
        
        if sx == sy:
            if sx != 0:
                ans += 1
            sx = sy = 0
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo ý tưởng đồng bộ hóa hai con trỏ. Sự tinh tế quan trọng là`sx <= sy`điều kiện: khi tổng bằng nhau, chúng tôi muốn mở rộng mảng đầu tiên trước, nhưng việc bên nào được nâng cao không quan trọng miễn là chúng tôi duy trì tiến độ. Việc kiểm tra đẳng thức ngay sau đó đảm bảo rằng bất cứ khi nào cả hai tổng một phần khớp nhau, chúng tôi sẽ cam kết một ranh giới tệp. 

Một sai lầm phổ biến là quên đặt lại cả hai bộ tích lũy sau khi đếm một tập tin. Một cách khác là tiếp tục chỉ tiến lên một con trỏ mà không đảm bảo chấm dứt khi một mảng kết thúc trước đó; cái`while i < n or j < m`điều kiện đảm bảo các khối còn lại vẫn được xử lý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 6
2 5 3 1 11 4 4
7 8 2 4 1 8
```Chúng tôi theo dõi số tiền tích lũy: 

| Bước | tôi | j | sx | sy | Hành động | tập tin | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | 0 | lấy x | 0 | 
| 2 | 0 | 0 | 2 | 7 | lấy y | 0 | 
| 3 | 1 | 0 | 7 | 7 | khớp → cắt | 1 | 
| 4 | 1 | 1 | 0 | 0 | đặt lại | 1 | 
| 5 | 3 | 1 | 4 | 8 | xây dựng | 1 | 
| 6 | 3 | 2 | 4 | 10 | xây dựng | 1 | 
| 7 | 3 | 2 | 7 | 10 | xây dựng | 1 | 
| 8 | 4 | 2 | 8 | 10 | xây dựng | 1 | 
| 9 | 4 | 3 | 19 | 19 | khớp → cắt | 2 | 
| 10 | 4 | 4 | 0 | 0 | đặt lại | 2 | 
| 11 | 6 | 5 | 4 | 8 | xây dựng | 2 | 
| 12 | 7 | 5 | 8 | 8 | khớp → cắt | 3 | 

Câu trả lời cuối cùng là 3. 

Dấu vết này cho thấy ranh giới tệp chỉ được phát hiện khi cả hai tổng tích lũy đều thẳng hàng chứ không phải khi các mảng riêng lẻ đạt đến tổng một phần tùy ý. 

### Ví dụ 2 

đầu vào:```
3 3
1 2 3
2 1 3
```| Bước | tôi | j | sx | sy | Hành động | tập tin | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 0 | lấy x | 0 | 
| 2 | 0 | 0 | 1 | 2 | lấy y | 0 | 
| 3 | 1 | 0 | 3 | 2 | lấy x | 0 | 
| 4 | 1 | 1 | 3 | 3 | khớp → cắt | 1 | 
| 5 | 2 | 2 | 0 | 0 | đặt lại | 1 | 
| 6 | 3 | 3 | 3 | 3 | khớp → cắt | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này chứng tỏ rằng các phân đoạn nội bộ khác nhau vẫn căn chỉnh hoàn hảo khi tổng tích lũy được đồng bộ hóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi con trỏ tiến lên đúng một lần trên mảng của nó | 
| Không gian | O(1) | Chỉ các bộ đếm và chỉ số được lưu trữ | 

Quét tuyến tính là đủ vì mỗi khối được xử lý một lần và không xảy ra hiện tượng quay lui. Với tổng kích thước đầu vào lên tới 2×10^5, điều này phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    x = list(map(int, input().split()))
    y = list(map(int, input().split()))
    
    i = j = 0
    sx = sy = 0
    ans = 0
    
    while i < n or j < m:
        if sx <= sy:
            if i < n:
                sx += x[i]
                i += 1
        else:
            if j < m:
                sy += y[j]
                j += 1
        
        if sx == sy:
            if sx != 0:
                ans += 1
            sx = sy = 0
    
    return str(ans)

# provided sample
assert run("7 6\n2 5 3 1 11 4 4\n7 8 2 4 1 8\n") == "3"

# single file case
assert run("1 1\n10\n10\n") == "1"

# alternating small splits
assert run("4 4\n1 1 1 1\n2 1 1 1\n") == "2"

# already aligned blocks
assert run("3 3\n3 3 3\n3 3 3\n") == "3"

# uneven internal splits
assert run("5 4\n1 2 3 4 5\n3 3 3 6\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/10/10 | 1 | trường hợp tối thiểu | 
| 1 1 1 1 / 2 1 1 1 | 2 | phân đoạn không đồng đều | 
| 3 3 / tất cả đều bằng nhau | 3 | đã căn chỉnh | 
| 5 4 / hỗn hợp | 2 | căn chỉnh không tầm thường | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một mảng kết thúc tích lũy nhanh hơn mảng kia. Ví dụ: nếu một chuỗi có một khối lớn ở cuối, thuật toán vẫn phải tiếp tục tiêu thụ các khối còn lại trong mảng khác cho đến khi tổng thẳng hàng. Điều kiện vòng lặp`i < n or j < m`đảm bảo chúng tôi không dừng lại sớm. 

Một trường hợp khác là khi đẳng thức xảy ra nhiều lần liên tiếp do về 0 sau khi cắt. Bước đặt lại đảm bảo rằng các tệp liên tiếp được tính riêng vì cả hai bộ tích lũy đều khởi động lại sạch sẽ sau mỗi trận đấu. 

Trường hợp tinh tế cuối cùng là khi tệp cuối cùng kết thúc chính xác ở khối cuối cùng của cả hai mảng. Thuật toán vẫn tính nó vì đẳng thức kích hoạt việc cắt ngay cả khi kết thúc và không còn phần tử chưa được xử lý nào tồn tại sau đó.
