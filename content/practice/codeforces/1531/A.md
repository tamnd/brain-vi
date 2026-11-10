---
title: "CF 1531A - \u0417\u0438\u043d\u0433\u0435\u0440 | màu sắc"
description: "Chúng ta cần mô phỏng hoạt động của chatbot điều khiển màu sắc chiếu sáng của mái vòm Singer House. Ban đầu, mái vòm có màu xanh lam và được phép thay đổi màu sắc. Bot nhận được một chuỗi tin nhắn."
date: "2026-06-10T16:46:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1531
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2021 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (Engine)"
rating: 0
weight: 1531
solve_time_s: 109
verified: true
draft: false
---

[CF 1531A - \u0417\u0438\u043d\u0433\u0435\u0440 | màu](https://codeforces.com/problemset/problem/1531/A) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần mô phỏng hoạt động của chatbot điều khiển màu sắc chiếu sáng của mái vòm Singer House. 

Ban đầu mái vòm có màu`blue`và cho phép thay đổi màu sắc. Bot nhận được một chuỗi tin nhắn. Một số tin nhắn thay đổi trạng thái khóa, trong khi một số khác yêu cầu thay đổi màu sắc. 

Một tin nhắn`lock`vô hiệu hóa những thay đổi màu sắc trong tương lai. Nếu hệ thống đã bị khóa thì không có gì xảy ra. 

Một tin nhắn`unlock`cho phép thay đổi màu sắc một lần nữa. Nếu hệ thống đã được mở khóa, không có gì xảy ra. 

Một thông báo màu như`red`,`green`, hoặc`violet`chỉ thay đổi màu vòm khi hệ thống hiện được mở khóa. Nếu hệ thống bị khóa, tin nhắn sẽ bị bỏ qua. 

Đầu vào cung cấp lịch sử tin nhắn đầy đủ theo thứ tự thời gian. Chúng ta phải xác định màu vòm cuối cùng sau khi xử lý tất cả tin nhắn. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 100 tin nhắn, vì vậy ngay cả một mô phỏng đơn giản cũng dễ dàng phù hợp với bất kỳ giới hạn thời gian hợp lý nào. Vì mỗi thông báo chỉ ảnh hưởng đến trạng thái hiện tại nên không cần bất kỳ cấu trúc hoặc tối ưu hóa dữ liệu nâng cao nào. 

Nguồn sai lầm chính là việc xử lý các thao tác bị bỏ qua một cách chính xác. 

Hãy xem xét ví dụ này:```
3
lock
red
green
```Câu trả lời đúng là:```
blue
```Sau đó`lock`, việc thay đổi màu sắc sẽ bị vô hiệu hóa. Cả hai thông báo màu đều bị bỏ qua nên màu ban đầu không thay đổi. 

Một trường hợp tinh vi khác là các thao tác khóa lặp đi lặp lại:```
4
lock
lock
unlock
red
```Câu trả lời đúng là:```
red
```thứ hai`lock`không làm gì cả vì hệ thống đã bị khóa. Việc triển khai bất cẩn làm thay đổi trạng thái thay vì thiết lập nó một cách rõ ràng sẽ tạo ra kết quả sai. 

Một trường hợp nữa liên quan đến thao tác mở khóa lặp đi lặp lại:```
3
unlock
unlock
violet
```Câu trả lời đúng là:```
violet
```Tin nhắn mở khóa không có hiệu lực vì hệ thống đã được mở khóa. Sự thay đổi màu sắc cuối cùng được áp dụng bình thường. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng bot chính xác như mô tả. Chúng tôi duy trì hai biến: màu hiện tại và liệu các thay đổi màu có bị khóa hay không. Chúng tôi xử lý từng tin nhắn một và cập nhật các biến này theo quy tắc. 

Việc giải thích vấn đề một cách thô bạo đã là tối ưu vì mọi thông báo đều có khả năng thay đổi trạng thái. Mọi giải pháp đúng đều phải kiểm tra tất cả các tin nhắn ít nhất một lần. Với tối đa 100 tin nhắn, việc này chỉ cần 100 thao tác, không đáng kể. 

Quan sát quan trọng là toàn bộ trạng thái hệ thống chỉ được biểu thị bằng hai phần thông tin: màu hiện tại và trạng thái khóa. Không cần thông tin lịch sử khi tin nhắn đã được xử lý. Vì mỗi thông báo chỉ ảnh hưởng đến trạng thái hiện tại nên chỉ cần quét tuyến tính một lần là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Theo dõi trạng thái tối ưu | O(n) | O(1) | Đã chấp nhận | 

Trong bài toán này, mô phỏng lực lượng vũ phu và giải pháp tối ưu thực sự là cùng một thuật toán. 

## Hướng dẫn thuật toán 

1. Khởi tạo màu hiện tại như`"blue"`bởi vì mái vòm bắt đầu với ánh sáng xanh lam. 
2. Khởi tạo trạng thái khóa như`False`bởi vì thay đổi màu sắc ban đầu được cho phép. 
3. Xử lý từng tin nhắn theo trình tự thời gian. 
4. Nếu tin nhắn được`"lock"`, đặt trạng thái khóa thành`True`. 

Điều này phù hợp với đặc điểm kỹ thuật mà các thay đổi màu sắc trong tương lai sẽ bị chặn. 
5. Nếu tin nhắn được`"unlock"`, đặt trạng thái khóa thành`False`. 

Điều này cho phép thay đổi màu sắc một lần nữa. 
6. Nếu không, thông báo sẽ đại diện cho một màu. 

Nếu trạng thái khóa là`False`, cập nhật màu hiện tại cho tin nhắn này. 

Nếu trạng thái khóa là`True`, bỏ qua tin nhắn. 
7. Sau khi tất cả các tin nhắn đã được xử lý, hãy xuất ra màu hiện tại. 

### Tại sao nó hoạt động 

Tại mọi thời điểm trong quá trình xử lý, cặp`(current_color, locked)`khớp chính xác với trạng thái của bot thực sau khi xử lý cùng một tiền tố của tin nhắn. 

Việc khởi tạo phù hợp với báo cáo vấn đề. Mỗi tin nhắn được xử lý theo các quy tắc chính thức, cập nhật trạng thái khóa hoặc màu sắc. Vì mọi chuyển đổi trạng thái đều được sao chép chính xác nên trạng thái được duy trì vẫn giống hệt với hệ thống thực sau mỗi bước. Sau thông báo cuối cùng, màu được lưu trữ chính xác là màu cuối cùng của mái vòm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    color = "blue"
    locked = False

    for _ in range(n):
        msg = input().strip()

        if msg == "lock":
            locked = True
        elif msg == "unlock":
            locked = False
        else:
            if not locked:
                color = msg

    print(color)

if __name__ == "__main__":
    solve()
```Giải pháp giữ chính xác hai phần trạng thái được xác định trong cuộc thảo luận về thuật toán. 

Biến`color`lưu trữ màu vòm hiện tại. Nó bắt đầu như`"blue"`bởi vì đó là điều kiện ban đầu được đưa ra trong câu lệnh. 

Biến`locked`ghi lại xem các thay đổi màu sắc hiện có bị chặn hay không. Nó bắt đầu như`False`, nghĩa là cho phép thay đổi màu sắc. 

Đối với mỗi tin nhắn, trước tiên chúng tôi kiểm tra xem đó là lệnh khóa hay lệnh mở khóa. Những lệnh đó trực tiếp đặt trạng thái khóa. Chúng tôi không chuyển đổi trạng thái vì lặp đi lặp lại`lock`hoặc`unlock`tin nhắn phải vô hại. 

Mọi thông báo còn lại nhất thiết phải là một trong bảy tên màu. Chúng tôi chỉ cập nhật màu khi hệ thống không bị khóa. Nếu nó bị khóa, tin nhắn sẽ bị bỏ qua đúng theo yêu cầu. 

Không cần xử lý ranh giới đặc biệt vì đầu vào đảm bảo mọi thông báo đều thuộc về tập hợp được phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
red
violet
unlock
red
orange
lock
indigo
```| Bước | Tin nhắn | Đã khóa | Màu sắc | 
| --- | --- | --- | --- | 
| Bắt đầu | - | Sai | màu xanh | 
| 1 | đỏ | Sai | đỏ | 
| 2 | màu tím | Sai | màu tím | 
| 3 | mở khóa | Sai | màu tím | 
| 4 | đỏ | Sai | đỏ | 
| 5 | cam | Sai | cam | 
| 6 | khóa | Đúng | cam | 
| 7 | chàm | Đúng | cam | 

Đầu ra cuối cùng:```
orange
```Dấu vết này cho thấy rằng khi hệ thống bị khóa, các thông báo màu tiếp theo sẽ bị bỏ qua. trận chung kết`indigo`tin nhắn không có tác dụng. 

### Ví dụ 2 

đầu vào:```
5
lock
red
unlock
green
violet
```| Bước | Tin nhắn | Đã khóa | Màu sắc | 
| --- | --- | --- | --- | 
| Bắt đầu | - | Sai | màu xanh | 
| 1 | khóa | Đúng | màu xanh | 
| 2 | đỏ | Đúng | màu xanh | 
| 3 | mở khóa | Sai | màu xanh | 
| 4 | màu xanh lá cây | Sai | màu xanh lá cây | 
| 5 | màu tím | Sai | màu tím | 

Đầu ra cuối cùng:```
violet
```Ví dụ này thể hiện cả hai hành vi. các`red`tin nhắn bị bỏ qua khi bị khóa, nhưng các thay đổi màu sau này vẫn hoạt động bình thường sau khi mở khóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tin nhắn được xử lý một lần | 
| Không gian | O(1) | Chỉ màu hiện tại và trạng thái khóa được lưu trữ | 

Từ`n ≤ 100`, thời gian chạy rất nhỏ. Thuật toán thực hiện một lượng công việc không đổi trên mỗi tin nhắn và chỉ sử dụng một vài biến bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())

    color = "blue"
    locked = False

    for _ in range(n):
        msg = input().strip()

        if msg == "lock":
            locked = True
        elif msg == "unlock":
            locked = False
        else:
            if not locked:
                color = msg

    return color

