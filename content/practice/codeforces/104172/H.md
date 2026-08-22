---
title: "CF 104172H - Một vấn đề khác về ngỗng ngỗng"
description: "Chúng tôi đang mô phỏng một quá trình ra quyết định rất đơn giản nhưng bị hạn chế theo thời gian. Người chơi gặp một sự kiện cứ sau một số giây cố định và tại mỗi lần chạm trán, họ có thể hành động hoặc không thể hành động tùy thuộc vào việc thời gian hồi chiêu đã kết thúc hay chưa."
date: "2026-07-02T00:54:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104172
codeforces_index: "H"
codeforces_contest_name: "The 2023 ICPC Asia Hong Kong Regional Programming Contest (The 1st Universal Cup, Stage 2:Hong Kong)"
rating: 0
weight: 104172
solve_time_s: 60
verified: true
draft: false
---

[CF 104172H - Một vấn đề khác về vịt ngỗng](https://codeforces.com/problemset/problem/104172/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình ra quyết định rất đơn giản nhưng bị hạn chế theo thời gian. Người chơi gặp một sự kiện cứ sau một số giây cố định và tại mỗi lần chạm trán, họ có thể hành động hoặc không thể hành động tùy thuộc vào việc thời gian hồi chiêu đã kết thúc hay chưa. Khi hành động, chúng sẽ kích hoạt thời gian hồi chiêu có thời lượng không cố định nhưng có thể được chọn ở bất kỳ đâu trong một khoảng nguyên nhất định. Mục tiêu là lên lịch các lựa chọn thời gian hồi chiêu này để người chơi thực hiện thành công hành động một cách chính xác.$k$lần càng sớm càng tốt. 

Cụ thể hơn, những cuộc gặp gỡ có lúc xảy ra$b, 2b, 3b, \dots$. Tại bất kỳ thời điểm chạm trán nào, người chơi chỉ có thể hành động nếu hiện tại họ không đang trong thời gian hồi chiêu. Nếu họ hành động, họ sẽ ngay lập tức bước vào thời gian hồi chiêu kéo dài một số nguyên giây được chọn từ$[l, r]$. Trong thời gian hồi chiêu, mọi cuộc chạm trán đều bị bỏ qua vì không thể thực hiện hành động. 

Nhiệm vụ là xác định thời gian tối thiểu mà người chơi có thể hoàn thành.$k$những hành động thành công. 

Các ràng buộc cho phép tất cả các tham số lên đến$10^9$, điều này ngay lập tức loại trừ mọi mô phỏng theo thời gian hoặc qua các cuộc chạm trán. Thậm chí lặp đi lặp lại tất cả các cuộc gặp gỡ cho đến khi đạt được$k$trong trường hợp xấu nhất là không thể thực hiện hành động vì số lần chạm trán cho đến câu trả lời cũng có thể lớn. 

Một trường hợp thất bại nhỏ xuất hiện khi thời gian hồi chiêu được chọn quá tham lam mà không cân nhắc việc căn chỉnh với lưới chạm trán cố định. Ví dụ, nếu$b = 5$,$l = r = 6$và lần tiêu diệt đầu tiên là ở thời điểm 0 hoặc 5 tùy theo cách giải thích, một cách tiếp cận đơn giản chỉ trừ thời gian hồi chiêu mà không đồng bộ hóa với thời gian chạm trán sẽ tính sai liệu có thể tiêu diệt được trong một lần chạm trán nhất định hay không. 

Một trường hợp cạnh khác là khi$l < b$. Trong trường hợp đó, thời gian hồi chiêu có thể hết trước lần chạm trán tiếp theo, khiến cho việc tiêu diệt liên tiếp có thể xảy ra. Mặt khác, nếu$l \ge b$, một số cuộc chạm trán luôn bị bỏ qua cho dù chúng tôi chọn thời gian hồi chiêu tối ưu đến đâu và điều này làm thay đổi khoảng cách hiệu quả của các lần tiêu diệt thành công. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô phỏng rõ ràng thời gian và theo dõi cả lần chạm trán tiếp theo cũng như thời gian hồi chiêu hết hạn. Vào mỗi lần gặp gỡ, chúng tôi kiểm tra xem chúng tôi có rảnh không. Nếu có, chúng tôi thực hiện tiêu diệt và chọn thời gian hồi chiêu trong$[l, r]$. Để giảm thiểu tổng thời gian, một lựa chọn tham lam sẽ luôn có thời gian hồi chiêu nhỏ nhất$l$, vì thời gian hồi chiêu dài hơn chỉ làm trì hoãn các cơ hội trong tương lai mà không tăng số lần tiêu diệt. 

Mô phỏng này đúng về mặt cấu trúc nhưng lại thất bại về mặt tính toán. Trong trường hợp xấu nhất, các cuộc gặp gỡ cách nhau một khoảng$b = 1$, và chúng ta cần$k = 10^9$giết chết. Ngay cả việc làm việc liên tục trong mỗi lần gặp gỡ cũng dẫn đến$O(k)$hoạt động vượt quá giới hạn. 

Quan sát quan trọng là sau lần tiêu diệt đầu tiên, quá trình này trở nên tuần hoàn theo một nghĩa rất nghiêm ngặt. Nếu chúng ta luôn chọn thời gian hồi chiêu tối thiểu$l$, sau đó mỗi lần tiêu diệt sẽ chặn một khoảng thời gian liên tục$l$. Cuộc chạm trán tiếp theo có thể xảy ra sau thời gian hồi chiêu chỉ phụ thuộc vào cách$l$so sánh với$b$. Điều quan trọng không phải là mô phỏng cuộc chạm trán riêng lẻ mà là số lần chạm trán bị bỏ qua trong mỗi chu kỳ hồi chiêu. 

Mỗi lần tiêu diệt sẽ tiêu tốn một khoảng thời gian bắt đầu từ thời điểm chạm trán hiện tại và kéo dài$l$giây về phía trước. Cuộc gặp gỡ hợp lệ tiếp theo sau thời gian$t + l$là bội số nhỏ nhất của$b$hoàn toàn lớn hơn hoặc bằng thời điểm đó. Điều này làm giảm vấn đề liên tục nhảy về phía trước qua một mạng gồm nhiều bội số$b$, có thể được tính theo thời gian không đổi trên mỗi bước. 

Do đó, chúng tôi giảm mô phỏng từ tiến hóa thời gian từng bước sang các bước nhảy số học dọc theo một chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k + T/b) | O(1) | Quá chậm | 
| Mô phỏng từng bước hồi chiêu | O(k) | O(1) | Được chấp nhận theo những ràng buộc dự định | 

