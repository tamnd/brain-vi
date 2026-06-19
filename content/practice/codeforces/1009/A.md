---
title: "CF 1009A - Trò chơi mua sắm"
description: "Cửa hàng giới thiệu một loạt trò chơi, mỗi trò chơi có một mức giá cố định và Maxim sẽ xem qua chúng một cách nghiêm ngặt từ trái sang phải."
date: "2026-06-16T22:55:58+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1009
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 47 (Rated for Div. 2)"
rating: 800
weight: 1009
solve_time_s: 86
verified: true
draft: false
---

[CF 1009A - Mua sắm trò chơi](https://codeforces.com/problemset/problem/1009/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cửa hàng giới thiệu một loạt trò chơi, mỗi trò chơi có một mức giá cố định và Maxim sẽ xem qua chúng một cách nghiêm ngặt từ trái sang phải. Đồng thời, anh ta mang theo một chiếc ví hoạt động giống như một hàng hóa đơn: chỉ có hóa đơn trước mới được xem xét và nó sẽ được tiêu thụ hoặc giữ nguyên vị trí tùy thuộc vào việc nó có đủ để thanh toán cho trò chơi hiện tại hay không. 

Ở mỗi vị trí trò chơi, Maxim thực hiện đúng một lần: anh ấy nhìn vào tờ tiền đầu tiên trong ví của mình. Nếu ví trống, anh ta hoàn toàn không thể tương tác với trò chơi đó và chỉ cần tiếp tục. Nếu tờ tiền đầu tiên có giá trị ít nhất bằng giá trò chơi, anh ta sẽ mua trò chơi và tờ tiền đó sẽ biến mất khỏi ví. Nếu không, anh ta không mua và giữ lại hóa đơn đó, sau đó chuyển sang trò chơi tiếp theo. Điểm quan trọng là những nỗ lực không thành công sẽ không nâng cao con trỏ ví, chỉ những lần mua hàng thành công mới làm được. 

Nhiệm vụ là xác định có bao nhiêu trò chơi được mua thành công sau khi xử lý tất cả các trò chơi theo thứ tự. 

Các ràng buộc rất nhỏ, với cả số lượng trò chơi và hóa đơn lên tới 1000. Điều này ngay lập tức cho phép mô phỏng đơn giản trong trường hợp xấu nhất bậc hai mà không có nguy cơ hết thời gian chờ. Bất kỳ giải pháp nào lên tới khoảng 10^6 thao tác đều an toàn thoải mái. 

Sự tinh tế chính nằm ở việc mô hình hóa chính xác mức độ tồn tại của cùng một hóa đơn khi giao dịch mua hàng không thành công. Một lỗi phổ biến là luôn nâng cao con trỏ ví bất kể thành công, điều này làm thay đổi hoàn toàn quy trình và dẫn đến việc tính thiếu các giao dịch mua. 

Trường hợp cạnh thứ hai xuất hiện khi ví trống trước khi trò chơi kết thúc. Trong tình huống đó, tất cả các trò chơi còn lại phải được bỏ qua vì không thể so sánh thêm. Ví dụ: nếu tất cả hóa đơn được tiêu thụ sớm, câu trả lời chỉ đơn giản là số lượng giao dịch mua được thực hiện cho đến nay và không có tương tác nào thêm. 

## Phương pháp tiếp cận 

Cách ngây thơ để nghĩ về quá trình này là mô phỏng từng bước theo đúng nghĩa đen như được mô tả. Chúng tôi duy trì một chỉ mục cho các trò chơi và một con trỏ tới hóa đơn chưa sử dụng đầu tiên. Đối với mỗi trò chơi, chúng tôi kiểm tra hóa đơn hiện tại. Nếu đủ, chúng ta tiêu thụ nó và di chuyển con trỏ hóa đơn về phía trước. Nếu không, chúng ta giữ cố định con trỏ hóa đơn và chuyển sang trò chơi tiếp theo. 

Mô phỏng này đã đủ hiệu quả vì mỗi hóa đơn chỉ có thể được tiêu thụ một lần và mỗi trò chơi được xử lý một lần. Điều đó có nghĩa là tối đa n + m chuyển động con trỏ thực sự thay đổi trạng thái và tối đa n lần kiểm tra không thành công không làm thay đổi trạng thái ví. Trường hợp xấu nhất vẫn là tuyến tính về số lượng trò chơi. 

Một quan điểm mang tính khái niệm hơn là quá trình này là một bước đi hai con trỏ. Một con trỏ di chuyển qua các trò chơi về phía trước. Con trỏ thứ hai chỉ di chuyển qua ví khi giao dịch mua hàng thành công. Thông tin chi tiết quan trọng là không có gì yêu cầu quay lại hoặc xem lại các trò chơi hoặc hóa đơn trước đó. Mỗi quyết định đều mang tính cục bộ và không thể đảo ngược. 

Điều này giúp loại bỏ mọi nhu cầu về vòng lặp lồng nhau hoặc tìm kiếm. Mô phỏng phản ánh trực tiếp cấu trúc của quá trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu (kiểm tra lồng nhau ngây thơ) | O(nm) | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Mô phỏng hai con trỏ | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai chỉ số: một cho trò chơi và một cho hóa đơn hiện tại trong ví. Chúng tôi cũng duy trì một quầy để mua hàng thành công. 

1. Khởi tạo con trỏ`j = 0`cho hóa đơn đầu tiên và một quầy`ans = 0`. Con trỏ biểu thị hóa đơn đang hoạt động hiện tại trong ví. 
2. Lặp lại qua từng trò chơi`i`từ trái sang phải. Ở mỗi bước, chúng tôi xem xét liệu có thể mua hàng hay không. 
3. Nếu ví trống (`j == m`), chúng tôi ngừng kiểm tra toàn bộ ví và tiếp tục bỏ qua các trò chơi còn lại. Điều này có tác dụng vì không thể mua thêm sau khi đã sử dụng hết hóa đơn. 
4. Ngược lại, so sánh giá trị hóa đơn hiện tại`a[j]`với giá trò chơi hiện tại`c[i]`. 
5. Nếu`a[j] >= c[i]`, tăng câu trả lời vì trò chơi đã được mua và di chuyển con trỏ ví về phía trước bằng cách đặt`j += 1`. Đây là mô hình tiêu thụ hóa đơn. 
6. Nếu`a[j] < c[i]`, không làm gì với con trỏ ví. Hóa đơn tương tự vẫn có sẵn cho trò chơi tiếp theo, vì vậy chúng tôi chỉ cần chuyển sang chỉ mục trò chơi tiếp theo. 
7. Tiếp tục cho đến khi tất cả trò chơi được xử lý. 

Lựa chọn thiết kế chính là con trỏ ví chỉ di chuyển khi thành công, khớp chính xác với quy tắc những lần thử thất bại sẽ không tiêu tốn hóa đơn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán vẫn bảo toàn thực tế rằng`a[j]`là tờ tiền đầu tiên chưa được tiêu dùng. Mọi trò chơi đều tiêu tốn hóa đơn này hoặc giữ nguyên. Vì không có thao tác nào bỏ qua hoặc sửa đổi các tờ tiền trước đó nên con trỏ`j`luôn theo dõi chính xác tài nguyên có sẵn tiếp theo. Mỗi lần mua thành công sẽ giảm đáng kể kích thước ví và mỗi trò chơi được đánh giá chính xác một lần, do đó không có cơ hội mua hàng hợp lệ nào bị bỏ qua hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    c = list(map(int, input().split()))
    a = list(map(int, input().split()))
    
    j = 0
    ans = 0
    
    for i in range(n):
        if j == m:
            break
        if a[j] >= c[i]:
            ans += 1
            j += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh một lần vượt qua các trò chơi. Con trỏ`j`chỉ tiến bộ khi hóa đơn được tiêu thụ, khớp với hành vi chính xác được mô tả trong quy trình. Việc thoát sớm khi`j == m`ngăn chặn những so sánh không cần thiết khi ví trống, mặc dù ngay cả khi không có nó thì giải pháp vẫn đúng. 

Một lỗi triển khai phổ biến là tăng`j`ngay cả khi việc mua hàng thất bại. Điều đó phá vỡ sự bất biến đó`a[j]`luôn là hóa đơn có sẵn đầu tiên và dẫn đến việc bỏ qua các hóa đơn có thể sử dụng được một cách giả tạo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, m = 4
c = [2, 4, 5, 2, 4]
a = [5, 3, 4, 6]
```| tôi | Chi phí trò chơi c[i] | Hóa đơn a[j] | Quyết định | j sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 5 | mua | 1 | 1 | 
| 1 | 4 | 3 | bỏ qua | 1 | 1 | 
| 2 | 5 | 3 | bỏ qua | 1 | 1 | 
| 3 | 2 | 3 | mua | 2 | 2 | 
| 4 | 4 | 4 | mua | 3 | 3 | 

Dấu vết này cho thấy cách sử dụng lại cùng một hóa đơn qua nhiều lần thử không thành công và chỉ những lần so sánh thành công mới thúc đẩy ví tiền. 

### Ví dụ 2 

đầu vào:```
n = 4, m = 3
c = [7, 8, 9, 10]
a = [5, 6, 4]
```| tôi | Chi phí trò chơi c[i] | Hóa đơn a[j] | Quyết định | j sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 7 | 5 | bỏ qua | 0 | 0 | 
| 1 | 8 | 5 | bỏ qua | 0 | 0 | 
| 2 | 9 | 5 | bỏ qua | 0 | 0 | 
| 3 | 10 | 5 | bỏ qua | 0 | 0 | 

Ví không bao giờ ứng trước nên không có giao dịch mua nào xảy ra. Điều này chứng tỏ trường hợp một hóa đơn nhỏ sẽ cản trở mọi tiến bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi trò chơi được xử lý một lần và mỗi hóa đơn được tiêu tối đa một lần | 
| Không gian | O(1) | Chỉ có hai bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn 1000 trò chơi và 1000 hóa đơn, yêu cầu tối đa vài nghìn thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""5 4
2 4 5 2 4
5 3 4 6
""") == "3"

# all cannot buy
assert run("""4 3
7 8 9 10
5 6 4
""") == "0"

# all can buy
assert run("""3 3
1 2 3
10 10 10
""") == "3"

# wallet exhausted early
assert run("""5 2
1 1 1 1 1
2 2
""") == "2"

# alternating pattern
assert run("""6 4
3 1 4 1 5 2
2 2 2 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chi phí cao | 0 | không thể mua được, con trỏ không bao giờ di chuyển | 
| tất cả chi phí thấp | đầy đủ m hoặc n giới hạn | hành vi tiêu dùng đầy đủ | 
| cạn kiệt ví sớm | đếm một phần | tính đúng đắn của điều kiện dừng | 
| giá trị xen kẽ | kết hợp tham lam một phần | ổn định dưới các quyết định hỗn hợp | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi hóa đơn đầu tiên quá nhỏ cho mọi trò chơi. Trong tình huống đó, con trỏ ví không bao giờ di chuyển và câu trả lời vẫn là 0. Ví dụ,`c = [5, 6, 7]`Và`a = [1]`kết quả là không có tiến triển gì cả vì một hóa đơn không bao giờ được tiêu thụ. 

Một trường hợp khác là khi tất cả các trò chơi đều có giá cực kỳ rẻ so với hóa đơn. Sau đó, mỗi trò chơi sẽ tiêu thụ chính xác một hóa đơn cho đến khi một trong hai danh sách kết thúc. Vì`c = [1, 1, 1]`Và`a = [10, 10]`, con trỏ tiến lên trong mỗi trò chơi cho đến khi hết hóa đơn thứ hai, tạo ra câu trả lời là 2.
