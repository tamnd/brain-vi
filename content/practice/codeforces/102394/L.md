---
title: "CF 102394L - Thuật toán LRU"
description: "Chúng ta có một chuỗi truy cập a[1..n]. Đối với dung lượng bộ đệm được chọn m, bộ đệm LRU duy trì các mục của nó từ được sử dụng gần đây nhất đến ít được sử dụng gần đây nhất. Bất cứ khi nào một mục được truy cập, nó sẽ trở thành mục đầu tiên trong danh sách."
date: "2026-08-10T19:15:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "L"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 158
verified: true
draft: false
---

[CF 102394L - Thuật toán LRU](https://codeforces.com/problemset/problem/102394/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi truy cập`a[1..n]`. Đối với dung lượng bộ đệm đã chọn`m`, bộ đệm LRU duy trì các mục của nó từ được sử dụng gần đây nhất đến ít được sử dụng gần đây nhất. Bất cứ khi nào một mục được truy cập, nó sẽ trở thành mục đầu tiên trong danh sách. Nếu bộ đệm đã đầy và mục được truy cập không có thì mục cuối cùng sẽ bị xóa. 

Mỗi truy vấn cung cấp một khả năng`m`và một danh sách LRU được đề xuất. Danh sách có thể chứa ít hơn`m`các mục, với các số 0 ở cuối chỉ được sử dụng làm phần đệm trong đầu vào. Chúng tôi phải xác định xem danh sách chính xác này có xuất hiện bất cứ lúc nào trong khi xử lý chuỗi truy cập cố định có dung lượng hay không`m`. 

Cách quan trọng để xem xét trạng thái LRU là từ hướng ngược lại. Giả sử việc thực thi vừa được xử lý`a[i]`. Bắt đầu ở vị trí`i`và đi lùi. Lần đầu tiên chúng ta gặp một giá trị, đó là lần xuất hiện gần đây nhất của giá trị đó. Giá trị khác biệt đầu tiên gặp phải là phần trước của danh sách LRU, giá trị khác biệt thứ hai là mục tiếp theo, v.v. Do đó, danh sách công suất LRU`m`là người đầu tiên`m`các giá trị riêng biệt gặp phải khi quét`a[i], a[i-1], ...`lạc hậu hoặc tất cả các giá trị riêng biệt nếu nhỏ hơn`m`đã xuất hiện. 

Quan sát này loại bỏ hoàn toàn nhu cầu mô phỏng danh sách liên kết riêng cho mỗi truy vấn. 

Các giới hạn được cố tình đủ nhỏ để`O(n^2)`bước tiền xử lý. Từ`n <= 5000`, giới hạn bậc hai có nghĩa là tối đa khoảng 25 triệu lần lặp cho một trường hợp. Giới hạn thời gian chính thức chỉ là 1 giây, vì vậy việc mô phỏng đơn giản mọi truy vấn sẽ tốn kém một cách không cần thiết, trong khi giải pháp khối rõ ràng là không thể. Tổng số phần tử danh sách truy vấn nhiều nhất là`2 * 10^6`, vì vậy việc đọc và băm các danh sách truy vấn là điều có thể chấp nhận được. Tổng của`n`Và`q`trên các trường hợp thử nghiệm cũng bị giới hạn, điều này giữ cho toàn bộ quá trình tiền xử lý bậc hai có thể quản lý được. 

Có một số trường hợp khó có thể bỏ sót. 

Đầu tiên, danh sách yêu cầu trống là hợp lệ. Coi như:```
1
1 1
1
1 0
```Bộ đệm ban đầu trống nên câu trả lời là`Yes`. Một giải pháp chỉ kiểm tra trạng thái sau lần truy cập đầu tiên và giả sử bộ đệm phải chứa thứ gì đó sẽ in không chính xác`No`. Giải pháp dự định cũng coi bộ đệm trống ban đầu là một thời điểm hợp lệ. 

Thứ hai, một truy vấn có thể chứa ít phần tử hơn khả năng của nó. Ví dụ:```
1
3 1
1 2 3
3 1 2 0
```Câu trả lời là`Yes`. Ngay sau khi truy cập`2`, bộ nhớ đệm dung lượng`3`chỉ chứa`[2, 1]`. Việc triển khai bất cẩn có thể yêu cầu bộ đệm phải có chính xác ba phần tử vì dung lượng là ba, điều này sẽ sai. 

Thứ ba, các giá trị trùng lặp không thể xảy ra trong danh sách LRU chính hãng. Ví dụ:```
1
2 1
1 2
2 1 1
```Câu trả lời là`No`. Danh sách LRU chứa mỗi mã định danh được lưu trong bộ nhớ cache nhiều nhất một lần. Về mặt lý thuyết, chỉ so sánh một hàm băm mà không kiểm tra thuộc tính này có thể chấp nhận một truy vấn trùng lặp do xung đột hàm băm, do đó việc triển khai sẽ từ chối rõ ràng các truy vấn có chứa các bản sao. 

Thứ tư, các truy cập lặp lại không tạo ra các mục lặp lại. Ví dụ:```
1
3 1
1 1 2
2 2 1
```Câu trả lời là`Yes`. Sau lần truy cập thứ hai vào`1`, trạng thái vẫn còn`[1]`và sau khi truy cập`2`nó trở thành`[2, 1]`. Giải pháp xử lý mọi quyền truy cập dưới dạng thành phần danh sách mới sẽ mất thuộc tính xác định của LRU. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn riêng biệt. Đối với một truy vấn có dung lượng`m`, chúng ta có thể mô phỏng bộ đệm LRU thông qua tất cả`n`truy cập và kiểm tra xem danh sách được yêu cầu có xuất hiện hay không. Với bản đồ băm thích hợp cộng với danh sách liên kết, mỗi hoạt động LRU có thể được thực hiện`O(1)`, do đó, chi phí cho một truy vấn`O(n)`và tất cả chi phí truy vấn`O(nq)`. Ở giới hạn trường hợp đơn lớn nhất, điều này là về`5000 * 2000 = 10 million`hoạt động truy cập. Trong nhiều trường hợp, sản phẩm của cá nhân`n`Và`q`các giá trị vẫn có thể trở nên lớn và vấn đề được thiết kế xoay quanh bước tiền xử lý có thể tái sử dụng nhiều hơn. Việc triển khai dựa trên mảng đơn giản thậm chí còn tệ hơn vì việc di chuyển một phần tử tùy ý lên phía trước có thể tốn kém.`O(n)`, cho`O(qn^2)`hoạt động, lên tới khoảng 50 tỷ ca cơ bản. 

Cách tiếp cận bạo lực hoạt động vì mọi truy vấn có thể tái tạo độc lập hành vi LRU chính xác. Nó thất bại vì trình tự truy cập giống hệt nhau cho mọi truy vấn, do đó việc phát hiện nhiều lần cùng một thông tin gần đây là lãng phí. 

Quan sát quan trọng là trạng thái LRU sau vị trí`i`không thực sự phụ thuộc vào dung lượng cho đến khi chúng ta quyết định cần lấy bao nhiêu phần tử. Thứ tự gần đây hoàn chỉnh đã được xác định bởi trình tự truy cập. Quét ngược từ`i`, lấy mọi giá trị ngay lần đầu tiên nó gặp. Điều này tạo ra một chuỗi phổ quát các giá trị riêng biệt được sắp xếp từ gần đây nhất đến ít gần đây nhất. một công suất`m`chỉ cần yêu cầu lần đầu tiên`m`các phần tử. 

Điều đó biến vấn đề thành vấn đề băm chuỗi. Đối với mọi điểm cuối`i`, chúng tôi quét ngược, chỉ giữ lại lần xuất hiện đầu tiên của mọi giá trị và xây dựng các giá trị băm đa thức của các tiền tố thu được. Một danh sách truy vấn sau đó có thể được biểu diễn bằng cùng một hàm băm. Nếu giá trị băm khớp với danh sách độ dài`L`, chúng tôi đã tìm thấy tiền tố LRU cần thiết. 

Có một điều kiện bổ sung cho truy vấn có danh sách ngắn hơn dung lượng của nó. Trong trường hợp đó, bộ nhớ đệm không chỉ có danh sách được yêu cầu làm danh sách đầu tiên.`L`các phần tử. Nó phải chứa chính xác những`L`các phần tử, vì bộ đệm chưa được lấp đầy. Như vậy, tại điểm cuối`i`, tổng số giá trị riêng biệt nhìn thấy trong`a[1..i]`cũng phải chính xác`L`. 

Chúng ta có thể cải thiện cách đơn giản`O(n^2 + nq)`giải pháp băm hơn nữa. Thay vì lưu trữ mọi hàm băm và sau đó quét tất cả các truy vấn ở mọi điểm cuối, trước tiên hãy đọc các truy vấn và đưa các hàm băm mục tiêu của chúng vào từ điển. Trong quá trình tiền xử lý bậc hai, hãy giải quyết ngay lập tức mọi truy vấn có hàm băm gặp phải. Điều này mang lại`O(n^2 + sum(m_i))`thời gian dự kiến ​​và chỉ`O(n + q)`bộ nhớ phụ, ngoài danh sách đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với mô phỏng LRU |`O(nq)`với một`O(1)`Triển khai LRU |`O(n)`| Quá lặp đi lặp lại | 
| Lực lượng vũ phu dựa trên mảng |`O(qn^2)`|`O(n)`| Quá chậm | 
| Lưu trữ mọi hàm băm điểm cuối, sau đó quét truy vấn |`O(n^2 + nq + sum(m_i))`|`O(n^2)`| Được chấp nhận trong C++, nặng về bộ nhớ | 
| Tra cứu hàm băm tối ưu trong quá trình tiền xử lý |`O(n^2 + sum(m_i))`dự kiến ​​|`O(n + q)`| Đã chấp nhận | 

Hàm băm là hàm băm đa thức 64 bit không dấu, do đó số học được thực hiện theo modulo`2^64`bằng cách che dấu sau khi nhân. Đây là kiểu chống va chạm tương tự thường được sử dụng trong các giải pháp được chấp nhận cho vấn đề này. 

## Hướng dẫn thuật toán 

1. Đọc mọi truy vấn trước khi xử lý trước chuỗi truy cập. Loại bỏ phần đệm số 0 ở cuối và ghi lại dung lượng, độ dài thực tế và hàm băm đa thức của nó. Danh sách trống được đánh dấu ngay lập tức`Yes`, vì bộ đệm bắt đầu trống. 
2. Từ chối mọi truy vấn không trống có chứa số nhận dạng trùng lặp. Danh sách LRU thực không thể chứa các bản sao, do đó không bao giờ có thể tạo ra một truy vấn như vậy. 
3. Tính toán`distinct[i]`, số lượng định danh khác nhau xuất hiện trong tiền tố`a[1..i]`. Giá trị này chỉ cần thiết cho các truy vấn có danh sách được yêu cầu có ít phần tử hơn dung lượng bộ đệm. 
4. Đặt mọi truy vấn hợp lệ vào một từ điển được khóa bởi`(length, hash)`. Giá trị là danh sách các chỉ mục truy vấn có cặp chính xác đó. Chúng tôi làm điều này để hàm băm được phát hiện trong quá trình tiền xử lý có thể giải quyết ngay lập tức tất cả các truy vấn phù hợp mà không cần quét lại tất cả`q`truy vấn. 
5. Hãy để`D`là độ dài danh sách được yêu cầu tối đa. Đối với mọi điểm cuối`i`, quét`a[i], a[i-1], ...`lùi lại cho đến khi đạt đến điểm bắt đầu của chuỗi hoặc`D`các giá trị riêng biệt đã được tìm thấy. Mảng dấu thời gian được sử dụng để phân biệt các giá trị nhìn thấy trong quá trình quét ngược hiện tại mà không xóa toàn bộ mảng mỗi lần. 
6. Bất cứ khi nào gặp một giá trị riêng biệt mới, hãy thêm nó vào chuỗi lần truy cập gần đây hiện tại và cập nhật hàm băm đa thức của nó. Nếu số lượng giá trị riêng biệt hiện tại là độ dài được yêu cầu, hãy tra cứu`(length, hash)`trong từ điển truy vấn. 
7. Đối với mọi truy vấn được tra cứu băm trả về, hãy chấp nhận truy vấn đó nếu dung lượng của nó bằng với độ dài được yêu cầu. Trong trường hợp đó bộ đệm đã đầy và lần đầu tiên nó`length`các phần tử chính xác là danh sách được truy vấn. 
8. Nếu dung lượng lớn hơn độ dài yêu cầu, chỉ chấp nhận truy vấn khi`distinct[i]`bằng với độ dài được yêu cầu. Điều này có nghĩa là chỉ những phần tử được yêu cầu đó mới xuất hiện ở bất kỳ đâu trong tiền tố được xử lý, do đó bộ đệm có chính xác số lượng phần tử được yêu cầu thay vì có thêm các phần tử cũ hơn. 
9. Sau khi tất cả các điểm cuối đã được xử lý, hãy in`Yes`cho mọi truy vấn đã được giải quyết và`No`cho mọi truy vấn chưa được giải quyết. 

### Tại sao nó hoạt động 

Xem xét việc thực hiện ngay sau vị trí`i`. Đối với bất kỳ định danh nào`x`, sự xuất hiện của`x`gần nhất với`i`là quyền truy cập cuối cùng hiện tại của nó. Nếu chúng ta quét ngược từ`i`, lần đầu tiên chúng ta nhìn thấy`x`chúng tôi gặp chính xác lần truy cập cuối cùng đó. Do đó, sắp xếp các lần gặp đầu tiên bằng cách quét ngược sẽ sắp xếp tất cả các số nhận dạng hiện đã biết từ được sử dụng gần đây nhất đến ít được sử dụng gần đây nhất. Lấy cái đầu tiên`m`các giá trị riêng biệt cung cấp chính xác danh sách LRU cho dung lượng`m`. 

Thuật toán kiểm tra mọi điểm cuối có thể`i`và xây dựng chính xác các tiền tố riêng biệt đó. Vì vậy mọi trạng thái LRU thực tế đều được xem xét. Truy vấn có dung lượng đầy đủ khớp chính xác khi chuỗi của nó bằng một trong các tiền tố này. Truy vấn ngắn hơn khớp chính xác khi tập hợp đầy đủ các giá trị được thấy cho đến nay có cùng kích thước với truy vấn, vì nếu không thì bộ nhớ đệm sẽ chứa các phần tử bổ sung. Bất biến là sau khi xử lý bất kỳ điểm cuối nào`i`, lần quét ngược đầu tiên`k`các giá trị riêng biệt chính xác là giá trị đầu tiên`k`vị trí của thứ tự gần đây của LRU tại điểm cuối đó. Vì mọi điểm cuối có thể đều được kiểm tra nên không bỏ sót trạng thái hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MASK = (1 << 64) - 1
BASE = 911382323

def solve():
    t = int(input())
    output = []

    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        queries = [None] * q
        answers = [False] * q

        # need[(length << 64) | hash] = query indices
        need = {}
        max_len = 0

        for qi in range(q):
            parts = list(map(int, input().split()))
            capacity = parts[0]
            values = parts[1:]

            while values and values[-1] == 0:
                values.pop()

            length = len(values)

            if length == 0:
                # The cache is empty before the first access.
                answers[qi] = True
                queries[qi] = (capacity, 0, 0, False)
                continue

            # An LRU list cannot contain duplicate identifiers.
            if len(set(values)) != length:
                queries[qi] = (capacity, length, 0, False)
                continue

            h = 0
            for x in values:
                h = (h * BASE + x) & MASK

            queries[qi] = (capacity, length, h, True)

            if length > max_len:
                max_len = length

            key = (length << 64) | h
            need.setdefault(key, []).append(qi)

        # distinct[i] = number of distinct values in a[0..i-1].
        distinct = [0] * (n + 1)
        seen_prefix = [0] * (n + 1)
        count = 0

        for i, x in enumerate(a, 1):
            if seen_prefix[x] == 0:
                seen_prefix[x] = 1
                count += 1
            distinct[i] = count

        if max_len > 0 and need:
            # Timestamp trick avoids clearing seen_backward for every endpoint.
            seen_backward = [0] * (n + 1)
            wanted_length = [False] * (max_len + 1)

            for capacity, length, h, valid in queries:
                if valid:
                    wanted_length[length] = True

            stamp = 0

            for end in range(n):
                stamp += 1
                h = 0
                cnt = 0
                j = end

                while j >= 0 and cnt < max_len:
                    x = a[j]

                    if seen_backward[x] != stamp:
                        seen_backward[x] = stamp
                        cnt += 1
                        h = (h * BASE + x) & MASK

                        if wanted_length[cnt]:
                            key = (cnt << 64) | h
                            matched = need.get(key)

                            if matched is not None:
                                total_distinct = distinct[end + 1]

                                for qi in matched:
                                    if answers[qi]:
                                        continue

                                    capacity, length, _, valid = queries[qi]

                                    if not valid:
                                        continue

                                    if capacity == length:
                                        answers[qi] = True
                                    elif total_distinct == length:
                                        answers[qi] = True

                    j -= 1

        for ok in answers:
            output.append("Yes" if ok else "No")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Quá trình xử lý trước truy vấn diễn ra trước quá trình quét trình tự truy cập tốn kém vì nó cho phép chúng tôi biến mọi mục tiêu có thể thành tra cứu từ điển. Dung lượng của truy vấn được lưu trữ riêng biệt với độ dài danh sách thực tế của nó vì hai số này có ý nghĩa khác nhau khi danh sách đầu vào được đệm bằng số 0. 

Việc kiểm tra trùng lặp được thực hiện có chủ ý trước khi chèn truy vấn vào`need`. Băm được sử dụng để so sánh nhanh, nhưng hàm băm đa thức có tính xác suất, do đó việc từ chối các danh sách không thể có cấu trúc sẽ tránh chấp nhận một chuỗi trùng lặp chỉ vì xung đột. 

các`distinct`mảng tiền tố đếm xem có bao nhiêu mã định danh khác nhau đã xuất hiện cho đến mỗi điểm cuối. Đối với một truy vấn với`capacity > length`, điều kiện`distinct[end + 1] == length`chính xác là tình trạng bộ đệm chưa được lấp đầy ngoài danh sách được yêu cầu. 

Quá trình quét ngược sử dụng mảng dấu thời gian thay vì xóa mảng boolean cho mọi điểm cuối. Khi`stamp`thay đổi, mọi nhãn hiệu cũ trở nên không hoạt động về mặt logic. Điều này tiết kiệm thêm một`O(n^2)`thao tác xóa và giữ cho vòng lặp bên trong tập trung vào quá trình quét ngược thực tế. 

Hàm băm được cập nhật theo thứ tự giống như danh sách LRU. Nếu quét ngược thấy`x`, sau đó`y`, sau đó`z`, giá trị kết quả là hàm băm của`[x, y, z]`, không phải ngược lại. Mặt nạ là bắt buộc vì số nguyên Python không tự nhiên tràn như số nguyên không dấu trong C++. Áp dụng`& MASK`tái tạo rõ ràng modulo số học`2^64`. 

Không có lỗi tràn số nguyên sau khi che giấu vì số học dự định chính xác là modulo`2^64`. Độ dài được dịch chuyển 64 bit khi xây dựng khóa từ điển, do đó độ dài danh sách khác nhau không thể chia sẻ khóa ngay cả khi giá trị băm 64 bit của chúng bằng nhau. 

Vòng lặp dừng khi`cnt == max_len`, thay vì khi chỉ số lùi đạt đến một ranh giới cụ thể. Điều này là đủ vì không có truy vấn nào yêu cầu nhiều hơn`max_len`các giá trị riêng biệt và bất kỳ giá trị nào ngoài giá trị đó đều không thể ảnh hưởng đến bất kỳ truy vấn nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trình tự truy cập là`4, 3, 4, 2, 3, 1, 4`. Trình tự phân biệt ngược ở mỗi điểm cuối là thứ tự gần đây phổ biến mà từ đó thu được mọi danh sách LRU theo dung lượng cụ thể. 

| Điểm cuối | Truy cập | Trình tự phân biệt ngược | Số tiền tố riêng biệt | 
| --- | --- | --- | --- | 
| 1 | 4 |`[4]`| 1 | 
| 2 | 3 |`[3, 4]`| 2 | 
| 3 | 4 |`[4, 3]`| 2 | 
| 4 | 2 |`[2, 4, 3]`| 3 | 
| 5 | 3 |`[3, 2, 4]`| 3 | 
| 6 | 1 |`[1, 3, 2, 4]`| 4 | 
| 7 | 4 |`[4, 1, 3, 2]`| 4 | 

Truy vấn đầu tiên có dung lượng`1`và mục tiêu`[4]`. Tại điểm cuối 1, trình tự phân biệt ngược là`[4]`, vậy là nó khớp rồi. 

Truy vấn thứ hai yêu cầu`[2, 3]`với năng lực`2`. Không có điểm cuối nào có giá trị này là hai giá trị riêng biệt đầu tiên. Tại điểm cuối 4, trình tự bắt đầu`[2, 4]`, trong khi ở điểm cuối 5 nó bắt đầu`[3, 2]`, vậy câu trả lời là`No`. 

Truy vấn thứ ba yêu cầu`[3, 2, 1]`với năng lực`3`. Không có chuỗi phân biệt ngược nào bắt đầu bằng tiền tố đó, vì vậy nó cũng`No`. 

Truy vấn thứ tư có dung lượng`4`nhưng chỉ yêu cầu`[4, 1, 3, 2]`. Tại điểm cuối 7, tổng số giá trị riêng biệt được nhìn thấy chính xác là bốn và chuỗi phân biệt ngược chính xác là danh sách được yêu cầu. Do đó, điều kiện ngắn hơn công suất được thỏa mãn và câu trả lời là`Yes`. 

Truy vấn thứ năm yêu cầu`[3, 4]`với năng lực`4`. Tại điểm cuối 2, trình tự lùi là`[3, 4]`và chỉ có hai giá trị riêng biệt xuất hiện. Vì độ dài được yêu cầu là hai và dung lượng là bốn nên bộ đệm chính xác là`[3, 4]`, vậy câu trả lời là`Yes`. 

Kết quả đầu ra là:```
Yes
No
No
Yes
Yes
```### Mẫu 2 

Hãy xem xét:```
1
4 4
1 2 1 3
2 1 2
2 2 3
3 3 1 2
4 2 1 0
```Trình tự gần đây lạc hậu là: 

| Điểm cuối | Truy cập | Trình tự phân biệt ngược | Số tiền tố riêng biệt | 
| --- | --- | --- | --- | 
| 1 | 1 |`[1]`| 1 | 
| 2 | 2 |`[2, 1]`| 2 | 
| 3 | 1 |`[1, 2]`| 2 | 
| 4 | 3 |`[3, 1, 2]`| 3 | 

Truy vấn đầu tiên,`[1, 2]`với dung lượng hai, khớp ở điểm cuối 3. 

Truy vấn thứ hai,`[2, 3]`với khả năng hai, không bao giờ xảy ra. Điểm cuối 4 bắt đầu bằng`[3, 1]`, không`[2, 3]`. 

Truy vấn thứ ba,`[3, 1, 2]`với dung lượng ba, khớp ở điểm cuối 4. 

Truy vấn thứ tư có dung lượng bốn nhưng chỉ yêu cầu`[2, 1]`. Tại điểm cuối 2 chỉ có hai mã định danh riêng biệt xuất hiện, do đó bộ nhớ đệm dung lượng 4 chứa chính xác`[2, 1]`. Do đó, truy vấn là`Yes`. 

Ví dụ này cho thấy tại sao dung lượng bộ đệm và độ dài danh sách thực tế phải được xử lý riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nD + sum(m_i))`dự kiến ​​|`D`là độ dài danh sách được yêu cầu lớn nhất và`D <= n`; trường hợp xấu nhất là`O(n^2 + sum(m_i))`| 
| Không gian |`O(n + q)`| Số lượng tiền tố riêng biệt, mảng dấu thời gian, truy vấn và từ điển băm | 

Quá trình tiền xử lý trong trường hợp xấu nhất thực hiện tối đa`n`quét ngược, mỗi lần thu thập nhiều nhất`n`các giá trị riêng biệt, do đó nó hoạt động tối đa`25 million`lặp lại bên trong cho`n = 5000`. Tổng số đầu vào truy vấn chứa tối đa`2 * 10^6`số nguyên. Điều này tốt hơn đáng kể so với việc chạy mô phỏng LRU đầy đủ riêng biệt cho mỗi truy vấn và tránh được`O(n^2)`ma trận băm được sử dụng bởi việc triển khai trực tiếp cùng một ý tưởng. Giới hạn bộ nhớ chính thức là 512 MB, nhưng cách triển khai ở trên chỉ sử dụng bộ nhớ phụ tuyến tính. 

Phần xác suất duy nhất là hàm băm đa thức 64 bit. Với modulo số học`2^64`và một cơ sở lớn hơn mọi định danh có thể có, sự bình đẳng ngẫu nhiên là cực kỳ khó xảy ra. Đây là sự cân bằng tiêu chuẩn được sử dụng bởi các giải pháp băm dự kiến. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample = """\
1
7 5
4 3 4 2 3 1 4
1 4
2 2 3
3 3 2 1
4 4 1 3 2
4 3 4 0 0
"""
assert run(sample) == "Yes\nNo\nNo\nYes\nYes\n", "provided sample"

# Minimum-size input and empty list.
case_min = """\
1
1 2
1
1 0
1 1
"""
assert run(case_min) == "Yes\nYes\n", "minimum size and empty cache"

# All accesses are equal. Duplicate target must be rejected.
case_equal = """\
1
5 4
1 1 1 1 1
1 1
3 1 0 0
2 1 1
2 2 1
"""
assert run(case_equal) == "Yes\nYes\nNo\nNo\n", "all equal values"

# Boundary cases around partial and full capacity.
case_boundary = """\
1
3 4
1 2 3
2 2 1
2 3 2
3 3 2 1
3 2 1 0
"""
assert run(case_boundary) == "Yes\nYes\nYes\nYes\n", "capacity and list-length boundaries"

# Maximum n with a simple exact state.
# At the final endpoint, the LRU list for capacity 5000 is
# [5000, 4999, ..., 1].
n = 5000
sequence = list(range(1, n + 1))
reverse_list = list(range(n, 0, -1))

case_max = (
    "1\n"
    f"{n} 2\n"
    + " ".join(map(str, sequence))
    + "\n"
    + f"{n} "
    + " ".join(map(str, reverse_list))
    + "\n"
    + "1 1\n"
)

assert run(case_max) == "Yes\nYes\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`, một truy vấn trống và một truy vấn`[1]`truy vấn |`Yes`,`Yes`| Bộ đệm trống ban đầu và đầu vào tối thiểu | 
| Năm lần lặp lại`1`truy cập |`Yes`,`Yes`,`No`,`No`| Truy cập nhiều lần, từ chối mục tiêu trùng lặp, bộ đệm một phần | 
| Sự liên tiếp`1 2 3`với năng lực hai và ba | bốn`Yes`câu trả lời | Ranh giới năng lực chính xác và danh sách ngắn hơn | 
|`n=5000`, sự liên tiếp`1..5000`|`Yes`,`Yes`| Độ dài chuỗi tối đa và quét hàm băm lớn | 

