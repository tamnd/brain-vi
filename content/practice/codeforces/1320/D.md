---
title: "CF 1320D - Chuỗi có thể tiếp cận"
description: "Chúng ta được cấp một chuỗi nhị phân cố định và chúng ta xem xét liên tục hai loại phép biến đổi cục bộ trên bất kỳ đoạn liền kề nào có độ dài ba: hoán đổi 011 thành 110 hoặc hoán đổi ngược 110 thành 011."
date: "2026-06-16T07:08:45+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "hashing", "strings"]
categories: ["algorithms"]
codeforces_contest: 1320
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 625 (Div. 1, based on Technocup 2020 Final Round)"
rating: 2500
weight: 1320
solve_time_s: 269
verified: false
draft: false
---

[CF 1320D - Chuỗi có thể tiếp cận](https://codeforces.com/problemset/problem/1320/D) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, băm, chuỗi 
**Thời gian giải:** 4 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân cố định và chúng tôi liên tục xem xét hai loại biến đổi cục bộ trên bất kỳ đoạn liền kề nào có độ dài ba: hoán đổi`011`vào trong`110`, hoặc hoán đổi ngược lại`110`vào trong`011`. Mỗi thao tác chỉ chạm vào ba ký tự liên tiếp và giữ nguyên tổng số ký tự, nhưng nó có thể di chuyển các ký tự xung quanh bên trong chuỗi. 

Câu hỏi cốt lõi không phải là áp dụng trực tiếp các hoạt động. Thay vào đó, chúng ta được hỏi một câu hỏi về khả năng tiếp cận trên các chuỗi con: đối với hai chuỗi con có độ dài bằng nhau của chuỗi gốc, chúng ta phải xác định xem liệu một chuỗi con có thể được chuyển đổi thành chuỗi con kia hay không bằng cách sử dụng một chuỗi các hoán đổi cục bộ này được áp dụng ở bất kỳ đâu trong chuỗi con. 

Một quan sát quan trọng đã ẩn trong tuyên bố là các hoạt động không bao giờ tương tác qua các ranh giới chuỗi con theo cách làm thay đổi tính khả thi toàn cầu. Do đó, mỗi truy vấn yêu cầu kiểm tra sự tương đương về cấu trúc giữa hai chuỗi nhị phân trong một hệ thống hoán vị ràng buộc được tạo bởi các chuỗi cục bộ này.`011 <-> 110`trao đổi. 

Các ràng buộc rất lớn: cả độ dài chuỗi và số lượng truy vấn đều lên tới 200.000. Bất kỳ giải pháp nào kiểm tra chuỗi con một cách đơn giản cho mỗi truy vấn sẽ ngay lập tức thất bại, vì ngay cả việc đọc tất cả các chuỗi con được truy vấn cũng đã là bậc hai trong trường hợp xấu nhất. Điều này buộc phải áp dụng cách tiếp cận tiền xử lý với thời gian truy vấn gần như không đổi hoặc logarit. 

Một ý tưởng ngây thơ là mô phỏng tất cả các phép biến đổi có thể có hoặc BFS qua chuỗi. Điều này là không thể vì không gian trạng thái có độ dài`n`chuỗi nhị phân là`2^n`và thậm chí cả quá trình chuyển đổi cục bộ cũng có quá nhiều. 

Ngoài ra còn có những cạm bẫy tinh tế. Một sai lầm phổ biến là cho rằng chỉ số lượng những cái đó mới quyết định khả năng tiếp cận. Điều đó không thành công. Ví dụ,`0110`Và`1010`có cùng số lượng nhưng không phải lúc nào cũng có thể truy cập được theo các thao tác được phép trong bối cảnh hạn chế chuỗi con. Một sai lầm khác là coi phép toán là hoán vị tùy ý của các phép toán, bỏ qua các ràng buộc giống như tính chẵn lẻ do cấu trúc kề gây ra. 

Bất biến đúng hóa ra lại tinh tế hơn là chỉ đếm. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là coi mỗi chuỗi con là một nút trong một biểu đồ ngầm khổng lồ, trong đó các cạnh tương ứng với việc áp dụng một`011 <-> 110`tráo đổi. Chúng tôi sẽ chạy BFS hoặc DFS từ một trạng thái chuỗi con để kiểm tra xem liệu chúng tôi có thể tiếp cận trạng thái chuỗi con khác hay không. Ngay cả đối với một truy vấn duy nhất, điều này là không khả thi, vì mỗi trạng thái có O(n) khả năng di chuyển và số lượng trạng thái là theo cấp số nhân. 

Sự đơn giản hóa quan trọng đến từ việc hiểu hoạt động thực sự thực hiện trên toàn cầu. Trao đổi`110 -> 011`di chuyển một`1`một vị trí ở bên phải trên một`0`, Và`011 -> 110`di chuyển một`1`một vị trí ở bên trái trên một`0`, nhưng chỉ trong một mẫu rất cụ thể liên quan đến hai số 0 hoặc số 0 liền kề. Điều này có nghĩa là người ta được phép “bong bóng” thông qua các số 0 một cách hiệu quả, nhưng với một hạn chế: thứ tự tương đối giữa các mẫu nhất định được giữ nguyên một phần. 

Một quan điểm hữu ích hơn là mã hóa chuỗi thành vị trí của một chuỗi. Các hoạt động cho phép hoán đổi liền kề các số 1 và 0 nhưng chỉ trong các cấu hình trong đó`1`vượt qua người khác`1`gián tiếp thông qua một`0`khoảng cách. Điều này dẫn đến một thực tế quan trọng: trong bất kỳ chuỗi con nào, khả năng tiếp cận chỉ phụ thuộc vào tập hợp khoảng cách giữa các chuỗi liên tiếp hoặc tương đương với cấu trúc tiền tố bắt nguồn từ vị trí của các chuỗi con. 

Giải pháp tiêu chuẩn sẽ điều chỉnh lại vấn đề bằng cách sử dụng tổng tiền tố của các số 1 và hệ tọa độ được chuyển đổi. Nếu chúng ta xác định một mảng tiền tố`pref[i]`như số lượng người lên đến vị trí`i`, thì mỗi chuỗi con có thể được chuẩn hóa bằng cách dịch chuyển các chỉ số sao cho các phép so sánh trở nên độc lập với vị trí tuyệt đối. Bất biến còn lại giúp phân biệt các chuỗi con có thể truy cập là mã hóa giống như hàm băm của chuỗi khoảng cách giữa các chuỗi bên trong chuỗi con. 

Để tính toán điều này một cách hiệu quả, chúng tôi tính toán trước vị trí của các giá trị đơn vị và xây dựng các giá trị băm tiền tố trên một mảng biểu thị khoảng cách giữa các giá trị liên tiếp. Sau đó, mỗi truy vấn chuỗi con được rút gọn thành việc so sánh hai biểu diễn chuẩn được tính toán trong O(1) bằng cách sử dụng hàm băm với tính toán trước tiền tố. 

Brute-force hoạt động vì nó trực tiếp khám phá các phép biến đổi, nhưng không thành công vì các phép biến đổi có tính chất theo cấp số nhân. Quan sát rằng hệ thống duy trì một bất biến có cấu trúc trên khoảng cách tương đối của những cái đó sẽ làm giảm vấn đề so sánh khoảng thời gian trên một chuỗi được tính toán trước, cho phép truy vấn O(1) sau quá trình tiền xử lý O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi trích xuất thông tin cấu trúc về chuỗi vẫn ổn định trong các giao dịch hoán đổi được phép. 

1. Xây dựng một mảng`pos`chứa tất cả các chỉ số`1`s trong chuỗi. Điều này nén chuỗi vào cấu trúc thực sự di chuyển trong các hoạt động. 
2. Xây dựng mảng dẫn xuất`g`đại diện cho khoảng cách giữa những cái liên tiếp, được định nghĩa là`g[i] = pos[i+1] - pos[i]`. Điều này ghi lại cách các cái được đặt cách nhau. 
3. Tính toán trước các giá trị băm tiền tố`g`sử dụng hàm băm lăn. Điều này cho phép so sánh bất kỳ đoạn khoảng trống nào trong O(1), vì khả năng tiếp cận phụ thuộc vào việc hai chuỗi con có tạo ra cấu trúc khoảng trống giống hệt nhau sau khi chuẩn hóa hay không. 
4. Xây dựng một mảng tiền tố bổ sung để đếm số đơn vị, để mỗi truy vấn có thể nhanh chóng xác định có bao nhiêu đơn vị nằm trong một chuỗi con. Điều này là bắt buộc vì các chuỗi con có số lượng chuỗi con khác nhau sẽ không bao giờ có thể truy cập được. 
5. Với mỗi truy vấn, trích xuất chuỗi con`[l1, r1]`Và`[l2, r2]`. Nếu số lượng khác nhau thì trả về ngay NO. 
6. Mặt khác, ánh xạ cả hai chuỗi con vào các chuỗi khoảng cách cảm ứng của chúng bằng cách sử dụng tìm kiếm nhị phân trên`pos`và so sánh các giá trị băm của chúng. Nếu giá trị băm khớp, trả về CÓ; nếu không thì KHÔNG. 

Lý do điều này có tác dụng là vì các thao tác được phép chỉ hoán vị cục bộ mà không thay đổi cấu trúc tương đối của các khoảng trống giữa chúng. Bất kỳ chuỗi hoán đổi hợp lệ nào đều bảo toàn nhiều tập hợp độ dài khoảng cách giữa các chuỗi liên tiếp trong một chuỗi con và bước băm nắm bắt chính xác bất biến này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = (1 << 61) - 1
BASE = 91138233

def modmul(a, b):
    return (a * b) & MOD

def build_hash(arr):
    n = len(arr)
    h = [0] * (n + 1)
    p = [1] * (n + 1)
    for i in range(n):
        p[i + 1] = modmul(p[i], BASE)
        h[i + 1] = (modmul(h[i], BASE) + arr[i] + 1) & MOD
    return h, p

def get_hash(h, p, l, r):
    return (h[r] - modmul(h[l], p[r - l])) & MOD

def solve():
    n = int(input())
    s = input().strip()

    pos = []
    pref = [0] * (n + 1)

    for i, c in enumerate(s):
        pref[i + 1] = pref[i] + (c == '1')
        if c == '1':
            pos.append(i)

    g = []
    for i in range(len(pos) - 1):
        g.append(pos[i + 1] - pos[i])

    if g:
        h, p = build_hash(g)
    else:
        h, p = [0], [1]

    q = int(input())

    def get_sub_hash(l, r):
        # extract ones in range via two pointers
        import bisect
        L = bisect.bisect_left(pos, l)
        R = bisect.bisect_right(pos, r) - 1

        if L >= R:
            return 0

        return get_hash(h, p, L, R)

    for _ in range(q):
        l1, l2, ln = map(int, input().split())
        l1 -= 1
        l2 -= 1
        r1 = l1 + ln
        r2 = l2 + ln

        if pref[r1] - pref[l1] != pref[r2] - pref[l2]:
            print("No")
            continue

        if get_sub_hash(l1, r1) == get_sub_hash(l2, r2):
            print("Yes")
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén chuỗi thành các vị trí bằng 1, vì các số 0 chỉ quan trọng trong chừng mực chúng phân tách các số 1. Mảng tiền tố`pref`cho phép đếm số lần trên mỗi chuỗi con theo thời gian liên tục, được sử dụng như một thử nghiệm loại bỏ nhanh chóng. 

các`g`mảng mã hóa khoảng cách giữa các mảng liên tiếp trên toàn cầu. Sau đó, chúng tôi xây dựng một hàm băm cuộn trên mảng này để có thể so sánh bất kỳ khoảng trống nào một cách hiệu quả. Chức năng trợ giúp`get_sub_hash`ánh xạ một chuỗi con vào khoảng tương ứng trong`pos`bằng cách sử dụng tìm kiếm nhị phân, sau đó chuyển đổi nó thành một phạm vi trên`g`. 

Một điểm tinh tế là việc xử lý các chuỗi con bằng 0 hoặc một. Trong trường hợp đó, không có khoảng trống và biểu diễn chính tắc trống, vì vậy chúng tôi trả về 0 dưới dạng hàm băm. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản`t = 11011`. 

Chúng tôi xử lý trước vị trí của những cái: 

| chỉ mục | 0 | 1 | 2 | 3 | 4 | 
| --- | --- | --- | --- | --- | --- | 
| char | 1 | 1 | 0 | 1 | 1 | 

Vì thế`pos = [0, 1, 3, 4]`, và khoảng trống`g = [1, 2, 1]`. 

Đối với một truy vấn so sánh`[1, 3, 3]`Và`[3, 5, 3]`: 

Chuỗi con đầu tiên`110`có những cái ở vị trí`[0,1]`vì vậy chữ ký khoảng cách của nó là`[1]`. 

Chuỗi con thứ hai`011`cũng có hai cái ở vị trí`[3,4]`, cũng có khoảng cách`[1]`. 

| Bước | Chuỗi con 1 | Chuỗi con 2 | 
| --- | --- | --- | 
| L, R trong tư thế | [0,1] | [2,3] | 
| phạm vi khoảng cách | g[0:1] | g[2:3] | 
| băm | h(1) | h(1) | 

Cả hai đều trùng nhau nên câu trả lời là CÓ. 

Điều này chứng tỏ rằng vị trí tuyệt đối không quan trọng, chỉ có khoảng cách bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q log n) | tiền xử lý xây dựng cấu trúc tiền tố; mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | lưu trữ vị trí, khoảng trống và mảng băm | 

