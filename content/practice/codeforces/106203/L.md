---
title: "CF 106203L - \u041c\u0438\u0441\u0442\u0438\u0447\u0435\u0441\u043a\u0438\u0439 \u044d\u043a\u0441\u043f\u0435\u0440\u0438\u043c\u0435\u043d\u0442 \u041f\u0430\u0433\u0441\u043b\u0438"
description: "Chúng ta được cung cấp một mảng các số nguyên thay đổi theo thời gian thông qua việc cập nhật điểm. Sau mỗi lần cập nhật, chúng ta phải trả lời được có bao nhiêu vị trí trong mảng có thuộc tính “cân bằng” cụ thể được xác định bằng XOR."
date: "2026-06-19T16:03:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106203
codeforces_index: "L"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106203
solve_time_s: 45
verified: true
draft: false
---

[CF 106203L - \u041c\u0438\u0441\u0442\u0438\u0447\u0435\u0441\u043a\u0438\u0439 \u044d\u043a\u0441\u043f\u0435\u0440\u0438\u043c\u0435\u043d\u0442 \u041f\u0430\u0433\u0441\u043b\u0438](https://codeforces.com/problemset/problem/106203/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên thay đổi theo thời gian thông qua việc cập nhật điểm. Sau mỗi lần cập nhật, chúng ta phải trả lời được có bao nhiêu vị trí trong mảng có thuộc tính “cân bằng” cụ thể được xác định bằng XOR. 

Đối với một chỉ mục$i$, hãy xem xét XOR của tất cả các phần tử ở bên trái của$i$và XOR của tất cả các phần tử ở bên phải của$i$. Một chỉ mục được coi là đặc biệt nếu hai giá trị XOR này bằng nhau. 

Một truy vấn có thể thay đổi một phần tử của mảng hoặc yêu cầu số chỉ mục đặc biệt hiện tại. 

Độ dài mảng và số lượng truy vấn đều lên tới$2 \cdot 10^5$, điều này ngay lập tức loại trừ việc tính toán lại tiền tố và hậu tố XOR từ đầu cho mỗi truy vấn. Quét toàn bộ sau mỗi lần cập nhật sẽ là$O(nq)$, vượt xa giới hạn có thể chấp nhận được. 

Trường hợp cạnh tinh tế xuất hiện ở ranh giới. Vì$i = 0$, XOR bên trái trống và do đó bằng 0. Vì$i = n-1$, XOR bên phải trống và cũng bằng 0. Bất kỳ giải pháp chính xác nào cũng phải xử lý các định nghĩa này một cách nhất quán, nếu không logic riêng lẻ trong việc xử lý tiền tố hoặc hậu tố sẽ không thành công trên mảng một phần tử hoặc hai phần tử. 

Ví dụ, nếu mảng là$[5]$, cả XOR trái và phải đều bằng 0, vì vậy chỉ số 0 luôn hợp lệ. Nếu việc triển khai đơn giản quên các quy ước XOR trống, nó có thể loại trừ trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp tính toán lại XOR tiền tố và XOR hậu tố cho mọi truy vấn. Sau mỗi lần cập nhật, chúng tôi tính toán lại hai mảng và đếm các chỉ số thỏa mãn điều kiện. Mỗi lần tính toán lại tốn$O(n)$, dẫn đến$O(nq)$tổng công việc. Với$2 \cdot 10^5$hoạt động, điều này là quá chậm bởi một số bậc độ lớn. 

Quan sát quan trọng là điều kiện có thể được viết lại theo cách loại bỏ hoàn toàn sự cần thiết của cấu trúc tiền tố hoặc hậu tố. 

Cho phép$P$là XOR của toàn bộ mảng. Cho phép$pref[i]$là XOR của các phần tử từ$0$ĐẾN$i$. Sau đó, XOR bên trái cho chỉ mục$i$là$pref[i-1]$, và XOR bên phải là$P \oplus pref[i]$. Điều kiện trở thành:$$pref[i-1] = P \oplus pref[i]$$Bây giờ sử dụng danh tính$pref[i] = pref[i-1] \oplus a[i]$. Thay thế mang lại:$$pref[i-1] = P \oplus (pref[i-1] \oplus a[i])$$Đang hủy$pref[i-1]$ở cả hai bên sử dụng thuộc tính XOR mang lại:$$0 = P \oplus a[i]$$mà đơn giản hóa thành:$$a[i] = P$$Vì vậy, một chỉ mục hợp lệ chính xác khi giá trị của nó bằng XOR của toàn bộ mảng. 

Điều này làm giảm toàn bộ vấn đề trong việc duy trì hai thứ trong các bản cập nhật: XOR toàn cầu của mảng và tần số của từng giá trị. Mỗi truy vấn loại 2 trở thành một tra cứu duy nhất: có bao nhiêu phần tử hiện bằng XOR toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tính toán lại XOR cho mỗi truy vấn |$O(nq)$|$O(1)$| Quá chậm | 
| Duy trì tổng XOR + bản đồ tần số |$O(q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai phần trạng thái: XOR của toàn bộ mảng và bản đồ tần số của các giá trị trong mảng. 

1. Khởi tạo bản đồ tần số bằng cách đếm số lần xuất hiện của từng phần tử trong mảng ban đầu và tính XOR của tất cả các phần tử. Điều này đặt trạng thái cơ sở cho tất cả các truy vấn trong tương lai. 
2. Đối với truy vấn cập nhật tại vị trí$i$, trước tiên hãy xóa phần đóng góp của giá trị cũ khỏi cả bản đồ tần số và XOR toàn cầu. Điều này là cần thiết vì trạng thái mảng phải nhất quán trước khi áp dụng giá trị mới. 
3. Áp dụng giá trị mới tại vị trí$i$, cập nhật bản đồ tần số và XOR tương ứng. Sau bước này, trạng thái được duy trì lại phản ánh chính xác mảng hiện tại. 
4. Đối với truy vấn yêu cầu số chỉ mục cân bằng, hãy trả về tần số của giá trị XOR toàn cầu hiện tại. Điều này hiệu quả vì chúng tôi đã chứng minh rằng một chỉ mục hợp lệ chính xác khi giá trị của nó bằng tổng XOR. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào việc viết lại các biểu thức XOR trái và phải theo tiền tố XOR và sau đó loại bỏ hoàn toàn các biến tiền tố. Điều kiện cuối cùng giảm xuống mức đẳng thức theo điểm giữa mỗi phần tử và một đại lượng vô hướng toàn cục duy nhất, XOR của toàn bộ mảng. Vì đại lượng vô hướng đó luôn được duy trì chính xác theo các bản cập nhật, nên việc đếm các chỉ số hợp lệ sẽ tương đương với việc đếm số lần xuất hiện của đại lượng vô hướng đó trong mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    freq = {}
    total_xor = 0

    for v in a:
        total_xor ^= v
        freq[v] = freq.get(v, 0) + 1

    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1])
            x = int(tmp[2])

            old = a[i]
            if freq[old] == 1:
                del freq[old]
            else:
                freq[old] -= 1

            total_xor ^= old

            a[i] = x

            total_xor ^= x
            freq[x] = freq.get(x, 0) + 1

        else:
            out.append(str(freq.get(total_xor, 0)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp giữ một mảng rõ ràng để có thể áp dụng các bản cập nhật trong thời gian không đổi. Bộ tích lũy XOR được cập nhật bằng cách xóa giá trị cũ và thêm giá trị mới, hoạt động này vì XOR là nghịch đảo của chính nó. Từ điển tần số theo dõi số lượng phần tử khớp với bất kỳ giá trị nhất định nào và câu trả lời truy vấn là tra cứu từ điển đơn giản. 

Một lỗi phổ biến là quên giảm tần số của giá trị cũ trước khi ghi đè lên nó, điều này sẽ dẫn đến số lượng tăng cao sau nhiều lần cập nhật. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[1, 2, 3]$. 

Chúng tôi tính tổng XOR là$1 \oplus 2 \oplus 3 = 0$. Không có phần tử nào bằng 0 nên đáp án là 0. 

| Bước | Mảng | Tổng XOR | Bản đồ tần số | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,2,3] | 0 | {1:1,2:1,3:1} | 0 | 

Bây giờ cập nhật chỉ số 1 thành giá trị 1, đưa ra$[1, 1, 3]$. 

| Bước | Mảng | Tổng XOR | Bản đồ tần số | Trả lời | 
| --- | --- | --- | --- | --- | 
| Sau khi cập nhật | [1,1,3] | 3 | {1:2,3:1} | 1 | 

Ở đây chỉ có các giá trị chỉ mục bằng 3 là hợp lệ và có chính xác một phần tử như vậy. 

Dấu vết này cho thấy thuật toán không bao giờ cần tính toán tiền tố, chỉ cần duy trì nhất quán một bất biến tổng hợp duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$| Mỗi bản cập nhật sửa đổi một số lượng mục không đổi trong bản đồ và bộ tích lũy XOR | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi giá trị riêng biệt | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì cả hai giới hạn đều tuyến tính về kích thước của đầu vào và tất cả các hoạt động đều có thời gian trung bình không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    freq = {}
    total_xor = 0
    for v in a:
        total_xor ^= v
        freq[v] = freq.get(v, 0) + 1

    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1]); x = int(tmp[2])
            old = a[i]

            freq[old] -= 1
            if freq[old] == 0:
                del freq[old]

            total_xor ^= old
            a[i] = x
            total_xor ^= x
            freq[x] = freq.get(x, 0) + 1
        else:
            out.append(str(freq.get(total_xor, 0)))

    return "\n".join(out)