# provided sample
assert run(
    "7\nred\nviolet\nunlock\nred\norange\nlock\nindigo\n"
) == "orange", "sample 1"

# minimum size input
assert run(
    "1\nred\n"
) == "red", "single color change"

# locked before color change
assert run(
    "2\nlock\nviolet\n"
) == "blue", "color must be ignored"

# repeated lock and unlock commands
assert run(
    "5\nlock\nlock\nunlock\nunlock\ngreen\n"
) == "green", "repeated state commands"

# many color changes while unlocked
assert run(
    "4\nred\nyellow\ngreen\nviolet\n"
) == "violet", "last applied color wins"

# boundary-style long test
assert run(
    "10\nlock\nred\nunlock\norange\nlock\nblue\nunlock\nindigo\nlock\nviolet\n"
) == "indigo", "mixed operations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / red`|`red`| Đầu vào hợp lệ nhỏ nhất | 
|`lock, violet`|`blue`| Bỏ qua màu khi bị khóa | 
| Lệnh khóa/mở khóa lặp đi lặp lại |`green`| Lệnh trạng thái không phải là chuyển đổi | 
| Nhiều màu khi mở khóa |`violet`| Chiến thắng màu sắc thành công mới nhất | 
| Trình tự khóa và mở khóa hỗn hợp |`indigo`| Tương tác đúng của cả hai trạng thái | 

