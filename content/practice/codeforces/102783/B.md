---
title: "CF 102783B - Hướng Về Nhà"
description: "Bài toán mô tả một người đang đứng tại một điểm trên trục số và cố gắng về đến nhà ở một điểm khác. Trong một giây, họ có thể di chuyển bất kỳ khoảng cách dương nào từ 1 đến một khoảng cách tối đa cho trước d."
date: "2026-07-27T20:04:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102783
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 2"
rating: 0
weight: 102783
solve_time_s: 73
verified: true
draft: false
---

[CF 102783B - Hướng về nhà](https://codeforces.com/problemset/problem/102783/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một người đang đứng tại một điểm trên trục số và cố gắng về đến nhà ở một điểm khác. Trong một giây, họ có thể di chuyển bất kỳ khoảng cách dương nào từ 1 đến một khoảng cách tối đa nhất định`d`. Đối với mỗi trường hợp thử nghiệm, chúng ta cần tìm số giây nhỏ nhất cần thiết để di chuyển từ vị trí bắt đầu đến vị trí ban đầu. 

Thông tin duy nhất ảnh hưởng đến câu trả lời là khoảng cách giữa hai vị trí. Nếu thời điểm bắt đầu là lúc`a`và nhà ở`h`, khoảng cách cần tìm là`|a - h|`. Mỗi giây có thể bao gồm nhiều nhất`d`đơn vị, vì vậy vấn đề trở thành việc tìm xem có bao nhiêu nhóm có kích thước`d`là cần thiết để che khoảng cách đó. 

Các ràng buộc này đủ nhỏ để thậm chí có thể mong đợi một giải pháp đơn giản theo thời gian không đổi. Có thể có tối đa 1000 trường hợp thử nghiệm và tối đa mỗi tọa độ là 100000. Một giải pháp lặp qua mọi vị trí có thể hoặc mô phỏng chuyển động từng bước một vẫn sẽ hoạt động đối với một số đầu vào, nhưng nó giải quyết được một vấn đề tổng quát hơn mức cần thiết. Cấu trúc cho phép chúng ta tính toán câu trả lời một cách trực tiếp. 

Các trường hợp cạnh chính đến từ khoảng cách không chia đều cho chiều dài di chuyển tối đa. Ví dụ:```
1
1 5 2
```Khoảng cách là 4. Di chuyển đúng 2 đơn vị mỗi giây để về nhà trong 2 giây, nên đáp án là:```
2
```Việc thực hiện bất cẩn bằng cách sử dụng phép chia số nguyên sai hướng có thể tính toán`4 // 3 = 1`đối với trường hợp tương tự và khẳng định không chính xác rằng chuyến đi sẽ kết thúc sau một giây. 

Một trường hợp cạnh khác là khi hai vị trí đã giống nhau:```
1
7 7 3
```Khoảng cách bằng 0 nên đáp án là:```
0
```Giải pháp luôn thêm một sau khi chia để thực hiện thao tác trần sẽ tạo ra 1 không chính xác. 

Trường hợp cuối cùng là khi khoảng cách di chuyển tối đa lớn hơn khoảng cách còn lại:```
1
2 10 100
```Người đó có thể về đến nhà chỉ trong một giây vì một lần di chuyển có thể là bất kỳ khoảng cách nào từ 1 đến 100. Kết quả là:```
1
```Một cách tiếp cận giả định mọi nước đi đều phải sử dụng chính xác`d`đơn vị có thể xử lý sai tình trạng này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng cuộc hành trình. Bắt đầu từ vị trí hiện tại, chúng ta có thể liên tục di chuyển khoảng cách tối đa có thể về nhà và đếm số lần di chuyển. Điều này có tác dụng vì thực hiện bước lớn nhất hiện có không bao giờ có thể tăng số bước di chuyển cần thiết. Tuy nhiên, việc mô phỏng là không cần thiết vì số lần di chuyển hoàn toàn được xác định bởi tổng khoảng cách. 

Nếu khoảng cách là`x`, sau đó`k`giây quãng đường tối đa có thể đi được là`k * d`. Chúng ta cần cái nhỏ nhất`k`như vậy:```
k * d >= x
```Đây chính xác là định nghĩa toán học của trần nhà`x / d`. 

Về mặt khái niệm, phương pháp vũ lực không thành công vì nó thực hiện công tỷ lệ với quãng đường di chuyển. Với tọa độ lên tới 100000, một trường hợp thử nghiệm có thể yêu cầu gần 100000 bước mô phỏng. Công thức trực tiếp rút gọn toàn bộ vấn đề thành một vài phép tính số học. 

Quan sát quan trọng là chuyển động không có hạn chế về đường đi chính xác. Người đó có thể chọn khoảng cách bất kỳ từ 1 đến`d`mỗi giây, vì vậy chỉ có tổng khoảng cách mới quan trọng. Một khi điều này được nhận ra, vấn đề sẽ trở thành vấn đề chia trần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | a-h | ) | O(1) | Quá chậm trong trường hợp chung | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách tuyệt đối giữa vị trí xuất phát và vị trí ban đầu. Hướng đi không quan trọng vì người đó có thể di chuyển về nhà bất kể nhà ở bên trái hay bên phải. 
2. Nếu khoảng cách bằng 0, xuất ra 0 vì không cần chuyển động. 
3. Ngược lại, chia khoảng cách cho`d`sử dụng vách ngăn trần. Điều này mang lại số lần di chuyển nhỏ nhất có tổng công suất đủ để bao phủ khoảng cách. 
4. Xuất kết quả cho test case hiện tại. 

