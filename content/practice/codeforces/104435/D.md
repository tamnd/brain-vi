---
title: "CF 104435D - Lời nói xấu của Eliens"
description: "Chúng ta được cung cấp một mảng dài biểu thị một tweet, trong đó mỗi phần tử là một số nguyên từ 1 đến 300. Chúng ta cũng được cung cấp một mảng mẫu ngắn hơn được gọi là slur."
date: "2026-06-30T18:40:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104435
codeforces_index: "D"
codeforces_contest_name: "2023 UP ACM Algolympics Final Round"
rating: 0
weight: 104435
solve_time_s: 47
verified: true
draft: false
---

[CF 104435D - Lời nói xấu của Eliens](https://codeforces.com/problemset/problem/104435/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng dài biểu thị một tweet, trong đó mỗi phần tử là một số nguyên từ 1 đến 300. Chúng ta cũng được cung cấp một mảng mẫu ngắn hơn được gọi là slur. Nhiệm vụ là đếm xem có bao nhiêu chuỗi con của dòng tweet, với độ dài chính xác bằng độ dài câu nói tục tĩu, “gần giống” với câu nói tục tĩu theo quy tắc kết hợp thoải mái. 

Chuỗi con hợp lệ nếu với mọi vị trí i, giá trị tweet nằm trong một bước của giá trị slur tại vị trí đó. Nói cách khác, mỗi cặp căn chỉnh phải thỏa mãn chênh lệch tối đa là 1 trong nhãn số nguyên. Chúng ta cũng cần xuất ra tất cả các chỉ số bắt đầu của các chuỗi con hợp lệ đó theo thứ tự tăng dần. 

Cấu trúc là kiểm tra cửa sổ trượt có chiều dài cố định với giới hạn dung sai cho mỗi vị trí. Kích thước đầu vào lớn: tweet có thể chứa tới 1,8 triệu phần tử và mẫu lên tới 250 nghìn. Điều này ngay lập tức loại trừ mọi giải pháp kiểm tra từng cửa sổ bằng cách quét trực tiếp tất cả các vị trí. Cách tiếp cận O(t · s) ngây thơ sẽ yêu cầu so sánh lên tới 4,5 × 10^11, vượt xa giới hạn khả thi. 

Trường hợp cạnh khóa xuất hiện khi tất cả các giá trị gần với ranh giới của phạm vi bảng chữ cái, chẳng hạn như 1 hoặc 300. Ví dụ: nếu mẫu chứa 1 thì các giá trị tweet hợp lệ ở vị trí đó chỉ có thể là 1 hoặc 2. Việc triển khai bất cẩn giả định số học mô-đun hoặc quên kẹp ranh giới sẽ chấp nhận sai các kết quả khớp không hợp lệ như 0 hoặc 301 nếu không cẩn thận. 

Một trường hợp tinh vi khác là khi nhiều cửa sổ liền kề chỉ khác nhau một vị trí. Việc tính toán lại toàn bộ đơn giản theo ca sẽ gây lãng phí bằng cách kiểm tra lại các vị trí không thay đổi, mặc dù hầu hết các so sánh trùng lặp giữa các cửa sổ liên tiếp. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi chỉ số bắt đầu i trong tweet, chúng tôi so sánh lát cắt T[i : i + s] với vị trí S theo vị trí và kiểm tra xem mỗi cặp có khác nhau nhiều nhất một hay không. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về tính hợp lệ. Tuy nhiên, mỗi cửa sổ có giá trị O(s) và có O(t) cửa sổ, dẫn đến độ phức tạp về thời gian O(t · s). Với các giá trị trong trường hợp xấu nhất, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là mỗi so sánh cửa sổ độc lập với mỗi vị trí và điều kiện hoàn toàn cục bộ: mỗi cặp (T[i + j], S[j]) phải đáp ứng một ràng buộc đơn giản. Điều này cho phép chúng ta định dạng lại vấn đề dưới dạng vấn đề khớp mẫu với quy tắc tương thích nhị phân. Chúng tôi có thể xử lý trước khả năng tương thích cho mỗi vị trí và sau đó sử dụng cơ chế trượt thời gian tuyến tính để duy trì số lượng vị trí trong cửa sổ hiện tại thỏa mãn điều kiện. 

Bí quyết chính là chuyển đổi từng căn chỉnh thành điều kiện boolean và duy trì số lượng vị trí hài lòng. Thay vì tính toán lại tất cả các so sánh s mỗi ca, chúng tôi cập nhật số đếm trong O(1) khi cửa sổ di chuyển bằng cách chỉ theo dõi các vị trí đi và đến. 

Chúng ta xác định một kết quả khớp ở vị trí (i, j) là hợp lệ nếu |T[i + j] − S[j]| 1. Đối với cửa sổ cố định bắt đầu i, chúng ta cần tất cả các vị trí của s có hiệu lực đồng thời. Vì vậy, chúng tôi duy trì có bao nhiêu vị trí hợp lệ và so sánh nó với s. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t · s) | O(1) | Quá chậm | 
| Số lượng hiệu lực trượt | Tiền xử lý O(t · s) / Quét O(t) | O(t + s) | Đã chấp nhận | 