## Vỏ cạnh 

### Danh sách yêu cầu trống 

cho```
1
1 1
1
1 0
```số 0 bị xóa, để lại danh sách có độ dài bằng 0. Thuật toán đánh dấu truy vấn này là`Yes`ngay lập tức. Không cần quét ngược vì bộ đệm trống trước lần truy cập đầu tiên. Điều này trực tiếp xử lý trường hợp thường khiến giải pháp đúng lại nhận được câu trả lời sai. 

### Danh sách ngắn hơn dung lượng của nó 

cho```
1
3 1
1 2 3
3 1 2 0
```danh sách được yêu cầu là`[1, 2]`, trong khi công suất là ba. Tại điểm cuối 2, chuỗi phân biệt ngược là`[2, 1]`, vì vậy truy vấn cụ thể này không khớp ở đó. Tại điểm cuối 1, trình tự là`[1]`. Tại điểm cuối 3 nó là`[3, 2, 1]`. Vì vậy, đầu ra đúng thực sự là`No`. 

Một trường hợp tích cực trực tiếp hơn là:```
1
3 1
1 2 1
3 1 2 0
```Tại điểm cuối 2, trình tự phân biệt ngược là`[2, 1]`, không`[1, 2]`, nhưng tại điểm cuối 3 nó là`[1, 2]`. Chỉ có hai giá trị riêng biệt xuất hiện, do đó bộ nhớ đệm dung lượng ba chứa chính xác`[1, 2]`. Đầu ra là`Yes`. 

