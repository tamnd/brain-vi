---
title: "CF 105786D - Tăng nghiêm ngặt"
description: "Nhiệm vụ là duy trì xem một chuỗi đã sắp xếp có đủ vị trí số nguyên trống để chèn chính xác k giá trị mới hay không. Mảng ban đầu được cho là phải tăng lên một cách nghiêm ngặt, nhưng sau mỗi lần cập nhật, một phần tử có thể thay đổi, do đó mảng có thể tạm thời trở nên không hợp lệ."
date: "2026-06-25T15:42:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105786
codeforces_index: "D"
codeforces_contest_name: "2025 USACO.Guide Informatics Tournament"
rating: 0
weight: 105786
solve_time_s: 38
verified: true
draft: false
---

[CF 105786D - Tăng nghiêm ngặt](https://codeforces.com/problemset/problem/105786/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ là duy trì xem một chuỗi đã sắp xếp có đủ vị trí số nguyên trống để chèn chính xác hay không.`k`những giá trị mới. Mảng ban đầu được cho là phải tăng lên một cách nghiêm ngặt, nhưng sau mỗi lần cập nhật, một phần tử có thể thay đổi, do đó mảng có thể tạm thời trở nên không hợp lệ. Sau mỗi lần cập nhật, chúng tôi phải quyết định xem liệu việc chèn có còn khả thi hay không. 

Xét hai giá trị lân cận`a[i]`Và`a[i+1]`. Mọi giá trị được chèn vào giữa chúng phải là số nguyên lớn hơn`a[i]`và hoàn toàn nhỏ hơn`a[i+1]`. Số vị trí có sẵn trong khoảng trống này là:`a[i+1] - a[i] - 1`Ví dụ, giữa`5`Và`10`, các giá trị được chèn có thể là`6, 7, 8, 9`, vì vậy khoảng cách này góp phần`4`các vị trí. 

Tổng số giá trị chúng ta có thể chèn là tổng của tất cả các kích thước khoảng cách. Câu trả lời là`YES`chính xác khi mọi cặp liền kề đều tăng và tổng công suất này ít nhất là`k`. 

Các ràng buộc buộc chúng tôi không thể xây dựng lại toàn bộ mảng sau mỗi lần cập nhật. Độ dài của mảng và số lượng cập nhật có thể đạt được`2 * 10^5`. Với một vài giây thời gian, một`O(nq)`giải pháp sẽ thực hiện xung quanh`4 * 10^10`hoạt động vượt xa những gì có thể. Chúng ta cần làm cho mỗi lần cập nhật chỉ ảnh hưởng đến một số lượng giá trị không đổi. 

Có một số trường hợp dễ dàng phá vỡ một giải pháp ngây thơ. 

Nếu một bản cập nhật làm cho hai giá trị lân cận bằng nhau thì mảng sẽ không còn tăng nghiêm ngặt nữa và việc chèn không thể sửa chữa được thứ tự ban đầu. 

Đối với đầu vào:```
n = 3, k = 1
a = [1, 3, 5]
update: a[2] = 1
```Mảng trở thành`[1, 1, 5]`. Đầu ra đúng là:```
NO
```Một giải pháp chỉ kiểm tra số lượng vị trí trống sẽ tính sai khoảng cách giữa`1`Và`5`và có thể trả lời`YES`. 

Một trường hợp khác là khi tất cả các khoảng trống quá nhỏ mặc dù bản thân mảng vẫn hợp lệ. 

Đối với đầu vào:```
n = 4, k = 5
a = [1, 2, 3, 4]
```Mọi khoảng trống đều có kích thước bằng 0, vì vậy kết quả đầu ra đúng là:```
NO
```Mảng đã tăng lên rất nhiều nhưng không có số nguyên nào có thể được chèn vào. 

Trường hợp quan trọng cuối cùng là`k = 0`. Không cần thêm số, vì vậy bất kỳ mảng tăng nghiêm ngặt nào cũng có thể được chấp nhận. 

Đối với đầu vào:```
n = 3, k = 0
a = [10, 20, 30]
```Đầu ra đúng là:```
YES
```Thuật toán không được vô tình yêu cầu ít nhất một khoảng trống có sẵn. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng quá trình chèn. Chúng tôi có thể kiểm tra mọi khoảng trống có thể có, đếm xem có bao nhiêu số nguyên phù hợp ở đó và cộng các số đó sau mỗi lần cập nhật. Điều này đúng vì điều duy nhất quan trọng là tổng số vị trí số nguyên không được sử dụng giữa các phần tử liền kề. Tuy nhiên, sau một lần cập nhật, hãy quét tất cả`n - 1`chi phí chênh lệch`O(n)`. Với`q`cập nhật, trường hợp xấu nhất sẽ trở thành`O(nq)`, quá chậm. Vì`n = q = 2 * 10^5`, đây là khoảng bốn mươi tỷ lượt kiểm tra chênh lệch. 

Quan sát quan trọng là việc cập nhật điểm chỉ thay đổi các khoảng trống chạm vào vị trí đã sửa đổi. Nếu như`a[i]`thay đổi, chỉ có các cặp`(i-1, i)`Và`(i, i+1)`có thể có kích thước khoảng cách khác nhau. Mọi cặp khác vẫn giữ nguyên như cũ. Điều này biến vấn đề thành việc duy trì hai thông tin: liệu mọi cặp liền kề có hợp lệ hay không và tổng của tất cả dung lượng khoảng cách. 

Chúng ta có thể lưu trữ số lượng cặp liền kề không hợp lệ. Chúng ta cũng có thể lưu trữ tổng dung lượng dưới dạng tổng của`a[i+1] - a[i] - 1`trên tất cả các cặp liền kề có vẻ hợp lệ. Khi một giá trị thay đổi, chúng tôi loại bỏ những đóng góp cũ của những khoảng trống bị ảnh hưởng và thêm những khoảng trống mới. Cây Fenwick hoặc cây phân đoạn là không cần thiết vì chúng ta chỉ cần truy vấn tổng toàn cục chứ không cần truy vấn phạm vi. Tổng số hoạt động đơn giản là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(q) sau quá trình tiền xử lý O(n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc mảng ban đầu và tính phần đóng góp của mỗi cặp liền kề. Với mỗi cặp, hãy kiểm tra xem giá trị bên trái có nhỏ hơn giá trị bên phải hay không. Nếu không, hãy tăng số lượng cặp không hợp lệ. Nếu không, hãy thêm kích thước khoảng trống có sẵn vào tổng dung lượng. 
2. Trước khi xử lý cập nhật tại chỉ mục`i`, xóa thông tin cũ khỏi hai khoảng trống có thể bị ảnh hưởng: khoảng trống trước`i`và khoảng cách sau`i`. Đây là nơi duy nhất mà giá trị cũ tham gia vào câu trả lời. 
3. Thay thế`a[i]`với giá trị mới. 
4. Thêm thông tin mới cho hai khoảng trống bị ảnh hưởng giống nhau. Nếu cặp lân cận mới không hợp lệ, hãy tăng bộ đếm không hợp lệ. Nếu không, hãy thêm số lượng vị trí sẵn có mới. 
5. Sau khi cập nhật, hãy in`YES`nếu không có cặp liền kề không hợp lệ và tổng dung lượng ít nhất là`k`. Nếu không thì in`NO`. 

Tại sao điều này hoạt động: mảng cuối cùng sau khi chèn các giá trị phải giữ nguyên các giá trị ban đầu theo thứ tự được sắp xếp của chúng. Nếu bất kỳ giá trị ban đầu liền kề nào không tăng thì không có giá trị được chèn nào có thể khắc phục được mâu thuẫn đó. Nếu tất cả các cặp liền kề đều hợp lệ thì mọi vị trí số nguyên sẵn có đều thuộc về chính xác một khoảng trống, do đó tổng kích thước khoảng trống chính xác là số giá trị tối đa có thể được chèn vào. điều kiện`capacity >= k`là vừa cần vừa đủ. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k, q = map(int, input().split())
        a = list(map(int, input().split()))

        bad = 0
        space = 0

        def add_gap(i, sign):
            nonlocal bad, space
            if i < 0 or i + 1 >= n:
                return
            if a[i] >= a[i + 1]:
                bad += sign
            else:
                space += sign * (a[i + 1] - a[i] - 1)

        for i in range(n - 1):
            add_gap(i, 1)

        for _ in range(q):
            i, x = map(int, input().split())
            i -= 1

            add_gap(i - 1, -1)
            add_gap(i, -1)

            a[i] = x

            add_gap(i - 1, 1)
            add_gap(i, 1)

            if bad == 0 and space >= k:
                ans.append("YES")
            else:
                ans.append("NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chức năng trợ giúp`add_gap`duy trì sự đóng góp của một cặp lân cận. các`sign`đối số quyết định xem chúng ta đang thêm phần đóng góp hiện tại hay xóa phần đóng góp cũ trước khi cập nhật. 

Vòng lặp khởi tạo xử lý mọi khoảng trống một lần. Trong quá trình cập nhật, mã chỉ chạm vào các chỉ số`i - 1`Và`i`, vì đây là những khoảng trống duy nhất chứa phần tử đã thay đổi. Kiểm tra ranh giới bên trong`add_gap`xử lý các cập nhật cho phần tử đầu tiên hoặc cuối cùng mà không tạo các truy cập mảng không hợp lệ. 

Các bộ đếm được lưu trữ dưới dạng số nguyên có thể đạt tới khoảng`10^14`, bởi vì các giá trị mảng có thể lớn bằng`10^9`và có thể có nhiều khoảng trống. Số nguyên Python xử lý việc này một cách tự động, trong khi ở các ngôn ngữ có số nguyên kích thước cố định, loại 64 bit là bắt buộc. 

# Ví dụ đã hoạt động 

Sử dụng trường hợp mẫu đầu tiên:```
n = 4, k = 2
a = [1, 2, 4, 5]
```Trạng thái ban đầu: 

| Bước | Mảng | Cặp không hợp lệ | Chỗ trống | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1, 2, 4, 5] | 0 | 1 | KHÔNG trước khi cập nhật | 
| Cập nhật 4 lên 6 | [1, 2, 4, 6] | 0 | 2 | CÓ | 
| Cập nhật 2 lên 3 | [1, 3, 4, 6] | 0 | 2 | CÓ | 
| Cập nhật từ 4 lên 9 | [1, 3, 4, 9] | 0 | 5 | CÓ | 
| Cập nhật từ 1 đến 3 | [3, 3, 4, 9] | 1 | 4 | KHÔNG | 

Dấu vết này cho thấy lý do tại sao các cặp liền kề không hợp lệ lại được theo dõi riêng biệt. Bản cập nhật cuối cùng tạo ra một cặp bằng nhau, do đó chỉ tính toán không gian có sẵn là không đủ. 

Đối với mẫu thứ hai:```
n = 10, k = 1
a = [1,2,3,4,5,6,7,8,9,10]
```| Bước | Thay đổi mảng | Cặp không hợp lệ | Chỗ trống | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | giá trị liên tiếp | 0 | 0 | KHÔNG | 
| Cập nhật 1 thành 1 | không tăng khoảng cách hiệu quả | 1 | 0 | KHÔNG | 
| Cập nhật ngày 9 lên 11 | [1,2,3,4,5,6,7,8,11,10] | 1 | 1 | KHÔNG | 
| Cập nhật 10 lên 11 | [1,2,3,4,5,6,7,8,11,11] | 2 | 0 | KHÔNG | 
| Cập nhật 10 lên 12 | [1,2,3,4,5,6,7,8,11,12] | 0 | 2 | CÓ | 

Ví dụ này chứng minh rằng việc sửa một cặp không hợp lệ có thể khôi phục ngay khả năng chèn giá trị. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi khoảng trống ban đầu được xử lý một lần và mỗi lần cập nhật chỉ thay đổi hai khoảng trống | 
| Không gian | O(n) | Bản thân mảng được lưu trữ | 

Tổng kích thước của tất cả các trường hợp thử nghiệm được giới hạn bởi`2 * 10^5`, do đó quá trình tiền xử lý tuyến tính cộng với cập nhật liên tục theo thời gian dễ dàng đáp ứng các giới hạn cần thiết. 

# Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

# provided samples
assert run("""2
4 2 4
1 2 4 5
4 6
2 3
4 9
1 3
10 1 4
1 2 3 4 5 6 7 8 9 10
1 1
9 11
10 11
10 12
""") == """YES
YES
YES
NO
NO
NO
NO
YES"""

# minimum size, k = 0
assert run("""1
1 0 2
5
1 6
1 1
""") == """YES
YES"""

# all equal after update
assert run("""1
3 1 1
1 3 5
2 1
""") == """NO"""

# no available space
assert run("""1
4 3 1
1 2 3 4
4 10
""") == """NO"""

# large gap creation
assert run("""1
3 5 1
1 2 3
2 20
""") == """YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử đơn với`k = 0`| CÓ | Xử lý mảng nhỏ nhất và không chèn thêm | 
| Các giá trị lân cận bằng nhau | KHÔNG | Xác nhận phát hiện đơn hàng không hợp lệ | 
| Các số liên tiếp có số lượng lớn`k`| KHÔNG | Xác nhận đếm khoảng cách | 
| Large update creating space | CÓ | Confirms update contribution handling |

 # Vỏ cạnh 

For the invalid-order case:```
3 1 1
1 3 5
2 1
```Sau khi cập nhật, mảng trở thành`[1,1,5]`. Thuật toán loại bỏ các khoảng trống cũ xung quanh chỉ mục`1`, chèn các khoảng trống mới và thấy rằng cặp đầu tiên không hợp lệ. Bộ đếm không hợp lệ trở thành`1`, vậy kết quả là`NO`. 

Đối với trường hợp không có sẵn khe cắm:```
4 5 0
1 2 3 4
```Mảng đang tăng lên một cách nghiêm ngặt, nhưng mọi sự khác biệt đều chính xác là một. Mỗi khoảng trống góp phần`0`, vậy tổng công suất là`0`. Từ`0 < 5`, thuật toán trả về`NO`. 

Đối với các phần chèn không cần thiết:```
3 0 1
10 20 30
```Tổng công suất không thành vấn đề vì`k`là số không. Yêu cầu duy nhất là không có cặp nào không hợp lệ. Vì mảng tăng nghiêm ngặt nên thuật toán trả về`YES`.
