---
title: "CF 104805M - Chọn tên"
description: "Chúng tôi được cung cấp bốn bộ sưu tập tên. Igor có một danh sách những cái tên anh ấy thích và một danh sách những cái tên anh ấy không thích. Ira cũng có một danh sách những cái tên cô ấy thích và một danh sách những cái tên cô ấy không thích. Một cái tên chỉ được coi là có thể sử dụng được nếu cả hai người đều thích nó và không ai trong số họ rõ ràng là không thích nó."
date: "2026-06-28T13:22:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104805
codeforces_index: "M"
codeforces_contest_name: "Central Russia Regional Contest, 2022"
rating: 0
weight: 104805
solve_time_s: 66
verified: true
draft: false
---

[CF 104805M - Chọn tên](https://codeforces.com/problemset/problem/104805/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp bốn bộ sưu tập tên. Igor có một danh sách những cái tên anh ấy thích và một danh sách những cái tên anh ấy không thích. Ira cũng có một danh sách những cái tên cô ấy thích và một danh sách những cái tên cô ấy không thích. Một cái tên chỉ được coi là có thể sử dụng được nếu cả hai người đều thích nó và không ai trong số họ rõ ràng là không thích nó. Nhiệm vụ là xuất ra mọi tên có thể sử dụng được như vậy, không trùng lặp và được sắp xếp theo từ điển. 

Một cách hữu ích để nghĩ về điều này là mỗi người định nghĩa hai tập hợp: một tập hợp “phải chứa” và một tập hợp “bị cấm”. Câu trả lời cuối cùng là giao điểm của cả hai phải chứa các tập hợp, với tất cả các phần tử xuất hiện trong tập hợp bị cấm đều bị loại bỏ. 

Kích thước đầu vào đủ nhỏ để tất cả các thao tác liên quan đến băm hoặc sắp xếp các tên riêng lẻ đều có thể thực hiện đủ nhanh một cách dễ dàng. Ngay cả trong trường hợp xấu nhất, tổng cộng cũng có tối đa vài nghìn cái tên. Điều này ngay lập tức loại trừ bất kỳ điều gì liên quan đến so sánh bậc hai của các chuỗi trong danh sách, vì điều đó có nghĩa là theo thứ tự 10^6 so sánh trên mỗi cặp danh sách, nhưng cũng gợi ý rằng chỉ cần sử dụng bộ băm hoặc sắp xếp đơn giản là đủ. 

Một vấn đề tế nhị là sự trùng lặp bên trong danh sách đầu vào. Một tên có thể xuất hiện nhiều lần trong cùng một danh sách, điều đó có nghĩa là việc coi danh sách là các mảng đơn giản và giao nhau theo vị trí sẽ tạo ra các bản sao ở đầu ra hoặc lọc không chính xác. Một vấn đề khác là một cái tên có thể được cùng một người thích và không thích. Trong trường hợp đó, việc không thích sẽ được ghi đè vì điều kiện yêu cầu rõ ràng rằng không ai không thích tên đã chọn. 

Trường hợp cụ thể là khi một cái tên được cả hai người thích nhưng cũng xuất hiện trong một danh sách không thích. 

đầu vào:```
1 1 1 0
alex
alex
alex
```Ở đây cả hai đều thích “alex” nhưng Igor không thích nó. Đầu ra đúng là trống. Một cách tiếp cận ngây thơ chỉ kiểm tra các danh sách tương tự sẽ tạo ra “alex” không chính xác. 

Một trường hợp khác là sự trùng lặp gây ra kết quả đầu ra lặp đi lặp lại. 

đầu vào:```
2 2 0 0
mike
mike
mike
mike
```Đầu ra đúng:```
mike
```Một giao điểm đơn giản trên danh sách thô sẽ xuất ra "mike" nhiều lần trừ khi việc loại bỏ trùng lặp được xử lý rõ ràng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là xử lý từng tên trong danh sách thích của Igor và kiểm tra xem nó có xuất hiện trong danh sách thích của Ira hay không đồng thời quét cả hai danh sách không thích để đảm bảo nó không bị cấm. Đối với mỗi tên ứng cử viên, điều này yêu cầu quét tuyến tính tối đa bốn danh sách. Trong trường hợp xấu nhất, nếu tất cả các danh sách chứa n phần tử thì điều này sẽ trở thành so sánh chuỗi O(n^2). Với việc so sánh chuỗi có giá trị lên tới 20 ký tự, điều này vẫn trở nên quá chậm ở giới hạn trên. 

Quan sát quan trọng là việc kiểm tra tư cách thành viên chứ không phải quét mới là những gì chúng tôi thực sự cần. Khi chúng tôi nhận ra rằng mỗi điều kiện hoàn toàn dựa trên tập hợp, mỗi danh sách có thể được chuyển đổi thành tập hợp băm. Sau đó, việc kiểm tra xem tên có hợp lệ hay không sẽ trở thành thời gian trung bình O(1) cho mỗi điều kiện. Toàn bộ vấn đề được rút gọn thành việc xây dựng các tập hợp thích và không thích, giao nhau giữa hai tập hợp giống nhau và trừ đi sự kết hợp của các tập hợp không thích. 

Điều này biến vấn đề từ việc quét lặp đi lặp lại thành một lần duyệt qua một tập hợp ứng cử viên, sau đó là lọc theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n1 + n2) · (n1 + n2 + m1 + m2)) | O(1) thêm | Quá chậm | 
| Tối ưu (bộ băm) | O(n1 + n2 + m1 + m2) | O(n1 + n2 + m1 + m2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả bốn số nguyên, sau đó đọc tất cả tên từ đầu vào thành bốn bộ sưu tập riêng biệt tương ứng với lượt thích của Igor, lượt thích của Ira, lượt không thích của Igor và lượt Ira không thích. Việc phân nhóm rất quan trọng vì mỗi nhóm thể hiện một vai trò ràng buộc khác nhau trong quá trình lọc cuối cùng. 
2. Chèn tất cả tên từ mỗi danh sách vào các bộ riêng biệt. Điều này tự động loại bỏ các bản sao và chuyển các truy vấn thành viên thành các hoạt động liên tục. 
3. Xây dựng tập ứng cử viên bằng cách lấy giao giữa tập thích của Igor và tập thích của Ira. Điều này đảm bảo rằng mọi cái tên còn lại đều thỏa mãn yêu cầu “cả hai đều thích”. 
4. Loại bỏ khỏi nhóm ứng cử viên này mọi tên xuất hiện trong nhóm không thích của Igor hoặc nhóm không thích của Ira. Điều này thực thi ràng buộc rằng không ai có thể không thích tên đã chọn. 
5. Chuyển tập hợp cuối cùng thành một danh sách được sắp xếp và xuất ra theo thứ tự từ điển. Việc sắp xếp là bắt buộc vì các tập hợp không giữ nguyên thứ tự, trong khi bài toán yêu cầu đầu ra theo thứ tự một cách rõ ràng. 

Lý do đằng sau tính đúng đắn là ở mỗi giai đoạn, chúng ta đang thu hẹp phạm vi tên bằng cách áp dụng một ràng buộc vừa cần vừa đủ. Một cái tên tồn tại trong bộ lọc đầu tiên khi và chỉ khi nó được cả hai người thích. Tên vẫn tồn tại trong bộ lọc thứ hai khi và chỉ khi nó không bị một trong hai bên cấm rõ ràng. Không có điều kiện nào khác tồn tại trong bài toán, vì vậy tập cuối cùng khớp chính xác với các câu trả lời hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n1, n2, m1, m2 = map(int, input().split())

    igor_like = [input().strip() for _ in range(n1)]
    ira_like = [input().strip() for _ in range(n2)]
    igor_bad = [input().strip() for _ in range(m1)]
    ira_bad = [input().strip() for _ in range(m2)]

    s1 = set(igor_like)
    s2 = set(ira_like)
    bad1 = set(igor_bad)
    bad2 = set(ira_bad)

    candidates = s1 & s2
    forbidden = bad1 | bad2

    result = sorted(x for x in candidates if x not in forbidden)

    sys.stdout.write("\n".join(result))

if __name__ == "__main__":
    main()
```Việc triển khai phản ánh thuật toán gần như trực tiếp. Mỗi khối đầu vào ngay lập tức được chuyển đổi thành một tập hợp để đảm bảo cả việc chống trùng lặp và tra cứu hiệu quả. Toán tử giao lộ được sử dụng để thực thi điều kiện ưu tiên chung. Sự kết hợp của các nhóm không thích tạo thành một bộ lọc loại trừ duy nhất, vì bất kỳ sự xuất hiện nào trong một trong hai danh sách sẽ loại bỏ tên. Quá trình hiểu cuối cùng thực hiện một lần duyệt qua tập ứng viên và việc sắp xếp chỉ được áp dụng ở cuối để đáp ứng các yêu cầu đầu ra. 

Một lỗi triển khai phổ biến là quên loại bỏ trùng lặp trước khi sắp xếp, dẫn đến việc lặp lại tên trong đầu ra. Một cách khác là áp dụng bộ lọc không thích trước giao lộ không chính xác, điều này có thể loại bỏ những tên vẫn cần được xem xét nếu chúng chỉ bị người không yêu cầu chúng không thích ở bước giao lộ cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4 2 3
kirill
ruslan
sonya
veronika
vasya
ruslan
alina
sonya
veronika
nastya
masha
sasha
masha
natasha
```Chúng tôi theo dõi quá trình chuyển đổi từng bước. 

| Bước | Igor thích | Tôi thích | Giao lộ | Bị cấm | Ứng viên đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | {kirill, ruslan, sonya, veronika, vasya} | {ruslan, alina, sonya, veronika} | - | - | - | 
| Sau ngã tư | - | - | {ruslan, sonya, veronika} | - | - | 
| Sau khi lọc | - | - | - | {masha, sasha, natasha, alina, sonya?, veronika?} | {ruslan, sonya, veronika} | 

Kết quả cuối cùng được sắp xếp:```
ruslan
sonya
veronika
```Điều này xác nhận rằng chỉ những cái tên được yêu thích đồng thời và không bị loại mới tồn tại trong quy trình lọc. 

### Ví dụ 2 

đầu vào:```
3 3 1 1
a
b
c
a
b
d
c
```Từng bước một: 

| Bước | Igor thích | Tôi thích | Giao lộ | Bị cấm | Ứng viên đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | {a, b, c} | {a,b,d} | - | - | - | 
| Sau ngã tư | - | - | {a,b} | - | - | 
| Sau khi lọc | - | - | - | {c,d} | {a,b} | 

Đầu ra cuối cùng:```
a
b
```Ví dụ này cho thấy những cái tên chỉ xuất hiện trong một danh sách không thích sẽ không ảnh hưởng đến các ứng cử viên hợp lệ trừ khi chúng nằm trong giao điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n1 + n2 + m1 + m2 + k log k) | Tập hợp xây dựng là tuyến tính, lọc tuyến tính theo kích thước ứng viên, việc sắp xếp chiếm ưu thế với k ứng viên | 
| Không gian | O(n1 + n2 + m1 + m2) | Lưu trữ tất cả các tên duy nhất trong danh sách đầu vào | 

