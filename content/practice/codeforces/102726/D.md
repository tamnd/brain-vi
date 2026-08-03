---
title: "CF 102726D - Cụm thu phóng"
description: "Sự cố mô hình hóa một hàng người trong cuộc gọi điện video. Sau khi mọi người quay đầu lại, mỗi người được đại diện bởi một ký tự: L nếu quay mặt sang trái và R nếu quay mặt sang phải. Một nhóm là một nhóm người liên tiếp tối đa quay về cùng một hướng."
date: "2026-08-01T22:06:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102726
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 1"
rating: 0
weight: 102726
solve_time_s: 94
verified: true
draft: false
---

[CF 102726D - Cụm thu phóng](https://codeforces.com/problemset/problem/102726/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Sự cố mô hình hóa một hàng người trong cuộc gọi điện video. Sau khi mọi người quay đầu lại, mỗi người được đại diện bởi một nhân vật:`L`nếu họ quay mặt sang trái và`R`nếu họ đối mặt đúng. Một nhóm là một nhóm người liên tiếp tối đa quay về cùng một hướng. Nhiệm vụ là đếm xem có bao nhiêu nhóm như vậy ở hàng cuối cùng. 

Ví dụ, hàng`R L L R`chứa ba cụm: cụm đầu tiên`R`, ở giữa`LL`, và cuối cùng`R`. 

Dữ liệu đầu vào chứa số lượng người được theo dõi theo chỉ dẫn của họ theo thứ tự. Đầu ra là số lượng các phân đoạn liền kề trong chuỗi này trong đó mọi ký tự bên trong một phân đoạn đều giống hệt nhau. 

Hạn chế cho phép lên tới 100.000 người. Điều đó có nghĩa là giải pháp sẽ hoạt động theo thời gian tuyến tính. Các thuật toán liên tục so sánh nhiều cặp vị trí hoặc thử mọi phân đoạn có thể có thể dễ dàng đạt được khoảng 10^10 thao tác, vượt xa những gì có thể có trong giới hạn thời gian của cuộc thi thông thường. Một lần quét qua hàng là đủ vì mỗi người chỉ cần tác động đến câu trả lời một lần. 

Các trường hợp cạnh chính đến từ sự chuyển tiếp giữa các cụm. Một hàng chỉ chứa một người vẫn phải tạo một cụm. Đối với đầu vào:```
1
L
```đầu ra đúng là:```
1
```Một giải pháp chỉ đếm các thay đổi giữa những người lân cận sẽ trả về 0 vì không có chuyển tiếp nào, nhưng cụm đầu tiên phải được tính. 

Một sai lầm phổ biến khác là quên rằng các hướng liền kề bằng nhau thuộc cùng một cụm. Vì:```
4
L
L
L
L
```đầu ra đúng là:```
1
```Toàn bộ hàng là một cụm. Đếm mọi lần xuất hiện của`L`hoặc`R`riêng biệt sẽ tạo ra bốn không chính xác. 

Trường hợp ranh giới cuối cùng là khi mọi cặp liền kề đều khác nhau:```
4
L
R
L
R
```Đầu ra đúng là:```
4
```Mỗi người bắt đầu một nhóm mới nên đáp án là số lượng người. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là kiểm tra mọi nhóm liền kề có thể và xác định xem đó có phải là một cụm hay không. Vì có thể có O(N^2) phân đoạn và việc kiểm tra một phân đoạn có thể mất O(N), nên tổng công việc có thể đạt tới O(N^3). Ngay cả một lực lượng vũ phu được cải thiện một chút chỉ kiểm tra các vị trí lân cận cho từng phân đoạn vẫn xem xét phạm vi O(N^2), trở nên quá chậm khi N đạt 100.000. 

Quan sát quan trọng là các cụm được phân tách chính xác tại các vị trí có hướng thay đổi. Chúng ta không cần biết nội dung của toàn bộ phân khúc. Chúng ta chỉ cần biết người hiện tại tiếp tục nhóm trước hay bắt đầu nhóm mới. 

Hàng luôn chứa ít nhất một cụm. Chúng tôi bắt đầu với một cụm cho người đầu tiên, sau đó tăng câu trả lời bất cứ khi nào một người quay mặt về hướng khác với người ngay trước họ. Điều này làm giảm vấn đề thành một lần quét từ trái sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hướng dẫn của tất cả mọi người theo thứ tự ban đầu của họ. 
2. Khởi tạo câu trả lời là 1 vì người đầu tiên luôn thuộc một nhóm. 
3. Quét hàng từ người thứ hai trở đi. Bất cứ khi nào hướng hiện tại khác với hướng trước đó, hãy tăng số lượng cụm. Sự thay đổi hướng là sự kiện duy nhất có thể tạo ra một cụm mới. 
4. Xuất số đếm cuối cùng. 

Tại sao nó hoạt động: 

Trong quá trình quét, câu trả lời hiện tại thể hiện chính xác số nhóm tối đa trong số những người được xử lý cho đến nay. Người đầu tiên tạo nhóm đầu tiên. Mỗi người sau sẽ tham gia nhóm trước khi hướng của họ phù hợp hoặc bắt đầu một nhóm mới khi hướng của họ thay đổi. Vì mọi ranh giới có thể có giữa các cụm đều được kiểm tra chính xác một lần nên số đếm cuối cùng là chính xác. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())
    prev = input().strip()
    
    ans = 1

    for _ in range(n - 1):
        cur = input().strip()
        if cur != prev:
            ans += 1
        prev = cur

    print(ans)

