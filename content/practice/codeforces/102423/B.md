---
title: "CF 102423B - Bộ đệm máy tính"
description: "Bộ đệm là một mảng gồm n byte địa chỉ, ban đầu được điền bằng 0. Chúng ta cũng có m phần dữ liệu độc lập, trong đó mỗi phần chính là một mảng byte. Thao tác tải sao chép toàn bộ phần vào một vùng liên tiếp của bộ nhớ đệm, thay thế bất kỳ phần nào ở đó."
date: "2026-08-14T15:15:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102423
codeforces_index: "B"
codeforces_contest_name: "North American Southeast Regional 2019 (Div 1)"
rating: 0
weight: 102423
solve_time_s: 180
verified: true
draft: false
---

[CF 102423B - Bộ nhớ đệm máy tính](https://codeforces.com/problemset/problem/102423/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bộ đệm là một mảng`n`địa chỉ byte, ban đầu được điền bằng 0. Chúng tôi cũng có`m`các phần dữ liệu độc lập, trong đó mỗi phần chính là một mảng byte. Thao tác tải sao chép toàn bộ phần vào một vùng liên tiếp của bộ nhớ đệm, thay thế bất kỳ phần nào ở đó. Thao tác tăng dần sẽ thay đổi một phạm vi liên tiếp bên trong phần dữ liệu được lưu trữ, modulo 256, nhưng không sửa đổi các bản sao đã được tải vào bộ đệm. Thao tác in yêu cầu byte hiện tại tại một địa chỉ bộ đệm. Tuyên bố ban đầu cho phép tối đa`5 * 10^5`địa chỉ bộ đệm, phần dữ liệu và hoạt động, với tổng chiều dài của tất cả các phần dữ liệu nhiều nhất`5 * 10^5`. Giới hạn thời gian đã nêu là 5 giây. 

Các giới hạn đó loại trừ việc mô phỏng tải bằng cách sao chép từng byte của phần dữ liệu. Một mảnh có thể có chiều dài`5 * 10^5`và cùng một phần có thể được tải trên hầu hết mọi thiết bị`5 * 10^5`hoạt động. Điều đó đưa ra một trường hợp xấu nhất về`2.5 * 10^11`phép gán byte. Chúng ta cần mỗi thao tác chỉ chạm vào nhiều nút cấu trúc theo logarit, ngoài việc đọc chính đầu vào. 

Phần khó khăn là bản sao bộ đệm là một ảnh chụp nhanh. Nếu phần dữ liệu`[10, 20]`được tải và phần này sau đó được tăng lên, bộ đệm vẫn chứa`[10, 20]`, không phải phiên bản mới. Ngược lại, các bước tăng được thực hiện trước một lần tải phải hiển thị trong lần tải đó. Việc triển khai trực tiếp chỉ lưu trữ mã định danh phần dữ liệu ở vị trí bộ nhớ đệm sẽ làm mất đi sự khác biệt này. 

Hãy xem xét bộ đệm nhỏ nhất có thể trước khi tải bất cứ thứ gì.```
1 1 2
1 0
2 1
2 1
```Đầu ra đúng là:```
0
0
```Việc triển khai bất cẩn có thể cho rằng mọi vị trí được truy vấn đều thuộc về phần dữ liệu mới nhất và trả về`0`vì lý do sai hoặc cố gắng truy cập phần bù dữ liệu chưa được khởi tạo. Bộ đệm phải thể hiện rõ ràng sự vắng mặt của lần tải trước đó. 

Trường hợp cạnh thứ hai là phần tăng trước khi tải.```
1 1 3
1 255
3 1 1 1
1 1 1
2 1
```Đầu ra là:```
0
```Phần gia tăng thay đổi phần được lưu trữ từ`255`ĐẾN`0`và các bản sao tải tiếp theo`0`. Coi dữ liệu là bất biến sẽ in không chính xác`255`. 

Thứ tự ngược lại cũng có ý nghĩa không kém.```
1 1 4
1 255
1 1 1
3 1 1 1
2 1
```Đầu ra là:```
255
```Sự gia tăng xảy ra sau khi tải, do đó nó không thể thay đổi byte đã được lưu trong bộ đệm. Một giải pháp chỉ cần xem xét phần dữ liệu hiện tại khi trả lời một truy vấn sẽ in sai`0`. 

Cuối cùng, các tải chồng chéo phải được xử lý ở mức chi tiết byte. Ví dụ:```
5 2 6
2 10 20
2 30 40
1 1 2
2 2
1 2 3
2 2
2 3
2 4
```Đầu ra là:```
10
30
40
40
```Lần tải thứ hai ghi đè địa chỉ`3`Và`4`, nhưng địa chỉ`2`còn lại từ lần tải đầu tiên. Một giải pháp xử lý tải dưới dạng toàn bộ trạng thái bộ đệm thay vì phạm vi có thể dễ dàng hiểu sai ranh giới này. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp duy trì mảng bộ đệm. Một tải mảnh`i`ở vị trí`p`vòng qua tất cả`k_i`byte và ghi chúng vào bộ đệm. Một vòng lặp tăng dần trong phạm vi được yêu cầu và thay đổi các byte tương ứng của phần dữ liệu. Một bản in là thời gian không đổi. Điều này đúng vì nó tuân theo chính xác ngữ nghĩa của mọi thao tác. 

Vấn đề là hoạt động tải. Giả sử có một đoạn dữ liệu có độ dài`500000`, và cái kia`499999`hoạt động cũng là vô số phần đó. Giải pháp brute-force thực hiện gần như`500000 * 500000 = 2.5 * 10^11`ghi byte. Điều đó vượt xa những gì thời hạn cho phép. 

Quan sát hữu ích đầu tiên là truy vấn bộ đệm không cần toàn bộ trạng thái bộ đệm. Nó chỉ cần biết lần tải nào cuối cùng đã ghi địa chỉ bộ đệm cụ thể đó và byte nào của phần được tải tương ứng với địa chỉ đó. Chúng ta có thể tìm thấy tải đó mà không cần sao chép dữ liệu bằng cách coi mọi tải là một phép gán phạm vi mang số hoạt động của nó. 

Cây phân đoạn có thể lưu trữ số lần tải mới nhất ảnh hưởng đến từng vị trí bộ đệm. Để cập nhật phạm vi, chúng tôi đặt số tải vào các nút cây phân đoạn được bao phủ hoàn toàn bởi tải đó. Đối với truy vấn điểm, chúng ta đi từ lá đến gốc và lấy số tải lớn nhất gặp phải. Vì số lượng thao tác tăng theo thời gian nên số lớn nhất chính xác là tải cuối cùng bao phủ vị trí. 

Có một quan sát thứ hai xử lý số gia tăng. Khi một truy vấn đã được liên kết với một tải cụ thể, giá trị bộ đệm của nó sẽ được cố định tại thời điểm thực hiện tải đó. Chúng ta có thể trì hoãn việc tính toán giá trị cho đến khi tải đó được xử lý. Trước tiên, chúng tôi quét tất cả các hoạt động để liên kết mọi truy vấn in với lần tải cuối cùng của nó. Sau đó chúng tôi quét lại các hoạt động. Trong khi quét lần thứ hai, các phần dữ liệu chứa chính xác các phiên bản mà chúng có tại mỗi thời điểm trong lần thực thi ban đầu. Khi chúng tôi đạt đến một tải, tất cả các truy vấn liên quan đến tải đó có thể được trả lời ngay lập tức. 

Đối với các phần dữ liệu, chúng ta cần tăng phạm vi và truy vấn điểm. Cây Fenwick cho ra chính xác sự kết hợp này. Chúng ta lưu trữ một mảng sai phân trong cây Fenwick, thêm vào`+1`ở điểm cuối bên trái và`-1`ngay sau điểm cuối bên phải. Tổng tiền tố tại một vị trí byte là số gia số ảnh hưởng đến byte đó. Vì các giá trị là byte nên tất cả số đếm này có thể giảm theo modulo 256. 

Hai phần như vậy là độc lập. Cây phân đoạn trả lời câu hỏi tạm thời "tải nào đã tạo ra byte bộ đệm này?", trong khi cây Fenwick trả lời câu hỏi lịch sử "giá trị của byte dữ liệu này khi tải đó xảy ra là bao nhiêu?" 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số byte được sao chép) | O(n + tổng k_i) | Quá chậm, lên tới khoảng 2,5 * 10^11 ghi | 
| Tối ưu | O(tổng k_i + q log n + q log S) | O(n + m + q + tổng k_i) | Đã chấp nhận | 