Các ràng buộc đủ nhỏ để cả các phép toán tập tuyến tính và sắp xếp vài nghìn chuỗi đều dễ dàng nằm gọn trong giới hạn. Ngay cả với kích thước đầu vào trong trường hợp xấu nhất, số lượng thao tác chuỗi vẫn không đáng kể trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    n1, n2, m1, m2 = map(int, sys.stdin.readline().split())
    igor_like = [sys.stdin.readline().strip() for _ in range(n1)]
    ira_like = [sys.stdin.readline().strip() for _ in range(n2)]
    igor_bad = [sys.stdin.readline().strip() for _ in range(m1)]
    ira_bad = [sys.stdin.readline().strip() for _ in range(m2)]

    s1 = set(igor_like)
    s2 = set(ira_like)
    bad1 = set(igor_bad)
    bad2 = set(ira_bad)

    res = sorted(x for x in (s1 & s2) if x not in (bad1 | bad2))
    return "\n".join(res).strip()

# sample 1
assert run("""5 4 2 3
kirill
ruslan
sonya
veronika
vasya
ruslan
alina
sonya
veronika
nastya
masha
sasha
masha
natasha
""") == "ruslan\nsonya\nveronika"

# minimal case, no overlap
assert run("""1 1 0 0
a
b
""") == ""

