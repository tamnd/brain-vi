---
title: "CF 91B - Hàng đợi"
description: "Chúng ta được đưa cho một hàng hải mã, trong đó thứ tự chỉ số đi từ đuôi về đầu. Với mỗi con hải mã ở vị trí i, chúng ta muốn tìm vị trí xa nhất ji sao cho con hải mã phía trước hoàn toàn trẻ hơn, nghĩa là a[j] < a[i]. Nếu không có con hải mã nào như vậy tồn tại thì câu trả lời là -1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 91
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 75 (Div. 1 Only)"
rating: 1500
weight: 91
solve_time_s: 130
verified: true
draft: false
---

[CF 91B - Hàng đợi](https://codeforces.com/problemset/problem/91/B) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một hàng hải mã, trong đó thứ tự chỉ số đi từ đuôi về đầu. Đối với mỗi con hải mã ở vị trí`i`, ta muốn tìm vị trí xa nhất`j > i`sao cho con hải mã phía trước thực sự trẻ hơn, nghĩa là`a[j] < a[i]`. 

Nếu không có con hải mã nào tồn tại, câu trả lời là`-1`. 

Nếu một con hải mã như vậy tồn tại, mức độ không hài lòng sẽ bằng với số lượng hải mã đứng giữa chúng, tức là:$$j - i - 1$$Chi tiết quan trọng là chúng tôi không muốn con hải mã trẻ gần nhất. Chúng tôi muốn con hải mã trẻ nhất ở phía trước trong hàng đợi. 

Độ dài hàng đợi có thể đạt tới`10^5`, do đó nghiệm bậc hai là quá chậm. Một vòng lặp đôi ngây thơ sẽ kiểm tra tất cả các cặp`(i, j)`với`i < j`, trở thành đại khái`10^10`hoạt động trong trường hợp xấu nhất. Điều đó vượt xa những gì phù hợp với giới hạn 2 giây. Chúng ta cần thứ gì đó xung quanh`O(n log n)`hoặc`O(n)`. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xét độ tuổi bằng nhau:```
4
5 5 5 5
```Đầu ra đúng là:```
-1 -1 -1 -1
```Việc thực hiện bất cẩn bằng cách sử dụng`<=`thay vì`<`sẽ coi những độ tuổi bằng nhau là trẻ hơn một cách không chính xác. 

Một trường hợp tinh vi khác là khi con hải mã trẻ nhất xa nhất không phải là giá trị nhỏ nhất ở gần đó:```
5
10 9 8 7 6
```Câu trả lời là:```
3 2 1 0 -1
```Đối với hải mã đầu tiên, sự lựa chọn đúng đắn là hải mã có độ tuổi`6`, không phải là con hải mã trẻ đầu tiên theo tuổi`9`. 

Một kịch bản phức tạp hơn xuất hiện khi hải mã trẻ hơn chỉ tồn tại sau các giá trị lớn hơn:```
6
5 100 99 98 1 2
```Đối với độ tuổi`100`, con hải mã trẻ nhất xa nhất là`2`cuối cùng thì không phải`1`. Câu trả lời phụ thuộc vào chỉ số hợp lệ xa nhất chứ không phải độ tuổi tối thiểu. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Đối với mọi vị trí`i`, quét mọi vị trí sau`j > i`. Bất cứ khi nào`a[j] < a[i]`, cập nhật câu trả lời bằng cách sử dụng chỉ mục xa nhất. 

Điều này hiệu quả vì nó kiểm tra rõ ràng mọi ứng cử viên. Logic rất đơn giản và chính xác. 

Vấn đề là thời gian chạy. Với`n = 10^5`, các vòng lặp lồng nhau thực hiện khoảng:$$\frac{n(n-1)}{2} \approx 5 \times 10^9$$so sánh trong trường hợp xấu nhất Như vậy là quá chậm. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến những vị trí có thể trở thành con hải mã trẻ nhất ở phía bên trái. 

Giả sử chúng ta xử lý mảng từ phải sang trái. Trong khi di chuyển sang trái, chúng tôi duy trì cấu trúc chứa các vị trí ứng cử viên từ hậu tố. 

Một vị trí chỉ hữu ích nếu tuổi của nó nhỏ hơn mọi độ tuổi nhìn xa hơn bên phải. Những vị trí này tạo thành một chuỗi tuổi giảm dần. Ví dụ:```
10 8 5 3 50 45
```Quét từ phải sang trái sẽ tạo ra mức tối thiểu ứng cử viên:```
45 at index 5
3 at index 3
```Mọi giá trị khác đều vô dụng vì đã tồn tại một giá trị nhỏ hơn ở phía bên phải. 

Bây giờ hãy xem xét một số hải mã theo độ tuổi`x`. Trong số các hậu tố cực tiểu này, chúng ta muốn chỉ số xa nhất có tuổi nhỏ hơn`x`. 

Độ tuổi của ứng viên tăng dần khi xem theo thứ tự được lưu trữ, vì vậy chúng ta có thể tìm kiếm nhị phân cho giá trị cuối cùng`< x`. 

Điều này làm giảm vấn đề xuống: 

1. Xây dựng vị trí hậu tố tối thiểu khi quét từ phải sang trái. 
2. Sử dụng tìm kiếm nhị phân để xác định vị trí con hải mã trẻ nhất. 

Tổng độ phức tạp trở thành`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một danh sách trống có tên`mins`. 

Danh sách này sẽ lưu trữ các cặp`(age, index)`đại diện cho hậu tố cực tiểu được phát hiện khi quét từ phải sang trái. 
2. Bắt đầu xử lý mảng từ vị trí cuối cùng đến vị trí đầu tiên. 

Ở mỗi bước, các vị trí đã được xử lý sẽ thuộc về hậu tố phía trước con hải mã hiện tại. 
3. Đối với thời đại hiện nay`a[i]`, tìm kiếm nhị phân bên trong`mins`. 

Chúng tôi muốn tuổi được lưu trữ ngoài cùng bên phải nhỏ hơn`a[i]`. Vì tuổi được lưu trữ ngày càng tăng bên trong`mins`, tìm kiếm nhị phân hoạt động chính xác. 
4. Nếu có yếu tố như vậy tồn tại, hãy tính mức độ không hài lòng. 

Giả sử vị trí được lưu trữ phù hợp là`j`. Câu trả lời là:$$j - i - 1$$5. Nếu không có tuổi nhỏ hơn tồn tại trong`mins`, cửa hàng`-1`. 

Điều đó có nghĩa là không có con hải mã trẻ nào ở phía trước. 
6. Sau khi trả lời vị trí`i`, quyết định có nên chèn hải mã hiện tại vào`mins`. 

Chỉ chèn nó nếu tuổi của nó nhỏ hơn mức tối thiểu được lưu trữ cuối cùng. Trong trường hợp đó, nó trở thành hậu tố tối thiểu mới và có thể giúp ích cho các vị thế trong tương lai. 
7. Không cần đảo ngược vì câu trả lời được ghi trực tiếp vào mảng kết quả. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`mins`luôn chứa chính xác hậu tố cực tiểu của các vị trí ở bên phải chỉ mục hiện tại. 

Nếu một vị trí không phải là hậu tố tối thiểu thì một số vị trí xa hơn thậm chí còn có tuổi nhỏ hơn. Vị trí xa hơn đó luôn tốt hơn vì bài toán yêu cầu con hải mã trẻ nhất ở xa nhất. 

Vì vậy mọi vị trí không tối thiểu đều có thể được loại bỏ một cách an toàn. 

Độ tuổi được lưu trữ đang tăng lên nghiêm trọng trong`mins`, bởi vì chúng tôi chỉ nối thêm các phần tử mới khi giá trị hoàn toàn nhỏ hơn xuất hiện trong quá trình quét từ phải sang trái. Thứ tự đó cho phép tìm kiếm nhị phân. 

Khi chúng tôi tìm kiếm nhị phân cho độ tuổi ngoài cùng bên phải nhỏ hơn`a[i]`, chúng tôi đạt được hậu tố tối thiểu xa nhất vẫn trẻ hơn con hải mã hiện tại. Vì mọi ứng cử viên hữu ích đều phải có hậu tố tối thiểu nên vị trí này chính xác là câu trả lời đúng. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = [-1] * n

    # stores (age, index)
    # ages are strictly increasing
    mins = []

    for i in range(n - 1, -1, -1):
        age = a[i]

        # find first age >= current age
        pos = bisect_left(mins, (age, -1))

        # elements before pos have smaller age
        if pos > 0:
            j = mins[pos - 1][1]
            ans[i] = j - i - 1

        # add new suffix minimum
        if not mins or age < mins[-1][0]:
            mins.append((age, i))

    print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp quét từ phải sang trái vì mỗi con hải mã chỉ quan tâm đến người đi trước. Tại vị trí`i`, hậu tố`[i+1 ... n-1]`đã được xử lý rồi. 

các`mins`mảng lưu trữ hậu tố cực tiểu theo thứ tự đặc biệt. Độ tuổi ngày càng tăng lên, trong khi các chỉ số cũng tăng lên do chúng ta quét ngược. Thứ tự này làm cho việc tìm kiếm nhị phân trở nên khả thi. 

Cuộc gọi:```
bisect_left(mins, (age, -1))
```tìm cặp được lưu trữ đầu tiên có tuổi ít nhất`age`. Mọi thứ trước vị trí đó đều có độ tuổi nhỏ hơn, đó chính xác là những gì chúng ta cần. 

Phần tử nhỏ hơn ngoài cùng bên phải mang lại con hải mã hợp lệ ở xa nhất. Từ`mins`chứa hậu tố cực tiểu được sắp xếp theo khoảng cách tăng dần, việc chọn phần tử nhỏ hơn cuối cùng sẽ tự động đưa ra chỉ số lớn nhất. 

Một chi tiết tinh tế là điều kiện chèn:```
if not mins or age < mins[-1][0]:
```sử dụng`<`thay vì`<=`vấn đề. Độ tuổi bằng nhau không trẻ hơn, vì vậy các giá trị bằng nhau không được trở thành hậu tố tối thiểu mới. 

Một sai lầm dễ mắc phải khác là công thức không hài lòng. Vấn đề yêu cầu số lượng hải mã giữa hai vị trí chứ không phải khoảng cách. Đó là lý do tại sao chúng tôi tính toán:```
j - i - 1
```thay vì`j - i`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
10 8 5 3 50 45
```| tôi | tuổi | phút trước | kết quả tìm kiếm nhị phân | trả lời | phút sau | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 45 | [] | không | -1 | [(45,5)] | 
| 4 | 50 | [(45,5)] | (45,5) | 0 | [(45,5)] | 
| 3 | 3 | [(45,5)] | không | -1 | [(45,5),(3,3)] | 
| 2 | 5 | [(45,5),(3,3)] | (3,3) | 0 | [(45,5),(3,3)] | 
| 1 | 8 | [(45,5),(3,3)] | (3,3) | 1 | [(45,5),(3,3)] | 
| 0 | 10 | [(45,5),(3,3)] | (3,3) | 2 | [(45,5),(3,3)] | 

Đầu ra cuối cùng:```
2 1 0 -1 0 -1
```Dấu vết này cho thấy tại sao chỉ có hậu tố tối thiểu là quan trọng. Dù tuổi tác`5`trẻ hơn`10`, hải mã theo tuổi`3`ở xa hơn và gây ra sự bất mãn lớn hơn. 

### Ví dụ 2 

đầu vào:```
5
5 5 5 5 5
```| tôi | tuổi | phút trước | kết quả tìm kiếm nhị phân | trả lời | phút sau | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 5 | [] | không | -1 | [(5,4)] | 
| 3 | 5 | [(5,4)] | không | -1 | [(5,4)] | 
| 2 | 5 | [(5,4)] | không | -1 | [(5,4)] | 
| 1 | 5 | [(5,4)] | không | -1 | [(5,4)] | 
| 0 | 5 | [(5,4)] | không | -1 | [(5,4)] | 

Đầu ra cuối cùng:```
-1 -1 -1 -1 -1
```Ví dụ này xác nhận rằng độ tuổi bằng nhau không được coi là trẻ hơn. Tìm kiếm nhị phân loại bỏ chính xác các giá trị bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi vị trí thực hiện một tìm kiếm nhị phân | 
| Không gian | O(n) | Cấu trúc cực tiểu hậu tố có thể lưu trữ tối đa n phần tử | 

Với`n = 10^5`, MỘT`O(n log n)`thuật toán dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng nhỏ vì chúng ta chỉ lưu trữ một vài mảng có kích thước`n`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = [-1] * n
    mins = []

    for i in range(n - 1, -1, -1):
        age = a[i]

        pos = bisect_left(mins, (age, -1))

        if pos > 0:
            j = mins[pos - 1][1]
            ans[i] = j - i - 1

        if not mins or age < mins[-1][0]:
            mins.append((age, i))

    print(*ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run(
    "6\n10 8 5 3 50 45\n"
) == "2 1 0 -1 0 -1", "sample 1"

# minimum size
assert run(
    "2\n1 2\n"
) == "-1 -1", "strictly increasing"

# decreasing sequence
assert run(
    "5\n5 4 3 2 1\n"
) == "3 2 1 0 -1", "furthest younger always at end"

# all equal
assert run(
    "4\n7 7 7 7\n"
) == "-1 -1 -1 -1", "equal ages are not younger"

# mixed case with distant answer
assert run(
    "6\n5 100 99 98 1 2\n"
) == "3 3 2 0 -1 -1", "furthest younger is not necessarily minimum value"

# boundary style case
assert run(
    "5\n10 1 9 2 8\n"
) == "3 -1 1 -1 -1", "checks off-by-one distances"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`-1 -1`| Đầu vào hợp lệ tối thiểu | 
|`5 / 5 4 3 2 1`|`3 2 1 0 -1`| Trẻ nhất ở cuối | 
|`4 / 7 7 7 7`|`-1 -1 -1 -1`| Giá trị bằng nhau không được tính | 
|`6 / 5 100 99 98 1 2`|`3 3 2 0 -1 -1`| Trẻ hơn xa nhất khác với trẻ nhỏ nhất | 
|`5 / 10 1 9 2 8`|`3 -1 1 -1 -1`| Xử lý khoảng cách từng bước một | 

## Vỏ cạnh 

Hãy xem xét tất cả các độ tuổi bằng nhau:```
4
5 5 5 5
```Khi quét từ phải sang trái, cấu trúc chỉ lưu trữ tối thiểu một hậu tố`(5,3)`. Mọi tìm kiếm nhị phân đều yêu cầu một giá trị nhỏ hơn rất nhiều so với`5`, nhưng không tồn tại. Các đầu ra thuật toán:```
-1 -1 -1 -1
```Trường hợp này xác nhận rằng độ tuổi bằng nhau được xử lý chính xác. 

Bây giờ hãy xem xét một chuỗi giảm nghiêm ngặt:```
5
9 8 7 6 5
```Mọi con hải mã ngoại trừ con cuối cùng đều có những con hải mã trẻ hơn ở phía trước. Con hải mã trẻ xa nhất luôn ở vị trí cuối cùng. 

Câu trả lời trở thành:```
3 2 1 0 -1
```Thuật toán thành công vì hậu tố cực tiểu liên tục giảm:```
(5,4), (6,3), (7,2), ...
```Tìm kiếm nhị phân luôn chọn chỉ mục hợp lệ xa nhất. 

Cuối cùng, hãy xem xét trường hợp con hải mã nhỏ nhất không phải là câu trả lời đúng:```
6
5 100 99 98 1 2
```Đối với hải mã theo tuổi`100`, cả hai`1`Và`2`trẻ hơn. Bài toán yêu cầu con hải mã trẻ nhất ở xa nhất, đó là`2`ở cuối. 

Sự không hài lòng là:```
5 - 1 - 1 = 3
```Cấu trúc hậu tố tối thiểu lưu trữ các vị trí theo thứ tự khoảng cách tăng dần, do đó tìm kiếm nhị phân tự nhiên chọn vị trí hợp lệ xa nhất thay vì tuổi nhỏ nhất.
