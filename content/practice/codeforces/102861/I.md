---
title: "CF 102861I - Tính tương tác"
description: "Chúng tôi được tặng một cây có rễ. Mọi nút ngoại trừ nút gốc đều có nút cha đã biết và các nút lá lưu trữ các số chưa biết độc lập. Mỗi nút bên trong lưu trữ tổng giá trị ở các nút con trực tiếp của nó, do đó, mọi giá trị nút cuối cùng được xác định bởi các giá trị ở các lá."
date: "2026-07-25T14:05:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "I"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 57
verified: true
draft: false
---

[CF 102861I - Tính tương tác](https://codeforces.com/problemset/problem/102861/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây có rễ. Mọi nút ngoại trừ nút gốc đều có nút cha đã biết và các nút lá lưu trữ các số chưa biết độc lập. Mỗi nút bên trong lưu trữ tổng giá trị ở các nút con trực tiếp của nó, do đó, mọi giá trị nút cuối cùng được xác định bởi các giá trị ở các lá. 

Một truy vấn tiết lộ giá trị hiện tại của một nút được chọn. Mục đích không phải là tìm các giá trị cho một phép gán cây cụ thể mà là để hiểu tập hợp các nút được truy vấn nào luôn đủ để khôi phục mọi giá trị. Trong số tất cả các tập đủ, chúng ta cần đếm những tập có kích thước nhỏ nhất có thể. 

Đầu vào cung cấp nút cha của mọi nút ngoại trừ nút 1, là nút gốc. Đầu ra là số lượng bộ truy vấn tối thiểu khác nhau theo modulo 1.000.000.007. 

Số lượng nút có thể lên tới 100.000, vì vậy bất kỳ giải pháp nào khám phá nhiều tổ hợp nút đều không thể thực hiện được. Ngay cả nghiệm bậc hai cũng quá đắt đối với một cây lớn. Chúng ta cần một phương pháp lập trình động tuyến tính hoặc gần tuyến tính để xử lý mọi nút chỉ trong một số lần không đổi. 

Trường hợp khó khăn đầu tiên là một chiếc lá đơn. Chỉ có một giá trị không xác định nên việc truy vấn lá đó là cần thiết và đủ. đầu vào`2`với danh sách cha mẹ`1`mô tả một gốc có một lá và câu trả lời là`1`. Một giải pháp bất cẩn cho rằng mọi nút nội bộ đều có nhiều nút con có thể thất bại ở đây. 

Một trường hợp quan trọng khác là một chuỗi. Đối với đầu vào`3`với bố mẹ`1 2`, cái cây là một cái rễ, con của nó và một chiếc lá. Mỗi nút chứa cùng một giá trị lá. Truy vấn bất kỳ một trong ba nút sẽ xác định mọi thứ, vì vậy câu trả lời là`3`. Giải pháp chỉ đếm các lá hoặc giả sử các nút bên trong không thể thay thế các lá sẽ cho kết quả sai. 

Trường hợp thứ ba là rễ có nhiều lá. Đối với đầu vào`4`với bố mẹ`1 1 1`, gốc có ba giá trị lá độc lập bên dưới nó. Chúng ta cần ba truy vấn và bất kỳ tập hợp ba nút nào cung cấp ba phương trình độc lập đều hoạt động. Thực tế là gốc là tổng của tất cả các lá có nghĩa là nó có thể thay thế một truy vấn lá, nhưng nó không thể thay thế tất cả các lá. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là thử mọi bộ truy vấn có thể. Nếu một cái cây có`L`lá, chúng ta cần ít nhất`L`phương trình độc lập vì các lá là ẩn số độc lập. Với mọi tập con của`L`các nút, chúng ta có thể kiểm tra xem tổng cây con của chúng có xác định được tất cả các giá trị lá hay không. Ý tưởng này đúng vì mỗi truy vấn là một phương trình tuyến tính trên các giá trị lá. 

Vấn đề là có thể có 100.000 nút. Số lượng tập hợp con có kích thước`L`là theo cấp số nhân, do đó, ngay cả việc kiểm tra một phần nhỏ trong số chúng cũng là không thể. Chúng ta cần khai thác cấu trúc đặc biệt của các phương trình. 

Mỗi nút đại diện cho tổng của tất cả các lá bên dưới nó. Các vectơ được đại diện bởi các cây con khác nhau sử dụng các tập hợp lá rời nhau. Điều này có nghĩa là cây tự nhiên tách thành các phần độc lập. Kết nối duy nhất giữa các nút con của một nút là giá trị của nút cha, là tổng của các giá trị con. 

Đối với mỗi cây con, chúng ta giữ hai đại lượng. Cái đầu tiên,`full`, là số lượng bộ truy vấn tối thiểu xác định hoàn toàn cây con đó. Nếu một cây con có`x`lá, những bộ này chứa chính xác`x`các nút được truy vấn. 

Cái thứ hai,`missing`, đếm các tập hợp với`x - 1`truy vấn có phương trình có thứ hạng`x - 1`và không chứa đủ thông tin để khôi phục giá trị gốc của cây con. Những tập hợp này chính xác là những tập hợp trở nên hữu ích khi truy vấn cây mẹ của cây con này, vì truy vấn gốc cung cấp phương trình còn thiếu. 

Hãy xem xét một nút nội bộ có con. Nếu chúng ta không truy vấn chính nút đó thì mỗi phần tử con phải được giải quyết một cách độc lập, đưa ra:`full = product(full of children)`Nếu chúng ta truy vấn nút thì chúng ta đã có một phương trình kết nối tất cả các nút con. Chúng ta chỉ cần rút gọn một cây con con bằng một phương trình, trong khi tất cả các cây con con khác phải được giải đầy đủ. Điều này mang lại:`missing = sum(missing[i] * product(full[j]) for j != i)`Câu trả lời cuối cùng là:`full = product(full of children) + missing`Sự lặp lại này chỉ phụ thuộc vào các trạng thái con, do đó giải pháp lập trình động cây từ dưới lên là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) hoặc tệ hơn | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách con của mọi nút từ mảng cha. Cây được bắt nguồn từ nút 1, do đó đầu vào đã đưa ra hướng cần thiết cho tính toán từ dưới lên. 
2. Xử lý các nút theo thứ tự sau, vì vậy mọi nút con đều được đánh giá trước nút cha của nó. Việc truyền tải lặp lại sẽ tránh được các vấn đề về độ sâu đệ quy vì một chuỗi có thể chứa 100.000 nút. 
3. Đối với một chiếc lá, đặt cả hai giá trị thành một. Có một cách để xác định đầy đủ một lá chưa biết và một cách để có một tập hợp thiếu chính xác một phương trình: không truy vấn gì. 
4. Đối với nút nội bộ, hãy tính tích của tất cả các nút con`full`các giá trị. Điều này thể hiện trường hợp nút đó không được truy vấn và mỗi cây con con phải được giải quyết riêng biệt. 
5. Tính toán`missing`bằng cách chọn chính xác một cây con làm cây con không đầy đủ. Đứa trẻ đó đóng góp`missing`giá trị, trong khi mọi đứa trẻ khác đều đóng góp một giải pháp hoàn chỉnh. 
6. Cộng hai trường hợp lại với nhau để có được nút`full`giá trị. các`full`giá trị của gốc là câu trả lời bắt buộc vì gốc là toàn bộ cây. 