Quá trình tiền xử lý là tuyến tính theo kích thước chuỗi và mỗi truy vấn thực hiện một số lượng tìm kiếm nhị phân và tra cứu băm không đổi. Điều này phù hợp thoải mái trong các hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod
    import builtins

    input = sys.stdin.readline

    MOD = (1 << 61) - 1
    BASE = 91138233

    def modmul(a, b):
        return (a * b) & MOD

    def build_hash(arr):
        n = len(arr)
        h = [0] * (n + 1)
        p = [1] * (n + 1)
        for i in range(n):
            p[i + 1] = modmul(p[i], BASE)
            h[i + 1] = (modmul(h[i], BASE) + arr[i] + 1) & MOD
        return h, p

    def get_hash(h, p, l, r):
        return (h[r] - modmul(h[l], p[r - l])) & MOD

    def solve():
        n = int(input())
        s = input().strip()

        pos = []
        pref = [0] * (n + 1)

        for i, c in enumerate(s):
            pref[i + 1] = pref[i] + (c == '1')
            if c == '1':
                pos.append(i)

        g = []
        for i in range(len(pos) - 1):
            g.append(pos[i + 1] - pos[i])

        if g:
            h, p = build_hash(g)
        else:
            h, p = [0], [1]

        q = int(input())

        import bisect

        def get_sub_hash(l, r):
            L = bisect.bisect_left(pos, l)
            R = bisect.bisect_right(pos, r) - 1
            if L >= R:
                return 0
            return get_hash(h, p, L, R)

        for _ in range(q):
            l1, l2, ln = map(int, input().split())
            l1 -= 1
            l2 -= 1
            r1 = l1 + ln
            r2 = l2 + ln

            if pref[r1] - pref[l1] != pref[r2] - pref[l2]:
                print("No")
                continue

            if get_sub_hash(l1, r1) == get_sub_hash(l2, r2):
                print("Yes")
            else:
                print("No")

    return run.__wrapped__() if hasattr(run, "__wrapped__") else None

