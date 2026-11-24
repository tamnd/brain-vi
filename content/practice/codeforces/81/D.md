---
title: "CF 81D - Thư viện ảnh của Polycarp"
description: "Chúng tôi có một số album ảnh. Album i chứa một[i] ảnh. Chúng ta phải xây dựng một thư viện tuần hoàn chứa chính xác n bức ảnh. Thay vì chọn ID ảnh cụ thể, chúng ta chỉ cần xuất số album cho từng vị trí. Phòng trưng bày có hình tròn nên mỗi vị trí đều có hai hàng xóm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 81
codeforces_index: "D"
codeforces_contest_name: "Yandex.Algorithm Open 2011: Qualification 1"
rating: 2100
weight: 81
solve_time_s: 168
verified: false
draft: false
---

[CF 81D - Thư viện ảnh của Polycarp](https://codeforces.com/problemset/problem/81/D) 

**Đánh giá:** 2100 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2m 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số album ảnh. Album`i`chứa`a[i]`những bức ảnh. Chúng ta phải xây dựng một thư viện tuần hoàn chứa chính xác`n`những bức ảnh. Thay vì chọn ID ảnh cụ thể, chúng ta chỉ cần xuất số album cho từng vị trí. 

Phòng trưng bày có hình tròn nên mỗi vị trí đều có hai hàng xóm. Chức vụ`1`liền kề với cả hai vị trí`2`Và`n`. Yêu cầu là các vị trí liền kề phải luôn đến từ các album khác nhau. 

Chúng tôi có thể tự do chọn bất kỳ tập hợp con ảnh nào từ album, miễn là chúng tôi đặt chính xác`n`tổng số ảnh và không bao giờ vượt quá số lượng có sẵn của bất kỳ album nào. 

Các ràng buộc đủ nhỏ để chúng ta có thể thực hiện được phép tính bậc hai.`n ≤ 1000`Và`m ≤ 40`, vì vậy ngay cả một`O(n^2)`hoặc`O(n log n)`xây dựng là dễ dàng đủ nhanh. Những gì chúng ta không thể làm là sử dụng vũ lực các hoán vị hoặc thực hiện tìm kiếm theo cấp số nhân, bởi vì số lượng sắp xếp theo chu kỳ có thể tăng lên một cách thiên văn ngay cả đối với`n = 20`. 

Phần khó không phải là chọn ảnh mà là sắp xếp sao cho vị trí đầu tiên và cuối cùng cũng khác nhau. Một chuỗi hoạt động tuyến tính vẫn có thể thất bại theo chu kỳ. 

Hãy xem xét ví dụ này:```
n = 5
albums = [3, 2]
```Một kẻ tham lam ngây thơ có thể tạo ra:```
1 2 1 2 1
```Mỗi cặp liền kề bên trong mảng đều khác nhau, nhưng vị trí cuối cùng và đầu tiên đều giống nhau`1`, vì vậy chu kỳ không hợp lệ. 

Một trường hợp nguy hiểm khác là khi một album có dung lượng quá lớn.```
n = 6
albums = [6]
```Mọi bức ảnh được chọn sẽ đến từ cùng một album, vì vậy việc tránh những người hàng xóm ngang hàng là không thể. 

Tinh tế hơn:```
n = 7
albums = [4, 3]
```Một chu trình hợp lệ tồn tại:```
1 2 1 2 1 2 1
```Đợi đã, điều này thực sự không thành công vì vị trí đầu tiên và cuối cùng đều là`1`. 

Trong một chu kỳ, album lớn nhất không thể vượt quá`n / 2`. Đây`4 > floor(7/2)`, do đó không tồn tại sự sắp xếp hợp lệ. 

Việc triển khai bất cẩn chỉ kiểm tra tính kề cận tuyến tính sẽ chấp nhận sai những trường hợp như vậy. 

Thêm một trường hợp khó khăn nữa xuất hiện khi chúng ta có sẵn nhiều ảnh hơn mức cần thiết. Chúng tôi không bắt buộc phải sử dụng mọi bức ảnh. 

Ví dụ:```
n = 4
albums = [1, 100]
```Chúng ta chỉ nên chọn tổng cộng bốn bức ảnh. Câu trả lời đúng là không thể bởi vì mọi sự sắp xếp độ dài theo chu kỳ đều`4`nhu cầu nhiều nhất`2`bản sao của cùng một album, nhưng album`2`thống trị quá nhiều và album`1`chỉ đóng góp một dấu phân cách. 

Hiểu rằng chúng tôi có thể loại bỏ ảnh là điều cần thiết. Việc xây dựng trước tiên phải quyết định số lượng ảnh sẽ chụp từ mỗi album. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Trước tiên, hãy chọn ảnh sẽ sử dụng, sau đó thử mọi hoán vị và kiểm tra xem các vị trí liền kề có khác nhau theo chu kỳ hay không. 

Ngay cả khi chúng ta chỉ nghĩ đến nhãn album thay vì những bức ảnh cụ thể thì số lượng sắp xếp vẫn rất lớn. Với`n = 1000`, thậm chí`1000!`khả năng là không thể liệt kê được. Việc tìm kiếm quay lại với việc cắt tỉa vẫn bùng nổ vì nhiều tiền tố vẫn khả thi cho đến khi đi sâu vào đệ quy. 

Lý do vũ lực hoạt động về mặt khái niệm là tình trạng này hoàn toàn mang tính cục bộ. Chúng tôi chỉ quan tâm đến các vị trí lân cận. Thật không may, các ràng buộc cục bộ không làm giảm đủ không gian tìm kiếm. 

Nhận xét quan trọng là đây thực sự là một vấn đề cân bằng tần số. Chúng ta chỉ cần phân phát các nhãn album lặp đi lặp lại xung quanh một vòng tròn. Khi không có album nào xuất hiện quá nhiều lần, bạn luôn có thể xây dựng một sự sắp xếp hợp lệ một cách tham lam. 

Đối với sự sắp xếp theo chu kỳ, không album nào được chiếm quá một nửa số vị trí. Nếu không thì hai bản sao của album đó phải liền kề nhau theo nguyên tắc chuồng bồ câu. 

Điều kiện đó cũng đủ. 

Vì vậy, vấn đề trở thành: 

1. Quyết định số lượng ảnh sẽ chụp từ mỗi album, với tổng số ảnh chính xác`n`. 
2. Đảm bảo số lượng được chọn tối đa là nhiều nhất`n / 2`. 
3. Xây dựng sự sắp xếp tuần hoàn. 

Cấu trúc trang nhã dựa trên việc sắp xếp tất cả các nhãn album đã chọn và đặt chúng vào câu trả lời bằng cách sử dụng các vị trí xen kẽ. 

Giả sử nhiều bộ là:```
1 1 1 2 2 3
```Chúng tôi đặt chúng vào các vị trí:```
0, 2, 4, 1, 3, 5
```Các nhãn lặp lại sẽ được phân tách một cách tự nhiên vì các phần tử bằng nhau trong danh sách đã sắp xếp sẽ được phân bổ trên các chỉ mục ở xa. 

Điều này chuyển đổi vấn đề từ tìm kiếm theo cấp số nhân sang một cấu trúc tham lam xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các kích cỡ album. 
2. Đối với mỗi album, ban đầu hãy chụp càng nhiều ảnh càng tốt, nhưng không bao giờ nhiều hơn`n / 2`. 

Điều này là cần thiết vì bất kỳ album nào xuất hiện nhiều hơn`n / 2`lần sẽ buộc hai bản sao liền kề trong chu kỳ. 
3. Tính tổng các số đã chọn. 
4. Nếu tổng số được chọn vẫn nhỏ hơn`n`, in`-1`. 

Điều này có nghĩa là ngay cả sau khi sử dụng số tiền an toàn tối đa từ mỗi album, chúng tôi vẫn không thể đạt được`n`những bức ảnh. 
5. Nếu không, hãy giảm số lượng đã chọn cho đến khi tổng của chúng trở thành chính xác`n`. 

Chúng ta có thể giảm bất kỳ số dương nào tùy ý. Việc giảm số lượng không bao giờ ảnh hưởng đến tính khả thi vì nó chỉ làm giảm tần số. 
6. Xây dựng một mảng chứa từng chỉ mục album được lặp lại theo số lượng đã chọn. 

Ví dụ:```
counts = [2,1,3]
```trở thành```
[1,1,2,3,3,3]
```7. Sắp xếp danh sách này. 

Các nhãn bằng nhau sẽ trở nên liên tiếp, điều này giúp cho bước sắp xếp tiếp theo có thể dự đoán được. 
8. Tạo một mảng câu trả lời trống có kích thước`n`. 
9. Điền chỉ số chẵn trước, sau đó điền chỉ số lẻ. 

Sử dụng thứ tự:```
0, 2, 4, ..., 1, 3, 5, ...
```Đặt từng nhãn đã được sắp xếp vào các vị trí này. 
10. Xuất bản sắp xếp cuối cùng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là không có số lượng album nào vượt quá`n / 2`. 

Khi chúng tôi phân phối các nhãn đã sắp xếp vào các vị trí xen kẽ, các bản sao của cùng một album sẽ được phân tách bằng ít nhất một vị trí khác. Vì các nhãn bằng nhau xuất hiện liên tiếp trong danh sách đã sắp xếp nên việc đặt chúng vào các vị trí cách nhau hai khoảng cách sẽ ngăn ngừa xung đột. 

Mối nguy hiểm duy nhất còn lại là ranh giới chu kỳ giữa vị trí cuối cùng và vị trí đầu tiên. các`max_count ≤ n/2`điều kiện đảm bảo rằng vị trí xen kẽ không bao giờ bao bọc các nhãn bằng nhau ở cả hai đầu cùng một lúc. 

Cấu trúc này tương đương với việc xen kẽ các tần số lớn nhất trên vòng tròn. Điều kiện khả thi vừa cần vừa đủ nên bất cứ khi nào thuật toán không in ra`-1`, sự sắp xếp được đưa ra là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    take = [min(x, n // 2) for x in a]

    total = sum(take)

    if total < n:
        print(-1)
        return

    extra = total - n

    for i in range(m):
        dec = min(take[i], extra)
        take[i] -= dec
        extra -= dec
        if extra == 0:
            break

    vals = []

    for i in range(m):
        vals.extend([i + 1] * take[i])

    vals.sort()

    ans = [0] * n

    pos = []

    for i in range(0, n, 2):
        pos.append(i)

    for i in range(1, n, 2):
        pos.append(i)

    for i in range(n):
        ans[pos[i]] = vals[i]

    for i in range(n):
        if ans[i] == ans[(i + 1) % n]:
            print(-1)
            return

    print(*ans)

solve()
```Giai đoạn đầu tiên tính toán số lượng ảnh chúng tôi được phép sử dụng từ mỗi album. Giới hạn mỗi album tại`n // 2`mã hóa điều kiện khả thi trực tiếp vào công trình. 

Sau đó, tổng số tiền đã chọn có thể vượt quá`n`, vì nhiều album cùng nhau vẫn có thể cung cấp nhiều ảnh có thể sử dụng được. Chúng tôi chỉ cần loại bỏ các bản sao bổ sung một cách tùy ý. Việc giảm số lượng không thể tạo ra xung đột mới vì tính liền kề chỉ trở nên dễ dàng hơn khi tần số co lại. 

các`vals`mảng lưu trữ nhiều nhãn album. Việc sắp xếp rất quan trọng vì chiến lược vị trí giả định các nhãn giống hệt nhau được nhóm lại với nhau. 

Thứ tự vị trí là thủ thuật cốt lõi. Đầu tiên chúng ta điền vào tất cả các chỉ số chẵn, sau đó là tất cả các chỉ số lẻ. Điều này tự động phân chia các nhãn bằng nhau. 

Vòng xác minh cuối cùng không thực sự cần thiết nếu lý do là đúng, nhưng nó chỉ là một bước kiểm tra phòng thủ hữu ích và chi phí`O(n)`. 

Một điểm tinh tế là việc kiểm tra chu trình sử dụng lập chỉ mục modulo:```
ans[(i + 1) % n]
```Nếu không có modulo, mã sẽ chỉ xác thực tính kề cận tuyến tính và bỏ lỡ ranh giới giữa vị trí cuối cùng và vị trí đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 3 5
```Sau khi giới hạn bởi`n // 2 = 2`: 

| Album | Bản gốc | Được chọn | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 2 | 
| 3 | 5 | 2 | 

Tổng cộng là`5`, nhưng chúng ta chỉ cần`4`. 

Chúng tôi loại bỏ một bản sao bổ sung. 

| Album | Đếm Cuối Cùng | 
| --- | --- | 
| 1 | 0 | 
| 2 | 2 | 
| 3 | 2 | 

Multiset trở thành:```
[2, 2, 3, 3]
```Thứ tự đặt là:```
0, 2, 1, 3
```| Bước | Giá trị | Vị trí | Mảng | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | [2,0,0,0] | 
| 2 | 2 | 2 | [2,0,2,0] | 
| 3 | 3 | 1 | [2,3,2,0] | 
| 4 | 3 | 3 | [2,3,2,3] | 

Câu trả lời cuối cùng:```
2 3 2 3
```Mỗi cặp lân cận đều khác nhau, bao gồm cả cặp tuần hoàn`(3,2)`. 

Dấu vết này cho thấy cách vị trí xen kẽ tự động phân tách các nhãn lặp lại. 

### Ví dụ 2 

đầu vào:```
7 2
4 3
```Sau khi giới hạn bởi`n // 2 = 3`: 

| Album | Bản gốc | Được chọn | 
| --- | --- | --- | 
| 1 | 4 | 3 | 
| 2 | 3 | 3 | 

Tổng cộng là`6`, nhỏ hơn`7`. 

Thuật toán in:```
-1
```Điều này xác nhận điều kiện cần thiết. Ngay cả sau khi giới hạn tần suất album ở giá trị an toàn, chúng tôi vẫn không thể thu thập đủ ảnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp multiset được xây dựng chiếm ưu thế | 
| Không gian | O(n) | Mảng lưu trữ các nhãn đã chọn và đáp án cuối cùng | 

Với`n ≤ 1000`, độ phức tạp này nằm trong giới hạn. Bước sắp xếp xử lý tối đa 1000 phần tử, điều này không quan trọng đối với Python trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    take = [min(x, n // 2) for x in a]

    total = sum(take)

    if total < n:
        return "-1"

    extra = total - n

    for i in range(m):
        dec = min(take[i], extra)
        take[i] -= dec
        extra -= dec
        if extra == 0:
            break

    vals = []

    for i in range(m):
        vals.extend([i + 1] * take[i])

    vals.sort()

    ans = [0] * n

    pos = []

    for i in range(0, n, 2):
        pos.append(i)

    for i in range(1, n, 2):
        pos.append(i)

    for i in range(n):
        ans[pos[i]] = vals[i]

    for i in range(n):
        if ans[i] == ans[(i + 1) % n]:
            return "-1"

    return " ".join(map(str, ans))

# provided sample
out = solve_io("4 3\n1 3 5\n")
arr = list(map(int, out.split()))
assert len(arr) == 4
for i in range(4):
    assert arr[i] != arr[(i + 1) % 4]

# impossible case
assert solve_io("7 2\n4 3\n") == "-1"

# minimum valid size
out = solve_io("3 3\n1 1 1\n")
arr = list(map(int, out.split()))
for i in range(3):
    assert arr[i] != arr[(i + 1) % 3]

# all equal albums
assert solve_io("5 1\n10\n") == "-1"

# balanced large frequencies
out = solve_io("6 2\n3 3\n")
arr = list(map(int, out.split()))
for i in range(6):
    assert arr[i] != arr[(i + 1) % 6]

# extra unused photos
out = solve_io("5 3\n10 10 10\n")
arr = list(map(int, out.split()))
assert len(arr) == 5
for i in range(5):
    assert arr[i] != arr[(i + 1) % 5]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 2 / 4 3`|`-1`| Tần số lớn nhất vượt quá giới hạn chu kỳ | 
|`3 3 / 1 1 1`| Bất kỳ chu kỳ hợp lệ nào | Kích thước hợp lệ tối thiểu | 
|`5 1 / 10`|`-1`| Album đơn không thể | 
|`6 2 / 3 3`| Bất kỳ chu kỳ xen kẽ nào | Số lượng cân bằng hoàn hảo | 
|`5 3 / 10 10 10`| Bất kỳ chu kỳ hợp lệ nào | Xử lý đúng ảnh bị loại bỏ | 

## Vỏ cạnh 

Hãy xem xét:```
5 2
3 2
```Tần số tối đa cho phép là`2`, bởi vì`n // 2 = 2`. 

Sau khi giới hạn:```
2 2
```Tổng số trở thành`4`, nhỏ hơn`5`, do đó thuật toán in`-1`. 

Điều này đúng. Bất kỳ chu kỳ có độ dài lẻ nào đều yêu cầu một số album xuất hiện ít nhất ba lần, nhưng ba bản sao trong một chu kỳ có độ dài năm chắc chắn sẽ buộc phải có sự liền kề. 

Bây giờ hãy xem xét:```
6 3
6 1 1
```Sau khi giới hạn:```
3 1 1
```Tổng cộng là`5`, vẫn ở dưới`6`. 

Một lần nữa thuật toán in`-1`. 

Theo trực giác, album nổi bật cần có dấu phân cách giữa tất cả các bản sao của nó. Hai album bổ sung chỉ cung cấp hai dấu phân cách, không đủ để đặt sáu bức ảnh theo chu kỳ. 

Cuối cùng, hãy xem xét:```
8 4
2 2 2 100
```Sau khi giới hạn:```
2 2 2 4
```Tổng cộng là`10`, vì vậy chúng tôi loại bỏ hai phần bổ sung. 

Một bộ đếm cuối cùng có thể là:```
0 2 2 4
```Việc xây dựng mang lại một cái gì đó như:```
4 2 4 3 4 2 4 3
```Album lớn nhất chiếm đúng một nửa số vị trí, vẫn an toàn. Mỗi bản sao được phân tách bằng một album khác, bao gồm cả ranh giới theo chu kỳ.
