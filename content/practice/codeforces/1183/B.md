---
title: "CF 1183B - Bình đẳng giá"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi giá đều có bảng giá sản phẩm, chúng ta được phép điều chỉnh từng mức giá tối đa k theo một trong hai hướng nhưng không được phép điều chỉnh nhiều lần."
date: "2026-06-13T11:39:02+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1183
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 570 (Div. 3)"
rating: 900
weight: 1183
solve_time_s: 580
verified: false
draft: false
---

[CF 1183B - Bình đẳng giá](https://codeforces.com/problemset/problem/1183/B) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 9 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi sản phẩm đều có bảng giá sản phẩm, chúng tôi được phép điều chỉnh tối đa từng mức giá.`k`theo một trong hai hướng, nhưng chúng tôi không được phép điều chỉnh nhiều lần. Sau những điều chỉnh này, chúng tôi muốn tất cả các sản phẩm đều có cùng một mức giá nguyên`B`. 

Điều kiện cho giá trị được chọn`B`đơn giản nhưng nghiêm ngặt: mọi giá gốc`a_i`phải có khả năng di chuyển đến`B`không vượt quá mức thay đổi cho phép, nghĩa là chênh lệch tuyệt đối`|a_i - B|`không được vượt quá`k`. Nếu thậm chí một sản phẩm không thể đạt được`B`, thì giá trị đó của`B`không hợp lệ. 

Trong số tất cả các giá trị nguyên hợp lệ của`B`, ta cần tìm giá trị lớn nhất Nếu không có số nguyên thỏa mãn điều kiện cho tất cả các phần tử, chúng ta xuất ra`-1`. 

Mỗi truy vấn là độc lập, do đó quá trình sẽ khởi động lại với một mảng mới mỗi lần. 

Các hạn chế là nhỏ:`n ≤ 100`,`q ≤ 100`. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài công việc tuyến tính hoặc log-tuyến tính cho mỗi truy vấn. Ngay cả cách tiếp cận O(n^2) cho mỗi truy vấn cũng có thể vượt qua một cách thoải mái. 

Trường hợp cạnh tinh tế xuất hiện khi các khoảng được xác định bởi các phần tử khác nhau hoàn toàn không trùng nhau. Ví dụ, nếu chúng ta có`a = [1, 100]`Và`k = 10`, thì phạm vi hợp lệ là`[−9, 11]`Và`[90, 110]`, không giao nhau nên không hợp lệ`B`tồn tại. 

Một trường hợp cạnh khác là khi giao điểm hợp lệ nhưng ranh giới tối đa không trực quan. Ví dụ: nếu tất cả các khoảng chồng chéo lên nhau nhiều thì câu trả lời chỉ đơn giản là điểm ngoài cùng bên phải của giao lộ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi số nguyên có thể`B`từ`1`lên đến`max(a_i) + k`và kiểm tra xem mọi phần tử có thỏa mãn không`|a_i - B| ≤ k`. Đối với mỗi ứng viên`B`, chúng tôi quét mảng, do đó độ phức tạp trở thành O(M·n), trong đó M là phạm vi các giá trị có thể có. Từ`a_i`có thể lên đến`10^8`, M cũng có thể lớn, khiến phương pháp này hoàn toàn không khả thi trong trường hợp xấu nhất. 

Quan sát quan trọng là mỗi phần tử`a_i`không cho phép các giá trị tùy ý của`B`, nhưng thay vào đó giới hạn nó trong một khoảng`[a_i - k, a_i + k]`. Điều kiện “tất cả các tích có thể bằng B” tương đương với việc yêu cầu B nằm trong giao điểm của tất cả các khoảng này. Khi chúng ta diễn giải bài toán theo cách này, nó sẽ trở thành bài toán giao khoảng tiêu chuẩn. 

Giao của nhiều khoảng chính là một khoảng`[L, R]`, Ở đâu`L`là mức tối đa của tất cả các điểm cuối bên trái và`R`là mức tối thiểu của tất cả các điểm cuối bên phải. Nếu như`L > R`, giao điểm trống và không có nghiệm nào tồn tại. Ngược lại, mọi số nguyên trong`[L, R]`là hợp lệ và vì chúng tôi muốn mức tối đa có thể`B`, chúng ta chỉ cần chọn`R`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · max(a_i + k)) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi truy vấn, hãy đọc mảng giá và giá trị`k`. 

Chúng tôi xử lý từng kịch bản một cách độc lập nên không có trạng thái nào được chia sẻ. 
2. Với mỗi mức giá`a_i`, tính khoảng giá trị cuối cùng hợp lệ:`[a_i - k, a_i + k]`. 

Điều này trực tiếp mã hóa các ràng buộc`|a_i - B| ≤ k`. 
3. Duy trì hai biến:`L`được khởi tạo ở một số rất nhỏ và`R`được khởi tạo với số lượng rất lớn. 

Chúng đại diện cho giao điểm của tất cả các khoảng được xử lý cho đến nay. 
4. Đối với mỗi khoảng thời gian`[l, r]`, cập nhật:`L = max(L, l)`Và`R = min(R, r)`. 

Điều này chỉ giữ giá trị của`B`có giá trị cho tất cả các yếu tố được thấy cho đến nay. 
5. Sau khi xử lý tất cả các phần tử, kiểm tra xem`L > R`. 