Đây`S`là độ dài tối đa của đoạn dữ liệu. Cách tiếp cận tối ưu không bao giờ hiện thực hóa một bản sao đã tải bên trong bộ đệm. 

## Hướng dẫn thuật toán 

1. Đọc từng phần dữ liệu và giữ nguyên byte gốc của nó. Cung cấp cho mỗi phần một phạm vi nhỏ gọn bên trong một mảng byte toàn cục. Đồng thời đặt trước một phạm vi cây Fenwick cho mỗi tác phẩm. Tổng kích thước của tất cả các mảnh tối đa là`5 * 10^5`, vì vậy việc lưu trữ tất cả các byte gốc sẽ rẻ. 
2. Lưu trữ tất cả`q`hoạt động thay vì thực hiện chúng ngay lập tức. Chúng tôi cần thực hiện các thao tác này hai lần, một lần để khám phá xem tải nào sở hữu từng truy vấn bộ đệm và một lần để xây dựng lại các phiên bản phần dữ liệu tại thời điểm tải. 
3. Tạo cây phân đoạn trên các địa chỉ bộ đệm. Đối với mỗi hoạt động tải`1 i p`, khoảng thời gian bộ đệm bị ảnh hưởng của nó là`[p, p + k_i - 1]`. Lưu trữ số hiệu hoạt động của tải trên mỗi nút cây phân đoạn hoàn toàn nằm trong khoảng này. Chúng ta không cần phải đẩy các thẻ này xuống vì các truy vấn có thể đơn giản kiểm tra tất cả các tổ tiên của lá của chúng. 
4. Đối với mỗi thao tác in`2 p`, đi bộ từ vị trí bộ đệm`p`về phía gốc cây phân đoạn và tìm số thao tác tải lớn nhất gặp phải. Nếu không có thao tác nào như vậy thì địa chỉ bộ đệm chưa bao giờ được ghi nên câu trả lời của nó là 0. 
5. Nếu tìm thấy một thao tác tải, hãy tính toán phần bù tương ứng bên trong phần dữ liệu của nó. Nếu tải bắt đầu ở địa chỉ bộ đệm`s`, sau đó truy vấn địa chỉ bộ đệm`p`đề cập đến dữ liệu bù đắp`p - s + 1`. Đính kèm truy vấn in này vào thao tác tải đó. Chúng tôi chưa tính toán giá trị byte của nó. 
6. Khởi tạo một cây Fenwick khái niệm cho mỗi phần dữ liệu. Chúng được lưu trữ trong một mảng byte phẳng, với phần bù cơ sở riêng cho từng phần. Để tăng thêm`3 i l r`, thêm vào`1`Tại`l`Và`-1`Tại`r + 1`khi vị trí đó tồn tại. Tổng tiền tố Fenwick tại vị trí`x`đưa ra số lượng gia tăng có byte bị ảnh hưởng`x`cho đến nay. 
7. Quét các thao tác đã lưu theo thứ tự ban đầu. Để tăng dần, hãy cập nhật cây Fenwick thích hợp. Đối với một lần tải, cây Fenwick hiện chứa chính xác các phần tăng đã xảy ra trước lần tải này, do đó, phần dữ liệu hiện ở chính xác là phiên bản được sao chép vào bộ đệm. 
8. Khi tải, hãy truy cập mọi truy vấn in được đính kèm với tải đó. Đối với mỗi truy vấn, hãy đọc byte gốc ở phần bù được lưu trữ của nó, thêm tổng tiền tố Fenwick cho phần bù đó và giảm modulo kết quả 256. Lưu trữ câu trả lời bằng cách sử dụng chỉ mục đầu ra ban đầu của truy vấn. 
9. In câu trả lời theo thứ tự truy vấn. Các truy vấn xảy ra trước bất kỳ tải nào đều có câu trả lời mặc định là 0, trong khi mọi truy vấn khác đều được trả lời khi tải riêng của nó được xử lý. 

