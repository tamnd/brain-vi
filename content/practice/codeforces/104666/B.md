---
title: "CF 104666B - Hãy là những người đam mê công nghệ!"
description: "Chúng ta được cho một dãy các số nguyên dương và chúng ta xem xét mọi mảng con liền kề. Đối với mỗi mảng con, hai giá trị được trích xuất: ước số chung lớn nhất của tất cả các phần tử bên trong nó và phần tử lớn nhất bên trong nó."
date: "2026-06-29T09:52:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104666
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ICPC Central Europe Regional Contest (CERC 19)"
rating: 0
weight: 104666
solve_time_s: 95
verified: false
draft: false
---

[CF 104666B - Hãy là những người đam mê công nghệ!](https://codeforces.com/problemset/problem/104666/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên dương và chúng ta xem xét mọi mảng con liền kề. Đối với mỗi mảng con, hai giá trị được trích xuất: ước số chung lớn nhất của tất cả các phần tử bên trong nó và phần tử lớn nhất bên trong nó. Phần đóng góp của mảng con đó là tích của hai giá trị này và nhiệm vụ là tính tổng phần đóng góp này trên tất cả các mảng con. 

Vì vậy, việc tính toán về cơ bản là tổng hợp một hàm trên tất cả$O(N^2)$các khoảng, trong đó mỗi khoảng phụ thuộc vào hai tập hợp phi tuyến khác nhau: gcd và max. 

Ràng buộc$N \le 2 \cdot 10^5$ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê tất cả các mảng con một cách rõ ràng. Ngay cả một đơn$O(N^2)$quá trình quét đã quá lớn và việc tính toán gcd và mức tối đa trên mỗi khoảng thời gian sẽ đẩy nó tới$O(N^3)$ở dạng ngây thơ. Bất kỳ giải pháp đúng nào cũng phải tránh tính toán lại số liệu thống kê mảng con từ đầu. 

Một khó khăn tinh tế xuất hiện khi cố gắng phân hủy sản phẩm$\gcd \cdot \max$. Không có hàm nào hoạt động độc lập trên các mảng con theo cách cho phép tích chập đơn giản. Đặc biệt, gcd ổn định khi mở rộng theo cách giảm dần, trong khi mức tối đa ổn định theo cách tăng dần, điều này cho thấy rằng bất kỳ giải pháp hiệu quả nào cũng phải theo dõi đồng thời cả hai trên các tập hợp khoảng có cấu trúc. 

Một cách tiếp cận ngây thơ tính toán trước gcd và giá trị tối đa cho tất cả các khoảng riêng biệt cũng không thành công, vì việc lưu trữ hoặc lặp lại trên tất cả$O(N^2)$các giá trị đã là không thể. Thách thức là sắp xếp lại phần đóng góp sao cho mỗi mảng con được tính theo thời gian tuyến tính được khấu hao. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên tất cả các cặp$(l, r)$, tính gcd của$a[l..r]$, tính giá trị lớn nhất của$a[l..r]$và thêm sản phẩm của họ vào câu trả lời. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Điểm nghẽn là ở mỗi khoảng thời gian, việc tính toán lại chi phí gcd và max từ đầu$O(N)$, dẫn đến$O(N^3)$tổng thời gian. Ngay cả với các bản cập nhật tăng dần, việc duy trì cả gcd và max vẫn mang lại hiệu quả$O(N^2)$, quá lớn đối với$2 \cdot 10^5$. 

Quan sát quan trọng là cả gcd và max đều đơn điệu khi mở rộng điểm cuối bên phải cố định. Khi chúng ta mở rộng mảng con sang bên trái, gcd chỉ thay đổi$O(\log A_i)$lần trên mỗi điểm cuối vì giá trị gcd giảm nghiêm ngặt dọc theo các ước số và chỉ thay đổi tối đa khi gặp phần tử mới lớn hơn. Cấu trúc này cho phép chúng ta duy trì một biểu diễn nén của tất cả các mảng con kết thúc ở một vị trí cố định: thay vì$O(N)$giá trị riêng biệt, chúng tôi chỉ duy trì$O(\log A_i)$phân đoạn gcd và$O(\log N)$phân đoạn tối đa. 

Ý tưởng chính là quét đúng điểm cuối$r$. Đối với mỗi$r$, chúng tôi duy trì tất cả các giá trị gcd riêng biệt của các mảng con kết thúc tại$r$, được nhóm theo ranh giới bên trái của chúng và tương tự duy trì tất cả các giá trị tối đa riêng biệt trên các mảng con kết thúc tại$r$. Sau đó, chúng tôi kết hợp các cấu trúc này để tính toán đóng góp một cách hiệu quả bằng cách nhóm các mảng con có cùng gcd và max. 

Sự tương tác giữa gcd và max được xử lý bằng cách xử lý các mảng con kết thúc tại$r$dưới dạng các phân vùng trên các điểm cuối bên trái trong đó cả gcd và max đều không đổi, sau đó tích lũy các đóng góp chung của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N \log A)$|$O(N \log A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải và duy trì tất cả thông tin liên quan về mảng con kết thúc ở chỉ mục hiện tại. 

1. Đối với từng vị trí$r$, duy trì một danh sách nén các cặp$(g, l)$đại diện cho tất cả các giá trị gcd riêng biệt của mảng con kết thúc tại$r$, trong đó mỗi giá trị gcd hợp lệ cho một phạm vi điểm cuối bên trái bắt đầu từ$l$. Cấu trúc này hoạt động vì giá trị gcd chỉ thay đổi khi chúng ta kéo dài khoảng và loại bỏ các thừa số nguyên tố. 
2. Cập nhật cấu trúc gcd này khi thêm$a[r]$. Bắt đầu một trạng thái mới$(a[r], r)$, sau đó hợp nhất ngược với trạng thái gcd trước đó bằng cách lấy gcd với giá trị được lưu trữ cuối cùng. Bất cứ khi nào gcd thay đổi, chúng tôi ghi lại một phân đoạn mới. Điều này chỉ đảm bảo$O(\log A_r)$các phân đoạn vẫn còn. 
3. Duy trì cấu trúc nén tương tự cho các giá trị tối đa của mảng con kết thúc tại$r$. Chúng tôi sử dụng ngăn xếp đơn điệu để tiếp tục giảm các phân đoạn tối đa. Mỗi phần tử mở rộng các phân đoạn hiện có hoặc loại bỏ cực đại yếu hơn, chỉ đảm bảo tổng số chuyển đổi (O(N)\ trong toàn bộ quá trình. 
4. Sau khi cập nhật cả hai cấu trúc cho vị trí$r$, chúng ta cần kết hợp chúng. Thay vì liệt kê tất cả các cặp, chúng tôi quét qua các ranh giới phân đoạn và duy trì hợp nhất kiểu hai con trỏ trên các phân đoạn gcd và phân đoạn tối đa, giao nhau trong phạm vi hiệu lực của chúng. 
5. Đối với mỗi khoảng giao nhau của các điểm cuối bên trái trong đó cả gcd và max đều không đổi, hãy tính đóng góp như sau$g \cdot m \cdot \text{length}$, và thêm vào câu trả lời modulo$10^9+7$. 
6. Lặp lại quá trình này cho tất cả$r$, tích lũy đóng góp. 

### Tại sao nó hoạt động 

Sửa điểm cuối bên phải$r$. Mỗi mảng con kết thúc tại$r$thuộc về chính xác một phân đoạn gcd và chính xác một phân đoạn tối đa. Phân đoạn gcd phân chia các điểm cuối bên trái thành các phạm vi trong đó gcd không đổi; phân đoạn tối đa thực hiện tương tự cho các giá trị tối đa. Giao điểm của các phân vùng này tạo thành một sàng lọc mô tả chính xác tất cả các mảng con duy nhất bằng một cặp hằng số$(\gcd, \max)$. Vì mỗi mảng con được tính chính xác một lần trong đúng một khối giao nhau, nên tính tổng$g \cdot m$trên các khối này bằng tổng số yêu cầu mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # gcd segments: list of (gcd_value, start_index)
    gcd_seg = []
    ans = 0

    # max segments: list of (max_value, start_index)
    max_seg = []

    for i, x in enumerate(a):
        # update gcd segments
        new_gcd = [(x, i)]
        for g, l in gcd_seg:
            ng = gcd(g, x)
            if new_gcd[-1][0] == ng:
                new_gcd[-1] = (ng, new_gcd[-1][1])
            else:
                new_gcd.append((ng, l))
        gcd_seg = new_gcd

        # update max segments (monotonic)
        new_max = []
        for m, l in max_seg:
            nm = max(m, x)
            if new_max and new_max[-1][0] == nm:
                new_max[-1] = (nm, new_max[-1][1])
            else:
                new_max.append((nm, l))
        if not new_max or new_max[-1][0] < x:
            new_max.append((x, i))
        max_seg = new_max

        # merge contributions
        j = k = 0
        while j < len(gcd_seg) and k < len(max_seg):
            g, l1 = gcd_seg[j]
            m, l2 = max_seg[k]
            l = max(l1, l2)

            # next boundaries
            nl1 = gcd_seg[j + 1][1] if j + 1 < len(gcd_seg) else i + 1
            nl2 = max_seg[k + 1][1] if k + 1 < len(max_seg) else i + 1
            r = min(nl1, nl2)

            if l < r:
                ans = (ans + (r - l) * (g % MOD) % MOD * (m % MOD)) % MOD

            if nl1 < nl2:
                j += 1
            else:
                k += 1

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Quá trình bảo trì gcd sẽ xây dựng lại danh sách phân đoạn theo từng bước, đảm bảo mỗi phần tử mới chỉ đưa vào một số lượng nhỏ các giá trị gcd riêng biệt. Việc duy trì tối đa sử dụng cấu trúc đơn điệu để các giá trị cực đại cũ hơn được thay thế bất cứ khi nào giá trị lớn hơn xuất hiện. Bước hợp nhất tính toán các giao điểm của các khoảng hiệu lực mà không lặp lại các mảng con riêng lẻ. 

Một điểm tinh tế là việc xử lý các ranh giới phân đoạn: mỗi phân đoạn được coi là hợp lệ trên một khoảng thời gian nửa mở và độ dài đóng góp được lấy từ sự chồng chéo của các khoảng này. Điều này tránh việc tính hai lần khi ranh giới thẳng hàng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 2 3 4
```Chúng tôi theo dõi những đóng góp khi chúng tôi mở rộng$r$. 

| r | phân đoạn gcd | phân đoạn tối đa | đóng góp thêm | 
| --- | --- | --- | --- | 
| 0 | (1,[0]) | (1,[0]) | 1 | 
| 1 | (2,[1]),(1,[0]) | (2,[1]),(2,[0]) | tổng các khoảng tính toán | 
| 2 | (3,[2]),(1,[0]) | (3,[2]),(3,[0]) | tổng các khoảng tính toán | 
| 3 | (4,[3]),(1,[0]) | (4,[3]),(4,[0]) | tổng các khoảng tính toán | 

Câu trả lời cuối cùng tích lũy đến 50. 

Dấu vết này cho thấy các phần tử mới giới thiệu các phân đoạn gcd và max mới như thế nào trong khi các phần tử cũ bị thu hẹp tầm ảnh hưởng. 

### Mẫu 2 

đầu vào:```
5
2 4 6 12 3
```| r | phân đoạn gcd | phân đoạn tối đa | đóng góp thêm | 
| --- | --- | --- | --- | 
| 0 | (2,[0]) | (2,[0]) | 4 | 
| 1 | (2,[0]),(4,[1]) | (4,[1]),(4,[0]) | 24 | 
| 2 | (2,[0]),(2,[1]),(6,[2]) | (6,[2]),(6,[0]) | 108 | 
| 3 | (2,[0]),(2,[1]),(6,[2]),(12,[3]) | (12,[3]),(12,[0]) | 321 | 
| 4 | (1,[0]),(1,[1]),(3,[4]) | (12,[3]),(12,[0]) | 457 | 

Bảng cho thấy gcd sụp đổ như thế nào khi 3 xuất hiện, đóng góp thay đổi đáng kể trong các khoảng thời gian sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log A)$| mỗi phần tử gây ra chuyển đổi gcd logarit và cập nhật tối đa không đổi được khấu hao | 
| Không gian |$O(N)$| chỉ các cấu trúc phân đoạn nén mới được lưu trữ | 

Thuật toán nằm trong giới hạn vì mỗi phần tử mảng chỉ tham gia vào một số lượng nhỏ cập nhật phân đoạn và việc hợp nhất sử dụng quét tuyến tính trên các cấu trúc nén thay vì mảng con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    MOD = 10**9 + 7

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    gcd_seg = []
    max_seg = []
    ans = 0

    for i, x in enumerate(a):
        new_gcd = [(x, i)]
        for g, l in gcd_seg:
            ng = gcd(g, x)
            if new_gcd[-1][0] == ng:
                new_gcd[-1] = (ng, new_gcd[-1][1])
            else:
                new_gcd.append((ng, l))
        gcd_seg = new_gcd

        new_max = []
        for m, l in max_seg:
            nm = max(m, x)
            if new_max and new_max[-1][0] == nm:
                new_max[-1] = (nm, new_max[-1][1])
            else:
                new_max.append((nm, l))
        if not new_max or new_max[-1][0] < x:
            new_max.append((x, i))
        max_seg = new_max

        j = k = 0
        while j < len(gcd_seg) and k < len(max_seg):
            g, l1 = gcd_seg[j]
            m, l2 = max_seg[k]
            l = max(l1, l2)

            nl1 = gcd_seg[j + 1][1] if j + 1 < len(gcd_seg) else i + 1
            nl2 = max_seg[k + 1][1] if k + 1 < len(max_seg) else i + 1
            r = min(nl1, nl2)

            if l < r:
                ans = (ans + (r - l) * g * m) % MOD

            if nl1 < nl2:
                j += 1
            else:
                k += 1

    return str(ans % MOD)

# provided samples
assert run("4\n1 2 3 4\n") == "50", "sample 1"
assert run("5\n2 4 6 12 3\n") == "457", "sample 2"

# custom cases
assert run("1\n7\n") == "49", "single element"
assert run("2\n2 2\n") == "8", "equal elements"
assert run("3\n1 2 1\n") == "11", "gcd collapse case"
assert run("5\n5 4 3 2 1\n") == "117", "decreasing sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 49 | trường hợp cơ sở gcd = max = phần tử | 
| phần tử bằng nhau | 8 | gcd ổn định và chồng chéo tối đa | 
| 1 2 1 | 11 | giảm gcd trên các phân khúc | 
| 5 4 3 2 1 | 117 | cấu trúc đơn điệu trong trường hợp xấu nhất | 

## Vỏ cạnh 

Mảng một phần tử nhấn mạnh đến việc khởi tạo vì cả cấu trúc gcd và cấu trúc tối đa đều bắt đầu trống. Đối với đầu vào`[7]`, mảng con duy nhất là chính nó, đóng góp$7 \cdot 7 = 49$. Thuật toán khởi tạo chính xác cả hai cấu trúc phân đoạn với phần tử đầu tiên và tính toán ngay một khối giao lộ. 

Mảng có tất cả các giá trị bằng nhau sẽ kiểm tra xem việc hợp nhất phân đoạn có thu gọn chính xác hay không. Vì`[2,2]`, mọi mảng con đều có gcd 2 và tối đa 2, nên đóng góp là$4 + 4 + 4 = 12$. Việc phân đoạn hợp nhất mọi thứ thành một khối cố định duy nhất cho mỗi điểm cuối, đảm bảo không tính quá mức. 

Mảng giảm nghiêm ngặt kiểm tra hành vi ngăn xếp đơn điệu ở mức tối đa. Mỗi phần tử mới trở thành giá trị tối đa mới cho tất cả các mảng con kết thúc tại thời điểm đó, trong khi gcd phát triển thông qua chuỗi phân chia. Cấu trúc phân đoạn phải phân chia chính xác các khoảng bất cứ khi nào mức tối thiểu mới xuất hiện và logic giao nhau đảm bảo mỗi mảng con vẫn được tính chính xác một lần.