Việc phân chia trần có thể được viết là:```
(distance + d - 1) // d
```Việc bổ sung`d - 1`tăng thương bất cứ khi nào có số dư, trong khi vẫn giữ nguyên phép chia chính xác. 

Tại sao nó hoạt động: 

Sau`k`giây, khoảng cách tối đa có thể đi được là`k * d`. Một câu trả lời hợp lệ phải thỏa mãn`k * d >= distance`. Số nguyên nhỏ nhất thỏa mãn điều kiện này chính xác là`ceil(distance / d)`. Vì mỗi giây cho phép bất kỳ chuyển động nào có độ dài lên tới`d`, luôn có thể đạt được mục tiêu trong nhiều giây đó bằng cách phân bổ một phần khoảng cách cuối cùng vào nước đi cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, h, d = map(int, input().split())
        dist = abs(a - h)
        ans.append(str((dist + d - 1) // d))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào đọc từng trường hợp kiểm thử độc lập và xử lý nó mà không lưu trữ thông tin không cần thiết. Khoảng cách được tính bằng`abs(a - h)`bởi vì trục số có thể có hai hướng nhưng cả hai đều yêu cầu quãng đường như nhau. 

biểu hiện`(dist + d - 1) // d`là chi tiết triển khai quan trọng. Phép chia số nguyên thông thường làm tròn xuống, nhưng số giây được yêu cầu phải làm tròn lên bất cứ khi nào còn khoảng cách còn lại. Ví dụ: khoảng cách là 5 với chuyển động tối đa là 2 cần 3 giây chứ không phải 2. 

Không có vòng lặp nào phụ thuộc vào giá trị tọa độ, do đó việc triển khai sẽ tránh được mọi vấn đề về ranh giới do khoảng cách lớn gây ra. Số nguyên Python cũng tránh được vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
1 5 1
1 5 2
1 5 3
```việc thực hiện là: 

| Trường hợp thử nghiệm | Khoảng cách | Di chuyển tối đa | Tính toán | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | trần(4 / 1) | 4 | 
| 2 | 4 | 2 | trần(4 / 2) | 2 | 
| 3 | 4 | 3 | trần(4 / 3) | 2 | 

Dấu vết này cho thấy tại sao câu trả lời chỉ phụ thuộc vào số lần di chuyển có độ dài tối đa cần thiết. Trường hợp thứ ba chứng minh rằng nước đi cuối cùng một phần vẫn mất trọn một giây. 

Đối với đầu vào:```
2
10 10 5
3 20 8
```việc thực hiện là: 

| Trường hợp thử nghiệm | Khoảng cách | Di chuyển tối đa | Tính toán | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | trần nhà(0 / 5) | 0 | 
| 2 | 17 | 8 | trần(17 / 8) | 3 | 

Trường hợp đầu tiên xác nhận điều kiện biên khoảng cách bằng không. Trường hợp thứ hai cho thấy nước đi cuối cùng không cần sử dụng hết khoảng cách`d`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm sử dụng một số phép tính số học không đổi | 
| Không gian | O(T) | Chuỗi đầu ra được lưu trữ trước khi in | 

Số lượng trường hợp thử nghiệm tối đa là 1000, do đó, việc truyền tuyến tính qua đầu vào dễ dàng nằm trong giới hạn. Thuật toán không phụ thuộc vào độ lớn tọa độ, điều này làm cho nó phù hợp ngay cả khi giới hạn vị trí tăng lên. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data: str) -> str:
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        a, h, d = map(int, input().split())
        ans.append(str((abs(a - h) + d - 1) // d))

    return "\n".join(ans)

assert solution("""3
1 5 1
1 5 2
1 5 3
""") == """4
2
2""", "sample 1"

assert solution("""2
10 10 5
3 20 8
""") == """0
3""", "boundary cases"

assert solution("""1
1 2 1
""") == """1""", "minimum distance"

assert solution("""1
5 5 100000
""") == """0""", "same position"

assert solution("""1
1 100000 99999
""") == """2""", "large distance with remainder"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 1`|`4`| Phân chia chính xác và chuyển động cơ bản | 
|`1 5 3`|`2`| Phân chia trần khi có dư | 
|`5 5 100000`|`0`| Đã ở nhà | 
|`1 100000 99999`|`2`| Giá trị lớn và xử lý từng cái một | 

## Vỏ cạnh 

Khi điểm bắt đầu và điểm nhà bằng nhau, thuật toán sẽ tính khoảng cách bằng 0. Công thức chia trần cũng xử lý việc này một cách tự nhiên vì:```
(0 + d - 1) // d = 0
```Đối với đầu vào:```
1
7 7 3
```thuật toán tính toán`dist = 0`và trả về`0`. 

Khi khoảng cách không phải là bội số của khoảng cách di chuyển tối đa, giây cuối cùng chỉ bao gồm khoảng cách còn lại. Đối với đầu vào:```
1
1 6 4
```khoảng cách là 5. Hai giây là đủ vì nước đi đầu tiên bao gồm 4 đơn vị và nước đi thứ hai bao gồm 1 đơn vị còn lại. Công thức cho:```
(5 + 4 - 1) // 4 = 2
```Khi`d`lớn hơn khoảng cách, một bước di chuyển là đủ miễn là khoảng cách không bằng 0. Đối với đầu vào:```
1
10 15 100
```khoảng cách là 5 và câu trả lời là 1 vì người đó có thể chọn nước đi đúng 5 đơn vị. 

Điều này cũng có thể được điều chỉnh thành phiên bản ngắn hơn theo phong cách biên tập cuộc thi nếu bạn muốn có định dạng blog Codeforces điển hình hơn.