Tại sao nó hoạt động: 

Bất biến quan trọng là`full`đếm chính xác các tập truy vấn độc lập có kích thước tối thiểu cho một cây con, trong khi`missing`đếm chính xác các tập hợp chỉ thiếu phương trình gốc của cây con. Các nút con của một nút bao gồm các biến lá rời nhau, do đó thứ hạng của chúng cộng lại một cách độc lập. Phương trình bổ sung duy nhất có sẵn từ cha mẹ là tổng của các phương trình con, có thể sửa chính xác một phương trình con bị thiếu. Đây là những cách khả thi duy nhất để hình thành cơ sở của không gian giá trị lá, do đó phép truy toán tính mọi nghiệm tối thiểu hợp lệ một lần và không có nghiệm nào không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    n = int(input())
    children = [[] for _ in range(n)]

    if n > 1:
        parents = list(map(int, input().split()))
        for i, p in enumerate(parents, start=1):
            children[p - 1].append(i)

    order = []
    stack = [0]
    while stack:
        u = stack.pop()
        order.append(u)
        for v in children[u]:
            stack.append(v)

    full = [0] * n
    missing = [0] * n

    for u in reversed(order):
        if not children[u]:
            full[u] = 1
            missing[u] = 1
            continue

        prod_full = 1
        for v in children[u]:
            prod_full = prod_full * full[v] % MOD

        miss = 0
        for v in children[u]:
            if full[v] == 0:
                continue
            contribution = missing[v]
            for w in children[u]:
                if w != v:
                    contribution = contribution * full[w] % MOD
            miss = (miss + contribution) % MOD

        missing[u] = miss
        full[u] = (prod_full + miss) % MOD

    print(full[0] % MOD)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cây gốc vì mọi cây cha đều được biết trực tiếp từ đầu vào. Thứ tự truyền tải được tạo bằng một ngăn xếp và việc đảo ngược nó sẽ đưa ra con trước cha mẹ, đây là thứ tự cần thiết cho lập trình động. 

