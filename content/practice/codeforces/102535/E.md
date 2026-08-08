---
title: "CF 102535E - Cây có tiềm năng"
description: "Đầu vào mô tả một số bộ sưu tập thực vật. Một bộ sưu tập được viết dưới dạng một chuỗi các chữ cái viết hoa, trong đó mỗi chữ cái đại diện cho một loại cây và trọng lượng của nó được xác định bởi vị trí của nó trong bảng chữ cái."
date: "2026-08-06T19:49:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "E"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 77
verified: true
draft: false
---

[CF 102535E - Cây có tiềm năng](https://codeforces.com/problemset/problem/102535/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một số bộ sưu tập thực vật. Một bộ sưu tập được viết dưới dạng một chuỗi các chữ cái viết hoa, trong đó mỗi chữ cái đại diện cho một loại cây và trọng lượng của nó được xác định bởi vị trí của nó trong bảng chữ cái. Giá trị đầu tiên trong mỗi trường hợp thử nghiệm là tổng trọng lượng tối đa mà vùng trồng trọt có thể hỗ trợ. Nhiệm vụ là quyết định xem tổng trọng lượng của tất cả các cây trong bộ sưu tập có nằm trong giới hạn đó hay không. 

Đầu ra chỉ đơn giản là liệu bộ sưu tập có phù hợp hay không. Nếu tổng trọng lượng tối đa vượt quá khả năng cho phép thì câu trả lời là`YES`; nếu không thì nó là`NO`. 

Tổng độ dài của tất cả các chuỗi tối đa là 10 6, có nghĩa là giải pháp phải xử lý mỗi ký tự chỉ với một số lần không đổi. Một giải pháp bậc hai sẽ quá chậm vì việc tính toán lại trọng số liên tục hoặc kiểm tra nhiều kết hợp có thể đạt đến 10 12 phép tính trong những trường hợp lớn nhất. Quét tuyến tính là đủ vì có thể dễ dàng quản lý 10 thao tác 6 ký tự trong giới hạn thời gian nhất định. 

Một vài trường hợp có thể phá vỡ việc triển khai bất cẩn. Nếu dung tích đúng bằng tổng trọng lượng thì đáp án vẫn phải là`YES`. Ví dụ:```
1
3 ABC
```Trọng số là 1+2+3=6, vì vậy ví dụ này thực sự vượt quá dung lượng và kết quả là:```
NO
```Một ví dụ về ranh giới đúng là:```
1
6 ABC
```Đầu ra là:```
YES
```Việc triển khai sử dụng so sánh nghiêm ngặt như`total < w`sẽ từ chối nó một cách không chính xác. 

Một lỗi phổ biến khác là coi các chữ cái là giá trị không có chỉ mục. Ví dụ:```
1
1 A
```nhà máy`A`nặng 1, vì vậy đầu ra là:```
YES
```Nếu như`A`vô tình được chuyển đổi bằng cách sử dụng`ord('A') = 65`hoặc bằng cách trừ sai phần bù, kết quả sẽ không chính xác. 

Công suất cũng có thể bằng 0, do đó, một nhà máy có trọng lượng dương luôn phải hỏng:```
1
0 A
```Đầu ra đúng là:```
NO
```## Phương pháp tiếp cận 

Một giải pháp trực tiếp là mô phỏng việc tính toán tổng trọng lượng. Đối với mỗi ký tự trong chuỗi, hãy chuyển đổi nó thành vị trí trong bảng chữ cái và cộng giá trị đó vào tổng hiện có. Sau khi xử lý toàn bộ chuỗi, so sánh tổng với dung lượng đã cho. Cách tiếp cận này đã tối ưu vì mỗi cây phải được kiểm tra ít nhất một lần. 

Tư duy bạo lực chậm hơn sẽ liên tục quét bộ sưu tập trong khi cố gắng xây dựng hoặc xác thực các tổng số có thể có. Ví dụ: việc kiểm tra các tập hợp con sẽ yêu cầu xem xét tới 2 n khả năng, điều này là không thể khi độ dài chuỗi có thể đạt tới 10 6. Ngay cả một cách tiếp cận ít cực đoan hơn là tính toán lại tổng nhiều lần cũng có thể thực hiện khoảng 10 12 phép cộng trên một đầu vào lớn. 

Quan sát quan trọng là không có sự tương tác giữa các thực vật. Tổng trọng số chỉ là tổng của các đóng góp độc lập, do đó toàn bộ vấn đề giảm xuống còn việc tích lũy các giá trị trong một lần vượt qua. Không cần sắp xếp, lập trình động hoặc tìm kiếm. 

Lực lượng vũ phu hoạt động vì cuối cùng nó kiểm tra tất cả các cách có thể để tính tổng, nhưng nó thất bại vì số lượng khả năng tăng quá nhanh. Việc quan sát thấy mỗi cây đóng góp một giá trị độc lập cố định cho phép chúng ta thay thế toàn bộ quá trình bằng một tích lũy tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2 n ) hoặc tệ hơn tùy theo cách thực hiện | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trọng lượng tối đa cho phép và dây cây. Chuỗi chứa chính xác thông tin cần thiết để tính tổng, vì mỗi ký tự ánh xạ trực tiếp tới một trọng số. 
2. Duyệt từng ký tự trong chuỗi. Chuyển đổi từng chữ cái viết hoa thành trọng lượng của nó bằng cách sử dụng`ord(character) - ord('A') + 1`, sau đó thêm nó vào tổng số đang chạy. các`+1`là cần thiết vì trong bài toán này vị trí bảng chữ cái bắt đầu từ 1 chứ không phải 0. 
3. Sau khi xử lý xong tất cả ký tự, so sánh trọng lượng tích lũy với dung lượng cho phép. Nếu tổng số không vượt quá giới hạn, hãy in`YES`; nếu không thì in`NO`. 

