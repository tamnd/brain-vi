---
title: "CF 102423G - Đường nhảy"
description: "Chúng ta có một cây có rễ. Mỗi đỉnh có một nhãn số nguyên. Đường nhảy là một chuỗi các đỉnh được đưa thẳng xuống dưới qua cây, trong đó mọi đỉnh trước đều là tổ tiên của mọi đỉnh sau."
date: "2026-08-12T01:17:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102423
codeforces_index: "G"
codeforces_contest_name: "North American Southeast Regional 2019 (Div 1)"
rating: 0
weight: 102423
solve_time_s: 161
verified: true
draft: false
---

[CF 102423G - Đường nhảy](https://codeforces.com/problemset/problem/102423/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có rễ. Mỗi đỉnh có một nhãn số nguyên. Đường nhảy là một chuỗi các đỉnh được đưa thẳng xuống dưới qua cây, trong đó mọi đỉnh trước đều là tổ tiên của mọi đỉnh sau. Chúng ta được phép bỏ qua bất kỳ số cạnh bình thường nào của cây, do đó các đỉnh liên tiếp trong dãy không nhất thiết phải là cha và con. 

Các nhãn dọc theo các đỉnh được chọn phải không giảm. Ví dụ: nếu các nhãn trên chuỗi từ gốc đến lá là (2,5,3,7), chúng ta có thể chọn (2,5,7) hoặc (2,3,7), nhưng chúng ta không thể chọn (2,5,3). 

Đối với mọi đỉnh (v), câu hỏi quy hoạch động tự nhiên là đường nhảy hợp lệ dài nhất có đỉnh cuối cùng là (v). Nếu đỉnh được chọn trước đó (u) là tổ tiên của (v) và có nhãn nhiều nhất là (v), thì đường đi hợp lệ kết thúc tại (u) có thể được mở rộng bởi (v). 

Đầu vào chứa (n) đỉnh, sau đó là nhãn của chúng, theo sau là cha của mọi đỉnh ngoại trừ gốc. Cha của đỉnh (i) được cho trước đỉnh (i), vì vậy các đỉnh đã có thứ tự tôpô từ gốc tới lá. Đầu ra chứa độ dài đường dẫn tối đa trên tất cả các điểm cuối có thể có và số lượng đường dẫn có độ dài đó, modulo (11092019). Vấn đề chính thức sử dụng (n\le 10^6) và giá trị nhãn trong ([0,10^6]). 

Kích thước của (n) loại trừ mọi thứ bậc hai. Trong một chuỗi, việc kiểm tra mọi đỉnh đối với mọi tổ tiên đã yêu cầu khoảng (n(n-1)/2) so sánh tổ tiên, tức là khoảng (5\cdot10^{11}) thao tác khi (n=10^6). Ngay cả cách tiếp cận (O(n\sqrt n)) cũng quá lớn so với giới hạn cuộc thi 10 giây. Chúng tôi cần công việc đại khái (O(n\log 10^6)). 

Trường hợp tinh tế đầu tiên là một đỉnh duy nhất.```
1
7
```Có đúng một đường đi chứa đỉnh đó nên đáp án là```
1 1
```Việc triển khai khởi tạo số lượng đường dẫn về 0 và chỉ tạo các đường dẫn bằng cách mở rộng tổ tiên hiện có sẽ tạo ra các đường dẫn bằng 0 không chính xác. 

Trường hợp cạnh thứ hai là nhãn bằng nhau. Coi như```
3
5
5
5
1
2
```Cây là một chuỗi có nhãn (5,5,5). Mỗi đỉnh có thể mở rộng đường đi kết thúc tại đỉnh mẹ của nó, vì vậy đường đi dài nhất có độ dài (3) và có chính xác một đường đi như vậy. 

Việc so sánh phải`ancestor_label <= current_label`, không nghiêm ngặt`<`. Sử dụng so sánh chặt chẽ sẽ trả lời sai`1 3`. 

Trường hợp thứ ba liên quan đến một số tiền thân khác nhau có cùng độ dài tối ưu. Coi như```
3
1
3
2
1
1
```Gốc có nhãn (1) và cả hai con đều có thể theo dõi nó. Cả hai đường dẫn đều có độ dài (2), vì vậy câu trả lời là```
2 2
```Một lỗi đếm phổ biến là chỉ giữ lại một phần trước khi một số phần trước có cùng độ dài tốt nhất. Số lượng của tất cả các tiền thân tối ưu phải được thêm vào. 

Cuối cùng, một nút không được sử dụng chính nó như nút trước đó. Nếu nhãn hiện tại được chèn vào cấu trúc dữ liệu trước khi truy vấn nó, các nhãn bằng nhau có thể vô tình làm cho đỉnh hiện tại tự mở rộng. Truy vấn phải xảy ra đầu tiên, sau đó là chèn. 

## Phương pháp tiếp cận 

Chương trình động trực tiếp rất dễ viết về mặt khái niệm. Cho phép`dp[v]`là đường nhảy hợp lệ dài nhất kết thúc ở đỉnh (v) và đặt`ways[v]`là số đường đi như vậy. Chúng tôi kiểm tra mọi tổ tiên (u) của (v) có nhãn nhiều nhất là nhãn của (v). Trong số đó, chúng tôi tìm thấy lớn nhất`dp[u]`. Sau đó`dp[v]`nhiều hơn giá trị đó một đơn vị và`ways[v]`là tổng của`ways[u]`trên tất cả các tổ tiên đạt được mức tối đa đó. Nếu không có tổ tiên phù hợp, đường đi một đỉnh`[v]`cho`dp[v] = 1`Và`ways[v] = 1`. 

Sự lặp lại mạnh mẽ này là đúng vì mọi đường đi hợp lệ kết thúc tại (v) đều có một đỉnh duy nhất trước đó (u) và đỉnh trước đó phải là tổ tiên của (v) với nhãn không lớn hơn nhãn của (v). Vấn đề là chi phí để tìm ra những tổ tiên đó. Trong một chuỗi, đỉnh (i) có thể có (i-1) tiền thân, cho 

[ 
1+2+\cdots+(n-1)=\frac{n(n-1)}2 
] 

séc. Đối với (n=10^6), tức là (499.999.500.000) séc. 

Cấu trúc giúp chúng ta tiết kiệm là mọi truy vấn được thực hiện trên chính xác một đường dẫn từ gốc đến đỉnh hiện tại. Chúng ta không cần thông tin từ các phần tùy ý của cây. Đối với một đỉnh hiện tại cố định (v), chúng ta cần một thao tác trên đường dẫn tổ tiên của nó: trong số các nhãn nhiều nhất (x_v), hãy tìm độ dài đường dẫn tối đa và tổng số đường dẫn đạt được độ dài đó. 

Hãy tưởng tượng việc duy trì thông tin thuộc về đường dẫn root-to-(v) hiện tại trong cây phân đoạn được lập chỉ mục bởi các nhãn. Tại nhãn (x), cây phân đoạn lưu trữ độ dài đường dẫn tốt nhất trong số các tổ tiên đang hoạt động có nhãn đó và số lượng đường dẫn đạt được độ dài đó. Truy vấn tiền tố trên các nhãn (0) đến (x_v) cung cấp chính xác thông tin trước đó được yêu cầu bởi sự lặp lại. 

Có một điều phức tạp. Khi chúng ta di chuyển từ nhánh này sang nhánh khác của cây, cấu trúc dữ liệu phải biểu thị một đường dẫn từ gốc đến đỉnh khác. Cây phân đoạn có thể thay đổi bình thường không thể giữ tất cả các nhánh cùng một lúc. Giải pháp sạch sẽ là sự kiên trì. Mỗi đỉnh có phiên bản cây phân đoạn riêng, thu được từ phiên bản gốc của nó bằng cách chèn đỉnh hiện tại. 

Bởi vì cha của mỗi đỉnh có chỉ số nhỏ hơn nên chúng ta có thể xử lý các đỉnh trực tiếp theo thứ tự đầu vào. Phiên bản`root[v]`đại diện chính xác cho tổ tiên của (v), bao gồm cả (v) chính nó. Khi tính toán (v), chúng ta truy vấn`root[parent[v]]`, do đó đỉnh hiện tại vẫn chưa được chèn vào. 

Cây phân đoạn chỉ có khoảng hai mươi cấp độ vì nhãn có nhiều nhất là (10^6). Bản cập nhật liên tục chỉ sao chép các nút trên một đường dẫn từ gốc đến lá. Do đó, mỗi đỉnh tạo ra (O(\log 10^6)) nút mới. 

Đối với Python, danh sách số nguyên Python thông thường sẽ tiêu tốn quá nhiều bộ nhớ ở quy mô này. Việc triển khai bên dưới lưu trữ các chỉ số con trong`array('i')`và đóng gói từng giá trị cây phân đoạn thành một số nguyên 64 bit. Các bit trên lưu trữ độ dài đường dẫn và 24 bit dưới lưu trữ modulo đếm (11092019). Điều này giữ cho cấu trúc liên tục trong phạm vi bộ nhớ hợp lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Cây phân đoạn liên tục | (O(n\log X)) | (O(n\log X)) | Đã chấp nhận | 

Ở đây (X\le 10^6) là giá trị nhãn tối đa. 

## Hướng dẫn thuật toán 

1. Lưu trữ nhãn của mỗi đỉnh và cha của nó. Bởi vì mọi cha mẹ đều có chỉ số nhỏ hơn nên tất cả thông tin cần thiết cho đỉnh (v) đều có sẵn khi chúng ta đạt tới (v). 
2. Xác định`root[v]`là gốc của phiên bản cây phân đoạn liên tục đại diện cho tổ tiên của (v), cùng với thông tin đường dẫn tốt nhất của chúng được lập chỉ mục theo nhãn. Đối với đỉnh gốc, phiên bản bắt đầu trống. 
3. Truy vấn phiên bản gốc trên khoảng nhãn từ (0) đến`label[v]`. Kết quả là một cặp bao gồm độ dài đường dẫn tiền nhiệm tối đa và tổng số đường dẫn tiền nhiệm có độ dài đó. 
4. Nếu truy vấn không trả về phần trước, hãy gán`dp[v] = 1`Và`ways[v] = 1`. Bản thân đỉnh đơn tạo thành một đường đi hợp lệ. 
5. Nếu không thì chỉ định`dp[v] = best_length + 1`Và`ways[v] = best_count`. Mọi đường dẫn tối ưu kết thúc tại một đường đi trước hợp lệ có thể được mở rộng duy nhất bằng (v), do đó số đường dẫn kết quả chính xác là số lượng đường đi trước. 
6. Tạo`root[v]`bằng cách liên tục chèn`(dp[v], ways[v])`Tại`label[v]`vào phiên bản của cha mẹ. Nếu một số tổ tiên có cùng nhãn và cùng độ dài tốt nhất thì số đếm của chúng sẽ được hợp nhất tại lá đó. 
7. Duy trì đáp án tổng thể trong khi xử lý các đỉnh. Nếu một đỉnh có kích thước lớn hơn`dp`, thay thế độ dài và số lượng toàn cầu. Nếu nó có cùng độ dài, hãy thêm nó`ways`tới modulo số lượng toàn cầu (11092019). 

Truy vấn phải xảy ra trước khi cập nhật. Thứ tự đó là chi tiết quan trọng ngăn không cho đỉnh hiện tại được coi là đỉnh trước của nó. 

### Tại sao nó hoạt động 

Bất biến là ngay trước khi xử lý đỉnh (v),`root[parent[v]]`chứa chính xác thông tin đường dẫn cho mọi tổ tiên của (v) và không có đỉnh nào khác. Tại mỗi nhãn, giá trị được lưu trữ biểu thị đường dẫn tốt nhất kết thúc tại tổ tiên có nhãn đó và số cách để đạt được độ dài tốt nhất đó. Một truy vấn tiền tố thông qua`label[v]`do đó xem xét chính xác tổ tiên có thể có trước hợp pháp (v). 

Sau đó, phép truy toán sẽ xem xét mọi tiền thân cuối cùng có thể có của đường đi tối ưu kết thúc tại (v). Việc chọn độ dài tối đa của phần trước sẽ cho độ dài tối đa có thể sau khi nối thêm (v). Khi nhiều phần trước có cùng độ dài, tập hợp đường đi của chúng sẽ rời rạc vì các đỉnh cuối cùng của chúng khác nhau, do đó số lượng của chúng có thể được cộng thêm. Bản cập nhật liên tục ghi lại trạng thái kết quả cho mọi hậu duệ mà không sửa đổi phiên bản thuộc một nhánh khác. 

Vì mọi đường nhảy hợp lệ đều có chính xác một đỉnh cuối cùng, lấy giá trị lớn nhất`dp[v]`trên tất cả các đỉnh sẽ cho kết quả tối ưu toàn cục và tính tổng`ways[v]`trên các đỉnh đạt được mức tối ưu đó sẽ tính mọi đường đi dài nhất đúng một lần. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 11092019
COUNT_MASK = (1 << 24) - 1

def solve():
    n = int(input())

    labels = array('i')
    max_label = 0

    for _ in range(n):
        x = int(input())
        labels.append(x)
        if x > max_label:
            max_label = x

    parent = array('i', [0]) * n
    for v in range(1, n):
        parent[v] = int(input()) - 1

    # Use a complete binary range [0, size - 1].
    # size is a power of two larger than every possible label.
    size = 1
    while size <= max_label:
        size <<= 1

    height = size.bit_length() - 1

    # Node 0 is the null node.
    left = array('i', [0])
    right = array('i', [0])
    value = array('Q', [0])

    roots = array('i', [0]) * n

    # Reused fixed-size buffer for the nodes copied on one update.
    path = [0] * (height + 1)

    best_global = 0
    count_global = 0

    for v in range(n):
        if v == 0:
            base_root = 0
        else:
            base_root = roots[parent[v]]

        x = labels[v]

        # Query [0, x] in the persistent binary segment tree.
        node = base_root
        best_len = 0
        best_cnt = 0

        for bit in range(height - 1, -1, -1):
            if node == 0:
                break

            if (x >> bit) & 1:
                child = left[node]
                if child:
                    z = value[child]
                    zlen = z >> 24
                    zcnt = z & COUNT_MASK

                    if zlen > best_len:
                        best_len = zlen
                        best_cnt = zcnt
                    elif zlen == best_len:
                        best_cnt += zcnt

                node = right[node]
            else:
                node = left[node]

        # Include the exact leaf x.
        if node:
            z = value[node]
            zlen = z >> 24
            zcnt = z & COUNT_MASK

            if zlen > best_len:
                best_len = zlen
                best_cnt = zcnt
            elif zlen == best_len:
                best_cnt += zcnt

        best_cnt %= MOD

        if best_len == 0:
            dp = 1
            ways = 1
        else:
            dp = best_len + 1
            ways = best_cnt

        # Persistently insert (dp, ways) at label x.
        #
        # Copy the root first, then copy one child per level.
        old = base_root

        new_root = len(value)
        left.append(left[old])
        right.append(right[old])
        value.append(value[old])
        path[0] = new_root

        cur_old = old
        cur_new = new_root

        for level, bit in enumerate(range(height - 1, -1, -1), 1):
            if (x >> bit) & 1:
                old_child = right[cur_old]

                new_child = len(value)
                left.append(left[old_child])
                right.append(right[old_child])
                value.append(value[old_child])

                right[cur_new] = new_child
                cur_old = old_child
                cur_new = new_child
            else:
                old_child = left[cur_old]

                new_child = len(value)
                left.append(left[old_child])
                right.append(right[old_child])
                value.append(value[old_child])

                left[cur_new] = new_child
                cur_old = old_child
                cur_new = new_child

            path[level] = cur_new

        # Merge the new value with whatever was already stored at label x.
        old_leaf_value = value[cur_new]
        old_len = old_leaf_value >> 24
        old_cnt = old_leaf_value & COUNT_MASK

        if dp > old_len:
            value[cur_new] = (dp << 24) | ways
        elif dp == old_len:
            value[cur_new] = (dp << 24) | ((old_cnt + ways) % MOD)

        # Rebuild the copied ancestors bottom-up.
        for level in range(height - 1, -1, -1):
            p = path[level]
            lv = value[left[p]]
            rv = value[right[p]]

            llen = lv >> 24
            rlen = rv >> 24

            if llen > rlen:
                value[p] = lv
            elif rlen > llen:
                value[p] = rv
            else:
                if llen == 0:
                    value[p] = 0
                else:
                    cnt = (lv & COUNT_MASK) + (rv & COUNT_MASK)
                    value[p] = (llen << 24) | (cnt % MOD)

        roots[v] = new_root

        if dp > best_global:
            best_global = dp
            count_global = ways
        elif dp == best_global:
            count_global += ways
            count_global %= MOD

    print(best_global, count_global % MOD)

if __name__ == "__main__":
    solve()
```Mảng đầu vào sử dụng`array('i')`thay vì danh sách Python vì một triệu số nguyên Python sẽ mang một chi phí đáng kể đối tượng. Cây phân đoạn cố định là đối tượng sử dụng bộ nhớ chiếm ưu thế, vì vậy cách biểu diễn này rất quan trọng trong Python. 

Giá trị cây phân đoạn được đóng gói dưới dạng`(length << 24) | count`. Mô-đun ở dưới (2^{24}), vì vậy 24 bit là đủ để đếm. Độ dài đường dẫn tối đa chỉ là (10^6), do đó các bit phía trên còn lại có thể lưu trữ độ dài một cách thoải mái. 

Truy vấn tuân theo biểu diễn nhị phân của nhãn. Bất cứ khi nào bit tương ứng của (x) là 1, toàn bộ cây con bên trái chứa các nhãn nhỏ hơn (x), do đó tổng hợp của nó có thể được đưa vào ngay trước khi tiếp tục vào cây con bên phải. Khi bit bằng 0, cây con bên phải chứa các giá trị lớn hơn (x) và phải được bỏ qua. Chiếc lá cuối cùng được bao gồm riêng biệt. 

Bản cập nhật sao chép chính xác một đường dẫn từ gốc đến lá. Mỗi nút được sao chép ban đầu sẽ kế thừa các nút con cũ và tổng hợp của nó, sau đó nhánh hướng tới nhãn hiện tại được thay thế bằng một nút con mới được sao chép. Sau khi đạt đến chiếc lá, cái mới`(dp, ways)`cặp đôi được hợp nhất ở đó và tổ tiên được sao chép được xây dựng lại từ hai đứa con của họ. 

cố định`path`array tránh phân bổ danh sách Python mới cho mọi đỉnh. Vì độ sâu cây của cây phân đoạn nhãn nhiều nhất là 20 nên kích thước của nó không đổi đối với (n). 

Không có đệ quy trong quá trình xử lý cây hoặc cây phân đoạn. Bản thân một cây có thể là một chuỗi gồm một triệu đỉnh, do đó DFS đệ quy sẽ có nguy cơ vượt quá giới hạn đệ quy của Python và cũng bổ sung thêm chi phí gọi hàm không cần thiết. 

## Ví dụ đã hoạt động 

Các mẫu chính thức bao gồm một chuỗi năm đỉnh có nhãn đều bằng nhau. Đầu vào là:```
5
3
3
3
3
3
1
2
3
4
```Sản lượng dự kiến ​​​​là`5 1`. 

Đối với chuỗi này, mọi đỉnh mới đều có thể mở rộng đường đi duy nhất kết thúc tại đỉnh gốc của nó. 

| Đỉnh | Nhãn | Chiều dài tiền thân tốt nhất |`dp`|`ways`| Kết quả toàn cầu | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 1 | 1 | 1, 1 | 
| 2 | 3 | 1 | 2 | 1 | 2, 1 | 
| 3 | 3 | 2 | 3 | 1 | 3, 1 | 
| 4 | 3 | 3 | 4 | 1 | 4, 1 | 
| 5 | 3 | 4 | 5 | 1 | 5, 1 | 

Điều kiện nhãn bằng được xử lý chính xác vì truy vấn đã bao gồm. Đỉnh chỉ được chèn sau đỉnh của nó`dp`giá trị đã được tính toán, vì vậy nó không bao giờ sử dụng chính nó như giá trị tiền thân. 

Mẫu chính thức thứ hai có nhãn giảm dần từ (4) xuống (0):```
5
4
3
2
1
0
1
2
3
4
```Sản lượng dự kiến ​​​​là`1 5`. 

Không có đỉnh nào có thể theo sau đỉnh trước đó vì mọi nhãn sau đều nhỏ hơn. 

| Đỉnh | Nhãn | Chiều dài tiền thân tốt nhất |`dp`|`ways`| Kết quả toàn cầu | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | 1 | 1 | 1, 1 | 
| 2 | 3 | 0 | 1 | 1 | 1, 2 | 
| 3 | 2 | 0 | 1 | 1 | 1, 3 | 
| 4 | 1 | 0 | 1 | 1 | 1, 4 | 
| 5 | 0 | 0 | 1 | 1 | 1, 5 | 

Điều này chứng tỏ tại sao câu trả lời lại tính các đường đi bắt đầu từ các đỉnh tùy ý. Mỗi đỉnh riêng lẻ đều là một đường đi hợp lệ có độ dài bằng một, vì vậy có năm đường đi dài nhất. 

Mẫu thứ ba là:```
4
1
5
3
6
1
2
3
```Câu trả lời dự kiến ​​là`3 2`. 

Cây là một chuỗi có nhãn (1,5,3,6). Đỉnh 3 không thể theo đỉnh 2 vì (5>3) nhưng có thể theo gốc. Đỉnh 4 có thể theo sau đỉnh 2 hoặc đỉnh 3. 

| Đỉnh | Nhãn | Chiều dài tiền thân tốt nhất |`dp`|`ways`| 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 5 | 1 | 2 | 1 | 
| 3 | 3 | 1 | 2 | 1 | 
| 4 | 6 | 2 | 3 | 2 | 

Hai con đường dài nhất là`[1,2,4]`Và`[1,3,4]`. Số lượng hai ở đỉnh 4 xuất phát trực tiếp từ việc cộng hai số tiền trước tốt như nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log X)) | Mỗi đỉnh thực hiện một truy vấn tiền tố và một cập nhật điểm liên tục, mỗi đỉnh thực hiện (O(\log X)) | 
| Không gian | (O(n\log X)) | Mỗi bản cập nhật liên tục sẽ tạo ra (O(\log X)) nút cây phân đoạn mới | 

Ở đây (X\le 10^6), vậy hệ số logarit nhiều nhất là khoảng hai mươi. Với một triệu đỉnh, cây bền vững chứa khoảng hai mươi triệu nút được sao chép trong trường hợp xấu nhất. Đóng gói`array`biểu diễn được sử dụng đặc biệt để làm cho thang đo đó trở nên thực tế trong Python. Giới hạn đầu vào chính thức là (10^6) đỉnh, trong khi bài toán cuộc thi có giới hạn thời gian là 10 giây. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng tương tự`solve()`thường lệ như việc nộp bài. Trình trợ giúp tạm thời thay thế đầu vào tiêu chuẩn và ghi lại đầu ra tiêu chuẩn.```python
import sys
import io
from array import array

MOD = 11092019
COUNT_MASK = (1 << 24) - 1

def solve():
    input = sys.stdin.readline

    n = int(input())

    labels = array('i')
    max_label = 0

    for _ in range(n):
        x = int(input())
        labels.append(x)
        max_label = max(max_label, x)

    parent = array('i', [0]) * n
    for v in range(1, n):
        parent[v] = int(input()) - 1

    size = 1
    while size <= max_label:
        size <<= 1

    height = size.bit_length() - 1

    left = array('i', [0])
    right = array('i', [0])
    value = array('Q', [0])
    roots = array('i', [0]) * n

    path = [0] * (height + 1)

    best_global = 0
    count_global = 0

    for v in range(n):
        base_root = 0 if v == 0 else roots[parent[v]]
        x = labels[v]

        node = base_root
        best_len = 0
        best_cnt = 0

        for bit in range(height - 1, -1, -1):
            if node == 0:
                break

            if (x >> bit) & 1:
                child = left[node]
                if child:
                    z = value[child]
                    zlen = z >> 24
                    zcnt = z & COUNT_MASK

                    if zlen > best_len:
                        best_len = zlen
                        best_cnt = zcnt
                    elif zlen == best_len:
                        best_cnt += zcnt

                node = right[node]
            else:
                node = left[node]

        if node:
            z = value[node]
            zlen = z >> 24
            zcnt = z & COUNT_MASK

            if zlen > best_len:
                best_len = zlen
                best_cnt = zcnt
            elif zlen == best_len:
                best_cnt += zcnt

        best_cnt %= MOD

        if best_len == 0:
            dp = 1
            ways = 1
        else:
            dp = best_len + 1
            ways = best_cnt

        old = base_root

        new_root = len(value)
        left.append(left[old])
        right.append(right[old])
        value.append(value[old])
        path[0] = new_root

        cur_old = old
        cur_new = new_root

        for level, bit in enumerate(range(height - 1, -1, -1), 1):
            if (x >> bit) & 1:
                old_child = right[cur_old]

                new_child = len(value)
                left.append(left[old_child])
                right.append(right[old_child])
                value.append(value[old_child])

                right[cur_new] = new_child
            else:
                old_child = left[cur_old]

                new_child = len(value)
                left.append(left[old_child])
                right.append(right[old_child])
                value.append(value[old_child])

                left[cur_new] = new_child

            cur_old = old_child
            cur_new = new_child
            path[level] = cur_new

        old_leaf_value = value[cur_new]
        old_len = old_leaf_value >> 24
        old_cnt = old_leaf_value & COUNT_MASK

        if dp > old_len:
            value[cur_new] = (dp << 24) | ways
        elif dp == old_len:
            value[cur_new] = (dp << 24) | ((old_cnt + ways) % MOD)

        for level in range(height - 1, -1, -1):
            p = path[level]
            lv = value[left[p]]
            rv = value[right[p]]

            llen = lv >> 24
            rlen = rv >> 24

            if llen > rlen:
                value[p] = lv
            elif rlen > llen:
                value[p] = rv
            elif llen == 0:
                value[p] = 0
            else:
                cnt = (lv & COUNT_MASK) + (rv & COUNT_MASK)
                value[p] = (llen << 24) | (cnt % MOD)

        roots[v] = new_root

        if dp > best_global:
            best_global = dp
            count_global = ways
        elif dp == best_global:
            count_global = (count_global + ways) % MOD

    return f"{best_global} {count_global % MOD}\n"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided sample 1
assert run(
    """5
3
3
3
3
3
1
2
3
4
"""
) == "5 1\n", "sample 1"

# Provided sample 2
assert run(
    """5
4
3
2
1
0
1
2
3
4
"""
) == "1 5\n", "sample 2"

# Provided sample 3
assert run(
    """4
1
5
3
6
1
2
3
"""
) == "3 2\n", "sample 3"

# Provided sample 4
assert run(
    """6
1
2
3
4
5
6
1
1
1
1
1
"""
) == "2 5\n", "sample 4"

# Minimum-size input
assert run(
    """1
42
"""
) == "1 1\n", "single vertex"

# All labels equal, chain
assert run(
    """4
7
7
7
7
1
2
3
"""
) == "4 1\n", "equal labels"

# Equal best predecessors, catches counting mistakes
assert run(
    """3
1
3
2
1
1
"""
) == "2 2\n", "two optimal predecessors"

# Boundary case where the root cannot precede a child
assert run(
    """3
5
4
3
1
2
"""
) == "1 3\n", "strictly decreasing chain"

# Maximum-size structural test.
# A million equal labels in a chain have exactly one longest path.
n = 1_000_000
max_input = (
    str(n)
    + "\n"
    + ("1\n" * n)
    + "".join(f"{i}\n" for i in range(1, n))
)
assert run(max_input) == "1000000 1\n", "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`|`1 1`| Kích thước tối thiểu và vỏ cơ bản không có phiên bản trước | 
| Bốn đỉnh, tất cả đều có nhãn`7`, trong một chuỗi |`4 1`| So sánh nhãn toàn diện và các giá trị bằng nhau lặp lại | 
| Gốc`1`, những đứa trẻ`3`Và`2`|`2 2`| Thêm số lượng từ nhiều người tiền nhiệm tối ưu như nhau | 
| Xích`5,4,3`|`1 3`| Người tiền nhiệm có nhãn hiệu lớn hơn phải bị từ chối | 
| Một triệu nhãn bằng nhau trong một chuỗi |`1000000 1`| Số đỉnh tối đa và cây có độ sâu tuyến tính | 

Thử nghiệm kích thước tối đa có chủ đích là thử nghiệm được tạo chứ không phải là khối hàng triệu dòng theo nghĩa đen. Nó thực hiện cùng một cấu trúc đầu vào trong khi vẫn giữ được nguồn kiểm tra có thể đọc được. Trong thực tế, bài kiểm tra này chủ yếu hữu ích để kiểm tra bộ nhớ và hành vi tiệm cận hơn là kiểm tra đơn vị thông thường. 

## Vỏ cạnh 

Đối với một đỉnh duy nhất, chẳng hạn như```
1
7
```phiên bản gốc trống nên truy vấn tiền tố trả về độ dài bằng 0. Do đó, thuật toán tạo ra đường dẫn`[7]`với độ dài một và đếm một. Đầu ra là`1 1`. 

Để có nhãn bằng nhau, hãy xem xét```
3
5
5
5
1
2
```Khi đỉnh 2 được xử lý, nhãn gốc (5) nằm trong phạm vi truy vấn bao gồm, do đó đỉnh 2 nhận được`dp = 2`. Khi đỉnh 3 được xử lý, phiên bản liên tục của đỉnh 2 chứa cả hai tổ tiên có nhãn (5) và tổng hợp của nó tại nhãn đó có độ dài hai và đếm một. Đỉnh 3 nhận`dp = 3`. Đầu ra là`3 1`. 

Đối với nhiều người tiền nhiệm tối ưu, hãy xem xét```
3
1
3
2
1
1
```Gốc tạo ra một đường dẫn có độ dài bằng một. Cả hai đứa trẻ đều có thể mở rộng nó vì cả hai nhãn ít nhất là một. Mỗi đứa trẻ nhận được chiều dài hai và đếm một. Câu trả lời chung kết hợp hai số lượng điểm cuối đó và tạo ra`2 2`. 

Đối với nhãn giảm dần,```
3
5
4
3
1
2
```truy vấn cho đỉnh 2 bị giới hạn ở các nhãn nhiều nhất là bốn, vì vậy gốc có nhãn năm sẽ bị loại trừ. Vertex 2 bắt đầu con đường riêng của mình. Điều tương tự cũng xảy ra với đỉnh 3. Mỗi đỉnh có`dp = 1`, cho`1 3`. 

Trường hợp nguy hiểm nhất khi triển khai là chèn trước khi truy vấn. Giả sử đỉnh hiện tại có nhãn năm và đỉnh cha của nó cũng có nhãn năm. Nếu đỉnh hiện tại được chèn trước, truy vấn có thể thấy trạng thái mới được chèn và tạo ra một đường dẫn dài hơn thực tế có thể. Việc triển khai tránh điều này bằng cách truy vấn`root[parent[v]]`đầu tiên và xây dựng`root[v]`chỉ sau`dp[v]`Và`ways[v]`đã được xác định. 

Sự tinh tế cuối cùng là đếm tại nút cây phân đoạn. Hai phần tử con có thể có cùng độ dài tốt nhất nhưng đại diện cho các nhóm đường đi khác nhau, vì vậy số lượng của chúng phải được cộng lại. Nếu một đứa trẻ có chiều dài lớn hơn hoàn toàn thì chỉ số của đứa trẻ đó còn tồn tại. Quy tắc hợp nhất tương tự được sử dụng ở mọi nút bên trong và ở lá nhãn khi một số nút tổ tiên chia sẻ nhãn đó.