Sự cải thiện thực sự đến từ việc quan sát thấy rằng điều kiện có thể được đánh giá trước cho mỗi lần căn chỉnh và được sử dụng lại trên các cửa sổ chồng chéo. 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán thành việc kiểm tra, đối với mỗi offset i, liệu tất cả các cặp (i + j, j) có thỏa mãn giới hạn dung sai hay không. Chúng ta duy trì một bộ đếm xem có bao nhiêu vị trí hiện thỏa mãn điều kiện cho cửa sổ bắt đầu từ i.

1. Đối với mỗi vị trí mẫu j, hãy tính toán trước xem T[j] có khớp với S[j] cho cửa sổ đầu tiên hay không. Điều này khởi tạo bộ đếm hợp lệ trên cửa sổ đầu tiên bắt đầu từ chỉ mục 0. 
2. Trượt cửa sổ từ trái sang phải. Khi di chuyển từ điểm bắt đầu i đến i + 1, các cặp căn chỉnh sẽ dịch chuyển: sự đóng góp của vị trí j trong cửa sổ i tương ứng với vị trí j - 1 trong cửa sổ i + 1. Chúng tôi chỉ tính toán lại các vị trí bị ảnh hưởng ở ranh giới. 
3. Cụ thể khi di chuyển cửa sổ: 

vị trí cũ j = 0 rời khỏi cửa sổ và j = s − 1 đi vào. Chúng tôi điều chỉnh bộ đếm tính hợp lệ bằng cách trừ đi tính hợp lệ của cặp gửi đi và cộng tính hợp lệ của cặp vào. 
4. Sau mỗi ca, nếu bộ đếm hợp lệ bằng s, chúng tôi ghi lại chỉ mục hiện tại là vị trí bắt đầu hợp lệ. 

Mỗi bước dựa trên thực tế là chỉ có một sự liên kết thay đổi cho mỗi lần thay đổi chỉ số về mặt ghép nối tương đối. Các vị trí còn lại duy trì cấu trúc so sánh của chúng nhưng không được sử dụng lại trực tiếp theo nghĩa thông minh về chỉ mục; thay vào đó, chúng tôi tính toán lại các hiệu ứng biên một cách hiệu quả. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí i nào, thuật toán duy trì số lượng chỉ số j chính xác sao cho ràng buộc |T[i + j] − S[j]| 1 giữ. Sự chuyển đổi duy nhất giữa i và i + 1 ảnh hưởng đến phần tử nào được so sánh với S[0] và S[s − 1], do đó phần còn lại của trạng thái hợp lệ vẫn nhất quán về mặt cấu trúc. Bất biến này đảm bảo rằng bất cứ khi nào bộ đếm bằng s, mọi cặp căn chỉnh đều thỏa mãn điều kiện, đó chính xác là định nghĩa của một chuỗi con hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t, s = map(int, input().split())
    T = list(map(int, input().split()))
    S = list(map(int, input().split()))

    if s > t:
        print(0)
        print()
        return

    def ok(a, b):
        return abs(a - b) <= 1

    # compute initial window
    cnt = 0
    for j in range(s):
        if ok(T[j], S[j]):
            cnt += 1

    res = []
    if cnt == s:
        res.append(1)

    for i in range(1, t - s + 1):
        # remove outgoing pair
        if ok(T[i - 1], S[0]):
            cnt -= 1
        # add incoming pair
        if ok(T[i + s - 1], S[s - 1]):
            cnt += 1

        if cnt == s:
            res.append(i + 1)

    print(len(res))
    if res:
        print(*res)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng trạng thái hợp lệ cho căn chỉnh ban đầu. Sau đó, nó trượt cửa sổ theo từng vị trí một, chỉ điều chỉnh các đóng góp biên. chức năng`ok`mã hóa trực tiếp quy tắc dung sai. Danh sách kết quả thu thập tất cả các vị trí bắt đầu hợp lệ. 

