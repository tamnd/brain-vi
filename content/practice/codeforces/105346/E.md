---
title: "CF 105346E - Ăn kẹo"
description: "Chúng tôi được tặng một số loại kẹo. Mỗi loại có sẵn một số lượng miếng cố định và độ ngon cố định trên mỗi miếng."
date: "2026-06-23T15:34:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105346
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 2 (Beginner)"
rating: 0
weight: 105346
solve_time_s: 84
verified: false
draft: false
---

[CF 105346E - Ăn kẹo](https://codeforces.com/problemset/problem/105346/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một số loại kẹo. Mỗi loại có sẵn một số lượng miếng cố định và độ ngon cố định trên mỗi miếng. Charlie có thể ăn kẹo trong một số ngày giới hạn, nhưng việc ăn uống của cậu ấy bị hạn chế theo ba cách độc lập: có một thời hạn cố định mà sau đó không được ăn kẹo, cậu ấy chỉ có thể ăn tối đa một số lượng cố định mỗi ngày và trong một ngày, cậu ấy không được phép ăn nhiều hơn một viên cùng loại. 

Nhiệm vụ là lên lịch ăn những miếng kẹo nào vào những ngày nào sao cho mọi ràng buộc đều được tôn trọng đồng thời tối đa hóa tổng giá trị độ ngon của những viên kẹo đã ăn. 

Từ quan điểm cấu trúc, mỗi loại kẹo hoạt động giống như một “tài nguyên” có dung lượng, nhưng khó khăn chính là ràng buộc mỗi ngày mang tính toàn cầu trong khi ràng buộc “không trùng lặp loại mỗi ngày” kết hợp tất cả các nhiệm vụ trong một ngày. 

Các ràng buộc rất lớn, lên tới hai trăm nghìn loại, ngày và giới hạn hàng ngày. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng gán kẹo theo ngày hoặc loại theo loại theo cách lồng nhau. Bất kỳ giải pháp nào giải thích về các nhiệm vụ trong ngày riêng lẻ đều có nguy cơ lên ​​tới$O(n \cdot d)$hoặc tệ hơn, vượt xa những gì có thể trôi qua trong một giây. 

Một hàm ý tinh tế hơn đến từ sự hạn chế theo từng loại. Một loại có$k_i$kẹo không thể đơn giản được rải tùy tiện qua các ngày, bởi vì nó có thể đóng góp nhiều nhất một viên kẹo mỗi ngày. Điều này có nghĩa là loại$i$được giới hạn một cách hiệu quả ở mức$d$tổng số kẹo bất kể$k_i$, vì chỉ có$d$ngày. 

Một trường hợp thất bại phổ biến phát sinh khi người ta bỏ qua giới hạn này. Ví dụ, nếu$d = 2$,$k_1 = 100$, Và$c_1 = 10$, một cách tiếp cận ngây thơ có thể cho rằng có thể ăn hết 100 viên kẹo nếu công suất cho phép, tạo ra 1000. Mức tối đa chính xác chỉ là 20, vì có thể tiêu thụ tối đa hai viên. 

Một vấn đề tế nhị khác là xử lý từng loại kẹo một cách độc lập mà không tôn trọng giới hạn “một loại mỗi ngày”. Nếu một người tham lam đóng gói những viên kẹo có giá trị cao nhất trên toàn cầu mà không thực thi ràng buộc này, thì người đó có thể chỉ định nhiều loại kẹo cùng loại cho một ngày, điều này không hợp lệ ngay cả khi không vượt quá dung lượng. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách tưởng tượng từng ngày riêng biệt. Mỗi ngày chúng tôi cố gắng chọn ra tối đa$x$kẹo, đảm bảo không có loại nào được lặp lại trong ngày đó và không có loại kẹo nào vượt quá số lượng sẵn có. Điều này biến thành một vấn đề đóng gói nhiều bộ lặp đi lặp lại$d$ngày. Ngay cả khi chúng tôi cố gắng lựa chọn tham lam mỗi ngày, chúng tôi vẫn cần theo dõi số lượng còn lại và thực thi các ràng buộc về tính duy nhất, điều này dẫn đến việc quét lặp lại trên tất cả các loại. 

Nút thắt phức tạp xuất hiện ngay lập tức. Mỗi ngày, hãy chọn những loại kẹo tốt nhất hiện có trong số những loại kẹo có khả năng$n$các loại sẽ yêu cầu sắp xếp hoặc quét, dẫn đến ít nhất$O(d \cdot n \log n)$, quá lớn đối với$2 \cdot 10^5$. 

Quan sát cấu trúc quan trọng là các ràng buộc “mỗi ngày” chỉ có ý nghĩa tổng hợp. Vì mỗi loại có thể đóng góp tối đa một viên kẹo mỗi ngày nên trong tất cả các ngày, nó có thể đóng góp nhiều nhất$\min(k_i, d)$kẹo. Một khi chúng ta chấp nhận điều này, vấn đề sẽ trở thành một vấn đề lựa chọn toàn cầu thuần túy: chúng ta đang chọn tối đa$d \cdot x$tổng số mặt hàng, trong đó mỗi mặt hàng có trọng lượng$c_i$và mỗi loại đóng góp nhiều nhất$\min(k_i, d)$bản sao giống hệt nhau của trọng lượng đó. 

Bây giờ vấn đề trở nên đơn giản. Chúng tôi mở rộng từng loại thành$\min(k_i, d)$các mục ảo và chọn trên cùng$d \cdot x$các giá trị tổng thể. Vì tất cả các mục thuộc một loại đều có giá trị giống nhau nên chúng ta thực sự không cần phải mở rộng chúng. Thay vào đó, chúng tôi sắp xếp các loại theo$c_i$và tham lam lấy càng nhiều càng tốt, tôn trọng giới hạn. 

Chúng tôi xử lý các loại theo thứ tự độ ngon giảm dần và luôn lấy càng nhiều càng tốt cho đến giới hạn mỗi loại và tổng công suất còn lại. Điều này hiệu quả vì các mặt hàng có độ ngon cao hơn phải luôn lấp đầy các vị trí toàn cầu có giới hạn trước tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(d \cdot n \log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta chuyển quan sát này thành một công trình trực tiếp. 

### Các bước 

1. Tính toán mức độ sẵn có hiệu quả của từng loại như sau:$a_i = \min(k_i, d)$. Điều này phản ánh thực tế là một loại không thể đóng góp nhiều hơn một viên kẹo mỗi ngày. 
2. Ghép từng loại với giá trị độ ngon của nó và sắp xếp tất cả các loại theo thứ tự giảm dần$c_i$. Điều này đảm bảo chúng tôi luôn xem xét các nguồn kẹo có giá trị nhất trước tiên. 
3. Duy trì một bộ đếm tổng số kẹo mà chúng ta vẫn được phép lấy, khởi tạo là$d \cdot x$. Điều này thể hiện tổng công suất hàng ngày được tổng hợp trong tất cả các ngày. 
4. Lặp lại các kiểu đã được sắp xếp. Với mỗi loại, lấy$t = \min(a_i, \text{remaining})$kẹo và thêm$t \cdot c_i$để trả lời. 
5. Trừ$t$từ công suất còn lại. Dừng sớm nếu công suất đạt 0. 

Lý do bước 1 quan trọng là vì nó mã hóa giới hạn tính duy nhất mỗi ngày thành giới hạn toàn cầu. Nếu không có nó, chúng tôi sẽ tính sai số lượng đóng góp từ các loại tần số cao. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán sẽ phân bổ các loại kẹo có giá trị nhất vào một số vị trí cố định. Mỗi ô tượng trưng cho một viên kẹo được ăn trong một ngày. Vì không có ràng buộc về cấu trúc nào phân biệt các vị trí ngoại trừ dung lượng chung và giới hạn lặp lại của mỗi loại, nên chiến lược tối ưu là luôn lấp đầy các vị trí sớm nhất bằng giá trị độ ngon cao nhất hiện có. Quá trình tham lam được sắp xếp đảm bảo rằng nếu một viên kẹo có giá trị thấp hơn được chọn trong khi vẫn còn một viên có giá trị cao hơn, chúng ta có thể trao đổi chúng mà không vi phạm bất kỳ ràng buộc nào và cải thiện hoặc duy trì hoàn toàn độ ngon. Đối số trao đổi này đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d, x = map(int, input().split())
    k = list(map(int, input().split()))
    c = list(map(int, input().split()))
    
    items = []
    for i in range(n):
        items.append((c[i], min(k[i], d)))
    
    items.sort(reverse=True)
    
    remaining = d * x
    ans = 0
    
    for val, cnt in items:
        if remaining == 0:
            break
        take = min(cnt, remaining)
        ans += take * val
        remaining -= take
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chuyển đổi từng loại kẹo thành một cặp giá trị và giới hạn đóng góp hiệu quả của nó. Việc sắp xếp theo giá trị đảm bảo chúng tôi luôn ưu tiên các loại có độ ngon cao hơn trước. Vòng tích lũy tham lam thực thi ràng buộc năng lực toàn cầu trong khi tôn trọng các giới hạn của mỗi loại. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng ngày một cách rõ ràng. Ràng buộc “nhiều nhất x mỗi ngày trong d ngày” được giảm xuống một cách an toàn thành một giới hạn duy nhất$d \cdot x$. Một chi tiết quan trọng khác là áp dụng$\min(k_i, d)$, điều cần thiết là phải tôn trọng quy tắc "tối đa một loại mỗi ngày". 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 3 3
1 1 2 1 3 2 2 12
7 6 9 4 3 5 8 10
```Đầu tiên chúng tôi tính toán giới hạn: 

| Loại | k_i | c_i | cap = min(k_i, d) | 
| --- | --- | --- | --- | 
| 1 | 1 | 7 | 1 | 
| 2 | 1 | 6 | 1 | 
| 3 | 2 | 9 | 2 | 
| 4 | 1 | 4 | 1 | 
| 5 | 3 | 3 | 3 | 
| 6 | 2 | 5 | 2 | 
| 7 | 2 | 8 | 2 | 
| 8 | 12 | 10 | 3 | 

Chúng tôi sắp xếp theo độ ngon: 10, 9, 8, 7, 6, 5, 4, 3. 

Tổng công suất là$d \cdot x = 3 \cdot 3 = 9$. 

Chúng tôi lấy một cách tham lam: 

| Giá trị | Mũ | Đi | Còn lại | Đạt được | 
| --- | --- | --- | --- | --- | 
| 10 | 3 | 3 | 6 | 30 | 
| 9 | 2 | 2 | 4 | 18 | 
| 8 | 2 | 2 | 2 | 16 | 
| 7 | 1 | 1 | 1 | 7 | 
| 6 | 1 | 1 | 0 | 6 | 

Tổng cộng = 77. 

Dấu vết này cho thấy các loại có giá trị cao sẽ bão hòa trước như thế nào và các loại có giá trị thấp hơn chỉ lấp đầy dung lượng còn sót lại. 

### Mẫu 2 

đầu vào:```
1 200000 200000
200000
200000
```mũ lưỡi trai là$\min(200000, 200000) = 200000$. Tổng công suất là$4 \cdot 10^{10}$. 

Chúng tôi lấy 200000 viên kẹo trị giá 200000: 

Trả lời =$200000 \cdot 200000 = 4 \cdot 10^{10}$. 

Điều này khẳng định rằng một loại hình duy nhất có thể lấp đầy hoàn toàn công suất khi không có sự cạnh tranh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Phân loại theo độ ngon chiếm ưu thế | 
| Không gian |$O(n)$| Lưu trữ cặp giá trị và giới hạn | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các loại, vì vậy một$O(n \log n)$giải pháp thoải mái phù hợp trong thời hạn. Việc sử dụng bộ nhớ vẫn tuyến tính về số lượng loại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, d, x = map(int, input().split())
    k = list(map(int, input().split()))
    c = list(map(int, input().split()))

    items = [(c[i], min(k[i], d)) for i in range(n)]
    items.sort(reverse=True)

    remaining = d * x
    ans = 0
    for val, cnt in items:
        take = min(cnt, remaining)
        ans += take * val
        remaining -= take

    return str(ans)

# sample 1 (fixed format)
assert run("8 3 3\n1 1 2 1 3 2 2 12\n7 6 9 4 3 5 8 10\n") == "77"

# sample 2
assert run("1 200000 200000\n200000\n200000\n") == "40000000000"

# minimum case
assert run("1 1 1\n1\n5\n") == "5"

# all equal
assert run("3 2 2\n5 5 5\n10 10 10\n") == "60"

# tight capacity
assert run("2 1 1\n5 10\n1 100\n") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loại đơn | 5 | độ đúng cơ sở | 
| giá trị thống nhất | 60 | xử lý các ưu tiên bình đẳng | 
| giá trị hỗn hợp | 100 | tham lam đặt hàng đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh phát sinh khi một loại có kích thước cực lớn$k_i$nhưng nhỏ$d$. Thuật toán giới hạn chính xác sự đóng góp của nó cho$d$, ngăn chặn việc đếm quá mức. Ví dụ, với$d = 2$,$k_1 = 1000$,$c_1 = 10$, giới hạn hiệu dụng trở thành 2, tạo ra tổng giá trị là 20. 

Một trường hợp khác là khi tổng công suất$d \cdot x$nhỏ hơn tổng của tất cả các chữ hoa. Vòng lặp tham lam đảm bảo chúng ta dừng lại sớm để không bao giờ vượt quá mức tiêu thụ cho phép. 

Trường hợp cuối cùng là khi$x = 1$. Sau đó mỗi ngày chỉ được phép mua một viên kẹo nên tổng sức chứa bằng$d$, và lời giải rút gọn thành việc chọn ra giải pháp tốt nhất$d$các loại kẹo có sẵn (có áp dụng mũ). Thứ tự tham lam tương tự vẫn được áp dụng và thuật toán tự nhiên chọn các giá trị hàng đầu trước tiên mà không cần bất kỳ thay đổi cấu trúc nào.