Tại sao nó hoạt động: đối với mỗi địa chỉ bộ đệm, cây phân đoạn sẽ lưu trữ mọi tải có phạm vi chứa địa chỉ đó và số tải lớn nhất là tải mới nhất như vậy. Do đó, mọi truy vấn in đều được đính kèm chính xác với thao tác xác định byte bộ nhớ đệm của nó lần cuối. Khi đạt được tải đó trong lần quét thứ hai, mọi gia số trước tải đã được chèn vào cây Fenwick tương ứng, trong khi mọi gia số sau tải vẫn chưa được chèn. Do đó, byte được tính toán chính xác là ảnh chụp nhanh được sao chép vào bộ đệm. Những thay đổi sau này đối với phần dữ liệu không thể ảnh hưởng đến câu trả lời được lưu trữ, phù hợp với ngữ nghĩa bộ đệm được yêu cầu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())

    lengths = array('i', [0]) * (m + 1)
    value_base = array('i', [0]) * (m + 1)
    fenwick_base = array('i', [0]) * (m + 1)

    values = bytearray()
    fenwick_total = 0

    for i in range(1, m + 1):
        tokens = input().split()
        k = int(tokens[0])

        lengths[i] = k
        value_base[i] = len(values)
        fenwick_base[i] = fenwick_total

        values.extend(int(x) for x in tokens[1:])
        fenwick_total += k + 1

    # Store operations compactly.
    typ = array('b', [0]) * q
    a = array('i', [0]) * q
    b = array('i', [0]) * q
    c = array('i', [0]) * q

    for t in range(q):
        op = list(map(int, input().split()))
        typ[t] = op[0]
        if op[0] == 1:
            a[t] = op[1]       # data piece
            b[t] = op[2]       # cache start
        elif op[0] == 2:
            a[t] = op[1]       # cache position
        else:
            a[t] = op[1]       # data piece
            b[t] = op[2]       # left
            c[t] = op[3]       # right

    # Segment tree for latest load covering a cache position.
    size = 1
    while size < n:
        size <<= 1

    tag = [0] * (2 * size)

    # For every load operation t, head[t] is the first query attached to it.
    head = array('i', [-1]) * q
    nxt = array('i', [-1]) * q
    query_offset = array('i', [0]) * q

    # Answers are bytes, so bytearray is enough.
    query_count = 0
    answers = bytearray()

    # First pass: resolve every cache query to its latest load.
    for t in range(q):
        if typ[t] == 1:
            data_id = a[t]
            start = b[t]
            end = start + lengths[data_id] - 1

            left = start - 1 + size
            right = end - 1 + size

            while left <= right:
                if left & 1:
                    tag[left] = t + 1
                    left += 1
                if not (right & 1):
                    tag[right] = t + 1
                    right -= 1
                left >>= 1
                right >>= 1

        elif typ[t] == 2:
            pos = a[t]
            node = pos - 1 + size
            load_id = 0

            while node:
                if tag[node] > load_id:
                    load_id = tag[node]
                node >>= 1

            answers.append(0)
            if load_id:
                load_idx = load_id - 1
                data_id = a[load_idx]
                offset = pos - b[load_idx] + 1

                query_offset[query_count] = offset
                nxt[query_count] = head[load_idx]
                head[load_idx] = query_count

            query_count += 1

    # Fenwick trees for range increment + point query.
    bit = bytearray(fenwick_total)

    def fenwick_add(base, length, pos, delta):
        while pos <= length:
            idx = base + pos
            bit[idx] = (bit[idx] + delta) & 255
            pos += pos & -pos

    def fenwick_sum(base, pos):
        result = 0
        while pos:
            result += bit[base + pos]
            pos -= pos & -pos
        return result & 255

    # Second pass: reconstruct each data piece exactly at each load time.
    for t in range(q):
        if typ[t] == 3:
            data_id = a[t]
            left = b[t]
            right = c[t]

            base = fenwick_base[data_id]
            length = lengths[data_id]

            fenwick_add(base, length, left, 1)

            after = right + 1
            if after <= length:
                fenwick_add(base, length, after, -1)

        elif typ[t] == 1:
            data_id = a[t]
            base = fenwick_base[data_id]
            original_base = value_base[data_id]

            query_id = head[t]

            while query_id != -1:
                offset = query_offset[query_id]
                increment = fenwick_sum(base, offset)

                value = values[original_base + offset - 1]
                answers[query_id] = (value + increment) & 255

                query_id = nxt[query_id]

    sys.stdout.write('\n'.join(map(str, answers)))