if __name__ == "__main__":
    solve()
```Mã chỉ giữ hướng trước đó thay vì lưu trữ toàn bộ hàng. Điều này là đủ vì một nhóm mới chỉ có thể được tạo ra bằng cách so sánh hai người liền kề. 

Biến`ans`bắt đầu từ một vì đầu vào được đảm bảo chứa ít nhất một người. Vòng lặp bắt đầu từ ngôi thứ hai và mỗi lần thay đổi hướng sẽ làm tăng câu trả lời. 

sử dụng`strip()`xóa ký tự dòng mới khỏi mỗi dòng đầu vào. Mã này tránh việc sử dụng bộ nhớ không cần thiết bằng cách xử lý các hướng dẫn khi chúng được đọc. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
4
R
L
L
R
```việc thực hiện là: 

| Người | Hướng | Hướng trước đó | Khối | 
| --- | --- | --- | --- | 
| 1 | R | R | 1 | 
| 2 | L | R | 2 | 
| 3 | L | L | 2 | 
| 4 | R | L | 3 | 

Câu trả lời là`3`. Dấu vết cho thấy chỉ những thay đổi giữa những người liền kề mới ảnh hưởng đến số lượng. 

Một ví dụ khác:```
5
L
L
R
R
L
```| Người | Hướng | Hướng trước đó | Khối | 
| --- | --- | --- | --- | 
| 1 | L | L | 1 | 
| 2 | L | L | 1 | 
| 3 | R | L | 2 | 
| 4 | R | R | 2 | 
| 5 | L | R | 3 | 

Ba cụm là`LL`,`RR`, Và`L`. Những người hàng xóm bình đẳng không tạo thêm nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi người được xử lý một lần. | 
| Không gian | O(1) | Chỉ hướng trước đó và số lượng hiện tại được lưu trữ. | 

Giải pháp tuyến tính dễ dàng xử lý 100.000 người vì nó thực hiện lượng công việc không đổi trên mỗi ký tự đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())
    prev = input().strip()
    ans = 1

    for _ in range(n - 1):
        cur = input().strip()
        if cur != prev:
            ans += 1
        prev = cur

    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return output

assert run("4\nR\nL\nL\nR\n") == "3\n", "sample 1"
assert run("1\nL\n") == "1\n", "single person"
assert run("5\nL\nL\nL\nL\nL\n") == "1\n", "all equal values"
assert run("4\nL\nR\nL\nR\n") == "4\n", "alternating directions"
assert run("6\nR\nR\nL\nL\nR\nR\n") == "3\n", "multiple boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 L`|`1`| Kích thước tối thiểu và xử lý số lượng ban đầu | 
|`L L L L L`|`1`| Tất cả các giá trị bằng nhau | 
|`L R L R`|`4`| Mỗi người tạo một cụm mới | 
|`R R L L R R`|`3`| Xử lý đúng một số chuyển đổi | 

## Vỏ cạnh 

Đối với một người độc thân:```
1
L
```thuật toán khởi tạo`ans`thành một và không thực hiện so sánh. Đầu ra là`1`, điều này phù hợp với thực tế là người duy nhất tự mình tạo thành một cụm hoàn chỉnh. 

Đối với một hàng hoàn toàn bằng nhau:```
4
L
L
L
L
```mỗi so sánh đều tìm ra hướng như trước nên đáp án không bao giờ tăng lên. Đầu ra cuối cùng là`1`, coi toàn bộ hàng là một cụm một cách chính xác. 

Đối với các hướng luân phiên:```
4
L
R
L
R
```mọi so sánh đều phát hiện ra một sự thay đổi. Câu trả lời bắt đầu từ một và tăng thêm ba lần nữa, tạo ra`4`. Mỗi người tách biệt với người trước nên mỗi người tạo thành một cụm riêng.