Sự bình đẳng`distinct[end] == length`là điều tách biệt trường hợp này khỏi bộ nhớ đệm đầy. Chỉ khớp tiền tố là không đủ vì một mục cũ hơn vẫn có thể xuất hiện sau danh sách được yêu cầu. 

### Số nhận dạng trùng lặp trong truy vấn 

cho```
1
2 1
1 2
2 1 1
```danh sách truy vấn là`[1, 1]`. Mỗi trạng thái LRU thực tế đều chứa các mã định danh riêng biệt, vì vậy câu trả lời là`No`. Việc triển khai phát hiện sự trùng lặp trước khi tính toán hoặc đăng ký hàm băm truy vấn. 

### Truy cập lặp lại liên tục 

cho```
1
3 1
1 1 2
2 2 1
```các tiểu bang là`[1]`,`[1]`, Và`[2, 1]`. Trạng thái cuối cùng khớp chính xác với truy vấn, vì vậy câu trả lời là`Yes`. Quá trình quét ngược xử lý việc này một cách tự nhiên vì khi nó gặp dữ liệu thứ hai`1`, càng sớm`1`bị bỏ qua như một bản sao trong quá trình quét đó. 

### Ít hơn`m`những giá trị riêng biệt đã xuất hiện 

cho```
1
3 1
1 2
3 1 2 0
```cuối cùng chỉ có hai số nhận dạng riêng biệt xuất hiện. Trình tự phân biệt ngược là`[2, 1]`và bộ đệm có dung lượng ba chứa chính xác hai phần tử đó. Một truy vấn yêu cầu`[2, 1]`được chấp nhận vì độ dài của nó là hai và`distinct[2]`cũng là hai. 