if __name__ == "__main__":
    solve()
```Tập hợp mảng đầu tiên lưu trữ các phần dữ liệu một cách gọn gàng.`value_base[i]`trỏ đến byte gốc đầu tiên của mảnh`i`, trong khi`fenwick_base[i]`chỉ điểm đầu của cây Fenwick của nó. Thêm một khe Fenwick bổ sung cho mỗi mảnh cho phép mọi cây hỗ trợ`r + 1`cập nhật sự khác biệt mà không phân bổ một đối tượng Python riêng cho từng phần dữ liệu. 

Các mảng hoạt động tránh lưu trữ nửa triệu bộ dữ liệu Python. Mỗi phép toán có một kiểu và tối đa ba đối số nguyên, rất nhỏ gọn`array`các đối tượng giữ cho dấu chân bộ nhớ có thể dự đoán được. 

Cây phân đoạn sử dụng các số hoạt động dựa trên một làm thẻ, với số 0 nghĩa là không có tải nào bao phủ nút. Khoảng thời gian cập nhật được chuyển đổi từ địa chỉ bộ đệm sang các lá dựa trên 0 bằng cách sử dụng`start - 1 + size`. Điểm cuối bên phải bao gồm là`start + length - 1`, vì vậy việc chuyển đổi phải sử dụng`end - 1 + size`. Đây là ranh giới chính mà một lỗi riêng lẻ có thể thay đổi tải nào sở hữu truy vấn. 

Cây phân đoạn chỉ được sử dụng để cập nhật phạm vi và truy vấn điểm. Tải lưu trữ số hoạt động của nó trên các nút được bao phủ hoàn toàn. Một truy vấn sẽ kiểm tra mọi nút trên đường dẫn từ gốc tới lá của nó và lấy giá trị tối đa. Không cần phải lan truyền lười biếng vì chúng ta không bao giờ cần hiện thực hóa trạng thái của toàn bộ phân khúc. 

Danh sách liên kết truy vấn tránh được một bộ sưu tập lớn các danh sách Python.`head[t]`trỏ tới các truy vấn có giá trị bộ đệm cuối cùng đến từ quá trình tải`t`, trong khi`nxt`liên kết các truy vấn thuộc cùng một tải. Do đó, một truy vấn được xử lý chính xác một lần trong lần chuyển thứ hai. 

Cây Fenwick lưu trữ sai phân theo modulo 256. Mặc dù cây Fenwick thông thường thường lưu trữ các tổng số nguyên tùy ý, nhưng các giá trị byte chỉ phụ thuộc vào tổng theo modulo 256, do đó mọi giá trị được lưu trữ có thể duy trì một cách an toàn trong một`bytearray`. Tổng tiền tố cũng được giảm theo modulo 256 trước khi được thêm vào byte gốc. 

Lần thứ hai xử lý số gia tăng trước bất kỳ lần tải nào sau đó, khớp chính xác với thứ tự thời gian của các thao tác ban đầu. Khi đạt đến một tải, chưa có phần tăng sau nào được đưa vào cây Fenwick, do đó, giá trị được tính cho mỗi truy vấn gắn với tải đó là ảnh chụp nhanh bộ nhớ đệm bất biến của nó. 

Số nguyên Python không bị tràn nhưng việc triển khai vẫn làm giảm rõ ràng các giá trị byte theo modulo 256. Điều này phản ánh định nghĩa vấn đề và cho phép sử dụng mảng byte nhỏ gọn một cách an toàn. 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
5 2 10
3 255 0 15
4 1 2 1 3
2 1
1 2 2
1 1 1
2 1
2 4
3 1 1 2
2 1
1 1 2
2 2
2 5
```Lượt đầu tiên xác định tải nào sở hữu mỗi truy vấn. 