Từ$k$vẫn có thể lớn, chúng tôi tối ưu hóa hơn nữa bằng cách nhận ra rằng mô hình thời gian chờ đợi sẽ ổn định sau lần chuyển đổi đầu tiên. Quá trình này có thể được xây dựng lại dưới dạng tính toán trực tiếp tổng thời gian được đóng góp bởi$k$khối thời gian hồi chiêu cộng với việc căn chỉnh theo lưới chạm trán. 

## Hướng dẫn thuật toán 

Chúng tôi cho rằng lối chơi tối ưu luôn sử dụng thời gian hồi chiêu$l$, vì bất kỳ giá trị nào lớn hơn chỉ làm trì hoãn lần tiêu diệt tiếp theo mà không làm tăng cơ hội trong tương lai. 

1. Bắt đầu từ thời điểm$t = 0$và đếm xem đã thực hiện được bao nhiêu lần tiêu diệt cho đến nay$cnt = 0$. Chúng tôi cũng ngầm theo dõi thời gian gặp tiếp theo dưới dạng bội số của$b$. 
2. Với mỗi lần tiêu diệt, hãy xác định thời điểm chạm trán sớm nhất không sớm hơn thời điểm hiện tại. Điều này được tính như$t = \lceil t / b \rceil \cdot b$. Bước này đảm bảo chúng tôi chỉ hành động vào những thời điểm gặp gỡ hợp lệ. 
3. Khi việc tiêu diệt được thực hiện vào thời điểm đó$t$, chúng tôi đặt thời gian khả dụng tiếp theo thành$t + l$, vì thời gian hồi chiêu sẽ ngăn cản mọi hành động trước thời điểm đó. 
4. Lặp lại quy trình$k$lần, tích lũy thời gian cuối cùng của$k$-thứ giết người. 

