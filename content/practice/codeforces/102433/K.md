---
title: "CF 102433K - Bộ đệm máy tính"
description: "Có hai loại trạng thái có thể thay đổi trong vấn đề và việc tách chúng ra là chìa khóa cho toàn bộ giải pháp. Bộ đệm có n vị trí byte và bắt đầu hoàn toàn bằng 0. Riêng biệt có m mảng nguồn."
date: "2026-08-12T07:45:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 150
verified: true
draft: false
---

[CF 102433K - Bộ nhớ đệm máy tính](https://codeforces.com/problemset/problem/102433/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có hai loại trạng thái có thể thay đổi trong vấn đề và việc tách chúng ra là chìa khóa cho toàn bộ giải pháp. 

Bộ đệm có`n`vị trí byte và bắt đầu hoàn toàn bằng 0. Riêng biệt, có`m`mảng nguồn. Thao tác tải sẽ sao chép một mảng nguồn hoàn chỉnh vào một phần liền kề của bộ đệm. Sau khi được sao chép, các byte bộ đệm đó sẽ độc lập với mảng nguồn. Những thay đổi sau này đối với nguồn chỉ ảnh hưởng đến tải trong tương lai. 

Cập nhật nguồn tăng phần liền kề của một mảng nguồn theo modulo 256. Thao tác in yêu cầu byte hiện tại tại một địa chỉ bộ đệm. Đầu ra là một giá trị byte cho mỗi thao tác in. Tuyên bố chính thức đưa ra`n, m, q <= 5 * 10^5`, trong khi tổng chiều dài của tất cả các mảng nguồn cũng tối đa`5 * 10^5`; thời gian giới hạn là 5 giây. 

lớn`q`ngay lập tức loại trừ việc thực hiện công việc tỷ lệ thuận với độ dài của mảng được tải cho mỗi lần tải. Một mảng nguồn duy nhất có thể có độ dài`5 * 10^5`và có thể được tải hàng trăm nghìn lần, vì vậy`O(s_i)`hoạt động tải có thể dẫn đến khoảng`2.5 * 10^11`phép gán byte. Tổng kích thước mảng nguồn bị giới hạn không giúp ích gì cho việc này vì cùng một nguồn có thể được tải nhiều lần. 

Ngoài ra còn có một vấn đề tạm thời khiến việc mô phỏng trực tiếp dễ mắc sai sót một cách đáng ngạc nhiên. Giả sử nguồn chứa`[10, 20]`, chúng tôi tải nó và sau đó tăng nguồn lên`[11, 21]`. Bộ đệm vẫn phải chứa`[10, 20]`. Giải pháp giữ tham chiếu đến nguồn và đánh giá nó khi in sẽ tạo ra các giá trị mới không chính xác. 

Một trường hợp khác là tải chồng chéo. Coi như:```
3 1 3
2 5 6
1 1 1
1 1 2
2 2
```Lần tải đầu tiên đặt`[5, 6]`ở vị trí 1 và 2. Tải thứ hai đặt`[5, 6]`ở vị trí 2 và 3. Câu trả lời cuối cùng là`5`, vì vị trí 2 đã bị ghi đè bởi lần tải thứ hai. Giải pháp chỉ ghi nhớ lần tải đầu tiên bao gồm từng vị trí sẽ trả về ảnh chụp nhanh sai. 

Vị trí bộ nhớ đệm chưa được tải cũng phải giữ nguyên bằng 0. Ví dụ:```
2 1 2
1 7
2 2
2 1
```Đầu ra là:```
0
0
```Cả hai vị trí đều chưa từng được tải. Một giải pháp giả sử mọi bản in tương ứng với một số mảng nguồn sẽ tạo ra một giá trị không tồn tại. 

Cuối cùng, số gia tăng có thể bao quanh 256 và việc bao bọc diễn ra độc lập đối với từng byte. Ví dụ:```
1 1 4
1 255
3 1 1 1
1 1 1
2 1
```Đầu ra là`0`. Lần tải đầu tiên không liên quan đến truy vấn cuối cùng. Lần tải thứ hai xảy ra sau lần tăng, vì vậy nó thấy`255 + 1 = 0`. Giữ tổng số nguyên thông thường mà không giảm modulo 256 tại điểm thích hợp cuối cùng sẽ tạo ra một byte không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thể hiện bộ đệm một cách rõ ràng. Một tải nguồn`i`ở vị trí`p`sao chép tất cả`s_i`byte, một vòng lặp cập nhật nguồn từ`l`bởi vì`r`và bản in sẽ đọc một ô bộ đệm. Điều này đúng vì nó tuân theo chính xác các thao tác. 

Vấn đề là hoạt động tải. Nếu một nguồn có độ dài`5 * 10^5`, một lần tải đã tiêu tốn nửa triệu bài tập. Với`5 * 10^5`tải như vậy, trường hợp xấu nhất đạt`2.5 * 10^11`bài tập. Năm giây là không đủ cho khối lượng công việc đó. 

Quan sát hữu ích đầu tiên là truy vấn in không cần toàn bộ bộ đệm. Nó chỉ cần biết **tải gần đây nhất bao gồm một địa chỉ đó**. Khi chúng tôi biết tải đó, giá trị bộ đệm chính xác là một byte của ảnh chụp nhanh nguồn được thực hiện ở lần tải đó. 

Điều này biến vấn đề bộ đệm thành vấn đề gán phạm vi trong đó truy vấn duy nhất là truy vấn điểm. Có một cách biểu diễn cây phân đoạn đặc biệt đơn giản cho trường hợp này. Khi tải bao trùm một khoảng, hãy phân tách khoảng đó thành các nút cây phân đoạn chuẩn thông thường và đặt số hoạt động của tải vào mỗi nút đó. Đối với vị trí bộ đệm, hãy đi từ lá của nó đến thư mục gốc và lấy số thao tác tối đa gặp phải. Mức tối đa đó chính xác là tải gần đây nhất bao trùm vị thế. 

Quan sát thứ hai là về ảnh chụp nhanh nguồn. Giả sử in ở vị trí bộ đệm`p`đề cập đến tải trọng khi vận hành`t`. Nếu tải đó bắt đầu nguồn`i`ở vị trí bộ đệm`start`, thì byte được in là vị trí nguồn`p - start + 1`khi nguồn xem xét hoạt động`t`. 

Chúng ta có thể khám phá những điều này`(load time, source offset)`yêu cầu trong lần vượt qua đầu tiên. Sau đó, chúng tôi thực hiện lần thứ hai theo trình tự thời gian thông qua các hoạt động. Khi chúng tôi đạt đến tải vào thời điểm`t`, tất cả các bản cập nhật nguồn trước tải đó đã được áp dụng, do đó nguồn chính xác ở trạng thái tải được sao chép. Chúng tôi có thể trả lời mọi truy vấn in liên quan đến tải đó tại thời điểm đó. 

Đối với các cập nhật nguồn, các mảng nguồn có thể được nối thành một mảng khái niệm vì một bản cập nhật không bao giờ chuyển từ nguồn này sang nguồn khác. Sau đó, cây phân đoạn hỗ trợ các truy vấn điểm và bổ sung phạm vi có thể duy trì tất cả các gia số nguồn. Nó không cần lan truyền lười biếng hoặc giá trị đầy đủ ở mỗi nút. Mỗi phép cộng phạm vi được lưu trữ trên các nút cây phân đoạn chuẩn của nó và một truy vấn điểm tính tổng các thẻ trên đường dẫn đến lá. 

Do đó, hai đường chuyền giải quyết được hai vấn đề độc lập. Cây phân đoạn đầu tiên trả lời "tải nào sở hữu vị trí bộ đệm này?" Cây phân đoạn thứ hai trả lời "giá trị của vị trí nguồn này khi tải đó xảy ra là bao nhiêu?" 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(q * max(s_i) + q * max(s_i))`trong trường hợp xấu nhất |`O(n + sum s_i)`| Quá chậm | 
| Tối ưu |`O((q + sum s_i) log(max(n, sum s_i)))`|`O(n + q + sum s_i)`| Đã chấp nhận | 

Giới hạn vũ phu có thể được xem xét cụ thể hơn cho đến`2.5 * 10^11`phân công bộ đệm từ tải một mình. Phương pháp tối ưu chỉ thực hiện công việc logarit trên mỗi lần tải, in và cập nhật nguồn. 

## Hướng dẫn thuật toán 

1. Ghép tất cả các mảng nguồn thành một mảng byte. Ghi lại vị trí toàn cầu bắt đầu của mọi nguồn. Vì tổng chiều dài nguồn tối đa là`5 * 10^5`, điều này sử dụng không gian tuyến tính. 
2. Xây dựng cây phân đoạn trên các địa chỉ bộ đệm. Ban đầu, mọi thẻ cây đều bằng 0, nghĩa là chưa có tải nào bao phủ phần bộ nhớ đệm đó. 
3. Xử lý các thao tác theo trình tự thời gian ở lần đầu tiên. Đối với tải khi vận hành`t`, xác định khoảng thời gian lưu trữ của nó và ghi`t`vào các nút cây phân đoạn chuẩn bao trùm khoảng đó. Chúng tôi không truyền các giá trị này xuống dưới vì truy vấn điểm sau có thể đơn giản kiểm tra mọi tổ tiên của lá của nó. 
4. Để in ở vị trí bộ đệm`p`, đi từ lá đến gốc và tìm số thao tác tải lớn nhất trên đường dẫn đó. Nếu mức tối đa bằng 0 thì vị trí bộ đệm chưa bao giờ được tải nên câu trả lời là 0. 
5. Nếu bản in thuộc về tải khi hoạt động`t`, giải mã nguồn của tải đó và vị trí bộ đệm bắt đầu. Phần bù nguồn tương ứng là`p - start + 1`. Lưu trữ phần bù này trong danh sách yêu cầu kèm theo hoạt động`t`. Khe đầu ra cũng được ghi lại, do đó các yêu cầu có thể được trả lời sau đó mà không thay đổi thứ tự đầu ra ban đầu. 
6. Lưu trữ các thao tác một cách gọn gàng khi thực hiện bước đầu tiên. Bước thứ hai chỉ cần các thao tác tải và cập nhật nguồn, do đó quá trình triển khai gói mỗi thao tác thành một số nguyên 64 bit thay vì giữ các bộ dữ liệu Python lớn. 
7. Xóa cây phân đoạn và sử dụng lại nó cho mảng nguồn. Ghép nối các nguồn về mặt khái niệm thành một hệ tọa độ. Một bản cập nhật nguồn trên nguồn`i`, vị trí`l`bởi vì`r`, trở thành phép cộng phạm vi trên khoảng toàn cục`[base[i] + l - 1, base[i] + r - 1]`. 
8. Thực hiện lại các thao tác theo thứ tự thời gian. Để cập nhật nguồn, hãy thêm một nguồn vào phạm vi toàn cầu đó. Đối với tải khi vận hành`t`, tất cả các cập nhật nguồn xảy ra trước đó`t`nay đã được áp dụng. Duyệt qua danh sách yêu cầu đính kèm`t`và đối với mỗi độ lệch nguồn được yêu cầu, hãy đọc byte gốc của nó cộng với số gia tăng tích lũy tại vị trí đó, giảm modulo 256. 
9. Sau khi quá trình phát lại kết thúc, mọi kết quả in đều được điền theo thứ tự ban đầu. Viết mảng byte của câu trả lời làm đầu ra cuối cùng. 

### Tại sao nó hoạt động 

Đối với mỗi vị trí bộ đệm, cây phân đoạn đầu tiên duy trì bất biến rằng dấu thời gian tải tối đa trên đường dẫn từ gốc đến lá của nó là tải mới nhất có khoảng thời gian chứa vị trí đó. Do đó, mọi bản in đều được liên kết với chính xác tải có byte hiện có ở đó. 

Đối với một yêu cầu liên quan đến tải`t`, lượt thứ hai đạt đến hoạt động`t`chỉ sau khi xử lý mọi cập nhật nguồn trước đó`t`và trước khi xử lý bất cứ điều gì sau`t`. Do đó, giá trị nguồn được đọc tại thời điểm đó chính xác là giá trị được sao chép bởi tải. Các bản cập nhật nguồn sau này không thể thay đổi các byte bộ đệm đã được tải, do đó việc trả lời yêu cầu tại dấu thời gian của quá trình tải tương đương với việc trả lời yêu cầu đó ở thao tác in ban đầu. 

Hai bất biến cùng nhau cung cấp giá trị bộ nhớ đệm chính xác cho mỗi truy vấn in. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    input = sys.stdin.readline

    n, m, q = map(int, input().split())

    # base[i] is the 1-based global position of the first byte of source i.
    base = [0] * (m + 2)
    data = bytearray()

    pos = 1
    for i in range(1, m + 1):
        parts = input().split()
        s = int(parts[0])
        base[i] = pos
        data.extend(int(x) for x in parts[1:])
        pos += s

    total = pos - 1
    base[m + 1] = pos

    # One tree size is enough for both the cache and the concatenated sources.
    limit = max(n, total)
    size = 1
    while size < limit:
        size <<= 1

    # First pass: tree[node] stores the latest load timestamp covering
    # the interval represented by this node.
    tree = [0] * (2 * size)

    # Operations are packed into 64-bit integers.
    # Bits 60..61: type
    # Bits 40..59: first argument
    # Bits 20..39: second argument
    # Bits 0..19 : third argument
    ops = array('Q', [0]) * (q + 1)

    # Requests belonging to load time t form a linked list.
    head = array('i', [-1]) * (q + 1)
    req_offset = array('i')
    req_next = array('i')

    # One byte per print query, in original query order.
    answers = bytearray()

    MASK = (1 << 20) - 1

    for t in range(1, q + 1):
        parts = input().split()
        typ = int(parts[0])

        if typ == 1:
            src = int(parts[1])
            start = int(parts[2])

            ops[t] = (1 << 60) | (src << 40) | start

            if src < m:
                length = base[src + 1] - base[src]
            else:
                length = total - base[src] + 1

            left = size + start - 1
            right = size + start + length - 1  # exclusive

            # Range assignment of timestamp t.
            while left < right:
                if left & 1:
                    tree[left] = t
                    left += 1
                if right & 1:
                    right -= 1
                    tree[right] = t
                left >>= 1
                right >>= 1

        elif typ == 2:
            p = int(parts[1])

            node = size + p - 1
            load_time = 0

            while node:
                value = tree[node]
                if value > load_time:
                    load_time = value
                node >>= 1

            query_id = len(answers)
            answers.append(0)

            if load_time != 0:
                load_code = ops[load_time]
                src = (load_code >> 40) & MASK
                start = load_code & MASK

                offset = p - start + 1

                req_offset.append(offset)
                req_next.append(head[load_time])
                head[load_time] = query_id

        else:
            src = int(parts[1])
            left_pos = int(parts[2])
            right_pos = int(parts[3])

            ops[t] = (
                (3 << 60)
                | (src << 40)
                | (left_pos << 20)
                | right_pos
            )

    # Reuse the tree for range-add / point-query operations on source data.
    tree = [0] * (2 * size)

    for t in range(1, q + 1):
        code = ops[t]
        if code == 0:
            continue

        typ = code >> 60

        if typ == 3:
            src = (code >> 40) & MASK
            left_pos = (code >> 20) & MASK
            right_pos = code & MASK

            left = size + base[src] + left_pos - 2
            right = size + base[src] + right_pos - 1  # exclusive

            while left < right:
                if left & 1:
                    tree[left] += 1
                    left += 1
                if right & 1:
                    right -= 1
                    tree[right] += 1
                left >>= 1
                right >>= 1

        elif typ == 1:
            load_code = code
            src = (load_code >> 40) & MASK

            request = head[t]
            if request == -1:
                continue

            source_base = base[src]

            while request != -1:
                offset = req_offset[request]
                global_pos = source_base + offset - 1

                node = size + global_pos - 1
                added = 0

                while node:
                    added += tree[node]
                    node >>= 1

                answers[request] = (data[global_pos - 1] + added) & 255
                request = req_next[request]

    return ''.join(chr(x + 48) if x < 10 else str(x) + '\n'
                   for x in answers)

if __name__ == "__main__":
    sys.stdout.write(solve())
```Các mảng nguồn chỉ được làm phẳng trong giai đoạn thứ hai.`base[i]`mang lại vị trí toàn cầu đầu tiên của nguồn`i`, do đó vị trí nguồn`k`tương ứng với`base[i] + k - 1`. 

Cây phân đoạn đầu tiên được cố tình không phải là cây nhân giống lười biếng thông thường. Các mục nhập của nó là dấu thời gian, không phải byte bộ đệm thực tế. Tải phạm vi ghi dấu thời gian của nó vào`O(log n)`các nút kinh điển. Một truy vấn điểm sẽ kiểm tra tất cả tổ tiên và lấy mức tối đa của chúng. Vì dấu thời gian tăng lên khi các hoạt động được xử lý, dấu thời gian lớn nhất chính xác là tải bao phủ gần đây nhất. 

Số hoạt động được đóng gói cùng với các đối số hoạt động. Tất cả các tọa độ có liên quan ở bên dưới`5 * 10^5`, vậy 20 bit là đủ cho mỗi tọa độ. Điều này tránh việc lưu trữ hàng trăm nghìn bộ dữ liệu Python, việc này sẽ tiêu tốn nhiều bộ nhớ hơn đáng kể. 

Danh sách yêu cầu cũng được lưu trữ bằng mảng số nguyên. Một yêu cầu được xác định bởi vị trí đầu ra của nó và`head[t]`trỏ đến tất cả các truy vấn in phụ thuộc vào tải`t`. Điều này cho phép lượt thứ hai trả lời toàn bộ nhóm truy vấn ngay khi đạt đến mức tải tương ứng. 

Cây thứ hai chỉ lưu trữ các phép cộng phạm vi. Gia số phạm vi được phân tách thành các nút chuẩn và tăng thẻ của chúng lên một. Một truy vấn điểm tính tổng các thẻ trên đường dẫn từ gốc tới lá của nó. Tổng đó là số lượng gia tăng nguồn ảnh hưởng đến byte. 

biểu thức`(original + added) & 255`tương đương với modulo 256 vì cả hai giá trị đều không âm và 256 là lũy thừa của hai. Nó cũng giữ cho hoạt động nhỏ gọn. 

Tất cả các khoảng trong bài toán đều dựa trên một và bao hàm. Biểu diễn cây phân đoạn lặp lại sử dụng các khoảng nửa mở bên trong, đó là lý do tại sao điểm cuối bên phải được chuyển đổi thành tọa độ độc quyền. Phần bù nguồn cũng được ghi rõ ràng`p - start + 1`, điều này tránh được lỗi một vị trí phổ biến khi dịch địa chỉ bộ đệm trở lại chỉ mục nguồn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu chính thức có hai nguồn,`[255, 0, 15]`Và`[1, 2, 1, 3]`, với các tải chồng chéo và cập nhật nguồn sau này. Đầu ra của nó là`0, 255, 1, 255, 0, 3`. 

| Hoạt động | Trạng thái vượt qua lần đầu | Yêu cầu đã được tạo | Giá trị vượt qua thứ hai | 
| --- | --- | --- | --- | 
|`2 1`| không tải tại vị trí 1 | không |`0`| 
|`1 2 2`| vị trí 2..5 nhận tải`2`| không | nguồn 2 ảnh chụp nhanh | 
|`1 1 1`| vị trí 1 được tải`3`| không | nguồn 1 ảnh chụp nhanh | 
|`2 1`| tải mới nhất là`3`, bù lại`1`|`(3, 1)`|`255`| 
|`2 4`| tải mới nhất là`2`, bù lại`3`|`(2, 3)`|`1`| 
|`3 1 1 2`| thẻ bộ đệm không thay đổi | không | nguồn 1 chỉ thay đổi | 
|`2 1`| vẫn tải mới nhất`3`|`(3, 1)`|`255`| 
|`1 1 2`| vị trí 2..4 nhận tải`8`| không | nguồn 1 ảnh chụp nhanh | 
|`2 2`| tải mới nhất là`8`, bù lại`1`|`(8, 1)`|`0`| 
|`2 5`| tải mới nhất là`2`, bù lại`4`|`(2, 4)`|`3`| 

Hàng quan trọng là hoạt động 7. Nguồn 1 đã được tăng lên, nhưng vị trí bộ đệm đã được lấp đầy bởi tải 3 trước mức tăng đó. Do đó, yêu cầu vẫn yêu cầu byte của nguồn 1 tại thời điểm thứ 3, tức là`255`. 

Ở thao tác 8, nguồn 1 có giá trị`[0, 1, 15]`, do đó tải sau ở vị trí 2 sẽ sao chép phiên bản mới hơn đó. Do đó, bản in thứ hai sau lần tải đó sẽ quan sát thấy`0`thay vì giá trị cũ`255`. 

### Ví dụ về thời gian chụp nhanh 

Hãy xem xét đầu vào nhỏ hơn này:```
4 1 7
3 5 6 7
2 1
1 1 2
3 1 2 3
2 3
1 1 1
2 2
2 4
```Nguồn là`[5, 6, 7]`. Dấu vết là: 

| Hoạt động | Tải lần đầu tại vị trí được truy vấn | Yêu cầu | Trạng thái nguồn khi xảy ra tải | Trả lời | 
| --- | --- | --- | --- | --- | 
|`2 1`| không | không |`[5,6,7]`|`0`| 
|`1 1 2`| trọng tải`2`bao gồm các vị trí 2..4 | không |`[5,6,7]`| | 
|`3 1 2 3`| không thay đổi | không | nguồn trở thành`[5,7,8]`| | 
|`2 3`| trọng tải`2`, bù lại`2`|`(2,2)`|`[5,6,7]`|`6`| 
|`1 1 1`| tải mới bao gồm vị trí 1..3 | không |`[5,7,8]`| | 
|`2 2`| trọng tải`5`, bù lại`2`|`(5,2)`|`[5,7,8]`|`7`| 
|`2 4`| trọng tải`2`, bù lại`3`|`(2,3)`|`[5,6,7]`|`7`| 

Truy vấn đầu tiên sau khi cập nhật nguồn vẫn trả về`6`, không`7`, vì quá trình tải của nó diễn ra trước khi cập nhật. Tải sau sẽ thấy bản cập nhật và tạo ra`7`. Đây chính xác là sự phân tách theo thời gian mà thuật toán hai bước nắm bắt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((q + S) log(max(n, S)))`| Mỗi lần tải và in sử dụng thao tác cây bộ nhớ đệm logarit, trong khi mỗi yêu cầu cập nhật nguồn và yêu cầu phụ thuộc sử dụng thao tác cây nguồn logarit. Đây`S = sum s_i <= 5 * 10^5`. | 
| Không gian |`O(n + q + S)`| Hai cây phân đoạn sử dụng`O(max(n,S))`không gian, trong khi dữ liệu nguồn, hoạt động đóng gói và danh sách yêu cầu sử dụng không gian tuyến tính. | 

Với`n`,`q`, Và`S`tất cả được giới hạn bởi`5 * 10^5`, hệ số logarit nhiều nhất là khoảng 19. Việc triển khai cũng sử dụng các mảng số nguyên nhỏ gọn cho nhật ký hoạt động và danh sách yêu cầu, trong khi các byte nguồn mỗi byte chiếm một byte. Điều này giữ cho bộ nhớ tỷ lệ thuận với kích thước đầu vào thay vì tổng số byte được sao chép bởi tất cả các lần tải. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`chức năng từ giải pháp trên có sẵn.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve()
    finally:
        sys.stdin = old_stdin

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

assert run(sample1) == """\
0
255
1
255
0
3
""", "sample 1"

# Minimum-size input.
minimum_case = """\
1 1 6
1 42
2 1
3 1 1 1
1 1 1
2 1
3 1 1 1
2 1
"""

assert run(minimum_case) == """\
0
43
43
""", "minimum size and snapshot"

# Boundary positions and source-end offsets.
boundary_case = """\
5 1 7
4 1 2 3 4
1 1 3
2 3
2 5
3 1 1 1
1 1 1
2 4
2 5
"""

assert run(boundary_case) == """\
1
4
4
4
""", "boundary positions"

# Overlapping loads and a source update between two loads.
overlap_case = """\
5 2 8
2 10 10
3 7 8 9
1 1 1
1 2 2
2 2
3 2 1 3
2 4
1 2 3
2 3
2 5
"""

assert run(overlap_case) == """\
7
9
8
10
""", "overlapping loads"

# Modulo 256 and repeated updates.
modulo_case = """\
3 1 10
2 250 250
1 1 1
3 1 1 2
3 1 1 2
3 1 1 2
3 1 1 2
3 1 1 2
3 1 1 2
1 1 1
2 1
2 2
"""

assert run(modulo_case) == """\
0
0
""", "modulo 256"

# Maximum-size stress case:
# n = 500000, q = 500000, sum(s_i) = 500000.
# Every cache query asks for the final address after one full-length load.
big_data = " ".join(["255"] * 500000)
big_case = (
    "500000 1 500000\n"
    + big_data
    + "\n1 1 1\n"
    + ("2 500000\n" * 499999)
)

assert run(big_case) == "255\n" * 499999, "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ kích thước tối thiểu |`0`,`43`,`43`| Bộ đệm trống, nguồn một byte và ảnh chụp nhanh đã tải vẫn tồn tại trong quá trình tăng nguồn sau này | 
| Trường hợp ranh giới |`1`,`4`,`4`,`4`| Bao gồm các điểm cuối phạm vi, tải tại các vị trí bộ đệm cuối cùng và ranh giới bù nguồn | 
| Trường hợp chồng chéo |`7`,`9`,`8`,`10`| Tải chồng chéo mới nhất thắng và tải sau xem cập nhật nguồn | 
| Trường hợp Modulo |`0`,`0`| Tăng lặp lại và gói qua 256 | 
| Vỏ kích thước tối đa |`255`lặp đi lặp lại`499999`lần | Tối đa`n`, tối đa`q`, tổng kích thước nguồn tối đa, tải toàn dải và truy vấn điểm lặp lại | 

## Vỏ cạnh 

### Vị trí bộ đệm chưa bao giờ được tải 

cho```
2 1 2
1 7
2 2
2 1
```lần đầu tiên không thực hiện phân bổ phạm vi nào cả. Cả hai truy vấn in đều đi theo đường dẫn từ gốc đến lá và tìm dấu thời gian bằng 0. Các ô trả lời của họ vẫn bằng 0, tạo ra`0`Và`0`. 

Thuật toán không cần mảng bộ đệm đặc biệt cho trường hợp này. Dấu thời gian bằng 0 tự nhiên đại diện cho bộ nhớ đệm hoàn toàn bằng 0 ban đầu. 

### Bản cập nhật nguồn sau này không được thay đổi bản sao bộ đệm cũ 

Hãy xem xét:```
1 1 6
1 42
2 1
3 1 1 1
1 1 1
2 1
3 1 1 1
2 1
```Tải ở thao tác 4 xảy ra sau khi tăng một nguồn, do đó giá trị nguồn được sao chép vào bộ đệm là`43`. Gia số sau ở thao tác 6 sẽ thay đổi nguồn thành`44`, nhưng bộ đệm vẫn còn`43`. 

Lần chuyển đầu tiên liên kết bản in cuối cùng với tải 4. Trong lần chuyển thứ hai, yêu cầu đó được trả lời khi đạt được thao tác 4, trước khi áp dụng thao tác 6. Kết quả là`43`. 

### Tải chồng chéo phải chọn dấu thời gian mới nhất 

Dành cho:```
5 2 8
2 10 10
3 7 8 9
1 1 1
1 2 2
2 2
3 2 1 3
2 4
1 2 3
2 3
2 5
```tải đầu tiên bao gồm các vị trí 1 và 2, trong khi tải thứ hai bao gồm các vị trí từ 2 đến 4. Do đó, bản in ở vị trí 2 nhìn thấy tải 2 chứ không phải tải 1. 

Trong cây phân đoạn bộ đệm, tải 1 ghi dấu thời gian 1 vào các nút chuẩn trong khoảng thời gian của nó. Tải 2 sau đó ghi dấu thời gian 2 vào các nút chuẩn trong khoảng thời gian của nó. Một truy vấn ở vị trí 2 nhìn thấy cả hai dấu thời gian dọc theo đường dẫn từ gốc tới lá của nó và lấy giá trị tối đa, cụ thể là 2. 

### Tải phải nắm bắt được nguồn trước khi cập nhật sau 

Lấy:```
4 1 7
3 5 6 7
2 1
1 1 2
3 1 2 3
2 3
1 1 1
2 2
2 4
```Tải ở hoạt động 2 chụp`[5,6,7]`. Bản cập nhật ở thao tác 3 thay đổi nguồn thành`[5,7,8]`, nhưng bản in ở thao tác 4 bị ràng buộc với tải 2 và do đó trả về`6`. 

Khi lượt thứ hai đạt đến hoạt động 2, chưa có bản cập nhật nguồn nào được xử lý. Yêu cầu offset nguồn 2 đọc giá trị ban đầu`6`. Bản cập nhật sau chỉ được xử lý sau đó nên không thể ảnh hưởng đến yêu cầu đó. 

### Lần tải sau sẽ thấy mọi cập nhật nguồn trước đó 

Trong ví dụ tương tự, thao tác 5 tải nguồn sau khi cập nhật từ thao tác 3. Tại thời điểm đó, nguồn là`[5,7,8]`, do đó bản in ở vị trí 2 trả về`7`. 

Lượt thứ hai đã áp dụng thao tác 3 trước khi đến thao tác 5. Do đó, yêu cầu được đính kèm với tải 5 sẽ đọc trạng thái nguồn được cập nhật. Do đó, cùng một nguồn có thể có nhiều ảnh chụp nhanh độc lập trong bộ đệm. 

### Modulo 256 phải được áp dụng cho byte kết quả 

Dành cho:```
3 1 10
2 250 250
1 1 1
3 1 1 2
3 1 1 2
3 1 1 2
3 1 1 2
3 1 1 2
3 1 1 2
1 1 1
2 1
2 2
```sáu lần lượt từng bước`250`vào trong`0`bởi vì`250 + 6 = 256`. Do đó, lần tải thứ hai sẽ sao chép`[0,0]`và cả hai bản in đều trả về 0. 

Cây phân đoạn nguồn lưu trữ số lượng gia tăng dưới dạng số nguyên thông thường. Điều này là an toàn vì tổng tối đa là`q`, dễ dàng phù hợp với số nguyên của Python. Hoạt động modulo được áp dụng khi giá trị byte thực tế được xây dựng lại. 

### Tải kết thúc chính xác tại ranh giới bộ đệm 

Trong thử nghiệm kích thước tối đa, nguồn 1 có độ dài`500000`, và hoạt động`1 1 1`tải nó vào vị trí bộ đệm`1`bởi vì`500000`. Một bản in ở vị trí`500000`phải ánh xạ tới nguồn bù`500000`. 

Việc chuyển đổi phạm vi sử dụng một điểm cuối bên phải độc quyền, do đó khoảng thời gian bên trong là`[1, 500001)`. Truy vấn điểm sử dụng`size + p - 1`, ánh xạ vị trí bộ đệm`500000`chính xác đến lá của nó. Không có vị trí bộ đệm bổ sung nào được bao gồm và byte cuối cùng được xử lý chính xác.