| Hoạt động | Hành động | Phạm vi bộ đệm bị ảnh hưởng | Vị trí truy vấn | Sở hữu tải | 
| --- | --- | --- | --- | --- | 
| 1 | Địa chỉ truy vấn 1 | không | 1 | không | 
| 2 | Tải mảnh 2 lúc 2 | 2..5 | không | không | 
| 3 | Tải mảnh 1 lúc 1 | 1..3 | không | không | 
| 4 | Địa chỉ truy vấn 1 | không | 1 | tải 3 | 
| 5 | Địa chỉ truy vấn 4 | không | 4 | tải 2 | 
| 6 | Phần tăng 1, 1..2 | không | không | không | 
| 7 | Địa chỉ truy vấn 1 | không | 1 | tải 3 | 
| 8 | Tải mảnh 1 lúc 2 | 2..4 | không | không | 
| 9 | Địa chỉ truy vấn 2 | không | 2 | tải 8 | 
| 10 | Địa chỉ truy vấn 5 | không | 5 | tải 2 | 

Ở thao tác 3, địa chỉ bộ đệm 1 nhận byte đầu tiên của phần 1, tức là`255`. Mức tăng sau đó ở thao tác 6 không ảnh hưởng đến bản sao bộ nhớ đệm đó. Tuy nhiên, ở thao tác 8, phần 1 đã được tăng lên nên byte đầu tiên của nó bây giờ là`0`. Do đó, hai lần tải của cùng một phần dữ liệu sẽ tạo ra các ảnh chụp nhanh bộ đệm khác nhau. 