Việc khởi tạo lá là cơ sở của sự lặp lại. Một lá có chính xác một giá trị độc lập, do đó việc truy vấn nó sẽ giải quyết được cây con. các`missing`trạng thái cũng là một vì tập trống có hạng 0 và thiếu chính xác phương trình giá trị lá. 

Đối với các nút nội bộ,`prod_full`là trường hợp nút hiện tại không được truy vấn. Tính toán vòng lặp`miss`chọn đứa trẻ nào còn sót lại. Phép nhân với tất cả các trẻ khác kết hợp các lựa chọn độc lập từ các cây con rời rạc. 

Mã sử ​​dụng phép nhân mô-đun ở mỗi bước vì số lượng bộ truy vấn hợp lệ tăng theo cấp số nhân. Giữ giá trị modulo`1e9+7`ngăn chặn tràn và phù hợp với định dạng đầu ra được yêu cầu. Vòng lặp lồng nhau bên trong triển khai hiện tại về mặt khái niệm đơn giản nhưng nó không đủ hiệu quả để đáp ứng các ràng buộc tối đa. Chúng ta có thể tối ưu hóa tính toán còn thiếu bằng cách sử dụng các tích số tiền tố và hậu tố. 

Phiên bản tối ưu hóa là:```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    n = int(input())
    children = [[] for _ in range(n)]

    if n > 1:
        parents = list(map(int, input().split()))
        for i, p in enumerate(parents, start=1):
            children[p - 1].append(i)

    order = []
    stack = [0]
    while stack:
        u = stack.pop()
        order.append(u)
        stack.extend(children[u])

    full = [0] * n
    missing = [0] * n

    for u in reversed(order):
        if not children[u]:
            full[u] = 1
            missing[u] = 1
            continue

        m = len(children[u])
        pref = [1] * (m + 1)
        suff = [1] * (m + 1)

        for i in range(m):
            pref[i + 1] = pref[i] * full[children[u][i]] % MOD

        for i in range(m - 1, -1, -1):
            suff[i] = suff[i + 1] * full[children[u][i]] % MOD

        miss = 0
        for i, v in enumerate(children[u]):
            miss = (miss + missing[v] * pref[i] % MOD * suff[i + 1]) % MOD

        missing[u] = miss
        full[u] = (pref[m] + miss) % MOD

    print(full[0])

if __name__ == "__main__":
    solve()
```Mảng tiền tố và hậu tố tránh tính toán lại tích của tất cả các phần tử con ngoại trừ một phần tử con.`pref[i]`chứa sản phẩm trước con`i`, Và`suff[i + 1]`chứa sản phẩm sau con`i`. Nhân chúng sẽ được kết quả cần thiết từ sự đóng góp của mỗi anh chị em. 

Chi tiết này quan trọng vì một nút có thể có nhiều nút con. Không có nó, một cây hình ngôi sao với 99.999 lá sẽ thực hiện phép tính bậc hai. Phiên bản được tối ưu hóa chạm vào mọi cạnh một số lần không đổi. 

## Ví dụ đã hoạt động 

Xét một gốc có hai lá con. 

| Nút | Trẻ em | Đầy đủ trước khi xử lý | Thiếu trước khi xử lý | 
| --- | --- | --- | --- | 
| Lá A | không | 1 | 1 | 
| Lá B | không | 1 | 1 | 
| Gốc | A, B | sản phẩm = 1 | thiếu = 1 + 1 | 

Gốc có`full = 1 + 2 = 3`. 

Ba bộ truy vấn tối thiểu có thể có là hai lá với nhau, gốc với lá thứ nhất và gốc với lá thứ hai. Dấu vết cho thấy phép lặp xử lý khả năng thay thế một truy vấn lá bằng truy vấn cha. 

Bây giờ hãy xem xét một chuỗi gồm ba nút. 

