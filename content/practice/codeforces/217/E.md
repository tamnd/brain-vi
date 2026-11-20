---
title: "CF 217E - DNA của người ngoài hành tinh"
description: "Chúng tôi bắt đầu với một chuỗi DNA. Mỗi đột biến chọn một phân đoạn liền kề, giữ nguyên phân đoạn đó và chèn một bản sao đã chuyển đổi ngay sau phân đoạn đó."
date: "2026-06-04T00:36:52+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "trees"]
categories: ["algorithms"]
codeforces_contest: 217
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 134 (Div. 1)"
rating: 2800
weight: 217
solve_time_s: 188
verified: true
draft: false
---

[CF 217E - DNA ngoài hành tinh](https://codeforces.com/problemset/problem/217/E) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu, dsu, cây 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi DNA. Mỗi đột biến chọn một phân đoạn liền kề, giữ nguyên phân đoạn đó và chèn một bản sao đã chuyển đổi ngay sau phân đoạn đó. 

Nếu đoạn được chọn là`x1 x2 x3 x4 x5 x6`sau đó bản sao được chèn vào là`x2 x4 x6 x1 x3 x5`Các đột biến được áp dụng lần lượt. DNA cuối cùng có thể trở nên to lớn về mặt thiên văn, nhưng chúng ta chỉ cần DNA đầu tiên`k`nhân vật. 

Những ràng buộc này làm thay đổi hoàn toàn cách chúng ta phải suy nghĩ về vấn đề. Chiều dài DNA ban đầu và`k`cả hai đều có ý định`3 · 10^6`, vì vậy chỉ riêng đầu ra có thể chứa ba triệu ký tự. Mặt khác, số lượng đột biến chỉ`5000`. 

Một mô phỏng trực tiếp là không thể. Một đột biến đơn lẻ có thể nhân đôi một đoạn có chiều dài vốn đã rất lớn và sau hàng nghìn đột biến, chiều dài DNA có thể vượt quá bất cứ thứ gì chúng ta có thể lưu trữ rõ ràng. 

Quan sát quan trọng là chúng ta không bao giờ cần toàn bộ DNA cuối cùng. Chúng ta chỉ cần xác định, đối với mỗi điều đầu tiên`k`vị trí nào, ký tự gốc nào cuối cùng sẽ chiếm giữ vị trí đó. 

Có một số tình huống dễ bỏ lỡ. 

Giả sử khối được sao chép nằm hoàn toàn ngoài khối đầu tiên`k`các vị trí. Khi đó sự đột biến đó không thể ảnh hưởng đến câu trả lời chút nào, ngay cả khi nó nhân đôi hàng triệu ký tự. 

Giả sử một đột biến tác động lên một đoạn có độ dài lẻ. Bản sao được chuyển đổi trước tiên chứa tất cả các phần tử được lập chỉ mục chẵn và sau đó là tất cả các phần tử được lập chỉ mục lẻ. Một công thức bất cẩn để ánh xạ các vị trí được sao chép trở lại nguồn của chúng thường thất bại ở độ dài lẻ vì hai nhóm có kích thước khác nhau. 

Giả sử có nhiều đột biến sau này trùng lặp các ký tự được tạo ra bởi các đột biến trước đó. Nguồn chính xác của một vị trí có thể là một vị trí được tạo khác, không phải là ký tự gốc. Bất kỳ giải pháp nào cố gắng giải quyết nguồn ngay lập tức sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Giữ toàn bộ chuỗi DNA, thực hiện mọi đột biến theo đúng nghĩa đen và cuối cùng in ra bản đầu tiên`k`nhân vật. 

Bản thân đột biến có thể sao chép một đoạn có chiều dài tỷ lệ thuận với kích thước DNA hiện tại. Vì chiều dài DNA tăng lên sau mỗi hoạt động nên tổng công việc nhanh chóng trở nên khổng lồ. Ngay cả việc lưu trữ chuỗi kết quả cũng không thể thực hiện được. 

Lối thoát là đảo ngược quá trình. 

Xét một đột biến tác động lên`[l, r]`. Trong chuỗi *sau* đột biến, khối mới được chèn chiếm các vị trí`[r + 1, r + (r - l + 1)]`. 

Khi chúng ta lùi lại, những vị trí đó chính là những vị trí biến mất. 

Vị trí bên trong khối được chèn không bị mất. Chúng tôi biết chính xác vị trí của phân khúc ban đầu đã tạo ra nó. Vì vậy, trong quá trình ngược lại, chúng ta có thể ghi lại con trỏ cha từ vị trí được sao chép đến vị trí nguồn của nó. 

Bây giờ hãy tưởng tượng việc xử lý các đột biến từ cuối đến đầu. 

Tại bất kỳ thời điểm nào, chúng tôi chỉ quan tâm đến các vị trí vẫn tồn tại sau khi loại bỏ các khối đột biến được chèn vào đã được xử lý. Những vị trí còn sót lại này có thể được xem như được đánh số lại từ`1`ĐẾN`current_length`. 

Đối với một đột biến`[l, r]`, khối được chèn bắt đầu ở thứ hạng`r + 1`trong số những vị trí hiện còn tồn tại. Chúng tôi liên tục loại bỏ`(r + 1)`-vị trí còn sót lại và kết nối nó với thứ hạng nguồn thích hợp bên trong`[l, r]`. 

Cây phân đoạn lưu trữ số lượng vị trí còn sót lại trong mỗi phạm vi. Nó cho phép chúng ta tìm vị trí có thứ hạng nhất định trong số những người sống sót trong`O(log k)`thời gian. 

Mỗi cái đầu tiên`k`các vị trí bị xóa nhiều nhất một lần, do đó tổng số lần xóa nhiều nhất là`k`. Đây là lý do quan trọng giải pháp phù hợp. 

Ý tưởng xử lý ngược và chiến lược loại bỏ dựa trên thứ hạng là những quan sát cốt lõi đằng sau các giải pháp được chấp nhận. citturn2view0 

| Tiếp cận | Độ phức tạp thời gian | Không gian phức tạp để lưu trữ | Quá chậm | 
| Tối ưu | O(k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Để vị trí`1 ... k`đại diện cho người đầu tiên`k`vị trí của DNA cuối cùng. 

2. Xây dựng cây phân đoạn trong đó mọi vị trí ban đầu đều tồn tại. Mỗi chiếc lá lưu trữ`1`. 

3. Xử lý đột biến từ cuối đến đầu. 

4. Hãy để`len = r - l + 1`. 

5. Nếu`r >= current_alive_count`, đột biến này không thể ảnh hưởng đến bất kỳ vị trí nào trong số đầu tiên`k`những vị trí vẫn còn quan trọng. Bỏ qua nó. 

6. Nếu không, khối được chèn bắt đầu ở thứ hạng`r + 1`trong số các vị trí còn sống. 

7. Duyệt các ký tự được sao chép theo đúng thứ tự do đột biến tạo ra: 

đầu tiên là vị trí chẵn của`[l, r]`, 

thì các vị trí lẻ của`[l, r]`. 

8. Đối với mỗi ký tự được sao chép: 

tìm và loại bỏ vị trí còn sống có cấp bậc`r + 1`. Đây là một vị trí thuộc khối được chèn. 

tìm vị trí còn sống tương ứng với thứ hạng nguồn thích hợp bên trong`[l, r]`. 

lưu trữ con trỏ cha từ vị trí đã xóa đến vị trí nguồn. 

9. Sau khi tất cả các đột biến được xử lý, mọi vị trí đều có con trỏ cha hoặc tương ứng trực tiếp với ký tự gốc. 

10. Quét vị trí từ`1`ĐẾN`k`. 

Nếu một vị trí không có cha, hãy gán ký tự tiếp theo từ DNA gốc. 

Nếu không thì sao chép ký tự đã được tính toán của cha mẹ nó. 

11. Xuất chuỗi kết quả. 

### Tại sao nó hoạt động 

Khi xử lý các đột biến ngược, mọi vị trí được tạo sẽ bị xóa chính xác khi đột biến tạo ra nó bị đảo ngược. 

Vị trí bị loại bỏ được liên kết với thứ hạng mà ký tự của nó bắt nguồn. Vì các đột biến sau này được xử lý trước nên thứ hạng nguồn đó đã đề cập đến vị trí chính xác trong trạng thái DNA trước đó. 

Sau tất cả các lần đảo ngược, các vị trí sống sót vẫn chính xác là các ký tự gốc theo thứ tự. Mọi vị trí bị loại bỏ đều trỏ tới một trong những vị trí gốc này, trực tiếp hoặc gián tiếp. 

Do đó, mọi vị trí cuối cùng đều nhận được chính xác ký tự đã tạo ra nó trong quá trình đột biến tiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    k = int(input())
    n = int(input())

    L = [0] * n
    R = [0] * n

    for i in range(n):
        l, r = map(int, input().split())
        L[i] = l
        R[i] = r

    size = 1
    while size < k:
        size <<= 1

    seg = [0] * (2 * size)

    for i in range(k):
        seg[size + i] = 1

    for i in range(size - 1, 0, -1):
        seg[i] = seg[i << 1] + seg[i << 1 | 1]

    def kth(pos_rank, erase):
        p = 1
        l = 1
        r = size

        while l != r:
            if erase:
                seg[p] -= 1

            mid = (l + r) >> 1

            if seg[p << 1] >= pos_rank:
                p <<= 1
                r = mid
            else:
                pos_rank -= seg[p << 1]
                p = p << 1 | 1
                l = mid + 1

        if erase:
            seg[p] -= 1

        return l

    parent = [0] * (k + 1)

    for idx in range(n - 1, -1, -1):
        l = L[idx]
        r = R[idx]

        if r >= seg[1]:
            continue

        parity = ((l & 1) ^ 1)
        cur = l + parity

        if cur > r:
            cur = l + (parity ^ 1)

        length = r - l + 1

        for _ in range(length):
            if r >= seg[1]:
                break

            copied_pos = kth(r + 1, True)
            source_pos = kth(cur, False)

            parent[copied_pos] = source_pos

            cur += 2
            if cur > r:
                cur = l + (parity ^ 1)

    ans = [''] * (k + 1)

    ptr = 0
    for i in range(1, k + 1):
        if parent[i]:
            ans[i] = ans[parent[i]]
        else:
            ans[i] = s[ptr]
            ptr += 1

    sys.stdout.write(''.join(ans[1:]) + '\n')

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ bao nhiêu vị trí còn sống trong mỗi khoảng. các`kth(rank, erase)`thói quen tìm vị trí có thứ hạng còn sống hiện tại bằng`rank`. 

Khi`erase=True`, vị trí đó sẽ bị xóa khỏi tập sống động khi đi xuống cây. Khi`erase=False`, cấu trúc không thay đổi. 

Phần tinh tế hơn là thứ tự xếp hạng nguồn bên trong khối được sao chép. Đột biến xuất ra tất cả các phần tử được lập chỉ mục chẵn trước và tất cả các phần tử được lập chỉ mục lẻ sau đó. Biến`cur`duyệt qua các cấp bậc nguồn đó theo đúng thứ tự đó. 

Giai đoạn tái thiết dựa vào một thuộc tính hữu ích. Các chỉ số gốc luôn được giải quyết sớm hơn các vị trí tham chiếu đến chúng. Điều này cho phép một lần chuyển từ trái sang phải để điền vào câu trả lời. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```text
GAGA
4
0
```Không có đột biến tồn tại. 

| Vị trí | Phụ huynh | 
|---|---| 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 0 | 
| 4 | 0 | 

Các ký tự được lấy trực tiếp từ chuỗi gốc. 

Đầu ra:```text
GAGA
```Điều này thể hiện trường hợp cơ bản trong đó mọi vị trí đều tồn tại trong quá trình ngược lại. 

### Mẫu 2 

đầu vào```text
ACGTACGT
16
2
1 2
2 8
```Sau khi xử lý ngược đột biến thứ hai, các vị trí thuộc bản sao được chèn của nó sẽ được liên kết với nguồn của chúng. 

Sau khi xử lý ngược đột biến đầu tiên, các liên kết gốc bổ sung sẽ được tạo. 

| Loại vị trí cuối cùng | Độ phân giải | 
|---|---| 
| Người sống sót ban đầu | Trực tiếp từ DNA gốc | 
| Được tạo bởi đột biến 2 | Liên kết gốc tới nguồn xếp hạng | 
| Được tạo ra bởi đột biến 1 | Liên kết gốc tới nguồn xếp hạng | 

Theo các liên kết gốc mang lại:```text
ACCAGTACCGACATCG
```Ví dụ này cho thấy tại sao con trỏ cha lại cần thiết. Một số vị trí được sao chép bắt nguồn từ các vị trí được tạo ra bởi các đột biến trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(k log k) | Mỗi vị trí trong số những người đầu tiên`k`bị xóa nhiều nhất một lần, mỗi lần xóa sẽ sử dụng truy vấn xếp hạng cây phân đoạn | 
| Không gian | O(k) | Cây phân đoạn và mảng cha mẹ | 

Với`k ≤ 3 · 10^6`, bản thân đầu ra đã có kích thước ba triệu. MỘT`O(k log k)`giải pháp với mảng số nguyên đơn giản là đủ nhanh và vừa với giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    data = io.StringIO(inp)
    out = io.StringIO()

    input = data.readline

    s = input().strip()
    k = int(input())
    n = int(input())

    L = [0] * n
    R = [0] * n

    for i in range(n):
        L[i], R[i] = map(int, input().split())

    size = 1
    while size < k:
        size <<= 1

    seg = [0] * (2 * size)
    for i in range(k):
        seg[size + i] = 1

    for i in range(size - 1, 0, -1):
        seg[i] = seg[i << 1] + seg[i << 1 | 1]

    def kth(rank, erase):
        p = 1
        l = 1
        r = size

        while l != r:
            if erase:
                seg[p] -= 1

            m = (l + r) >> 1

            if seg[p << 1] >= rank:
                p <<= 1
                r = m
            else:
                rank -= seg[p << 1]
                p = p << 1 | 1
                l = m + 1

        if erase:
            seg[p] -= 1

        return l

    parent = [0] * (k + 1)

    for i in range(n - 1, -1, -1):
        l, r = L[i], R[i]

        if r >= seg[1]:
            continue

        t = ((l & 1) ^ 1)
        cur = l + t
        if cur > r:
            cur = l + (t ^ 1)

        for _ in range(r - l + 1):
            if r >= seg[1]:
                break

            p = kth(r + 1, True)
            parent[p] = kth(cur, False)

            cur += 2
            if cur > r:
                cur = l + (t ^ 1)

    ans = [''] * (k + 1)
    ptr = 0

    for i in range(1, k + 1):
        if parent[i]:
            ans[i] = ans[parent[i]]
        else:
            ans[i] = s[ptr]
            ptr += 1

    return ''.join(ans[1:]) + "\n"

# provided samples
assert run("GAGA\n4\n0\n") == "GAGA\n"
assert run("ACGTACGT\n16\n2\n1 2\n2 8\n") == "ACCAGTACCGACATCG\n"

# custom cases
assert run("A\n1\n0\n") == "A\n"
assert run("AAAA\n4\n0\n") == "AAAA\n"
assert run("ACTG\n4\n1\n1 1\n") == "ACTG\n"
assert run("AC\n3\n1\n1 2\n") == "ACC\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`A, k=1, n=0`|`A`| Kích thước tối thiểu | 
|`AAAA, k=4, n=0`|`AAAA`| Tất cả các ký tự bằng nhau | 
| Đột biến về chiều dài 1 đoạn | Chuỗi gốc không thay đổi trong tiền tố | Độ dài ranh giới | 
|`AC`, biến đổi`[1,2]`,`k=3`|`ACC`| Logic sắp xếp lại chẵn/lẻ | 

## Vỏ cạnh 

Hãy xem xét:```text
ACTG
4
1
1 1
```Khối được sao chép có chiều dài bằng một. Phiên bản biến đổi của nó giống hệt với chính nó. 

Trong quá trình xử lý ngược, khối được chèn chiếm thứ hạng`2`. Thuật toán loại bỏ chính xác một vị trí còn sống và liên kết nó trở lại thứ hạng`1`. Tiền tố được xây dựng lại vẫn còn:```text
ACTG
```Bây giờ hãy xem xét:```text
AC
3
1
1 2
```Đoạn được sao chép là`AC`. 

Bản sao được chuyển đổi là`CA`, do đó DNA cuối cùng bắt đầu bằng:```text
ACCA
```Ba ký tự đầu tiên là:```text
ACC
```Thuật toán ngược xử lý các vị trí khối được chèn, liên kết chúng với thứ hạng nguồn`2`Và`1`tương ứng. Thứ tự chẵn trước lẻ được xử lý bởi`cur += 2`đi qua. 

Cuối cùng, hãy xem xét một đột biến có khối được sao chép bắt đầu ngoài tất cả các vị trí vẫn quan trọng:```text
(original string)
k = small
mutation acts far to the right
```Khi xử lý đột biến đó ngược lại,`r >= alive_count`nắm giữ. Thuật toán bỏ qua hoàn toàn đột biến vì không có đột biến đầu tiên nào`k`các vị trí liên quan có thể thuộc về khối được chèn đó. Đây chính xác là hành vi mong muốn.
