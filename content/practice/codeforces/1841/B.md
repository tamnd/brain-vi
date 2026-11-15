---
title: "CF 1841B - Giữ Đẹp"
description: "Chúng ta bắt đầu với một mảng trống và nhận từng số một. Đối với mỗi giá trị x đến, chúng ta phải quyết định xem việc thêm nó vào mảng hiện tại có giữ cho mảng \"đẹp\" hay không. Nếu có, chúng tôi sẽ nối nó vĩnh viễn. Nếu không chúng tôi bỏ qua nó."
date: "2026-06-09T06:20:24+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1841
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 150 (Rated for Div. 2)"
rating: 1000
weight: 1841
solve_time_s: 106
verified: true
draft: false
---

[CF 1841B - Giữ nó đẹp](https://codeforces.com/problemset/problem/1841/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng trống và nhận từng số một. 

Đối với mỗi giá trị đến`x`, chúng ta phải quyết định xem việc thêm nó vào mảng hiện tại có giữ cho mảng đó "đẹp" hay không. Nếu có, chúng tôi sẽ nối nó vĩnh viễn. Nếu không chúng tôi bỏ qua nó. Sau mỗi truy vấn chúng tôi xuất ra`1`nếu giá trị được thêm vào và`0`nếu nó bị từ chối. 

Một mảng đẹp có cấu trúc rất cụ thể. Phải có khả năng xoay mảng để nó được sắp xếp theo thứ tự không giảm. Một cách khác để nói điều này là mảng là một chuỗi được sắp xếp có thể bị cắt ở một vị trí và quấn quanh. 

Ví dụ:`[1,2,3,4,5]`thật đẹp vì nó đã được sắp xếp rồi.`[3,7,7,9,2,3]`đẹp vì quay ở vị trí trước đó`2`cho`[2,3,3,7,7,9]`.`[5,2,2,1]`không đẹp vì không có phép quay nào tạo ra chuỗi được sắp xếp. 

Tổng số truy vấn trên tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Bất kỳ giải pháp nào quét lại toàn bộ mảng sau mỗi lần chèn sẽ yêu cầu khoảng$$O(q^2)$$hoạt động trong trường hợp xấu nhất, có thể đạt tới khoảng`4 · 10^10`hoạt động. Điều đó vượt xa giới hạn. 

Chúng ta cần một giải pháp xử lý từng truy vấn theo thời gian không đổi hoặc logarit. 

Một số trường hợp khó xử lý. 

Coi như:```
1
5
1 2 3 0 2
```Mảng được chấp nhận trở thành`[1,2,3,0]`. Sau khi điểm xoay đã xuất hiện, hãy nối thêm`2`hợp lệ vì đoạn thứ hai vẫn không giảm và`2 ≤ 1`là sai. Việc triển khai bất cẩn chỉ kiểm tra thứ tự cục bộ sẽ chấp nhận các giá trị không hợp lệ. 

Một trường hợp phức tạp khác là:```
1
4
5 1 2 6
```Sau khi chấp nhận`5`Và`1`, mảng là`[5,1]`, đã chứa phần thả duy nhất. Đang bổ sung`6`phải bị từ chối vì thứ tự sắp xếp xoay vòng sẽ bị hỏng. Chỉ nhìn vào phần tử được chấp nhận cuối cùng sẽ cho phép nó một cách không chính xác. 

Bản sao cũng quan trọng:```
1
5
1 1 1 1 1
```Mọi giá trị đều phải được chấp nhận. So sánh nghiêm ngặt thay vì so sánh không nghiêm ngặt sẽ từ chối các phần chèn hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu tuân theo định nghĩa trực tiếp. 

Sau mỗi truy vấn, hãy tạm thời thêm giá trị mới và kiểm tra xem mảng kết quả có đẹp không. Để làm điều đó, hãy tìm mọi phép xoay có thể và kiểm tra xem mảng đã xoay đã được sắp xếp chưa. Đối với một mảng có độ dài`k`, chi phí này`O(k²)`thời gian. Ngay cả một phiên bản được cải tiến một chút để kiểm tra tất cả các phép quay trong`O(k)`vẫn dẫn đến`O(q²)`tổng công việc vì bài kiểm tra được lặp lại sau mỗi truy vấn. 

Quan sát quan trọng là một mảng đẹp có thể chứa tối đa một vị trí có thứ tự giảm dần. 

Đối với một mảng được sắp xếp xoay:```
3 7 7 9 2 3
```có đúng một giọt:```
9 -> 2
```Mọi thứ trước khi giảm đều không giảm, và mọi thứ sau khi giảm cũng không giảm. 

Trong khi xây dựng mảng, chúng ta chỉ cần biết liệu phần thả đó đã xuất hiện hay chưa. 

Đặt giá trị được chấp nhận đầu tiên là`first`. 

Trước khi sự sụt giảm xuất hiện, mảng chỉ đơn giản là không giảm. Bất kỳ giá trị nào ít nhất lớn bằng giá trị được chấp nhận cuối cùng đều có thể được thêm vào. 

Khi chúng tôi thấy một giá trị nhỏ hơn giá trị được chấp nhận cuối cùng, chúng tôi đang cố gắng tạo điểm xoay duy nhất. Điều này chỉ được phép nếu giá trị mới không lớn hơn`first`. Nếu không thì không có phép quay nào có thể làm cho mảng được sắp xếp. 

Sau khi tồn tại sự sụt giảm, mọi giá trị trong tương lai phải thỏa mãn hai điều kiện: 

Nó phải giữ cho phân đoạn thứ hai không giảm, do đó ít nhất nó phải bằng giá trị cuối cùng hiện tại. 

Nó phải nằm trong phạm vi của đoạn thứ hai, vì vậy nó không thể vượt quá`first`. 

Những điều kiện đó đủ để mô tả mọi lần chèn hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q²) | O(q) | Quá chậm | 
| Tối ưu | O(q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một mảng được chấp nhận trống. 
2. Chấp nhận truy vấn đầu tiên vô điều kiện. Mảng một phần tử luôn đẹp. 
3. Lưu trữ giá trị được chấp nhận đầu tiên dưới dạng`first`. 
4. Duy trì một boolean`wrapped`.`wrapped = False`nghĩa là điểm quay chưa xuất hiện.`wrapped = True`có nghĩa là chúng tôi đã tạo ra phần thả duy nhất. 
5. Đối với mỗi giá trị mới`x`, kiểm tra giá trị được chấp nhận cuối cùng hiện tại. 
6. Nếu`wrapped`là`False`:

Nếu như`x >= last`, chấp nhận nó. Mảng vẫn không giảm. 

Nếu không, chúng tôi đang cố gắng tạo điểm xoay. Chỉ chấp nhận nó khi`x <= first`. Nếu được chấp nhận thì đặt`wrapped = True`. 
7. Nếu`wrapped`là`True`:

Chấp nhận`x`chỉ khi cả hai điều kiện giữ:`x >= last`

`x <= first`Điều kiện đầu tiên bảo toàn thứ tự không giảm của đoạn thứ hai. Điều kiện thứ hai giữ cho mọi phần tử sau điểm xoay không lớn hơn phần đầu của đoạn đầu tiên. 
8. Đầu ra`1`cho các giá trị được chấp nhận và`0`cho các giá trị bị từ chối. 

### Tại sao nó hoạt động 

Điều bất biến là mảng được chấp nhận luôn bao gồm một phân đoạn được sắp xếp hoặc hai phân đoạn được sắp xếp cách nhau đúng một giọt. 

Khi`wrapped`là`False`, mảng vẫn chưa sử dụng phần thả của nó. Chúng tôi có thể tiếp tục phân đoạn đã sắp xếp hoặc tạo phần thả duy nhất bằng cách chèn một giá trị không lớn hơn phần tử đầu tiên. 

Khi`wrapped`là`True`, phần thả đã tồn tại. Bất kỳ phần tử tương lai nào đều thuộc về phân đoạn thứ hai. Nó phải lớn ít nhất bằng phần tử trước đó trong phân đoạn đó và lớn nhất bằng phần tử đầu tiên của mảng. Nếu không, việc sắp xếp thứ tự xoay vòng sẽ trở thành không thể. 

Mọi phần chèn được chấp nhận sẽ bảo tồn bất biến này và mọi phần chèn bị từ chối sẽ vi phạm nó. Vì một mảng đẹp chính xác là một mảng có nhiều nhất một phần giảm như vậy và các ràng buộc biên tương ứng, nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        q = int(input())
        xs = list(map(int, input().split()))

        ans = []

        arr = []
        wrapped = False
        first = None

        for x in xs:
            if not arr:
                arr.append(x)
                first = x
                ans.append('1')
                continue

            last = arr[-1]

            ok = False

            if not wrapped:
                if x >= last:
                    ok = True
                elif x <= first:
                    ok = True
                    wrapped = True
            else:
                if last <= x <= first:
                    ok = True

            if ok:
                arr.append(x)
                ans.append('1')
            else:
                ans.append('0')

        print(''.join(ans))

if __name__ == "__main__":
    solve()
```Giá trị được chấp nhận đầu tiên trở thành điểm tham chiếu cố định`first`. Mọi quyết định sau này đều dựa trên mối quan hệ giữa giá trị đến, giá trị được chấp nhận cuối cùng hiện tại và`first`. 

Biến`wrapped`ghi lại liệu mức giảm duy nhất đã được sử dụng hay chưa. Trước khi được sử dụng, mảng hoạt động giống như một chuỗi không giảm thông thường. Sau khi được sử dụng, mọi giá trị tương lai phải nằm trong khoảng`[last, first]`. 

Một chi tiết tinh tế đó là`wrapped`chỉ được thay đổi khi giá trị tạo ra sự sụt giảm thực sự được chấp nhận. Các giá trị bị từ chối không được ảnh hưởng đến trạng thái. 

Một chi tiết khác là việc sử dụng các so sánh không nghiêm ngặt. Cho phép các giá trị bằng nhau vì thứ tự mục tiêu không giảm thay vì tăng nghiêm ngặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Trình tự đầu vào:```
3 7 7 9 2 4 6 3 4
```| x | cuối cùng | đầu tiên | bọc trước | quyết định | 
| --- | --- | --- | --- | --- | 
| 3 | - | 3 | Sai | 1 | 
| 7 | 3 | 3 | Sai | 1 | 
| 7 | 7 | 3 | Sai | 1 | 
| 9 | 7 | 3 | Sai | 1 | 
| 2 | 9 | 3 | Sai | 1, bọc trở thành True | 
| 4 | 2 | 3 | Đúng | 0 | 
| 6 | 2 | 3 | Đúng | 0 | 
| 3 | 2 | 3 | Đúng | 1 | 
| 4 | 3 | 3 | Đúng | 0 | 

Đầu ra:```
111110010
```Ví dụ này cho thấy thời điểm thả duy nhất`9 -> 2`được tạo ra. Sau thời điểm đó, mọi giá trị được chấp nhận phải duy trì tối đa`3`, phần tử đầu tiên 

### Ví dụ 2 

Trình tự đầu vào:```
3 2 1 2 3
```| x | cuối cùng | đầu tiên | bọc trước | quyết định | 
| --- | --- | --- | --- | --- | 
| 3 | - | 3 | Sai | 1 | 
| 2 | 3 | 3 | Sai | 1, bọc trở thành True | 
| 1 | 2 | 3 | Đúng | 0 | 
| 2 | 2 | 3 | Đúng | 1 | 
| 3 | 2 | 3 | Đúng | 1 | 

Đầu ra:```
11011
```Dấu vết này chứng tỏ rằng một khi sự sụt giảm tồn tại, các giá trị nhỏ hơn phân đoạn thứ hai hiện tại sẽ bị từ chối, ngay cả khi chúng không lớn hơn`first`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được xử lý với số lượng so sánh không đổi | 
| Không gian | O(q) | Mảng được chấp nhận có thể lưu trữ tất cả các giá trị được chấp nhận | 

Vì tổng của tất cả các truy vấn nhiều nhất là`2 · 10^5`, xử lý tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng thoải mái trong phạm vi 512 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        q = int(input())
        xs = list(map(int, input().split()))

        arr = []
        wrapped = False
        first = None
        ans = []

        for x in xs:
            if not arr:
                arr.append(x)
                first = x
                ans.append('1')
                continue

            last = arr[-1]
            ok = False

            if not wrapped:
                if x >= last:
                    ok = True
                elif x <= first:
                    ok = True
                    wrapped = True
            else:
                if last <= x <= first:
                    ok = True

            if ok:
                arr.append(x)
                ans.append('1')
            else:
                ans.append('0')

        out.append(''.join(ans))

    return "\n".join(out)

# provided samples
assert run(
"""3
9
3 7 7 9 2 4 6 3 4
5
1 1 1 1 1
5
3 2 1 2 3
"""
) == """111110010
11111
11011"""

# minimum size
assert run(
"""1
1
42
"""
) == "1"

# all equal values
assert run(
"""1
5
7 7 7 7 7
"""
) == "11111"

# wrap occurs immediately
assert run(
"""1
4
5 1 2 6
"""
) == "1110"

# catches boundary x == first
assert run(
"""1
5
3 5 1 2 3
"""
) == "11111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`42`|`1`| Mảng một phần tử | 
|`7 7 7 7 7`|`11111`| Xử lý bình đẳng | 
|`5 1 2 6`|`1110`| Từ chối các giá trị lớn hơn`first`sau khi bọc | 
|`3 5 1 2 3`|`11111`| Trường hợp ranh giới trong đó`x == first`hợp lệ | 

## Vỏ cạnh 

Hãy xem xét:```
1
4
5 1 2 6
```Thuật toán chấp nhận`5`, sau đó chấp nhận`1`bởi vì`1 <= first`. Điều này tạo ra điểm xoay và đặt`wrapped = True`. 

giá trị`2`thỏa mãn`1 <= 2 <= 5`, nên nó được chấp nhận. 

giá trị`6`vi phạm`6 <= first`, nên nó bị từ chối. 

Đầu ra là:```
1110
```Điều này hoàn toàn chính xác vì một mảng được sắp xếp xoay không thể chứa phần tử lớn hơn phân đoạn đầu tiên sau khi quá trình bao bọc xảy ra. 

Bây giờ hãy xem xét các bản sao:```
1
5
1 1 1 1 1
```Mọi phép chèn đều thỏa mãn điều kiện không giảm. Không bao giờ cần bọc. 

Đầu ra là:```
11111
```Sử dụng so sánh nghiêm ngặt sẽ từ chối các giá trị bằng nhau một cách không chính xác. 

Cuối cùng hãy xem xét:```
1
5
3 4 5 1 0
```Sau khi chấp nhận`1`, gói đã tồn tại. 

Giá trị cuối cùng`0`nhỏ hơn giá trị được chấp nhận cuối cùng hiện tại`1`, vì vậy nó sẽ tạo ra giọt thứ hai. Thuật toán từ chối nó vì`0 >= last`là sai. 

Đầu ra trở thành:```
11110
```Điều này chứng tỏ tính bất biến trung tâm: một mảng đẹp có thể chỉ chứa một phần giảm. Khi mức giảm đó đã được sử dụng, tất cả các giá trị sau này phải nằm trong phân đoạn được sắp xếp thứ hai.