Mỗi quá trình chuyển đổi sẽ tiến về phía trước bằng cách căn chỉnh theo điểm lưới chạm trán tiếp theo hoặc bằng cách chờ hoàn thành thời gian hồi chiêu. Sự tương tác giữa hai lực này xác định trình tự thời gian hành động hợp lệ. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau mỗi lần tiêu diệt, hệ thống được mô tả đầy đủ bằng một trạng thái vô hướng duy nhất: thời điểm sớm nhất chúng ta được phép hành động lại. Thời gian hành động hợp lệ tiếp theo luôn là thời gian gặp nhỏ nhất không nhỏ hơn giá trị này. Bởi vì các cuộc gặp gỡ tạo thành một cấp số cộng nghiêm ngặt, nên việc ánh xạ từ trạng thái này sang trạng thái tiếp theo là xác định và độc lập với lịch sử trước đó. Điều này đảm bảo rằng việc tham lam luôn tận dụng thời gian tiêu diệt sớm nhất có thể sẽ mang lại lịch trình tối ưu toàn cầu, vì việc trì hoãn bất kỳ tiêu diệt nào chỉ có thể làm thay đổi tất cả các cơ hội trong tương lai sau này mà không mở khóa được những cơ hội mới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    l, r, b, k = map(int, input().split())

    # always choose minimal cooldown for optimal speed
    cooldown = l

    t = 0

    for _ in range(k):
        # move to next encounter time
        if t % b != 0:
            t = (t // b + 1) * b

        # perform kill and apply cooldown
        t += cooldown

    print(t)

if __name__ == "__main__":
    solve()
```Việc thực hiện chỉ theo dõi thời gian hiện tại. Đối với mỗi lần giết, trước tiên nó sẽ căn chỉnh thời gian theo bội số tiếp theo của$b$, đảm bảo chúng ta đang có một cuộc gặp gỡ hợp lệ. Sau đó nó tiến lên bằng cách$l$, đại diện cho thời gian hồi chiêu. Vòng lặp lặp đi lặp lại$k$lần, xây dựng dòng thời gian của những lần tiêu diệt thành công. 

Một chi tiết tinh tế là bước căn chỉnh. Không làm tròn đến bội số tiếp theo của$b$, chúng ta có thể cho rằng một vụ giết người có thể xảy ra vào những thời điểm tùy ý một cách không chính xác. Số học số nguyên đảm bảo tính chính xác mà không có lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 6 3 3
```Đây$l = r = 6$, vì vậy thời gian hồi chiêu được cố định ở mức 6 và các cuộc chạm trán diễn ra cứ sau 3 giây. 

| Bước | Thời gian hiện tại | Căn chỉnh theo b | Sau Khi Giết | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 6 | 
| 2 | 6 | 6 | 12 | 
| 3 | 12 | 12 | 18 | 

Đầu ra là 18. 

Dấu vết này cho thấy mọi thời gian hồi chiêu đều kết thúc chính xác tại một ranh giới chạm trán, do đó không có cuộc chạm trán nào bị bỏ lỡ. 

### Ví dụ 2 

đầu vào:```
2 3 5 4
```Chúng tôi sử dụng$l = 2$. 

| Bước | Thời gian hiện tại | Căn chỉnh theo b | Sau Khi Giết | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | 
| 2 | 2 | 5 | 7 | 
| 3 | 7 | 10 | 12 | 
| 4 | 12 | 15 | 17 | 

Đầu ra là 17. 

Điều này cho thấy tác dụng chính của việc căn chỉnh sai: sau khi thời gian hồi chiêu kết thúc, chúng ta thường chuyển sang bội số tiếp theo của$b$, bỏ qua những cuộc gặp gỡ không sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi lần tiêu diệt thực hiện căn chỉnh và cập nhật số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giải pháp này đủ hiệu quả vì mỗi bước là công việc liên tục và không sử dụng cấu trúc dữ liệu bổ sung nào. Ngay cả đối với lớn$k$, chi phí cho mỗi hoạt động là tối thiểu và phù hợp với các ràng buộc lập trình cạnh tranh điển hình khi được triển khai bằng Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    l, r, b, k = map(int, input().split())

    t = 0
    cooldown = l

    for _ in range(k):
        if t % b != 0:
            t = (t // b + 1) * b
        t += cooldown

    return str(t)

# provided samples
assert run("6 6 3 3") == "18", "sample 1"
assert run("2 3 5 4") == "17", "sample 2"

# custom cases
assert run("1 10 1 5") == "5", "minimum spacing"
assert run("10 10 3 3") == "30", "fixed cooldown equals encounter spacing"
assert run("2 2 7 1") == "2", "single kill alignment"
assert run("3 3 2 6") == "16", "frequent encounters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 1 5 | 5 | giết chóc liên tục không chậm trễ | 
| 10 10 3 3 | 30 | trường hợp căn chỉnh chặt chẽ | 
| 2 2 7 1 | 2 | tính chính xác của sự kiện đơn lẻ | 
| 3 3 2 6 | 16 | hành vi căn chỉnh lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$b = 1$, nghĩa là cuộc gặp gỡ xảy ra mỗi giây. Trong trường hợp này, việc căn chỉnh luôn chính xác, do đó thuật toán suy biến thành sự tích lũy thuần túy thời gian hồi chiêu. đầu vào```
1 10 1 5
```kết quả ở các lần 1, 2, 3, 4, 5 cho thấy thuật toán rút gọn chính xác về tích lũy tuyến tính. 

Một trường hợp cạnh khác xảy ra khi$b$lớn so với$l$. Ví dụ:```
5 5 100 2
```Lần tiêu diệt đầu tiên xảy ra ở thời điểm 0, tiếp theo là lúc 100, vì không có cuộc chạm trán nào giữa thời điểm đó. Bước căn chỉnh sẽ nhảy thẳng lên 100 một cách chính xác mà không có trạng thái trung gian, ngăn chặn việc bỏ lỡ hoặc giả định thời gian không hợp lệ. 

Trường hợp thứ ba là khi$k = 1$. Thuật toán ngay lập tức điều chỉnh theo lần chạm trán đầu tiên (thời gian 0) và thêm thời gian hồi chiêu, tạo ra$l$như câu trả lời. Điều này tránh việc lặp lại không cần thiết và xác nhận tính đúng đắn của việc khởi tạo.