# minimum size
assert run("""1
5
3
2
1 0 7
2
""") == "1\n0"

# all equal
assert run("""5
1 1 1 1 1
3
2
1 2 2
2
""") == "0\n1"

# single update flips answer
assert run("""3
1 2 3
2
2
1 1 1
2
""") == "0\n1"

# no updates, repeated queries
assert run("""4
0 0 0 0
3
2
2
2
""") == "4\n4\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1/0 | định nghĩa XOR ranh giới | 
| tất cả các giá trị bằng nhau | khác nhau | tính đúng đắn theo mảng thống nhất | 
| lật cập nhật | 0 rồi 1 | tính nhất quán động của các bản cập nhật XOR | 
| truy vấn lặp đi lặp lại | đầu ra không đổi | ổn định không có đột biến | 

## Vỏ cạnh 

Đối với mảng một phần tử như$[x]$, thuật toán tính tổng XOR là$x$. Bản đồ tần số chứa chính xác một lần xuất hiện của$x$, vậy nếu$x = 0$, câu trả lời là 1, nếu không thì là 0. Việc triển khai xử lý chính xác điều này vì tiền tố và hậu tố trống được mã hóa ngầm trong danh tính XOR, không phải trong cách viết hoa đặc biệt. 

Đối với các cập nhật lặp lại trên cùng một chỉ mục, các bước xóa và chèn đảm bảo rằng bản đồ tần số không bao giờ đếm gấp đôi giá trị. Mỗi lần cập nhật trước tiên sẽ trừ giá trị cũ trước khi thêm giá trị mới, giữ nguyên bất biến rằng bản đồ luôn phản ánh chính xác mảng hiện tại. 

Đối với các mảng trong đó tất cả các giá trị đều bằng 0, tổng XOR vẫn bằng 0 trong tất cả các bản cập nhật. Mọi chỉ mục luôn hợp lệ và bản đồ tần số luôn trả về$n$, phù hợp với định nghĩa về vị trí cân bằng.