# all overlap, no bad
assert run("""2 2 0 0
a
b
a
b
""") == "a\nb"

# conflict removal
assert run("""2 2 1 0
a
b
a
b
a
""") == "b"

# full exclusion
assert run("""2 2 1 1
a
b
a
b
a
b
""") == ""

# duplicates inside lists
assert run("""4 4 0 0
x
x
y
z
x
y
y
z
""") == "x\ny\nz"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu không chồng chéo | trống | không có kết quả dương tính giả khi giao lộ trống | 
| tất cả trùng lặp không tệ | sắp xếp trọn bộ | chống trùng lặp và đặt hàng | 
| loại bỏ xung đột | b | không thích ghi đè tư cách thành viên thích đặt | 
| loại trừ hoàn toàn | trống | tập cấm kết hợp tác phẩm | 
| trùng lặp bên trong danh sách | x y z | loại bỏ trùng lặp đúng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một cái tên xuất hiện trong cả danh sách thích và danh sách không thích của cùng một người. Ví dụ:```
1 0 1 0
alex
alex
```Igor đồng thời thích và không thích “alex”. Trong giai đoạn xây dựng tập hợp, cả tập hợp “thích” và “không thích” đều chứa cùng một phần tử. Trong quá trình hình thành ứng cử viên, “alex” chỉ được bao gồm nếu nó tồn tại trong cả hai bộ giống nhau, nhưng vì Ira không có lượt thích nên nó không bao giờ đi vào giao lộ. Đầu ra đúng là trống. Thuật toán xử lý vấn đề này một cách tự nhiên vì giao điểm được tính toán trước khi lọc, do đó, những mâu thuẫn trong danh sách của một người sẽ không giữ nguyên tên một cách sai lầm. 

Một trường hợp đặc biệt khác là khi tất cả các danh sách đều có tên giống nhau nhưng tất cả đều bị ít nhất một người không thích. Ví dụ:```
1 1 1 1
bob
bob
bob
bob
```Sau khi giao nhau, “bob” là một ứng cử viên, nhưng nó bị loại bỏ vì xuất hiện trong cả hai nhóm không thích. Đầu ra cuối cùng trống, phù hợp với yêu cầu rằng bất kỳ lượt không thích nào đều bị loại.