Trong lần vượt qua thứ hai, các trạng thái tải liên quan là: 

| Tải | Phần dữ liệu | Dữ liệu hiện tại tại thời điểm tải | Phần bù truy vấn đính kèm | Đáp án | 
| --- | --- | --- | --- | --- | 
| 2 | 2 |`[1, 2, 1, 3]`| địa chỉ 4 → offset 3 |`1`| 
| 3 | 1 |`[255, 0, 15]`| địa chỉ 1 → offset 1 |`255`| 
| 8 | 1 |`[0, 1, 15]`| địa chỉ 2 → offset 1 |`0`| 

Truy vấn trước khi tải bất kỳ vẫn còn`0`và truy vấn cuối cùng tại địa chỉ 5 thuộc về tải 2 và trả về byte thứ tư của nó,`3`. Đầu ra cuối cùng là`0, 255, 1, 255, 0, 3`, phù hợp với mẫu 

Đối với ví dụ thứ hai, hãy xem xét:```
5 2 8
3 10 20 30
2 40 50
1 1 2
2 2
2 4
1 2 3
2 2
2 3
2 4
2 5
```Lần tải đầu tiên ghi địa chỉ`2..4`. 

| Hoạt động | Hành động | Tải mới nhất tại địa chỉ được truy vấn | Bù đắp | 
| --- | --- | --- | --- | 
| 1 | Tải mảnh 1 lúc 2 | không | không | 
| 2 | Địa chỉ truy vấn 2 | tải 1 | 1 | 
| 3 | Địa chỉ truy vấn 4 | tải 1 | 3 | 
| 4 | Tải mảnh 2 lúc 3 | không | không | 
| 5 | Địa chỉ truy vấn 2 | tải 1 | 1 | 
| 6 | Địa chỉ truy vấn 3 | tải 4 | 1 | 
| 7 | Địa chỉ truy vấn 4 | tải 4 | 2 | 
| 8 | Địa chỉ truy vấn 5 | không | không | 