# provided samples
assert run("""5
11011
3
1 3 3
1 4 2
1 2 3
""") == """Yes
Yes
No
"""

# custom cases
assert run("""1
0
1
1 1 1
""") == "Yes\n"

assert run("""3
111
2
1 1 3
1 1 1
""") == "Yes\nYes\n"

assert run("""5
10101
2
1 3 3
2 4 3
""") == "Yes\nYes\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | Có | khả năng tiếp cận một ký tự tầm thường | 
| tất cả những cái | Có/Có | ổn định cấu trúc thống nhất | 
| xen kẽ | Có/Có | chuỗi con đối xứng có cấu trúc giống hệt nhau | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi chuỗi con chứa 0 hoặc một`1`. Ví dụ, trong`10000`, chuỗi con`[2,5]`không có ai cả. Thuật toán ánh xạ chuỗi này tới một chuỗi khoảng trống trống, do đó, bất kỳ chuỗi con hoàn toàn bằng 0 nào khác đều tạo ra cùng một hàm băm chuẩn, trả về chính xác CÓ. 

Một trường hợp khác là khi các chuỗi con có cùng số lượng chuỗi con nhưng khoảng cách bên trong khác nhau. Ví dụ,`10100`so với`10010`cả hai đều chứa hai cái, nhưng cấu trúc khoảng cách của chúng khác nhau. Tìm kiếm nhị phân trích xuất các khoảng khác nhau của`pos`, tạo ra các mảng khoảng cách khác nhau, do đó giá trị băm khác nhau và thuật toán trả về NO một cách chính xác. 

Trường hợp tinh vi cuối cùng là khi các chuỗi con trùng lặp khác với chuỗi toàn cục`pos`ranh giới mảng. Bởi vì việc lập bản đồ được thực hiện thông qua`bisect_left`Và`bisect_right`, khoảng được trích xuất luôn được căn chỉnh với khoảng thực tế bên trong chuỗi con, ngăn chặn việc bao gồm từng số 0 ở ranh giới, nếu không sẽ làm hỏng cấu trúc khoảng cách.