Nếu thay vào đó trình tự là```
1
3 1
1 2 3
3 2 1 0
```yêu cầu`[2, 1]`không phải là bộ đệm hoàn chỉnh tại bất kỳ thời điểm nào có ba giá trị riêng biệt xuất hiện. Một lần`3`đã xuất hiện, bộ nhớ đệm dung lượng ba không thể loại bỏ nó chỉ vì truy vấn chỉ có hai phần tử. Điều kiện tiền tố riêng biệt ngăn chặn kết quả dương tính giả như vậy. 

### Danh sách được yêu cầu dài hơn chuỗi lần truy cập gần đây có sẵn 

Giả sử trình tự truy cập là```
1
3 1
1 1 1
3 1 2 3
```Không bao giờ có hai hoặc ba mã định danh riêng biệt, vì vậy không có quá trình quét ngược nào có thể tạo ra một chuỗi lần truy cập gần đây có độ dài ba. Câu hỏi vẫn chưa được giải quyết và câu trả lời là`No`. Việc triển khai xử lý vấn đề này một cách tự nhiên vì quá trình quét bên trong dừng ở đầu chuỗi truy cập trước khi đạt đến độ dài được yêu cầu. 

### Cùng một mục tiêu xuất hiện ở nhiều điểm cuối 

Một truy vấn có thể khớp nhiều lần. Ví dụ,```
1
4 1
1 2 1 2
2 2 1
```khớp sau lần truy cập thứ hai và một lần nữa sau lần truy cập thứ tư. Thuật toán không cần nhớ điểm cuối nào là điểm khớp đầu tiên. Khi hàm băm được tìm thấy và điều kiện dung lượng được thỏa mãn, câu trả lời là vĩnh viễn`Yes`. 

Tính chính xác không phụ thuộc vào sự xuất hiện nào được tìm thấy đầu tiên bởi vì vấn đề chỉ hỏi liệu có tồn tại ít nhất một điểm cuối hợp lệ hay không.