Nếu điều này xảy ra, không có số nguyên nào nằm trong giao điểm, nên đáp án là`-1`. 
6. Ngược lại, xuất ra`R`, vì nó là số nguyên lớn nhất thỏa mãn mọi ràng buộc. 

### Tại sao nó hoạt động 

Mỗi phần tử hạn chế một cách độc lập`B`đến một khoảng đóng của các giá trị hợp lệ. Một giá trị của`B`chỉ hợp lệ cho toàn bộ mảng nếu nó thỏa mãn đồng thời mọi ràng buộc của phần tử, đây chính xác là định nghĩa thuộc về giao điểm của tất cả các khoảng. Thuật toán duy trì giao điểm này tăng dần và không có bước nào loại bỏ bất kỳ giá trị nào vẫn có thể đáp ứng tất cả các ràng buộc được xử lý. Do đó, nếu một giá trị tồn tại đến cuối thì giá trị đó hợp lệ cho tất cả các phần tử và nếu phần giao trở nên trống thì không có giá trị hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    L = -10**18
    R = 10**18

    for x in a:
        L = max(L, x - k)
        R = min(R, x + k)

    if L > R:
        print(-1)
    else:
        print(R)
```Giải pháp chuyển đổi từng mức giá thành một khoảng thời gian cho phép và tiếp tục giao nhau với chúng. Chi tiết triển khai tinh tế duy nhất là chọn giới hạn ban đầu đủ lớn cho`L`Và`R`, vì giá có thể lên tới`10^8`Và`k`lên đến`10^8`, do đó khoảng kết quả có thể vượt ra ngoài phạm vi ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 1
a = [1, 1, 2, 3, 1]
```Chúng tôi tính toán các khoảng: 

| Bước | a_i | Khoảng thời gian | L | R | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [0, 2] | 0 | 2 | 
| 2 | 1 | [0, 2] | 0 | 2 | 
| 3 | 2 | [1, 3] | 1 | 2 | 
| 4 | 3 | [2, 4] | 2 | 2 | 
| 5 | 1 | [0, 2] | 2 | 2 | 

Giao điểm cuối cùng là`[2, 2]`, vậy đáp án là`2`. 

Điều này xác nhận rằng thuật toán theo dõi chính xác sự chồng chéo thu hẹp và xử lý nhiều ràng buộc hội tụ về một điểm duy nhất. 

### Ví dụ 2 

đầu vào:```
n = 2, k = 2
a = [1, 6]
```| Bước | a_i | Khoảng thời gian | L | R | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [-1, 3] | -1 | 3 | 
| 2 | 6 | [4, 8] | 4 | 3 | 

Tại thời điểm này`L > R`, do đó giao điểm trống và chúng tôi xuất ra`-1`. 

Điều này chứng tỏ trường hợp thất bại trong đó hai phạm vi khả thi hoàn toàn không trùng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | Mỗi phần tử đóng góp một lần cập nhật | 
| Không gian | O(1) | Chỉ có hai giới hạn đang chạy được lưu trữ | 

Các ràng buộc cho phép tổng cộng tối đa 10.000 phần tử trên tất cả các truy vấn, do đó, việc quét tuyến tính cho mỗi truy vấn có thể dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        L = -10**18
        R = 10**18

        for x in a:
            L = max(L, x - k)
            R = min(R, x + k)

        out.append(str(-1 if L > R else R))

    return "\n".join(out)

# provided samples
assert run("""4
5 1
1 1 2 3 1
4 2
6 4 8 5
2 2
1 6
3 5
5 2 5
""") == """2
6
-1
7"""

# custom cases
assert run("""1
1 10
5
""") == "15", "single element expands range"

assert run("""1
2 0
5 5
""") == "5", "zero movement edge"

assert run("""1
3 1
1 10 20
""") == "-1", "disjoint intervals"

assert run("""1
4 5
10 10 10 10
""") == "15", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 15 | tính đúng đắn của việc mở rộng khoảng thời gian | 
| chuyển động bằng không | 5 | hành vi k = 0 | 
| khoảng rời rạc | -1 | ngã tư vắng | 
| tất cả đều bình đẳng | 15 | tăng trưởng giao lộ ổn định | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`k = 0`. Trong trường hợp này, mọi khoảng đều thu gọn thành`[a_i, a_i]`, vì vậy giải pháp khả thi duy nhất là nếu tất cả các phần tử đều bằng nhau. Thuật toán xử lý việc này một cách tự nhiên vì giao điểm sẽ chỉ tồn tại nếu tất cả các giá trị khớp chính xác. 

Một trường hợp cạnh khác là khi`n = 1`. Câu trả lời đơn giản là`a_1 + k`, vì khoảng đơn là`[a_1 - k, a_1 + k]`và chúng tôi chọn giá trị hợp lệ tối đa. 

Một trường hợp tinh vi hơn xảy ra khi các giá trị lớn và`k`cũng lớn, có khả năng đẩy giới hạn ra ngoài phạm vi đầu vào ban đầu. Việc sử dụng giới hạn số nguyên rộng trong quá trình triển khai đảm bảo không bị tràn hoặc cắt bớt ảnh hưởng đến tính chính xác, vì số nguyên Python không bị giới hạn và logic chỉ phụ thuộc vào so sánh. 

Cuối cùng, các trường hợp giao điểm thu hẹp về một điểm sớm được xử lý chính xác vì thuật toán liên tục thắt chặt cả hai đầu, không bao giờ mất đi một ứng cử viên hợp lệ mà vẫn có thể đáp ứng mọi ràng buộc.