Tải thứ hai chỉ bao gồm các địa chỉ`3`Và`4`. Nó không thể thay đổi địa chỉ`2`, và nó không đến được địa chỉ`5`. Kết quả đầu ra là`10, 30, 10, 40, 50, 0`. Dấu vết này chứng tỏ tại sao cây phân đoạn phải ghi nhớ tải mới nhất một cách độc lập cho mỗi vị trí bộ nhớ đệm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng k_i + q log n + q log S) | Đọc dữ liệu là tuyến tính trong tổng kích thước dữ liệu. Mỗi truy vấn cập nhật phạm vi tải và truy vấn bộ nhớ đệm sử dụng O(log n), trong khi mỗi truy vấn tăng dần và mỗi truy vấn được giải quyết sử dụng O(log S). | 
| Không gian | O(n + m + q + tổng k_i) | Cây phân đoạn sử dụng O(n), các thao tác và liên kết truy vấn sử dụng O(q), siêu dữ liệu phần sử dụng O(m) và dữ liệu gốc cộng với bộ lưu trữ Fenwick sử dụng O(sum k_i + m). | 

Với tất cả các giới hạn có liên quan tại`5 * 10^5`, hệ số logarit nhiều nhất là khoảng 19 hoặc 20. Thuật toán không bao giờ thực hiện công tỷ lệ với độ dài của một đoạn dữ liệu được tải, do đó việc tải liên tục một đoạn lớn không gây ra các bản sao lớn lặp đi lặp lại. Giới hạn 5 giây làm cho cấu trúc logarit này trở nên phù hợp, trong khi các mảng nhỏ gọn giúp kiểm soát việc sử dụng bộ nhớ của Python. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`hàm từ giải pháp trên có sẵn trong cùng một tệp hoặc được nhập từ giải pháp đã gửi.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = globals()["input"]

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    globals()["input"] = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        globals()["input"] = old_input

# Provided sample
sample1 = """\
5 2 10
3 255 0 15
4 1 2 1 3
2 1
1 2 2
1 1 1
2 1
2 4
3 1 1 2
2 1
1 1 2
2 2
2 5
"""

assert run(sample1) == "0\n255\n1\n255\n0\n3", "sample 1"

# Minimum-size cache, query before any load.
case_min = """\
1 1 2
1 7
2 1
2 1
"""

assert run(case_min) == "0\n0", "minimum-size input"

# Increment before a load, then increment after the load.
case_snapshot = """\
3 1 5
3 255 0 1
3 1 1 2
1 1 1
3 1 1 3
2 1
"""

assert run(case_snapshot) == "0", "snapshot semantics and modulo 256"

# Overlapping loads and exact boundaries.
case_boundaries = """\
5 2 8
3 10 20 30
2 40 50
1 1 2
2 2
2 4
1 2 3
2 2
2 3
2 4
2 5
"""

assert run(case_boundaries) == "10\n30\n10\n40\n50\n0", "load boundaries"

# All equal values, followed by a range increment.
case_equal = """\
3 1 5
3 7 7 7
1 1 1
3 1 1 3
2 1
2 2
2 3
"""

assert run(case_equal) == "8\n8\n8", "all-equal values"

# Maximum cache size and maximum total data size.
# The single query is at the last cache address, catching right-boundary errors.
max_data = " ".join(["0"] * 500000)
case_max_data = (
    "500000 1 2\n"
    "500000 " + max_data + "\n"
    "1 1 1\n"
    "2 500000\n"
)

assert run(case_max_data) == "0", "maximum n and total data size"

# Maximum number of operations.
# No address is ever loaded, so every query must remain zero.
case_max_q = "1 1 500000\n1 0\n" + "2 1\n" * 500000
assert run(case_max_q) == "0\n" * 500000, "maximum q"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 2`, dữ liệu một byte, hai truy vấn |`0\n0`| Kích thước tối thiểu và truy vấn bộ đệm chưa được xử lý | 
| Tăng trước khi tải, sau khi tải |`0`| Hành vi chụp nhanh và modulo 256 | 
| Hai tải chồng chéo |`10\n30\n10\n40\n50\n0`| Ranh giới phạm vi và ghi đè một phần | 
| Ba byte bằng nhau tăng lên cùng nhau |`8\n8\n8`| Cập nhật phạm vi ảnh hưởng đến từng byte | 
|`n = 500000`, độ dài dữ liệu`500000`|`0`| Ranh giới dữ liệu và bộ đệm tối đa | 
|`q = 500000`truy vấn |`500000`dòng không | Số lượng hoạt động tối đa và hành vi bộ nhớ đệm chưa được xử lý | 

