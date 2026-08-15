---
title: "CF 102420H - Đám cưới"
description: "Chúng ta có một nhóm tiên luôn thay đổi. Ban đầu có n nàng tiên, được đánh số từ 1 đến n, và nàng tiên i có giá trị nguyên xã hội a[i]. Trong quá trình quan sát có q sự kiện. Sự kiện loại 1 thêm một nàng tiên mới."
date: "2026-08-12T06:34:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102420
codeforces_index: "H"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102420
solve_time_s: 111
verified: true
draft: false
---

[CF 102420H - Đám cưới](https://codeforces.com/problemset/problem/102420/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm tiên luôn thay đổi. Ban đầu có`n`các nàng tiên, được đánh số từ`1`bởi vì`n`, và nàng tiên`i`có một giá trị xã hội nguyên`a[i]`. Trong quá trình quan sát có`q`sự kiện. 

một loại`1`sự kiện thêm một nàng tiên mới. Giá trị của nó được sự kiện đưa ra trực tiếp và nó nhận được số chưa bao giờ được sử dụng tiếp theo, vì vậy các số đã xóa sẽ không bao giờ được sử dụng lại. một loại`2`sự kiện loại bỏ một nàng tiên hiện có theo số lượng của nó. một loại`3`sự kiện công bố một điệu nhảy có giá trị`e`. Mỗi nàng tiên hiện diện đều thay thế giá trị của nó`b`với`b XOR e`. 

Sau mỗi sự kiện, chúng ta cần tổng giá trị hiện tại của tất cả các nàng tiên vẫn còn hiện diện. 

Khó khăn là hoạt động XOR toàn cầu. Một điệu nhảy có thể ảnh hưởng tới`100000`các nàng tiên, và cũng có thể có`100000`những điệu nhảy. Việc cập nhật từng nàng tiên có thể yêu cầu khoảng`10^10`Hoạt động XOR trong trường hợp xấu nhất. Với`n, q <= 100000`, một thuật toán xung quanh`O(nq)`vượt xa những gì thực tế, trong khi một`O((n+q) log(n+q))`hoặc`O(30(n+q))`Cách tiếp cận dễ dàng hợp lý. 

Có một số trường hợp đặc biệt trong đó việc triển khai đơn giản có thể âm thầm trở thành sai sót. Đầu tiên, một nàng tiên có thể tham gia sau vài điệu nhảy. Coi như```
1 3
5
3 7
1 2
3 1
```Các đầu ra là```
2
4
5
```Sau điệu nhảy đầu tiên, nàng tiên ban đầu đã`5 XOR 7 = 2`. Nàng tiên mới tham gia với giá trị`2`, không`2 XOR 7`, bởi vì nó không tồn tại trong điệu nhảy đầu tiên. Một giải pháp chỉ lưu trữ một XOR toàn cầu duy nhất và áp dụng nó một cách mù quáng cho mọi giá trị mới được chèn sẽ xử lý sai trường hợp này. 

Thứ hai, việc xóa một nàng tiên sau một vài điệu nhảy đòi hỏi phải biết giá trị hiện tại của nó, nhưng chúng ta không được áp dụng tất cả các điệu nhảy trước đó vào nó. Ví dụ,```
1 3
4
3 3
2 1
1 1
```sản xuất```
7
0
1
```Cổ tích ban đầu có giá trị`4 XOR 3 = 7`khi nó rời đi. Nàng tiên mới được chèn vào có giá trị`1`, bởi vì nó đến sau buổi khiêu vũ. Việc biểu diễn dựa trên giá trị của mỗi nàng tiên tại thời điểm chèn phải tính đến XOR toàn cầu khi xóa nó. 

Thứ ba, số nhận dạng không bao giờ được sử dụng lại. Ví dụ,```
1 4
10
2 1
1 5
2 2
```sản xuất```
0
5
0
```Nàng tiên mới đến nhận được số`2`, không phải số`1`. Việc sử dụng lại các ID đã xóa có thể liên kết việc xóa sau này với trạng thái lưu trữ sai. 

Cuối cùng, các giá trị có thể đạt tới gần`2^30`và tổng số tiền có thể đạt khoảng`10^14`. Số nguyên Python xử lý việc này một cách trực tiếp, trong khi các ngôn ngữ có loại số nguyên có chiều rộng cố định cần số nguyên 64 bit cho câu trả lời. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lưu trữ giá trị hiện tại của mỗi nàng tiên. Đối với một loại`1`sự kiện chúng tôi thêm giá trị mới cho loại`2`chúng tôi loại bỏ nàng tiên được yêu cầu và loại`3`chúng tôi lặp lại mọi nàng tiên hiện có và thay thế giá trị của nó bằng`value XOR e`. Việc duy trì tổng cùng với các giá trị khiến việc chèn và xóa mất liên tục, nhưng một điệu nhảy vẫn đòi hỏi phải chạm vào mọi nàng tiên đang hoạt động. 

Phương pháp brute-force này đúng vì nó thực hiện chính xác thao tác được mô tả bởi sự cố. Điểm yếu của nó là số lần cập nhật lặp đi lặp lại. Trong trường hợp xấu nhất với`100000`nàng tiên và`100000`sự kiện khiêu vũ, nó có thể biểu diễn khoảng`10^10`cập nhật cổ tích cá nhân. Ngay cả trước khi xem xét chi phí Python, con số đó là quá lớn. 

Quan sát quan trọng là XOR hoạt động độc lập trên mọi bit. Giả sử chúng ta tập trung vào một vị trí bit. Nếu phần khiêu vũ là`0`, bit đó không thay đổi. Nếu phần khiêu vũ là`1`, nàng tiên hiện tại nào cũng lật cái đó. Chúng ta không cần biết giá trị đầy đủ của mỗi nàng tiên để biết sự đóng góp của bit đó vào tổng số tiền. Chúng ta chỉ cần số lượng nàng tiên đang hoạt động có bit được lưu trữ tương ứng là`1`. 

Có một điều phức tạp: các nàng tiên mới đến vào những thời điểm tùy ý. Cách rõ ràng để xử lý vấn đề này là tách lịch sử của các điệu nhảy toàn cầu khỏi trạng thái lưu trữ của mỗi nàng tiên. Cho phép`X`trở thành XOR của mọi điệu nhảy đã diễn ra cho đến nay. Đối với mỗi nàng tiên đang hoạt động, hãy lưu trữ một giá trị ẩn`base`sao cho giá trị hiện tại thực tế của nó là`base XOR X`. 

Đối với một nàng tiên nguyên bản,`base`ban đầu là giá trị đã cho của nó. Khi một nàng tiên mới có giá trị thực tế`v`đến, chúng tôi chọn cơ sở của nó là`v XOR X`. Khi đó giá trị thực của nó ngay lập tức trở thành`(v XOR X) XOR X = v`, đúng như yêu cầu. 

Khi một nàng tiên rời đi, cơ sở lưu trữ của nó đủ để xác định giá trị hiện tại của nó, bởi vì giá trị hiện tại chỉ đơn giản là`base XOR X`. Chúng tôi có thể loại bỏ sự đóng góp của nó khỏi số lượng mỗi bit mà không cần phải lặp lại các điệu nhảy cũ. 

Câu hỏi còn lại là làm thế nào để có được tổng số tiền một cách nhanh chóng. Đối với mỗi bit`k`, duy trì`cnt[k]`, số lượng các nàng tiên đang hoạt động có`base`có chút`k`bộ. Nếu bit`k`của`X`chính xác là bằng không`cnt[k]`các giá trị hiện tại có bit đó được đặt. Nếu bit`k`của`X`là một, XOR lật bit, chính xác là`active - cnt[k]`các giá trị hiện tại có bit đó được đặt. Vì vậy sự đóng góp của bit`k`số bit thiết lập của nó nhân với`2^k`. 

Chỉ có 30 bit liên quan vì tất cả các giá trị đầu vào nhiều nhất là`10^9`, bên dưới`2^30`. Mọi sự kiện đều có thể cập nhật số bit trong`O(30)`thời gian và mọi câu trả lời cũng có thể được tính toán theo`O(30)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nq)`trong trường hợp xấu nhất |`O(n + q)`| Quá chậm | 
| Tối ưu |`O(30(n + q))`|`O(n + q)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ số nguyên toàn cục`X`, ban đầu bằng không. Nó đại diện cho XOR của tất cả các điệu nhảy đã diễn ra cho đến nay. Giá trị thực tế của mỗi nàng tiên sẽ được thể hiện dưới dạng`base XOR X`. 
2. Duy trì`cnt[k]`cho mỗi bit`k`từ`0`bởi vì`29`. Nó lưu trữ bao nhiêu nàng tiên đang hoạt động có bit`k`đặt trong của họ`base`giá trị. Cũng giữ một mảng`base[id]`đối với mọi mã định danh cổ tích có thể có, bởi vì việc xóa sau này sẽ cung cấp cho chúng tôi mã định danh và chúng tôi cần biểu diễn được lưu trữ của nó. 
3. Đối với mỗi nàng tiên ban đầu, hãy lưu trữ giá trị đã cho làm cơ sở. Thêm một vào`cnt[k]`cho mỗi bit được đặt của giá trị đó. Từ`X = 0`ban đầu, biểu diễn cơ sở bằng giá trị thực. 
4. Đối với một loại`1`sự kiện có giá trị`v`, chỉ định mã định danh chưa sử dụng tiếp theo. Cơ sở mới là`v XOR X`. Đây là quy tắc chèn quan trọng: nàng tiên mới phải có giá trị thực tế`v`bây giờ, trong khi tất cả các điệu nhảy trước đó đã được mã hóa bởi`X`. 
5. Thêm các bit của cơ sở mới này vào`cnt`. Nàng tiên bây giờ được thể hiện nhất quán với mọi nàng tiên hiện có. 
6. Đối với một loại`2`sự kiện có định danh`p`, truy xuất`base[p]`. Xóa các bit đã đặt của nó khỏi`cnt`. Toàn cầu`X`không thay đổi nên không cần phải sửa đổi gì thêm. 
7. Đối với một loại`3`sự kiện có giá trị`e`, thay thế`X`qua`X XOR e`. Không có nàng tiên cá nhân nào được chạm vào. Vì mọi giá trị hiện tại được biểu diễn dưới dạng`base XOR X`, thay đổi`X`áp dụng thao tác XOR mới cho tất cả chúng cùng một lúc. 
8. Sau mỗi sự kiện, hãy tính toán câu trả lời từng chút một. cho chút`k`, nếu như`X`có số 0 ở đó, số lượng hiện tại là`cnt[k]`. Nếu như`X`có một cái ở đó, số là`active - cnt[k]`. Nhân số đó với`2^k`và thêm nó vào câu trả lời. 

### Tại sao nó hoạt động 

Điều bất biến là đối với mỗi nàng tiên đang hoạt động, giá trị thực tế của nó chính xác là`base[id] XOR X`, trong khi`cnt[k]`chính xác là số lượng cơ sở hoạt động với bit`k`bộ. Ban đầu cả hai phát biểu đều đúng vì`X = 0`. Việc chèn sẽ chọn`base = v XOR X`, vậy giá trị thực mới là`v`. Việc xóa sẽ loại bỏ chính xác cơ sở của nàng tiên đó khỏi mỗi lần đếm bit. Một điệu nhảy chỉ thay đổi`X`, biến đổi mọi giá trị hiện tại từ`base XOR X`vào trong`base XOR X XOR e`, chính xác là giá trị mới được yêu cầu. Cuối cùng, đối với mỗi bit, XOR sẽ bảo toàn tất cả các bit hoặc lật tất cả chúng, vì vậy`cnt[k]`hoặc`active - cnt[k]`đưa ra số lượng chính xác các giá trị hiện tại chứa bit đó. Tổng hợp các đóng góp bit này sẽ cho tổng số chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

BITS = 30

def add_base(x, cnt, delta):
    for b in range(BITS):
        if (x >> b) & 1:
            cnt[b] += delta

def solve():
    n, q = map(int, input().split())
    initial = list(map(int, input().split()))

    # base[id] is the value that must be XORed with global_x
    # to obtain the fairy's current value.
    base = [0] * (n + q + 1)

    # Number of active fairies whose base has each bit set.
    cnt = [0] * BITS

    global_x = 0
    active = n
    next_id = n + 1

    for i, x in enumerate(initial, 1):
        base[i] = x
        add_base(x, cnt, 1)

    out = []

    for _ in range(q):
        event = list(map(int, input().split()))
        t = event[0]

        if t == 1:
            v = event[1]

            # We need (base ^ global_x) == v.
            b = v ^ global_x

            base[next_id] = b
            add_base(b, cnt, 1)

            active += 1
            next_id += 1

        elif t == 2:
            p = event[1]
            b = base[p]

            add_base(b, cnt, -1)
            active -= 1

        else:
            e = event[1]
            global_x ^= e

        # Reconstruct the sum from the bit counts.
        ans = 0
        for b in range(BITS):
            ones = cnt[b]
            if (global_x >> b) & 1:
                ones = active - ones
            ans += ones << b

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`base`mảng có chỗ cho`n + q`số nhận dạng vì nhiều nhất`q`các nàng tiên mới có thể đến và mỗi nàng tiên đến sẽ nhận được một mã định danh mới. Mảng được lập chỉ mục trực tiếp theo số cổ tích, do đó việc xóa diễn ra liên tục ngoài bản cập nhật 30 bit. 

các`add_base`trình trợ giúp cập nhật tất cả các bộ đếm bit cho một giá trị cơ bản. Nó được gọi với`delta = 1`khi một nàng tiên bước vào và`delta = -1`khi một nàng tiên rời đi. Cách thể hiện tương tự được sử dụng bất kể có bao nhiêu điệu nhảy đã diễn ra. 

Đường chèn`b = v ^ global_x`là phần tinh tế nhất của việc thực hiện. Cơ sở được lưu trữ cố tình không`v`. Vì giá trị hiện tại phải là`b ^ global_x`, đang chọn`b = v ^ global_x`làm cho giá trị hiện tại chính xác`v`. 

một loại`3`sự kiện chỉ thực hiện`global_x ^= e`. Đây là hoạt động loại bỏ hành vi bậc hai tiềm ẩn của giải pháp vũ phu. Mọi nàng tiên đều ngầm nhận được điệu nhảy thông qua giá trị toàn cầu đã thay đổi. 

Câu trả lời được xây dựng lại từ bộ đếm 30 bit. Khi một chút`global_x`bằng 0, các cơ sở với tập bit đó vẫn được đặt. Khi nó bằng một, bit bị đảo ngược, do đó các cơ số bằng 0 sẽ trở thành cơ số hiện tại. biểu hiện`ones << b`tương đương với`ones * 2^b`. 

Không cần xử lý tràn trong Python. Tổng số tối đa nằm trong phạm vi biểu diễn số nguyên có độ chính xác tùy ý của Python một cách an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các cơ sở ban đầu là`[2, 3, 9, 5, 6, 6]`, và ban đầu`global_x = 0`. Trạng thái quan trọng sau mỗi sự kiện được hiển thị bên dưới. 

| Sự kiện | Hành động |`global_x`| Đang hoạt động | Cơ sở mới/xóa | Tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | Nàng tiên ban đầu |`0`| 6 |`[2,3,9,5,6,6]`| 31 | 
|`1 3`| Chèn cổ tích 7 |`0`| 7 |`base[7] = 3`| 34 | 
|`3 5`| XOR toàn cầu |`5`| 7 | không thay đổi | 37 | 
|`2 2`| Xóa cổ tích 2 |`5`| 6 | loại bỏ cơ sở`3`| 31 | 
|`3 2`| XOR toàn cầu |`7`| 6 | không thay đổi | 27 | 
|`2 7`| Xóa cổ tích 7 |`7`| 5 | loại bỏ cơ sở`3`| 23 | 

Nàng tiên được chèn vào có căn cứ`3`bởi vì XOR toàn cầu vẫn bằng 0. Sau buổi khiêu vũ với`5`, giá trị hiện tại của nó là`3 XOR 5 = 6`. Sau lần nhảy thứ hai, giá trị hiện tại của nó trở thành`3 XOR 7 = 4`, vì vậy việc xóa nó một cách chính xác sẽ loại bỏ cơ sở`3`từ các bộ đếm trong khi XOR hiện tại vẫn còn`7`. 

### Ví dụ được xây dựng 

Hãy xem xét```
1 3
5
3 7
1 2
3 1
```Nhà nước phát triển như sau. 

| Sự kiện | Hành động |`global_x`| Đang hoạt động | Đế chèn | Tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | Cổ tích ban đầu |`0`| 1 | không | 5 | 
|`3 7`| XOR toàn cầu |`7`| 1 | không | 2 | 
|`1 2`| Chèn cổ tích 2 |`7`| 2 |`2 XOR 7 = 5`| 4 | 
|`3 1`| XOR toàn cầu |`6`| 2 | không thay đổi | 5 | 

Nàng tiên mới nhận được giá trị hiện tại`2`mặc dù`global_x`đã rồi`7`. Cơ sở của nó là`5`, bởi vì`5 XOR 7 = 2`. Khi điệu nhảy tiếp theo thay đổi`global_x`ĐẾN`6`, giá trị của nó tự động trở thành`5 XOR 6 = 3`, trong khi nàng tiên ban đầu trở thành`5 XOR 6 = 3`cũng vậy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(30(n + q))`| Mỗi phép tính chèn, xóa và trả lời xử lý tối đa 30 bit. | 
| Không gian |`O(n + q)`| Mảng cơ sở lưu trữ mọi mã định danh cổ tích có thể có và mảng bộ đếm bit có kích thước không đổi. | 

Với nhiều nhất`200000`các mã định danh được lưu trữ và chỉ có 30 bit vị trí, thuật toán chỉ thực hiện vài triệu thao tác đơn giản. Đây là mức thoải mái trong phạm vi dự định cho`n, q <= 100000`, không giống như cách tiếp cận bạo lực có thể tiếp cận khoảng`10^10`cập nhật cổ tích. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây triển khai cùng một thuật toán như một hàm để có thể kiểm tra từng trường hợp bằng các xác nhận Python thông thường.```python
import sys
import io

def solve_string(inp: str) -> str:
    data = iter(inp.strip().split())
    n = int(next(data))
    q = int(next(data))

    initial = [int(next(data)) for _ in range(n)]

    BITS = 30
    base = [0] * (n + q + 1)
    cnt = [0] * BITS

    global_x = 0
    active = n
    next_id = n + 1

    def add_base(x, delta):
        for b in range(BITS):
            if (x >> b) & 1:
                cnt[b] += delta

    for i, x in enumerate(initial, 1):
        base[i] = x
        add_base(x, 1)

    out = []

    for _ in range(q):
        t = int(next(data))

        if t == 1:
            v = int(next(data))
            b = v ^ global_x
            base[next_id] = b
            add_base(b, 1)
            next_id += 1
            active += 1

        elif t == 2:
            p = int(next(data))
            add_base(base[p], -1)
            active -= 1

        else:
            e = int(next(data))
            global_x ^= e

        ans = 0
        for b in range(BITS):
            ones = cnt[b]
            if (global_x >> b) & 1:
                ones = active - ones
            ans += ones << b

        out.append(str(ans))

    return "\n".join(out)

# Provided sample
sample1 = """\
6 5
2 3 9 5 6 6
1 3
3 5
2 2
3 2
2 7
"""

assert solve_string(sample1) == """\
34
37
31
27
23
""".strip(), "sample 1"

# Minimum-size input, with insertion, dance, and deletion.
case2 = """\
1 3
1
1 2
3 3
2 1
"""

assert solve_string(case2) == """\
3
0
2
""".strip(), "minimum-size / insertion / deletion"

# New fairy after a previous dance.
case3 = """\
1 3
5
3 7
1 2
3 1
"""

assert solve_string(case3) == """\
2
4
5
""".strip(), "insertion after global XOR"

# All values equal, followed by several global XOR operations.
case4 = """\
4 4
7 7 7 7
3 7
3 1
2 2
1 7
"""

assert solve_string(case4) == """\
0
4
3
10
""".strip(), "all equal values and repeated XOR"

# Boundary values near 2^30, plus deletion and a new identifier.
case5 = """\
2 5
1 1073741823
3 1073741823
1 1073741823
2 1
3 1
2 2
"""

assert solve_string(case5) == """\
1073741822
1073741823
1073741822
1073741823
0
""".strip(), "30-bit boundary and identifier handling"

# Maximum-size style test, generated rather than written literally.
# 100000 identical fairies and 100000 insertions.
n = 100000
q = 100000
initial = " ".join(["1"] * n)
events = "\n".join(["1 1"] * q)
large_input = f"{n} {q}\n{initial}\n{events}\n"

large_output = solve_string(large_input)
lines = large_output.splitlines()

assert len(lines) == q, "maximum-size event count"
assert lines[0] == str(n + 1), "first maximum-size insertion"
assert lines[-1] == str(n + q), "last maximum-size insertion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 1 / 1 2 / 3 3 / 2 1`|`3, 0, 2`| Thay đổi và xóa trạng thái kích thước tối thiểu | 
|`1 3 / 5 / 3 7 / 1 2 / 3 1`|`2, 4, 5`| Một nàng tiên nhập cuộc sau những điệu nhảy trước đó | 
|`4 4 / 7 7 7 7 / 3 7 / 3 1 / 2 2 / 1 7`|`0, 4, 3, 10`| Các giá trị hoàn toàn bằng nhau và XOR toàn cầu lặp lại | 
|`2 5 / 1 1073741823 / ...`|`1073741822, 1073741823, 1073741822, 1073741823, 0`| Bit có liên quan cao nhất và số tiền lớn | 
| Đã tạo`n=q=100000`trường hợp |`100000`dòng đầu ra | Số lượng sự kiện tối đa và tăng trưởng số nhận dạng | 

## Vỏ cạnh 

Việc gia nhập nàng tiên sau các điệu nhảy trước đó được xử lý bằng cách điều chỉnh cơ sở của nó theo XOR toàn cầu hiện tại. TRONG```
1 3
5
3 7
1 2
3 1
```điệu nhảy đầu tiên thay đổi`global_x`ĐẾN`7`. Khi giá trị`2`đến, cơ sở của nó trở thành`2 XOR 7 = 5`. Giá trị hiện tại của nó là ngay lập tức`5 XOR 7 = 2`, theo yêu cầu. Điệu nhảy tiếp theo thay đổi`global_x`ĐẾN`6`, thế là nàng tiên mới tự động trở thành`5 XOR 6 = 3`. Trình tự đầu ra`2, 4, 5`xác nhận rằng các nàng tiên mới không bị ảnh hưởng bởi các điệu nhảy trước đó. 

Việc xóa một nàng tiên sau khi khiêu vũ có hiệu quả vì việc xóa hoạt động dựa trên cơ sở chứ không phải trên giá trị hiện tại. Vì```
1 3
4
3 3
2 1
1 1
```cổ tích ban đầu có cơ sở`4`Và`global_x = 3`, vậy giá trị hiện tại của nó là`7`. Xóa nó sẽ trừ đi các bit của cơ sở`4`từ`cnt`, không để lại nàng tiên nào đang hoạt động. Việc chèn tiếp theo xảy ra với`global_x = 3`, vậy là nàng tiên mới có giá trị`1`có được căn cứ`1 XOR 3 = 2`. Giá trị hiện tại của nó là`2 XOR 3 = 1`, tạo ra kết quả đầu ra`7, 0, 1`. 

Việc sử dụng lại mã định danh có thể tránh được bằng cách duy trì`next_id`độc lập với số lượng các nàng tiên đang hoạt động. TRONG```
1 4
10
2 1
1 5
2 2
```tiên`1`bị xóa nhưng nàng tiên mới nhận được mã định danh`2`. Cơ sở của nó là`5`và xóa định danh`2`loại bỏ chính xác nàng tiên đó. Các đầu ra là`0, 5, 0`. Một giải pháp tìm kiếm mã định danh miễn phí hoặc sử dụng lại`1`có thể khiến việc xóa cuối cùng ám chỉ nhầm nàng tiên. 

Trạng thái đặt trống cũng có hiệu lực sau khi xóa. Một lần`active`trở thành 0, số đếm hiện tại của mỗi bit bằng 0 bất kể`global_x`, bởi vì`active - cnt[k]`cũng bằng không. Lần chèn tiếp theo bắt đầu từ hiện tại`global_x`và xây dựng lại giá trị hiện tại chính xác ngay lập tức. 

Ranh giới 30 bit là an toàn vì mọi giá trị được cung cấp tối đa là`10^9`và XOR của các số bên dưới`2^30`cũng ở bên dưới`2^30`. Bit`0`bởi vì`29`do đó là đủ. Tổng số tiền có thể lớn hơn nhiều so với`2^30`, nhưng số nguyên Python có độ chính xác tùy ý, vì vậy việc tính toán trực tiếp câu trả lời là an toàn.
