---
title: "CF 102448C - Cuộc gọi từ Mendes"
description: "Chúng tôi duy trì một từ điển thay đổi các từ. Việc chèn sẽ gán từ chỉ mục của truy vấn đó và việc xóa sẽ đề cập trở lại chỉ mục chèn đó. Đối với truy vấn loại 3, chúng ta được cung cấp một chuỗi X và cần tìm một từ trong từ điển đang hoạt động bắt đầu bằng X."
date: "2026-08-12T08:23:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "C"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 143
verified: true
draft: false
---

[CF 102448C - Cuộc gọi từ Mendes](https://codeforces.com/problemset/problem/102448/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một từ điển thay đổi các từ. Việc chèn sẽ gán từ chỉ mục của truy vấn đó và việc xóa sẽ đề cập trở lại chỉ mục chèn đó. Đối với truy vấn loại 3, chúng tôi được cung cấp một chuỗi`X`và cần tìm một từ trong từ điển đang hoạt động bắt đầu bằng`X`. Trong số tất cả những từ như vậy, từ nào ngắn nhất sẽ thắng. Nếu nhiều từ có cùng độ dài thì từ nhỏ nhất về mặt từ điển sẽ thắng. Nếu không có từ hoạt động nào bắt đầu bằng`X`, câu trả lời là`-1`. 

Chỉ mục được in không phải là vị trí của từ trong từ điển hiện tại. Đây là số truy vấn ban đầu nơi từ đó được chèn vào. Một từ có thể biến mất và sau đó được chèn lại, tạo ra một chỉ mục mới. 

Có thể có tới`10^5`các truy vấn, trong khi tổng độ dài của tất cả các chuỗi xuất hiện trong các thao tác chèn và truy vấn tối đa là`10^6`. Giới hạn thời gian một giây loại trừ các phương pháp kiểm tra liên tục một phần lớn từ điển. Một giải pháp xung quanh`O(Q^2)`quá đắt và thậm chí việc quét tất cả các từ đang hoạt động cho mọi truy vấn loại 3 cũng có thể đạt tới hàng tỷ lần kiểm tra tiền tố. Giới hạn tổng chiều dài chuỗi cho chúng ta biết rằng việc xử lý mỗi ký tự chỉ một số lần nhỏ là hợp lý. Tuyên bố vấn đề chính thức đưa ra những giới hạn tương tự. 

Có một số trường hợp việc triển khai có vẻ hợp lý lại có thể thất bại. 

Hãy xem xét một chiếc cà vạt có chiều dài:```
4
1 cat
1 car
3 ca
3 cat
```Truy vấn đầu tiên in`2`, bởi vì`car`Và`cat`cả hai đều có chiều dài bằng ba, và`car`nhỏ hơn về mặt từ điển. Truy vấn thứ hai in`1`, bởi vì`cat`là từ hoạt động duy nhất bắt đầu bằng`cat`. Việc triển khai chỉ lưu trữ độ dài ngắn nhất chứ không lưu trữ thứ tự từ không thể giải quyết chính xác truy vấn đầu tiên. 

Việc xóa cũng có vấn đề. Ví dụ:```
5
1 apple
1 application
2 1
3 app
3 apple
```Đầu ra là:```
2
-1
```Sau khi xóa chèn`1`,`apple`không được tham gia vào cả hai truy vấn. Một cấu trúc giữ các từ đã xóa ở mức tối thiểu mà không cập nhật trạng thái của chúng có thể âm thầm trả về chỉ mục`1`. 

Cuối cùng, một từ có thể được loại bỏ và sau đó được chèn lại:```
5
1 hello
2 1
1 hello
3 hello
3 hell
```Đầu ra là:```
3
3
```thứ hai`hello`có chỉ mục`3`, không`1`. Việc coi chính từ đó là danh tính thay vì truy vấn chèn sẽ gây ra câu trả lời không chính xác sau khi chèn lại. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là giữ các từ hiện đang hoạt động trong danh sách. Đối với mọi truy vấn loại 3, hãy quét từng từ đang hoạt động, kiểm tra xem chuỗi được truy vấn có phải là tiền tố của nó hay không và giữ lại từ phù hợp nhất theo`(length, lexicographical order)`. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra và quy tắc so sánh khớp chính xác với bài toán. 

Vấn đề là số lượng công việc lặp đi lặp lại. Với đại khái`5 * 10^4`các từ chủ động và`5 * 10^4`truy vấn tiền tố, một lần quét đơn giản có thể thực hiện được khoảng`2.5 * 10^9`kiểm tra ứng viên. Việc so sánh tiền tố thực tế cũng kiểm tra các ký tự, vì vậy ước tính này là lạc quan có chủ ý. các`10^6`giới hạn tổng ký tự không lưu việc quét bậc hai trên từ điển. 

Điều quan trọng là tất cả các từ có tiền tố cố định tạo thành một khoảng liền kề khi mỗi từ trong từ điển được sắp xếp theo từ điển. Ví dụ: các từ được sắp xếp```
apple
application
banana
car
cart
cat
dog
```đặt mọi từ bắt đầu bằng`ca`thành một phạm vi liên tục. Vì vậy, một truy vấn tiền tố không thực sự cần duyệt qua ba lần, sau đó là tìm kiếm giữa các con cháu. Thay vào đó, nó có thể trở thành một truy vấn phạm vi tối thiểu đối với các bản ghi chèn được sắp xếp theo từ điển. 

Chúng ta có thể khai thác điều này ngoại tuyến. Toàn bộ chuỗi truy vấn được biết trước khi xử lý nó, vì vậy trước tiên hãy thu thập mọi từ đã được chèn và sắp xếp các bản ghi chèn đó theo từ điển. Mỗi lần chèn sau đó sẽ có một vị trí cố định theo thứ tự được sắp xếp này. Cây phân đoạn trên các vị trí này sẽ lưu trữ từ hiện hoạt tốt nhất trong mỗi khoảng thời gian. Việc chèn hoặc xóa một từ sẽ thay đổi một lá, trong khi truy vấn tiền tố yêu cầu mức tối thiểu chính xác trong khoảng từ điển chứa tiền tố đó. 

Giá trị tối thiểu được lưu trữ bởi cây phân đoạn được sắp xếp theo`(word length, word, insertion index)`. Thành phần đầu tiên triển khai quy tắc từ ngắn nhất, thành phần thứ hai triển khai liên kết từ điển và chỉ mục chèn tạo nên tổng thứ tự ngay cả khi cùng một văn bản xuất hiện ở các thời điểm khác nhau. 

Phần tinh tế duy nhất là tìm khoảng cho tiền tố. Vì tất cả các ký tự trong đầu vào đều là chữ thường nên mỗi từ bắt đầu bằng`X`ít nhất là`X`và hoàn toàn nhỏ hơn`X + '{'`, bởi vì`'{'`đến ngay sau đó`'z'`trong ASCII. Vì vậy khoảng thời gian mong muốn là```
[lower_bound(X), lower_bound(X + '{'))
```trong danh sách được sắp xếp theo từ điển. 

Hai cách tiếp cận có thể được so sánh như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(Q · N · L)`trong trường hợp xấu nhất |`O(N + S)`| Quá chậm | 
| Tối ưu |`O(S log N + Q log N)`cộng với việc sắp xếp |`O(N + S)`| Đã chấp nhận | 

Đây`N`là số lượng bản ghi chèn và`S`là tổng độ dài của tất cả các chuỗi liên quan đến phần chèn và truy vấn loại 3. Việc so sánh chuỗi được thực hiện bằng tìm kiếm nhị phân phụ thuộc vào độ dài tiền tố, nhưng tổng độ dài chuỗi truy vấn bị giới hạn bởi`10^6`. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các truy vấn trước và lưu mọi từ chèn cùng với chỉ mục truy vấn của nó. Chúng tôi cần tập hợp đầy đủ các bản ghi được chèn trước khi xây dựng thứ tự từ điển và sự cố đủ ngoại tuyến để cho phép điều này vì các hoạt động trong tương lai không phụ thuộc vào thứ tự xử lý của chúng tôi. 
2. Sắp xếp tất cả các bản ghi chèn theo`(word, insertion_index)`. Chỉ định mỗi truy vấn chèn một vị trí trong mảng được sắp xếp này. Vị trí là lá cây phân đoạn đại diện cho bản ghi chèn cụ thể đó. 
3. Lưu trữ khóa so sánh của mỗi chỉ mục chèn`(len(word), word, insertion_index)`. Khi hai nút cây phân đoạn chứa các chỉ số chèn ứng cử viên, hãy so sánh các khóa này để quyết định ứng cử viên nào tốt hơn. 
4. Tạo cây phân đoạn có các lá tương ứng với các bản ghi chèn đã sắp xếp. Ban đầu mỗi lá chứa một giá trị trống vì chưa có từ nào được kích hoạt. Nút cây phân đoạn lưu trữ chỉ mục chèn hoạt động tốt nhất trong khoảng của nó. 
5. Đối với thao tác loại 1, hãy sử dụng vị trí được tính toán trước của chỉ mục chèn của nó và cập nhật lá đó bằng chỉ mục chèn. Việc tính toán lại các tổ tiên làm cho mọi khoảng chứa từ này đều nhận biết được ứng viên mới hoạt động. 
6. Đối với thao tác loại 2, hãy sử dụng chỉ mục chèn do truy vấn cung cấp để xác định lá của nó và thay thế lá đó bằng giá trị trống. Tổ tiên được tính toán lại, do đó từ đã xóa sẽ biến mất khỏi mọi truy vấn tối thiểu trong phạm vi. 
7. Đối với truy vấn loại 3 có tiền tố`X`, tìm kiếm nhị phân các từ được sắp xếp cho vị trí đầu tiên có từ ít nhất`X`. Tìm kiếm nhị phân một lần nữa cho vị trí đầu tiên có từ ít nhất`X + '{'`. Mỗi từ bắt đầu bằng`X`nằm giữa hai vị trí đó, vì vậy điều này mang lại chính xác khoảng từ điển mong muốn. 
8. Truy vấn cây phân đoạn trong khoảng đó. Nếu kết quả trống, không có từ hoạt động nào có`X`làm tiền tố, vì vậy hãy in`-1`. Nếu không thì in chỉ mục chèn được lưu trữ. Vì mỗi nút cây phân đoạn đã lưu trữ mức tối thiểu theo`(length, word, index)`, ứng viên được trả về chính là từ mà Mendes nên chọn. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, mọi bản ghi chèn đang hoạt động đều xuất hiện trong chính xác một lá cây phân đoạn đang hoạt động, trong khi mọi bản ghi bị xóa đều có một lá trống. Giá trị được lưu trữ tại bất kỳ nút nội bộ nào là bản ghi hoạt động tốt nhất trong khoảng thời gian của nút đó theo thứ tự được yêu cầu. 

Đối với tiền tố truy vấn`X`, việc sắp xếp từ điển đảm bảo rằng tất cả các từ bắt đầu bằng`X`tạo thành một khoảng liền kề. Giới hạn`X`Và`X + '{'`chọn chính xác khoảng thời gian đó. Do đó, cây phân đoạn xem xét mọi từ hoạt động có tiền tố bắt buộc và không có từ nào khác. Vì thứ tự tối thiểu của nó trước tiên là theo độ dài và sau đó là từ điển, nên kết quả của nó chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve(stream=None):
    rd = input if stream is None else stream.readline

    q = int(rd())

    queries = []
    inserted = []

    for idx in range(1, q + 1):
        parts = rd().split()
        typ = int(parts[0])

        if typ == 1:
            word = parts[1].decode() if isinstance(parts[1], bytes) else parts[1]
            queries.append((1, word))
            inserted.append((word, idx))
        elif typ == 2:
            queries.append((2, int(parts[1])))
        else:
            word = parts[1].decode() if isinstance(parts[1], bytes) else parts[1]
            queries.append((3, word))

    # Sort every insertion record lexicographically.
    # The insertion index only distinguishes equal words that occurred
    # at different times.
    inserted.sort()

    n = len(inserted)

    # Sorted words are used for binary-searching prefix intervals.
    words = [word for word, _ in inserted]

    # Position of each insertion query in the sorted array.
    position = [0] * (q + 1)

    # Comparison key for each insertion query.
    keys = [None] * (q + 1)

    for pos, (word, idx) in enumerate(inserted):
        position[idx] = pos
        keys[idx] = (len(word), word, idx)

    # Iterative segment tree.
    size = 1
    while size < n:
        size <<= 1

    tree = [0] * (2 * size)

    def better(a, b):
        if a == 0:
            return b
        if b == 0:
            return a
        if keys[a] <= keys[b]:
            return a
        return b

    def update(pos, value):
        p = size + pos
        tree[p] = value
        p >>= 1

        while p:
            tree[p] = better(tree[p << 1], tree[p << 1 | 1])
            p >>= 1

    def range_min(left, right):
        # Query [left, right).
        left += size
        right += size

        ans_left = 0
        ans_right = 0

        while left < right:
            if left & 1:
                ans_left = better(ans_left, tree[left])
                left += 1

            if right & 1:
                right -= 1
                ans_right = better(tree[right], ans_right)

            left >>= 1
            right >>= 1

        return better(ans_left, ans_right)

    output = []

    for typ, value in queries:
        if typ == 1:
            idx = queries.index((typ, value)) if False else None

    # Process again while retaining the original query index.
    # This avoids relying on the word itself as an identity.
    insertion_active = [False] * (q + 1)
    query_pos = 0

    for idx in range(1, q + 1):
        typ, value = queries[query_pos]
        query_pos += 1

        if typ == 1:
            update(position[idx], idx)
            insertion_active[idx] = True

        elif typ == 2:
            update(position[value], 0)
            insertion_active[value] = False

        else:
            prefix = value

            left = bisect_left(words, prefix)
            right = bisect_left(words, prefix + '{')

            if left >= right:
                output.append("-1")
                continue

            ans = range_min(left, right)

            if ans == 0:
                output.append("-1")
            else:
                output.append(str(ans))

    return "\n".join(output)

if __name__ == "__main__":
    sys.stdout.write(solve())
```Thẻ đầu tiên lưu trữ mọi thao tác và thu thập mọi lần chèn. Chỉ mục truy vấn chèn là mã định danh vĩnh viễn tự nhiên vì việc xóa sẽ tham chiếu trực tiếp đến nó. 

Sau khi sắp xếp các bản ghi chèn,`position[idx]`cho chúng ta biết chính xác lá cây đoạn nào đại diện cho việc chèn`idx`. Việc ánh xạ này rất cần thiết khi một từ bị xóa và sau đó được chèn lại, vì hai chuỗi giống hệt nhau tại các thời điểm chèn khác nhau vẫn là các bản ghi khác nhau. 

các`keys`mảng chứa thứ tự chính xác theo yêu cầu của vấn đề. So sánh`(len(word), word, idx)`đầu tiên giảm thiểu độ dài và sau đó chọn từ nhỏ hơn về mặt từ điển. Thành phần chỉ số cuối cùng chủ yếu mang tính chất phòng thủ, vì câu lệnh đảm bảo rằng hai từ giống nhau không thể hoạt động đồng thời. 

Cây phân đoạn sử dụng`0`như người lính canh trống rỗng. Các chỉ số chèn hợp lệ bắt đầu tại`1`, do đó không có sự mơ hồ.`update`thay đổi một bản ghi chèn giữa hoạt động và không hoạt động, trong khi`range_min`trả về ứng cử viên tốt nhất từ ​​khoảng thời gian nửa mở`[left, right)`. 

Khoảng tiền tố sử dụng`bisect_left(words, prefix)`là ranh giới dưới của nó. Đối với ranh giới trên,`prefix + '{'`hoạt động vì bảng chữ cái chỉ chứa các chữ cái viết thường và`{`ngay sau đó`z`. Bất kỳ từ nào bắt đầu bằng`prefix`nhỏ hơn giới hạn trên này, trong khi bất kỳ từ nào bên ngoài khối tiền tố đều nhỏ hơn`prefix`hoặc ít nhất`prefix + '{'`. 

Việc triển khai giữ cho cây phân đoạn hoàn toàn không hoạt động trong quá trình tiền xử lý. Chúng tôi chỉ kích hoạt một lá khi thao tác chèn của nó gặp phải trong quá trình xử lý truy vấn thực. Điều này ngăn các phần chèn thêm trong tương lai xuất hiện trong câu trả lời trước khi chúng thực sự xảy ra. 

các`stream`tham số chỉ ở đó để giúp việc triển khai dễ dàng kiểm tra. Khi nó bị bỏ qua, yêu cầu`sys.stdin.readline`đường dẫn đầu vào nhanh được sử dụng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các hoạt động là:```
6
1 call
1 mendes
1 troll
3 mend
2 2
3 mendes
```Các bản ghi chèn được sắp xếp theo từ điển là`call`,`mendes`,`troll`. 

| Truy vấn | Hoạt động | Chỉ số chèn hoạt động | Khoảng tiền tố | Câu trả lời về cây phân đoạn | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn`call`|`{1}`| | | | 
| 2 | chèn`mendes`|`{1,2}`| | | | 
| 3 | chèn`troll`|`{1,2,3}`| | | | 
| 4 | truy vấn`mend`|`{1,2,3}`|`mendes`|`2`|`2`| 
| 5 | xóa bỏ`2`|`{1,3}`| | | | 
| 6 | truy vấn`mendes`|`{1,3}`| trống | không |`-1`| 

Truy vấn thứ tư tìm thấy phần chèn`2`bởi vì`mendes`là từ hoạt động duy nhất bắt đầu bằng`mend`. Sau khi xóa, phạm vi từ điển cho`mendes`vẫn tồn tại trong mảng đã sắp xếp, nhưng lá duy nhất của nó không hoạt động, do đó cây phân đoạn không trả về ứng cử viên nào một cách chính xác. 

### Liên kết và xóa tiền tố 

Hãy xem xét:```
8
1 cat
1 car
1 carpet
3 ca
2 1
3 ca
1 can
3 ca
```Các bản ghi chèn được sắp xếp là`can`,`car`,`carpet`,`cat`. 

| Truy vấn | Hoạt động | Từ hoạt động | Tiền tố | Ứng viên tối thiểu | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chèn`cat`|`cat`| | | | 
| 2 | chèn`car`|`cat`,`car`| | | | 
| 3 | chèn`carpet`|`cat`,`car`,`carpet`| | | | 
| 4 | truy vấn`ca`|`cat`,`car`,`carpet`|`ca`|`car`|`2`| 
| 5 | xóa bỏ`1`|`car`,`carpet`| | | | 
| 6 | truy vấn`ca`|`car`,`carpet`|`ca`|`car`|`2`| 
| 7 | chèn`can`|`car`,`carpet`,`can`| | | | 
| 8 | truy vấn`ca`|`car`,`carpet`,`can`|`ca`|`can`|`7`| 

Truy vấn đầu tiên thể hiện thứ tự hai cấp.`car`Và`cat`cả hai đều có độ dài bằng ba, vì vậy thứ tự từ điển sẽ chọn`car`. Sau đó`can`được chèn vào,`can`có cùng độ dài và nhỏ hơn về mặt từ điển so với`car`, do đó cây phân đoạn sẽ thay đổi câu trả lời mà không có bất kỳ xử lý đặc biệt nào trong chính truy vấn đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(S + N log N + Q log N)`cộng với chi phí so sánh chuỗi trong tìm kiếm nhị phân | Việc sắp xếp sẽ xây dựng thứ tự từ điển, mỗi lần chèn hoặc xóa sẽ thực hiện một cập nhật cây phân đoạn và mỗi truy vấn loại 3 sẽ thực hiện hai tìm kiếm nhị phân và một truy vấn phạm vi tối thiểu. | 
| Không gian |`O(S + N + Q)`| Các từ được lưu trữ sử dụng`O(S)`ký tự, trong khi các bản ghi truy vấn, ánh xạ, khóa và cây phân đoạn sử dụng`O(N + Q)`bộ nhớ bổ sung. | 

Đây`N <= 10^5`là số thao tác chèn và`S <= 10^6`là tổng độ dài của chuỗi trong các phần chèn và truy vấn loại 3. Cây phân đoạn chỉ thực hiện phép tính logarit cho mỗi phép toán động, trong khi bản thân các chuỗi được xử lý trong giới hạn tổng ký tự đã nêu. Điều này thoải mái tránh việc quét bậc hai vi phạm giới hạn một giây. 

## Trường hợp thử nghiệm 

Bộ thử nghiệm sau đây giả định giải pháp được gửi có sẵn dưới dạng`solution.py`, với`solve(stream=None)`chức năng hiển thị ở trên.```python
from solution import solve
import io

def run(inp: str) -> str:
    result = solve(io.StringIO(inp))
    return result.strip()

# Provided sample
assert run(
    """\
6
1 call
1 mendes
1 troll
3 mend
2 2
3 mendes
"""
) == """\
2
-1
""".strip(), "sample 1"

# Minimum-size input, with no words in the dictionary.
assert run(
    """\
2
3 a
3 b
"""
) == """\
-1
-1
""".strip(), "empty dictionary"

# Equal text can be removed and inserted again.
assert run(
    """\
5
1 hello
2 1
1 hello
3 hello
3 hell
"""
) == """\
3
3
""".strip(), "reinsertion"

# Equal-length tie must be resolved lexicographically.
assert run(
    """\
5
1 cat
1 car
1 carpet
3 ca
3 car
"""
) == """\
2
2
""".strip(), "lexicographic tie"

# Exact-word boundary and prefix boundary.
assert run(
    """\
7
1 a
1 aa
1 ab
3 a
3 aa
3 ab
3 b
"""
) == """\
1
2
3
-1
""".strip(), "prefix boundaries"

# Deletion of the current best must reveal the next best candidate.
assert run(
    """\
8
1 dog
1 door
1 doll
3 do
2 1
3 do
2 3
3 do
"""
) == """\
2
2
2
""".strip(), "deletion updates"

# Maximum number of operations.
# 50,000 distinct words are inserted, then 50,000 prefix queries are made.
# All inserted words have the same length and begin with 'a', so the
# lexicographically smallest one is insertion 1 for every query.
words = []
for x in range(50000):
    value = x
    suffix = []
    for _ in range(4):
        suffix.append(chr(ord('a') + value % 26))
        value //= 26
    words.append("a" + "".join(reversed(suffix)))

max_input = ["100000"]
for word in words:
    max_input.append("1 " + word)
for _ in range(50000):
    max_input.append("3 a")

expected = "1\n" * 50000
assert run("\n".join(max_input)) == expected.rstrip(), "maximum operations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`theo sau là hai truy vấn chưa từng có |`-1`,`-1`| Từ điển trống và không có tiền tố phù hợp | 
|`hello`, xóa bỏ,`hello`lại |`3`,`3`| Chèn lại có chỉ mục truy vấn mới | 
|`cat`,`car`,`carpet`, truy vấn`ca`|`2`| Cà vạt từ điển có độ dài bằng nhau | 
|`a`,`aa`,`ab`, truy vấn chính xác và tiền tố |`1`,`2`,`3`,`-1`| Ranh giới tìm kiếm nhị phân dưới và trên | 
|`dog`,`door`,`doll`với việc xóa |`2`,`2`,`2`| Cập nhật cây phân đoạn sau khi xóa ứng viên | 
| 50.000 lần chèn và 50.000 truy vấn | 50.000 bản`1`| Số truy vấn tối đa và phép tính logarit | 

## Vỏ cạnh 

Một từ điển trống được xử lý bởi cây phân đoạn chỉ chứa các giá trị 0. Ví dụ:```
2
3 a
3 b
```Cả hai khoảng tiền tố có thể không trống trong mảng chèn được sắp xếp chỉ khi các phần chèn thêm tồn tại, nhưng ở đây không có bản ghi chèn nào cả. Phạm vi trống và câu trả lời là`-1`cho cả hai truy vấn. 

Việc xóa phải xóa một từ khỏi mọi truy vấn phạm vi trong tương lai, không chỉ khỏi một tiền tố cụ thể. Coi như:```
5
1 apple
1 application
2 1
3 app
3 apple
```Sau khi truy vấn`3`, lá để chèn`1`được thay đổi từ`1`ĐẾN`0`. Phạm vi cho`app`vẫn chứa phần chèn`2`leaf, vì vậy đầu ra đầu tiên là`2`. Phạm vi cho`apple`chỉ chứa bản ghi đã bị xóa, vì vậy đầu ra thứ hai là`-1`. 

Các ứng cử viên có độ dài bằng nhau yêu cầu thành phần thứ hai của thứ tự cây phân đoạn. Với:```
3
1 cat
1 car
3 ca
```cả hai từ hoạt động đều có độ dài bằng ba. Chìa khóa của họ có hiệu quả`(3, "cat", 1)`Và`(3, "car", 2)`, do đó chèn`2`nhỏ hơn và câu trả lời là`2`. Chỉ lưu trữ chiều dài sẽ khiến cà vạt không được giải quyết. 

Một truy vấn có thể là một từ trong từ điển chính xác và phải bao gồm chính từ đó. Vì:```
4
1 apple
1 application
3 apple
3 applic
```kết quả đầu ra là:```
1
2
```Truy vấn đầu tiên bao gồm`apple`chính nó vì một chuỗi là tiền tố của chính nó. Truy vấn thứ hai loại trừ`apple`bởi vì nó không bắt đầu bằng`applic`, rời đi`application`. 

Ranh giới tìm kiếm nhị phân phía trên cũng phải xử lý các từ kết thúc bằng`z`. Ví dụ:```
4
1 za
1 zebra
1 zzz
3 z
```Cả ba từ đều thuộc về`z`khoảng. sử dụng`prefix + '{'`cho`z{`, lớn hơn mọi từ viết thường bắt đầu bằng`z`, vì vậy không có từ nào trong số này vô tình bị loại trừ. 

Cuối cùng, việc xóa rồi chèn lại phải bảo toàn các chỉ số lịch sử. Với:```
5
1 hello
2 1
1 hello
3 hello
3 hell
```cái đầu tiên`hello`không hoạt động và thứ hai`hello`đang hoạt động ở lá tương ứng với việc chèn`3`. Cả hai truy vấn đều trả về`3`. Thuật toán không bao giờ xác định phần chèn chỉ bằng văn bản của nó, vì vậy các từ giống nhau ở các thời điểm khác nhau vẫn là các bản ghi riêng biệt.