## Vỏ cạnh 

### Màu tin nhắn khi bị khóa 

đầu vào:```
3
lock
red
green
```Việc thực thi bắt đầu với`(blue, unlocked)`. Sau đó`lock`, nhà nước trở thành`(blue, locked)`. Cả hai thông báo màu đều đến khi bị khóa nên chúng bị bỏ qua. Còn lại màu cuối cùng`blue`. 

Đầu ra:```
blue
```### Thao tác khóa lặp đi lặp lại 

đầu vào:```
4
lock
lock
unlock
red
```Sau lần đầu tiên`lock`, hệ thống sẽ bị khóa. thứ hai`lock`để nó bị khóa. Nó không chuyển về trạng thái mở khóa. Sau đó`unlock`, việc thay đổi màu sắc lại được cho phép và`red`được áp dụng. 

Đầu ra:```
red
```### Thao tác mở khóa lặp đi lặp lại 

đầu vào:```
3
unlock
unlock
violet
```Hệ thống bắt đầu mở khóa, vì vậy cả hai`unlock`tin nhắn giữ nguyên trạng thái. Thông báo màu cuối cùng được xử lý bình thường và thay đổi màu vòm thành`violet`. 

Đầu ra:```
violet
```### Không đổi màu thành công 

đầu vào:```
4
lock
red
yellow
green
```Mái vòm bắt đầu như`blue`. Lệnh khóa vô hiệu hóa tất cả các thay đổi màu tiếp theo. Mọi thông báo màu đều bị bỏ qua, vì vậy màu ban đầu sẽ tồn tại cho đến hết. 

Đầu ra:```
blue
```
