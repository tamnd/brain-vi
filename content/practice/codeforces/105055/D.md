---
title: "CF 105055D - Đến lượt ai?"
description: "Hai vận động viên luân phiên thực hiện các khối lặp lại có kích thước cố định tại phòng tập thể dục. Marcel luôn thực hiện khối đầu tiên, sau đó là Joãozão, rồi lại Marcel, v.v. Mỗi khối chứa đúng M lần lặp lại."
date: "2026-06-28T00:22:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "D"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 56
verified: true
draft: false
---

[CF 105055D - Đến lượt ai?](https://codeforces.com/problemset/problem/105055/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai vận động viên luân phiên thực hiện các khối lặp lại có kích thước cố định tại phòng tập thể dục. Marcel luôn thực hiện khối đầu tiên, sau đó là Joãozão, rồi lại Marcel, v.v. Mỗi khối chứa chính xác`M`sự lặp lại. Họ không theo dõi những đóng góp của từng cá nhân; thay vào đó, có một bộ đếm duy nhất`N`ghi lại tổng số lần lặp lại đã được hoàn thành. 

Nhiệm vụ là xác định chính xác đến lượt ai đến sau`N`sự lặp lại đã được thực hiện, với điều kiện là lần lượt thay thế nghiêm ngặt theo các khối có kích thước`M`. 

Một cách hữu ích để diễn đạt lại tình huống là hãy tưởng tượng chuỗi lặp lại được chia thành các phần có kích thước khác nhau.`M`. Đoạn 0 là của Marcel, đoạn 1 là của Joãozão, đoạn 2 lại là của Marcel, v.v. Tổng cộng`N`cho chúng ta biết chúng ta đã tiến được bao xa trong chuỗi phân đoạn này và chúng ta cần xác định xem chúng ta hiện đang ở phân đoạn nào và liệu nó đã hoàn thành chưa. 

Những ràng buộc cho phép`N`Và`M`lên tới`10^9`. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào làm tăng số lần lặp lại, vì điều đó sẽ yêu cầu tới một tỷ bước trong trường hợp xấu nhất. Thay vào đó, lời giải phải tính toán câu trả lời theo thời gian không đổi bằng cách sử dụng số học. 

Trường hợp cạnh tinh tế xuất hiện khi`N`chính xác là bội số của`M`. Ví dụ, nếu`M = 3`Và`N = 6`, thì có đúng hai khối đầy đủ đã được hoàn thành. Trong tình huống này, chúng ta đang bắt đầu một khối mới, vì vậy Marcel sẽ là người tiếp theo vì các lượt luôn bắt đầu lại với Marcel sau khi Joãozão hoàn thành một khối đầy đủ. Một triển khai đơn giản chỉ kiểm tra`(N // M) % 2`không xem xét liệu chúng ta đang ở bên trong hay ở ranh giới của một khối có thể dễ dàng dịch chuyển câu trả lời không chính xác nếu nó xử lý việc hoàn thành và tiến trình một phần theo cùng một cách. 

Một trường hợp cạnh quan trọng khác là khi`N < M`. Ví dụ, nếu`M = 8`Và`N = 6`, chúng tôi vẫn đang ở trong dãy nhà đầu tiên của Marcel. Câu trả lời đúng vẫn là Marcel, mặc dù chưa đạt đến ranh giới toàn khối tiếp theo. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ theo dõi từng lần lặp lại, chuyển đổi người hoạt động mỗi khi một khối`M`lần lặp lại được hoàn thành. Chúng tôi sẽ duy trì một bộ đếm cho tiến trình khối hiện tại và chuyển đổi xem lượt của ai. Mỗi lần lặp lại sẽ tăng bộ đếm và khi nó đạt đến`M`, chúng tôi đặt lại nó và chuyển đổi người chơi. 

Cách tiếp cận này đúng vì nó phản ánh trực tiếp quá trình được mô tả trong bài toán. Tuy nhiên, nó thực hiện`N`lặp đi lặp lại, và kể từ đó`N`có thể lớn như`10^9`, quá chậm để chạy trong thời gian giới hạn. 

Quan sát quan trọng là chỉ có các khối đầy đủ mới quan trọng để xác định lượt tiếp theo của ai. Mọi`M`sự lặp lại lật người chơi đang hoạt động. Do đó, thay vì mô phỏng các lần lặp lại riêng lẻ, chúng ta chỉ cần đếm xem có bao nhiêu khối đầy đủ đã được hoàn thành, tức là`N // M`, rồi xác định xem chúng ta đang ở trong một khối đã hoàn thiện một phần hay chính xác là ở một ranh giới. 

Sự khác biệt giữa “bên trong một khối” và “ranh giới chính xác” là yếu tố quyết định liệu chúng ta nên xem xét khối hiện tại hay khối tiếp theo. Nếu như`N % M == 0`, chúng ta đang bắt đầu một khối mới, vì vậy người chơi tiếp theo được xác định trực tiếp bằng tính chẵn lẻ của các khối đã hoàn thành. Nếu như`N % M != 0`, chúng tôi vẫn ở trong khối hiện tại, vì vậy người chơi tiếp theo là người hiện đang hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem có bao nhiêu khối có kích thước hoàn chỉnh`M`đã hoàn tất bằng cách sử dụng phép chia số nguyên`q = N // M`. Điều này đếm số lần lượt đã chuyển hoàn toàn. Điều này quan trọng vì mỗi khối hoàn thành sẽ lật người chơi đang hoạt động. 
2. Tính xem chúng ta đang ở bao xa trong khối hiện tại bằng cách sử dụng`r = N % M`. Điều này cho biết chúng ta đang ở chính xác ranh giới hay vẫn ở giữa khu nhà. 
3. Nếu`r == 0`, thì hiện tại không có sự lặp lại nào được thực hiện trong khối được lấp đầy một phần. Khối tiếp theo để bắt đầu là khối`q`, do đó danh tính của người chơi tiếp theo chỉ phụ thuộc vào việc`q`là chẵn hoặc lẻ. Vì Marcel bắt đầu ở khối 0, nên các chỉ số chẵn tương ứng với Marcel và các chỉ số lẻ tương ứng với Joãozão. 
4. Nếu`r != 0`, chúng tôi đang ở trong khối`q`. Điều đó có nghĩa là người chơi đang hoạt động hiện tại vẫn đang làm việc trên khối này, vì vậy lần lặp lại tiếp theo thuộc về cùng một người chơi được chỉ định chặn`q`. Quy tắc chẵn lẻ tương tự được áp dụng. 
5. Xuất “MARCEL” nếu`q`là số chẵn, nếu không thì xuất ra “JOAOZAO”. 

Quyết định cốt lõi làm giảm toàn bộ quá trình để theo dõi xem chúng tôi đang ở trong khối nào và sử dụng tính chẵn lẻ để ánh xạ các khối tới người chơi. 

### Tại sao nó hoạt động 

Mỗi khối hoàn chỉnh có kích thước`M`tương ứng với chính xác một lượt không bị gián đoạn của một người chơi và các lượt thay thế được xác định bắt đầu từ Marcel. Do đó, chỉ số khối sẽ xác định duy nhất người chơi. Phần còn lại`r`không thay đổi khối chúng ta đang ở, chỉ là chúng ta đang ở cuối hay bên trong khối đó. Vì hành động tiếp theo luôn được xác định bởi việc gán khối hiện tại, tính chẵn lẻ của`q`là đủ để xác định người chơi chính xác trong mọi trường hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = int(input())
M = int(input())

q = N // M
r = N % M

if q % 2 == 0:
    print("MARCEL")
else:
    print("JOAOZAO")
```Đầu tiên, mã đọc hai số nguyên và tính xem có bao nhiêu khối đầy đủ đã được hoàn thành. Phần còn lại được tính toán nhưng không ảnh hưởng đến quyết định cuối cùng ngoài việc xác nhận xem chúng ta đang ở trong một khối hay ở một ranh giới và cả hai trường hợp đều giải quyết theo cùng một quy tắc dựa trên tính chẵn lẻ. 

Lựa chọn triển khai chính là bỏ qua hoàn toàn mô phỏng. Không cần phải theo dõi tiến trình trong một khối ngoài việc xác nhận hành vi phân chia. Phép chia số nguyên và modulo mã hóa đầy đủ trạng thái của hệ thống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
3
```Đây`q = 10 // 3 = 3`,`r = 1`. 

| Bước | N | M | q | r | Khối chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 10 | 3 | 0 | 0 | - | - | 
| Tính toán | 10 | 3 | 3 | 1 | lẻ | JOAOZAO | 

Từ`q = 3`thật kỳ lạ, khối hiện tại thuộc về Joãozão. Chúng tôi đang ở trong khối này (`r != 0`), vì vậy Joãozão vẫn là người tham gia tích cực và do đó là người tiếp theo. 

### Ví dụ 2 

đầu vào:```
6
8
```Đây`q = 6 // 8 = 0`,`r = 6`. 

| Bước | N | M | q | r | Khối chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 8 | 0 | 6 | thậm chí | MARCEL | 

Chúng tôi vẫn đang ở dãy nhà đầu tiên thuộc về Marcel. Từ`r != 0`, Marcel vẫn đang thực hiện khối này nên anh ấy vẫn là người tham gia tiếp theo. 

Những ví dụ này xác nhận rằng cả trường hợp khối một phần và khối toàn bộ đều giảm nhất quán về mức chẵn lẻ của`q`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp là thời gian không đổi và dễ dàng thỏa mãn các ràng buộc lên đến`10^9`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(input())
    M = int(input())

    q = N // M

    if q % 2 == 0:
        return "MARCEL"
    else:
        return "JOAOZAO"

# provided samples
assert run("10\n3\n") == "JOAOZAO"
assert run("6\n8\n") == "MARCEL"
assert run("20\n1\n") == "MARCEL"

# custom cases
assert run("0\n5\n") == "MARCEL", "start of first block"
assert run("5\n5\n") == "JOAOZAO", "exact boundary flips block"
assert run("9\n4\n") == "JOAOZAO", "inside second block"
assert run("8\n4\n") == "MARCEL", "exact full cycles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 5 | MARCEL | trạng thái ban đầu trước khi lặp lại | 
| 5 5 | JOAOZAO | ranh giới chính xác giữa các khối | 
| 9 4 | JOAOZAO | độ chính xác chuyển tiếp giữa khối | 
| 8 4 | MARCEL | thiết lập lại tính chẵn lẻ toàn chu kỳ | 

## Vỏ cạnh 

Khi nào`N = 0`, không có sự lặp lại nào được thực hiện. Thuật toán tính toán`q = 0`, chẵn nên nó cho ra Marcel. Điều này phù hợp với thực tế là Marcel luôn xuất phát trước. 

Khi`N`chính xác là chia hết cho`M`, chẳng hạn như`N = 20, M = 5`, chúng tôi có`q = 4`. Thuật toán coi đây là sự khởi đầu của một khối mới và vì khối 4 lại tương ứng với Marcel (chỉ mục chẵn), nên kết quả đầu ra là chính xác. 

Khi`M = 1`, mỗi lần lặp lại là khối riêng của nó. Thuật toán giảm bài toán xuống việc xen kẽ từng bước và tính chẵn lẻ của`N`trực tiếp xác định kết quả phù hợp với trình tự xen kẽ dự kiến.
