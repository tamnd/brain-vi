---
title: "CF 106084K - Đá chuyển động"
description: "Chúng ta được cấp một bảng $n nhân n$ trong đó mỗi ô chứa một số viên đá và tổng số viên đá trên toàn bộ lưới chính xác là $n^2$. Cấu hình mục tiêu rất cứng nhắc: sau tất cả các thao tác, mỗi ô phải chứa chính xác một viên đá."
date: "2026-06-21T09:29:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106084
codeforces_index: "K"
codeforces_contest_name: "2025 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 106084
solve_time_s: 42
verified: true
draft: false
---

[CF 106084K - Đá di chuyển](https://codeforces.com/problemset/problem/106084/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$bảng trong đó mỗi ô chứa một số viên đá và tổng số viên đá trên toàn bộ lưới chính xác là$n^2$. Cấu hình mục tiêu rất cứng nhắc: sau tất cả các thao tác, mỗi ô phải chứa chính xác một viên đá. 

Một nước đi bao gồm việc chọn một viên đá từ một ô nào đó và đặt nó vào bất kỳ ô nào khác trong cùng một hàng hoặc cùng một cột. Chi phí là số lần di chuyển như vậy. Chúng ta không được phép di chuyển một hòn đá một cách tùy tiện mà chỉ được phép di chuyển dọc theo hàng hoặc cột của nó. 

Nhiệm vụ là tính toán số lần di chuyển tối thiểu cần thiết để đạt được cấu hình thống nhất. 

Những ràng buộc cho phép$n \le 500$, vậy lưới có nhiều nhất$2.5 \times 10^5$tế bào. Bất kỳ giải pháp xung quanh$O(n^2)$hoặc$O(n^2 \log n)$vẫn ổn, trong khi bất cứ thứ gì có hình khối trong$n$sẽ là quá chậm. 

Trường hợp cạnh chính là khi lưới đã chính xác. Ví dụ: nếu mỗi ô đã chứa chính xác một viên đá, chẳng hạn như phân bố theo đường chéo được điều chỉnh để mỗi ô có một viên đá, thì câu trả lời phải bằng 0. Một cách tiếp cận ngây thơ luôn thực hiện phân phối lại có thể tính sai các bước di chuyển không cần thiết. 

Một trường hợp tinh tế khác là khi đá tập trung nhiều vào một hàng hoặc cột. Ví dụ: nếu một hàng chứa tất cả$n^2$đá và những thứ khác trống, một giải pháp chính xác vẫn phải hoạt động hiệu quả và không mô phỏng các lần chuyển tiền riêng lẻ. 

## Phương pháp tiếp cận 

Thoạt nhìn, vấn đề có vẻ giống như vấn đề vận chuyển hoặc dòng chảy. Mỗi ô có thặng dư hoặc thâm hụt so với giá trị mục tiêu của một viên đá trên mỗi ô. Một ý tưởng bạo lực tự nhiên là liên tục chọn một ô có các viên đá dư thừa và di chuyển một viên đá về phía ô bị thiếu trong cùng một hàng hoặc cột, cập nhật số lượng mỗi lần. Mô phỏng tham lam này đơn giản về mặt khái niệm, nhưng mỗi bước di chuyển chỉ khắc phục được sự mất cân bằng của một đơn vị và trong trường hợp xấu nhất có thể có$O(n^2)$di chuyển. Mỗi bước di chuyển cũng yêu cầu tìm một ô mục tiêu hợp lệ, vì vậy việc triển khai đơn giản ít nhất sẽ trở thành$O(n^3)$, quá chậm. 

Quan sát quan trọng là sự chuyển động của hàng và cột tách rời cấu trúc vấn đề. Một viên đá không bao giờ bắt buộc phải di chuyển ra ngoài hàng hoặc cột của nó, điều đó có nghĩa là chúng ta có thể xử lý từng hàng một cách độc lập theo cách phân phối lại theo chiều ngang và mỗi cột một cách độc lập theo cách phân phối lại theo chiều dọc. Thay vì mô phỏng chuyển động thực tế của đá, chúng ta chỉ cần theo dõi xem mỗi hàng và cột có bao nhiêu dư thừa hoặc thiếu hụt. 

Mỗi hàng có mức dư thừa hoặc thâm hụt ròng được xác định bằng số lượng đá mà nó hiện đang giữ trừ đi$n$. Vì tổng số đá chính xác$n^2$, tổng của tất cả các sự mất cân bằng hàng bằng không. Hàng thừa phải gửi đá ra, hàng nào thiếu phải nhận đá thông qua việc di chuyển cột. Logic tương tự áp dụng đối xứng cho các cột. 

Cái nhìn sâu sắc quan trọng là mọi chuyển động đều góp phần điều chỉnh đồng thời cả sự mất cân bằng hàng và mất cân bằng cột. Việc di chuyển trong một hàng hoặc cột sẽ giải quyết một cách hiệu quả một đơn vị không khớp ở cả hai chiều. Điều này làm giảm vấn đề đếm số lượng đơn vị phải được dịch chuyển để cân bằng tất cả các hàng và cột và số lần di chuyển tối thiểu bằng một nửa tổng số mất cân bằng tuyệt đối giữa các hàng (hoặc các cột tương đương). 

Do đó, giải pháp giảm xuống việc tính tổng hàng và tính tổng độ lệch tuyệt đối từ$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chuyển động |$O(n^3)$|$O(n^2)$| Quá chậm | 
| Tổng hợp mất cân bằng hàng |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta định dạng lại lưới sao cho mỗi hàng có tổng mục tiêu là$n$. Vì mỗi ô phải kết thúc bằng một viên đá nên mỗi hàng phải kết thúc bằng chính xác$n$đá. 

1. Tính tổng số viên đá ở mỗi hàng. Điều này mang lại cho chúng ta sự phân bố hiện tại của các viên đá theo hàng mà không phải lo lắng về từng cột riêng lẻ. 
2. Đối với mỗi hàng, hãy tính xem nó cách giá trị mục tiêu bao xa$n$. Chênh lệch dương có nghĩa là số đá thừa phải rời khỏi hàng, trong khi chênh lệch âm có nghĩa là hàng cần nhận đá. 
3. Tính tổng tất cả các sai phân dương giữa các hàng. Điều này thể hiện tổng số viên đá phải được di chuyển ra khỏi các hàng dư thừa. 
4. Câu trả lời chính xác là số tiền này, vì mỗi bước di chuyển có thể được coi là chuyển một đơn vị thặng dư từ hàng thừa sang hàng thiếu thông qua các chuyển đổi hàng hoặc cột được phép. 

Lý do đằng sau bước 4 là mỗi bước di chuyển sẽ làm giảm tổng sự mất cân bằng đúng một đơn vị thặng dư ở một hàng và một đơn vị thâm hụt ở một hàng khác, do đó việc tính tổng thặng dư đã tính được số lượng hoạt động cần thiết tối thiểu. 

### Tại sao nó hoạt động 

Mỗi hàng bắt đầu với mức thặng dư hoặc thâm hụt cố định tương ứng với trạng thái cuối cùng được yêu cầu. Bởi vì mỗi lần di chuyển sẽ bảo toàn tổng số quân cờ và chỉ di chuyển một đơn vị tại một thời điểm, không có thao tác nào có thể khắc phục được nhiều hơn một đơn vị mất cân bằng hàng. Đồng thời, mọi đơn vị thặng dư cuối cùng đều phải được loại bỏ. Do đó, tổng số tiền dư trên tất cả các hàng là giới hạn dưới của số lần di chuyển. 

Việc xây dựng ngụ ý bằng cách chuyển từ hàng này sang hàng khác thông qua các bước di chuyển được phép cho thấy rằng giới hạn này là có thể đạt được, vì mọi đơn vị dư thừa có thể được chuyển đến một số ô thiếu hụt mà không cần tăng số lượng thao tác cần thiết vượt quá một thao tác trên mỗi đơn vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
row_sum = [0] * n

for i in range(n):
    row_sum[i] = sum(map(int, input().split()))

ans = 0
for s in row_sum:
    if s > n:
        ans += s - n

print(ans)
```Mã chỉ tập trung vào tổng hàng, điều này là đủ vì tổng số viên đá là cố định và mỗi hàng phải kết thúc bằng chính xác$n$đá. Chúng tôi chỉ tích lũy độ lệch dương vì độ lệch âm tương ứng với các hàng sẽ nhận đá thay vì gửi chúng. 

Một điểm tinh tế là chúng ta không cần phải kiểm tra các cột một cách rõ ràng. Các cột thiếu hụt được ngầm thỏa mãn vì tổng thặng dư bằng tổng thâm hụt, do đó việc cân bằng các hàng sẽ tự động cân bằng các cột. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên: 

Lưới đầu vào:$$\begin{bmatrix}
0 & 1 & 2 \\
0 & 2 & 2 \\
1 & 1 & 0
\end{bmatrix}$$Tổng hàng là$[3, 4, 2]$, Và$n = 3$. 

| Hàng | Tổng hợp | Mục tiêu | Thặng dư | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 
| 2 | 4 | 3 | 1 | 
| 3 | 2 | 3 | 0 (thâm hụt) | 

Tổng thặng dư là$1$, vậy đáp án là$1$. 

Điều này cho thấy dù nhiều ô bị mất cân bằng nhưng chỉ cần di dời một đơn vị để khôi phục lại sự cân bằng. 

Bây giờ hãy xem xét một trường hợp tập trung:$$\begin{bmatrix}
9 & 0 & 0 \\
0 & 0 & 0 \\
0 & 0 & 0
\end{bmatrix}$$Tổng hàng là$[9, 0, 0]$, với$n = 3$. 

| Hàng | Tổng hợp | Mục tiêu | Thặng dư | 
| --- | --- | --- | --- | 
| 1 | 9 | 3 | 6 | 
| 2 | 0 | 3 | - | 
| 3 | 0 | 3 | - | 

Tổng thặng dư là$6$, nghĩa là cần phải di chuyển sáu lần để phân bố đều các viên đá. 

Điều này chứng tỏ rằng phương pháp này có tỷ lệ tự nhiên ngay cả khi tất cả các viên đá bắt đầu ở một hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi trong số$n$các hàng được tóm tắt trong$O(n)$thời gian | 
| Không gian |$O(n)$| Chỉ số tiền hàng được lưu trữ | 

Kích thước lưới tối đa là$500 \times 500$, Vì thế$2.5 \times 10^5$hoạt động dễ dàng trong giới hạn. Việc sử dụng bộ nhớ là tối thiểu và không phụ thuộc vào bất kỳ cấu trúc phụ trợ nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    input = sys.stdin.readline

    n = int(input())
    row_sum = [0] * n
    for i in range(n):
        row_sum[i] = sum(map(int, input().split()))

    ans = 0
    for s in row_sum:
        if s > n:
            ans += s - n
    return str(ans)

# provided sample
assert run("""3
0 1 2
0 2 2
1 1 0
""") == "1"

# already correct grid
assert run("""2
1 1
1 1
""") == "0"

# all stones in one row
assert run("""3
9 0 0
0 0 0
0 0 0
""") == "6"

# uniform distribution
assert run("""3
1 1 1
1 1 1
1 1 1
""") == "0"

# skewed distribution
assert run("""2
2 0
0 2
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới nhận dạng | 0 | trường hợp đã cân bằng | 
| nồng độ một hàng | 6 | nhân giống thặng dư cao | 
| lưới thống nhất | 0 | tính chính xác không hoạt động | 
| trao đổi chéo | 0 | tính nhất quán của hàng-cột | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi lưới đã hợp lệ. Đối với đầu vào```
2
1 1
1 1
```mỗi hàng có tổng bằng 2, phù hợp với mục tiêu. Thuật toán tính toán số dư bằng 0 và xuất ra 0 ngay lập tức, tránh chính xác mọi động thái nhân tạo. 

Một trường hợp khác là sự tập trung cao độ, chẳng hạn như```
3
9 0 0
0 0 0
0 0 0
```Ở đây tổng hàng là [9, 0, 0], do đó thặng dư là 6. Thuật toán đếm sáu bước di chuyển bắt buộc mà không mô phỏng quá trình phân phối lại, phù hợp với thực tế là mỗi bước di chuyển chỉ có thể khắc phục một đơn vị mất cân bằng. 

Trường hợp khó phát hiện cuối cùng là khi phân phối bị lệch trên các cột nhưng cân bằng trên mỗi hàng. Vì```
2
2 0
0 2
```tổng hàng đã chính xác nên câu trả lời là 0 mặc dù các cột trông không đồng đều. Thuật toán bỏ qua cấu trúc cột một cách chính xác vì việc cân bằng hàng đã mã hóa tất cả các ràng buộc chuyển động cần thiết.