## Vỏ cạnh 

Đối với một truy vấn trước bất kỳ lần tải nào, đường dẫn cây phân đoạn chỉ chứa các thẻ bằng 0. Thuật toán để lại câu trả lời ở giá trị byte mặc định`0`. Ví dụ, với`1 1 2`, dữ liệu`[7]`, và hai`2 1`truy vấn, cả hai kết quả đầu ra đều là`0`. Không có liên kết phần dữ liệu nhân tạo nào được tạo cho địa chỉ bộ đệm chưa được xử lý. 

Đối với phần tăng trước khi tải, cây Fenwick được cập nhật trước khi tải được xử lý ở lần tải thứ hai. Với dữ liệu`[255]`và vận hành`3 1 1 1`, tổng tiền tố Fenwick ở vị trí 1 là`1`. Do đó tải tính toán`(255 + 1) mod 256 = 0`, đó là ảnh chụp nhanh chính xác. 

Để tăng dần sau lần tải, truy vấn sẽ được đính kèm với lần tải trước đó. Khi tải được xử lý ở lần thứ hai, phần tăng sau vẫn chưa đến được cây Fenwick. Với dữ liệu`[255]`, theo sau là tải và sau đó là tăng dần, câu trả lời được lưu trữ vẫn còn`255`, mặc dù bản thân phần dữ liệu cuối cùng sẽ trở thành`0`. 

Đối với các tải chồng chéo, cây phân đoạn so sánh các số thao tác thay vì chỉ lưu trữ xem một vị trí đã từng được tải hay chưa. Giả sử phần 1 được tải ở địa chỉ 2 và phần 2 sau đó được tải ở địa chỉ 3. Truy vấn tại địa chỉ 2 chỉ nhìn thấy lần tải đầu tiên, trong khi các truy vấn tại địa chỉ 3 và 4 nhìn thấy lần tải thứ hai. Thuộc tính thẻ tối đa cho kết quả chính xác này vì phạm vi của lần tải thứ hai không chứa địa chỉ 2. 

Đối với tải kết thúc chính xác tại địa chỉ bộ đệm cuối cùng, điểm cuối bên phải được tính như sau`start + length - 1`. Cây phân đoạn chuyển đổi điểm cuối bao gồm này bằng cách sử dụng`end - 1 + size`. Một tải có chiều dài`500000`bắt đầu từ`1`do đó bao phủ lá cho địa chỉ`500000`, được thực hiện bằng thử nghiệm kích thước tối đa. 

Đối với khoảng tăng phạm vi kết thúc ở byte cuối cùng của phần dữ liệu, sự khác biệt sẽ cập nhật tại`r + 1`không được thực hiện. Việc kiểm tra việc thực hiện`after <= length`trước khi cập nhật vị trí đó. Nếu không có sự kiểm tra này, điểm đánh dấu sự khác biệt sẽ rò rỉ vào phần logic tiếp theo trong kho lưu trữ phẳng của Fenwick. 

Đối với tràn byte, cả đóng góp của Fenwick và byte gốc đều được kết hợp với`& 255`. Như vậy`255 + 1`trở thành`0`và các gia số lặp lại sẽ xoay vòng tự nhiên qua tất cả các giá trị 256 byte mà không yêu cầu biểu diễn số nguyên lớn.