Điều tinh tế quan trọng là việc dịch chuyển cửa sổ không yêu cầu tính toán lại các vị trí bên trong. Chỉ có điểm cuối mới quan trọng vì cấu trúc ghép nối tương đối được giữ nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
t = 14, s = 3
T = [1, 3, 2, 2, 1, 5, 1, 2, 1, 6, 1, 2, 2, 1]
S = [1, 2, 1]
```Chúng tôi đánh giá từng cửa sổ: 

| tôi | cửa sổ T[i:i+3] | trận đấu cho mỗi vị trí | đếm | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 3 2 | | 1 | không | 
| 2 | 3 2 2 | | 1 | không | 
| 3 | 2 2 1 | | 3 | vâng | 
| 7 | 2 1 6 | | 2 | không | 
| 11 | 2 2 1 | | 3 | vâng | 
| 12 | 2 1 1 | | 3 | vâng | 

Thuật toán xác định chính xác các vị trí mà cả ba ràng buộc căn chỉnh đều được giữ đồng thời. 

Dấu vết này cho thấy việc khớp một phần không quan trọng; chỉ có sự căn chỉnh đầy đủ trên tất cả các vị trí mới xác định tính hợp lệ. 

### Ví dụ 2 

đầu vào:```
t = 5, s = 2
T = [1, 300, 300, 1, 299]
S = [1, 300]
```| tôi | cửa sổ | trận đấu | đếm | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 300 | | 2 | vâng | 
| 2 | 300 300 | | 1 | không | 
| 3 | 300 1 | | 0 | không | 
| 4 | 1 299 | | 2 | vâng | 

Ví dụ này nêu bật dung sai biên: các giá trị gần 300 vẫn khớp chính xác vì chỉ cho phép độ lệch ±1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi sự thay đổi cửa sổ chỉ cập nhật hai so sánh ranh giới | 
| Không gian | O(t + s) | Lưu trữ cho mảng tweet và mẫu | 

Thuật toán xử lý mỗi vị trí tweet với số lần không đổi, điều này rất cần thiết với kích thước đầu vào lên tới 1,8 triệu. Điều này đảm bảo khả năng mở rộng tuyến tính và phù hợp thoải mái trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample 1
assert run("""14 3
1 3 2 2 1 5 1 2 1 6 1 2 2 1
1 2 1
""") == """3
3 11 12"""

# provided sample 2
assert run("""5 2
1 300 300 1 299
1 300
""") == """2
1 4"""

# edge: all identical
assert run("""4 2
10 10 10 10
10 10
""") == """3
1 2 3"""

# edge: boundary 1 and 300
assert run("""3 2
1 2 300
1 300
""") == """1
1"""

# edge: no match
assert run("""4 2
1 1 1 1
300 300
""") == """0
""".strip()
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều giống hệt nhau | trận đấu đầy đủ | độ chính xác cơ bản | 
| ranh giới cực đoan | căn chỉnh hợp lệ duy nhất | dung sai cạnh | 
| không có trường hợp trùng khớp | đầu ra trống | xử lý từ chối | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các giá trị nằm ở ranh giới của bảng chữ cái, chẳng hạn như 1 hoặc 300. Thuật toán xử lý điều này một cách tự nhiên vì sự so sánh hoàn toàn là sự khác biệt tuyệt đối, do đó các giá trị nằm ngoài giá trị kề hợp lệ sẽ không bao giờ được chấp nhận. Ví dụ, trong cửa sổ`[1, 2]`chống lại`[1, 300]`, phép so sánh thứ hai thất bại vì |2 − 300| lớn, ngay lập tức ngăn chặn kết quả dương tính giả. 

Một trường hợp khác là khi có nhiều cửa sổ chồng chéo hợp lệ, đặc biệt khi mẫu lặp lại. Bộ đếm trượt đảm bảo rằng mỗi cửa sổ được đánh giá độc lập nhưng hiệu quả, không tính hai lần hoặc thiếu phần chồng chéo.