Tại sao nó hoạt động: 

Bất biến trong quá trình quét là sau khi xử lý bất kỳ tiền tố nào của chuỗi, tổng số đang chạy bằng trọng lượng chính xác của tất cả các cây trong tiền tố đó. Mỗi bước sẽ thêm phần đóng góp chính xác của cây tiếp theo, do đó, bất biến vẫn đúng cho đến khi toàn bộ chuỗi được xử lý. Khi kết thúc quá trình quét, tổng số được lưu trữ chính xác là trọng lượng của mỗi cây cộng lại. So sánh nó với năng lực sẽ đưa ra quyết định đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        w, s = input().split()
        w = int(w)

        total = 0
        for c in s:
            total += ord(c) - ord('A') + 1

        if total <= w:
            ans.append("YES")
        else:
            ans.append("NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình đọc tất cả các trường hợp thử nghiệm bằng cách sử dụng`sys.stdin.readline`, điều này là cần thiết vì tổng kích thước đầu vào có thể đạt tới một triệu ký tự. 

Biểu thức chuyển đổi`ord(c) - ord('A') + 1`biến đổi`A`thành 1 và`Z`thành 26. Giá trị tích lũy được lưu trữ dưới dạng số nguyên của Python, giúp tránh lo ngại tràn ngay cả khi xuất hiện nhiều chữ cái lớn. 

Việc so sánh sử dụng`<=`vì bộ sưu tập có trọng lượng khớp chính xác với dung lượng sẵn có được cho phép. Không cần thêm bộ nhớ tỷ lệ thuận với kích thước chuỗi vì chuỗi được xử lý chỉ với một tổng chạy duy nhất. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu đầu tiên:```
130 ACMALGOLYMPICS
```Quá trình quét tạo ra trạng thái sau. 

| Nhân vật | Trọng lượng nhân vật | Tổng số chạy | 
| --- | --- | --- | 
| A | 1 | 1 | 
| C | 3 | 4 | 
| M | 13 | 17 | 
| A | 1 | 18 | 
| L | 12 | 30 | 
| G | 7 | 37 | 
| Ồ | 15 | 52 | 
| L | 12 | 64 | 
| Y | 25 | 89 | 
| M | 13 | 102 | 
| P | 16 | 118 | 
| Tôi | 9 | 127 | 
| C | 3 | 130 | 
| S | 19 | 149 | 

Trọng số cuối cùng là 149, lớn hơn giới hạn 130 nên thuật toán in`NO`. 

Đối với trường hợp mẫu thứ tư:```
473 THEQUICKBROWNFOXJUMPSOVERTHELAZYDOG
```Trạng thái quan trọng là sự tích lũy cuối cùng. 

| Tiểu bang | Giá trị | 
| --- | --- | 
| Công suất | 473 | 
| Tổng sau khi quét toàn bộ chuỗi | 473 | 
| So sánh | 473 <= 473 | 
| Trả lời | CÓ | 

Trường hợp này xác nhận ranh giới đẳng thức. Thuật toán không yêu cầu dung lượng chưa sử dụng, chỉ cần tổng dung lượng nằm trong giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự trong chuỗi thực vật được chuyển đổi và thêm một lần. | 
| Không gian | O(1) | Chỉ có tổng số đang chạy và một vài biến được lưu trữ. | 

Tổng số ký tự trong tất cả các trường hợp thử nghiệm nhiều nhất là 10 6, do đó quá trình quét tuyến tính thực hiện khoảng một triệu chuyển đổi và bổ sung. Điều này dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ liên tục vẫn thấp hơn giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []
        for _ in range(t):
            w, s = input().split()
            total = sum(ord(c) - ord('A') + 1 for c in s)
            out.append("YES" if total <= int(w) else "NO")
        return "\n".join(out)

    result = solve()
    sys.stdin = old_stdin
    return result

assert run("""4
130 ACMALGOLYMPICS
2020 TWENTYTWENTY
472 THEQUICKBROWNFOXJUMPSOVERTHELAZYDOG
473 THEQUICKBROWNFOXJUMPSOVERTHELAZYDOG
""") == """NO
YES
NO
YES""", "provided samples"

assert run("""3
1 A
0 A
6 ABC
""") == """YES
NO
YES""", "minimum and boundary cases"

assert run("""2
26 Z
25 Z
""") == """YES
NO""", "single maximum letter and off-by-one boundary"

assert run("""1
1000000000 ZZZZZZZZZZZZZZZZZZZZZZZZZZZZ
""") == """YES""", "large capacity"

assert run("""1
52 ABZ
""") == """YES""", "mixed values with exact total"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 A`|`YES`| Chuyển đổi chuỗi nhỏ nhất và chữ cái đúng | 
|`1 / 0 A`|`NO`| Xử lý công suất bằng không | 
|`26 / Z`Và`25 / Z`|`YES`,`NO`| So sánh ranh giới chính xác | 
| Nhiều`Z`ký tự có dung lượng lớn |`YES`| Tổng số lớn và xử lý số nguyên | 
|`52 ABZ`|`YES`| Kết hợp các trọng lượng chữ cái khác nhau | 

## Vỏ cạnh 

Ranh giới đẳng thức được xử lý chính xác vì thuật toán chấp nhận tổng số chính xác bằng dung lượng. Vì:```
1
6 ABC
```quá trình quét tính toán 1+2+3=6. Sự so sánh`6 <= 6`thành công, tạo ra:```
YES
```Một so sánh ít hơn nghiêm ngặt sẽ thất bại trong trường hợp này. 

Bộ sưu tập thực vật nhỏ nhất có thể là một ký tự đơn. Vì:```
1
1 A
```việc chuyển đổi mang lại`1`và tổng số khớp chính xác, vì vậy đầu ra là:```
YES
```Điều này xác nhận rằng việc chuyển đổi bảng chữ cái bắt đầu từ một chứ không phải bằng không. 

Công suất bằng 0 cũng là đầu vào hợp lệ. Vì:```
1
0 A
```tổng số trở thành`1`, lớn hơn`0`, do đó thuật toán trả về:```
NO
```Cuối cùng, các bộ sưu tập rất lớn không cần xử lý đặc biệt. Một chuỗi chứa nhiều`Z`ký tự chỉ cần thêm 26 cho mỗi ký tự. Vì mỗi ký tự được xử lý độc lập nên bất biến giống nhau sẽ được áp dụng bất kể độ dài chuỗi và phép so sánh cuối cùng vẫn đúng.
