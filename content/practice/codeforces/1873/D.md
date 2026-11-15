---
title: "CF 1873D - Tẩy 1D"
description: "Chúng ta được cung cấp một chuỗi nhị phân đại diện cho một dải ô, trong đó mỗi vị trí có màu đen hoặc trắng. Thao tác duy nhất được phép là chọn bất kỳ đoạn liền kề nào có độ dài cố định $k$ và sơn lại tất cả các ô trong đoạn đó thành màu trắng."
date: "2026-06-08T23:14:17+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1873
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 898 (Div. 4)"
rating: 800
weight: 1873
solve_time_s: 92
verified: true
draft: false
---

[CF 1873D - Cục tẩy 1D](https://codeforces.com/problemset/problem/1873/D) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, hai con trỏ 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân đại diện cho một dải ô, trong đó mỗi vị trí có màu đen hoặc trắng. Thao tác duy nhất được phép là chọn bất kỳ đoạn liền kề nào có độ dài cố định$k$và sơn lại tất cả các ô trong đoạn đó thành màu trắng. Mục tiêu là loại bỏ mọi ô đen bằng cách sử dụng càng ít thao tác sơn lại phân đoạn như vậy càng tốt. 

Quan sát chính là mỗi thao tác ảnh hưởng đến một cửa sổ có kích thước liền kề$k$và nhiều ô đen bên trong cửa sổ đó có thể bị xóa đồng thời. Tuy nhiên, các thao tác tương tác thông qua sự chồng chéo, vì vậy vấn đề không phải là đếm các ô đen mà là làm thế nào để chúng ta có thể đặt những “cục tẩy” có độ dài cố định này một cách tối ưu để bao phủ tất cả các vị trí màu đen. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số ký tự trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các cửa sổ có thể có cho mỗi ô đen hoặc mô phỏng từng thao tác một cách nguyên bản trên chuỗi theo thời gian bậc hai. MỘT$O(n^2)$Cách tiếp cận này sẽ liên quan đến việc quét hoặc cập nhật các phân đoạn chồng chéo nhiều lần và sẽ không hoàn thành trong thời gian giới hạn. 

Một vài trường hợp phức tạp xuất hiện trong bài toán này. 

Nếu như$k = 1$, mỗi thao tác chỉ xóa một ô nên đáp án chính xác là số ô đen. Ý tưởng cửa sổ trượt ngây thơ có thể làm phức tạp thêm trường hợp này. 

Nếu chuỗi không có ô đen thì câu trả lời là 0. Bất kỳ thuật toán nào giả định cần ít nhất một thao tác sẽ tạo ra giá trị dương không chính xác. 

Nếu các ô màu đen xuất hiện liên tiếp trong các khối dài, phương pháp tham lam phải đảm bảo chúng ta không đếm quá nhiều thao tác bằng cách áp dụng các thao tác xóa chồng chéo một cách không cần thiết. 

Ví dụ, trong chuỗi`BBBW`với$k = 3$, một thao tác ở vị trí 0 sẽ xóa cả ba ô đen. Một cách tiếp cận bất cẩn xử lý từng ô đen một cách độc lập có thể áp dụng nhiều thao tác chồng chéo và đếm quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi chuỗi hoạt động có thể xảy ra. Ở mỗi bước, chúng ta có thể thử chọn bất kỳ cửa sổ có kích thước nào$k$bao phủ ít nhất một ô đen còn lại, áp dụng nó và lặp lại. Mặc dù điều này đúng về nguyên tắc nhưng hệ số phân nhánh lớn và không gian trạng thái tăng theo cấp số nhân. Ngay cả một biến thể tham lam liên tục quét tìm cửa sổ tốt nhất và cập nhật chuỗi cũng sẽ giảm xuống$O(n^2)$trong trường hợp xấu nhất là mỗi thao tác chỉ loại bỏ được một số lượng nhỏ ô đen mới. 

Cấu trúc của hoạt động gợi ý một chiến lược trực tiếp hơn. Khi chúng ta gặp một ô đen, quyết định có ý nghĩa duy nhất là áp dụng một thao tác bao phủ ô đó. Vì bất kỳ thao tác nào cũng ảnh hưởng đến toàn bộ đoạn chiều dài$k$, lựa chọn tối ưu là sử dụng ô màu đen ngoài cùng bên trái làm điểm neo và xóa đoạn bắt đầu từ đó. Bất kỳ hoạt động nào bắt đầu sớm hơn hoặc muộn hơn đều không bao phủ được ô này hoặc lãng phí phạm vi phủ sóng trên các vùng vốn đã sạch. 

Điều này dẫn đến một thao tác quét tham lam đơn giản: chúng ta di chuyển từ trái sang phải và bất cứ khi nào chúng ta tìm thấy một ô đen chưa được thao tác trước đó che phủ, chúng ta sẽ áp dụng một thao tác bắt đầu từ vị trí đó. Thao tác này ngay lập tức xóa ô đó và tất cả các ô màu đen trong ô tiếp theo$k-1$vị trí, vì vậy chúng ta có thể bỏ qua phía trước một cách an toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Quét tham lam |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải trong khi vẫn duy trì khoảng cách kéo dài của thao tác được áp dụng cuối cùng. 

1. Khởi tạo con trỏ$i = 0$và một biến$covered = -1$, theo dõi chỉ mục ngoài cùng bên phải được làm sạch bởi các thao tác trước đó. 
2. Quét chuỗi từ trái sang phải. 
3. Nếu$i \leq covered$, ô hiện tại đã có màu trắng do thao tác trước đó nên chúng ta chuyển sang chỉ mục tiếp theo. 
4. Nếu$s[i] = 'B'$, đây là một ô màu đen không được che phủ, vì vậy chúng ta phải áp dụng một thao tác mới bắt đầu từ$i$. 
5. Tăng bộ đếm câu trả lời và đặt$covered = i + k - 1$, đại diện cho toàn bộ phạm vi được làm sạch bằng thao tác này. 
6. Tiếp tục quét. 

Mỗi thao tác bị ép buộc ở ô đen chưa được phát hiện đầu tiên vì việc trì hoãn hoặc dịch chuyển nó sang phải sẽ khiến vị trí này không được xử lý. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là mọi thao tác đều bao gồm một khoảng thời gian có độ dài cố định. Khi chúng tôi quyết định che ô đen không được che chắn ngoài cùng bên trái ở vị trí$i$, bất kỳ giải pháp tối ưu nào cũng phải bao gồm ít nhất một thao tác bao gồm$i$. Trong số tất cả các hoạt động như vậy, bắt đầu từ$i$không bao giờ tệ hơn việc bắt đầu muộn hơn vì nó tối đa hóa phạm vi phủ sóng ở bên phải trong khi vẫn sửa lỗi$i$. Điều này tạo ra thuộc tính lựa chọn tham lam: khi chúng tôi sửa ô đen chưa được phát hiện đầu tiên, chúng tôi sẽ giảm vấn đề xuống một hậu tố nhỏ hơn hoàn toàn, độc lập với các quyết định trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()

    ans = 0
    covered = -1

    for i in range(n):
        if i <= covered:
            continue
        if s[i] == 'B':
            ans += 1
            covered = i + k - 1

    print(ans)
```Chi tiết triển khai chính là`covered`con trỏ, tránh việc sửa đổi chuỗi một cách rõ ràng. Thay vì mô phỏng việc sơn lại, chúng tôi ghi lại thao tác cuối cùng kéo dài bao xa. Điều này ngăn cản sự không cần thiết$O(k)$cập nhật cho mỗi thao tác và giữ cho giải pháp tuyến tính. 

điều kiện`i <= covered`đảm bảo rằng chúng tôi không bao giờ xem xét lại các vị trí đã được xử lý bởi cục tẩy trước đó. Việc kiểm tra cho`'B'`đảm bảo chúng tôi chỉ kích hoạt hoạt động khi cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n = 6, k = 3, s = WBWWWB`Chúng tôi theo dõi quá trình quét từng bước. 

| tôi | s[i] | được bảo hiểm | Hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | W | -1 | bỏ qua | 0 | 
| 1 | B | -1 | áp dụng, được bảo hiểm = 3 | 1 | 
| 2 | W | 3 | bỏ qua | 1 | 
| 3 | W | 3 | bỏ qua | 1 | 
| 4 | W | 3 | bỏ qua | 1 | 
| 5 | B | 3 | áp dụng, được bảo hiểm = 7 | 2 | 

Điều này cho thấy rằng một thao tác duy nhất có thể bao gồm nhiều ô đen gần đó và chỉ những ô chưa được phát hiện mới kích hoạt các hoạt động mới. 

### Ví dụ 2 

đầu vào:`n = 5, k = 4, s = BWBWB`| tôi | s[i] | được bảo hiểm | Hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | B | -1 | áp dụng, được bảo hiểm = 3 | 1 | 
| 1 | W | 3 | bỏ qua | 1 | 
| 2 | B | 3 | bỏ qua (được bảo hiểm) | 1 | 
| 3 | W | 3 | bỏ qua | 1 | 
| 4 | B | 3 | áp dụng, được bảo hiểm = 7 | 2 | 

Ô màu đen thứ hai ở chỉ số 2 đã bị xóa bởi thao tác đầu tiên, mặc dù nó không được kiểm tra rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí được truy cập một lần và mỗi thao tác có thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó, quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ và phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        s = sys.stdin.readline().strip()

        ans = 0
        covered = -1

        for i in range(n):
            if i <= covered:
                continue
            if s[i] == 'B':
                ans += 1
                covered = i + k - 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""8
6 3
WBWWWB
7 3
WWBWBWW
5 4
BWBWB
5 5
BBBBB
8 2
BWBWBBBB
10 2
WBBWBBWBBW
4 1
BBBB
3 2
WWW
""") == """2
1
2
1
4
3
4
0"""

# custom cases
assert run("""1
1 1
B
""") == "1", "single cell"

assert run("""1
5 5
BBBBB
""") == "1", "whole covered in one move"

assert run("""1
6 2
BBBBBB
""") == "3", "tight packing"

assert run("""1
6 3
WBWBWB
""") == "2", "alternating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`B, k=1`| 1 | hoạt động đơn bào tối thiểu | 
| tất cả B, k=n | 1 | bảo hiểm đầy đủ trong một lần di chuyển | 
| khối dày đặc, k nhỏ | 3 | hành vi đóng gói đúng | 
| mô hình xen kẽ | 2 | tính đúng đắn tham lam không chồng chéo | 

## Vỏ cạnh 

Khi nào$k = 1$, thuật toán tự nhiên xử lý từng ô đen theo yêu cầu hoạt động riêng của nó. Mỗi cái không được che chắn`'B'`tăng câu trả lời và đặt`covered = i`, do đó không xảy ra lỗi bỏ qua. 

Khi không có ô đen, vòng lặp không bao giờ kích hoạt thao tác vì điều kiện`s[i] == 'B'`không bao giờ được thỏa mãn. Kết quả vẫn bằng không. 

Khi các ô màu đen chồng lên nhau nhiều trong một khoảng cách nhỏ hơn$k$, ô đen gặp đầu tiên sẽ kích hoạt một thao tác tự động bao phủ tất cả chúng. Ví dụ,`BBBW`với$k = 3$bộ`covered = 2`, bỏ qua phần còn lại mà không cần thực hiện thêm thao tác nào phù hợp với chiến lược tối ưu.