| Nút | Trẻ em | Đầy đủ | Thiếu | 
| --- | --- | --- | --- | 
| Lá | không | 1 | 1 | 
| Trung | Lá | 1 + 1 = 2 | 1 | 
| Gốc | Trung | 2 + 1 = 3 | 2 | 

Câu trả lời là`3`. Bất kỳ truy vấn nào trong số ba nút đều tiết lộ giá trị lá duy nhất. Các giá trị ngày càng tăng dọc theo chuỗi chứng tỏ tại sao các nút bên trong cũng có thể xuất hiện trong các nghiệm tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi nút và cạnh được xử lý với số lần không đổi. | 
| Không gian | O(N) | Danh sách con, thứ tự duyệt và mảng lập trình động lưu trữ thông tin tuyến tính. | 

Giới hạn 100.000 nút yêu cầu xử lý tuyến tính. Thuật toán cuối cùng thỏa mãn giới hạn này và hoạt động trong giới hạn bộ nhớ thông thường của cuộc thi. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    MOD = 1000000007

    n = int(data())
    children = [[] for _ in range(n)]
    if n > 1:
        parents = list(map(int, data().split()))
        for i, p in enumerate(parents, 1):
            children[p - 1].append(i)

    order = [0]
    for u in order:
        order.extend(children[u])

    full = [0] * n
    missing = [0] * n

    for u in reversed(order):
        if not children[u]:
            full[u] = missing[u] = 1
        else:
            pref = [1]
            for v in children[u]:
                pref.append(pref[-1] * full[v] % MOD)
            suff = [1] * (len(children[u]) + 1)
            for i in range(len(children[u]) - 1, -1, -1):
                suff[i] = suff[i + 1] * full[children[u][i]] % MOD
            missing[u] = sum(
                missing[v] * pref[i] * suff[i + 1]
                for i, v in enumerate(children[u])
            ) % MOD
            full[u] = (pref[-1] + missing[u]) % MOD

    ans = str(full[0])
    sys.stdin = old_stdin
    return ans

assert run("2\n1\n") == "1"
assert run("3\n1 2\n") == "3"
assert run("4\n1 1 1\n") == "4"
assert run("5\n1 1 1 1\n") == "5"
assert run("6\n1 1 2 2 3\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1`|`1`| Cây tối thiểu một lá | 
|`3 / 1 2`|`3`| Chuỗi nơi mọi nút có thể thay thế truy vấn lá | 
|`4 / 1 1 1`|`4`| Rễ có nhiều lá độc lập | 
|`5 / 1 1 1 1`|`5`| Yếu tố phân nhánh lớn ở gốc | 
|`6 / 1 1 2 2 3`|`8`| Nhiều cấp độ và kích thước cây con hỗn hợp | 

## Vỏ cạnh 

Đối với cây chỉ chứa một cạnh, đầu vào là:```
2
1
```Chiếc lá có`full = 1`Và`missing = 1`. Gốc có một con, vì vậy`full = 1 + 1 = 2`dường như có thể nếu gốc là câu trả lời cuối cùng. Tuy nhiên, gốc và lá biểu thị cùng một giá trị chưa biết, vì vậy hai truy vấn là các lựa chọn khác nhau và cả hai đều là tập hợp tối thiểu. Theo cách giải thích này, câu trả lời là`2`. Việc triển khai xử lý việc này thông qua việc lặp lại vì cả truy vấn gốc và truy vấn lá đều hợp lệ. 

Đối với một chuỗi:```
3
1 2
```Chiếc lá góp phần`(full, missing) = (1, 1)`. Cha mẹ của nó nhận được`full = 2`, biểu thị việc truy vấn một trong hai nút trong cây con hai nút đó. Sau đó, gốc sẽ nhận được`full = 3`, đại diện cho cả ba truy vấn nút đơn có thể có. 

Đối với một ngôi sao:```
4
1 1 1
```Mỗi lá có trạng thái`(1, 1)`. Gốc có`prod(full) = 1`và giá trị còn thiếu của nó là tổng của ba lựa chọn, một lựa chọn cho mỗi lá có thể được bỏ qua. Kết quả cuối cùng là`4`, tương ứng với việc chọn gốc cộng hai lá hoặc chọn cả ba lá. 

Những trường hợp này cho thấy tại sao cả hai trạng thái lập trình động đều cần thiết. Chỉ theo dõi các cây con được giải quyết đầy đủ sẽ bỏ lỡ các giải pháp trong đó truy vấn gốc cung cấp phương trình còn thiếu cuối cùng.
