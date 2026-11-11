---
title: "CF 161E - Kẻ phá két Polycarpus"
description: "Chúng ta được cho một số số nguyên tố. Mỗi số nguyên tố đại diện cho hàng đầu tiên của ma trận vuông gồm các chữ số. Nếu số nguyên tố có độ dài n thì ma trận là n × n. Ma trận phải thỏa mãn hai điều kiện. Đầu tiên, mỗi hàng được hiểu là số thập phân thì bản thân nó phải là số nguyên tố."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp"]
categories: ["algorithms"]
codeforces_contest: 161
codeforces_index: "E"
codeforces_contest_name: "VK Cup 2012 Round 1"
rating: 2500
weight: 161
solve_time_s: 111
verified: true
draft: false
---

[CF 161E - Polycarpus the Safecracker](https://codeforces.com/problemset/problem/161/E) 

**Đánh giá:** 2500 
**Tags:** vũ lực, dp 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số số nguyên tố. Mỗi số nguyên tố đại diện cho hàng đầu tiên của ma trận vuông gồm các chữ số. Nếu số nguyên tố có độ dài`n`, thì ma trận là`n × n`. 

Ma trận phải thỏa mãn hai điều kiện. 

Đầu tiên, mỗi hàng được hiểu là số thập phân thì bản thân nó phải là số nguyên tố. Các hàng không phải hàng đầu tiên được phép bắt đầu bằng số 0. 

Thứ hai, ma trận có tính đối xứng. Chữ số ở vị trí`(i, j)`phải bằng chữ số tại`(j, i)`. 

Nhiệm vụ là đếm xem có bao nhiêu ma trận như vậy tồn tại ở mỗi hàng đầu tiên cho trước. 

Một cách hữu ích để suy nghĩ về ma trận là thế này: khi hàng đầu tiên được cố định, tính đối xứng ngay lập tức cũng cố định cột đầu tiên. Sau đó, mọi lựa chọn còn lại phải đồng thời thỏa mãn tính nhất quán của hàng và cột. 

Độ dài nguyên tố tối đa chỉ là`5`, vì số đầu vào nhiều nhất là`99999`. Điều đó làm thay đổi hoàn toàn bản chất của vấn đề. Mặc dù không gian tìm kiếm của tất cả các ma trận chữ số là rất lớn nhưng kích thước thực tế lại rất nhỏ. Điều này gợi ý rõ ràng rằng phương pháp lập trình động hoặc phương pháp lập trình động được cắt tỉa cẩn thận có thể hoạt động. 

Số lượng ca kiểm thử nhiều nhất là`30`, vì vậy quá trình tiền xử lý rất hấp dẫn. Có ít hơn`100000`tổng số chuỗi có năm chữ số và chỉ khoảng`10^4`số nguyên tố dưới đây`100000`. Chúng ta có thể liệt kê các số nguyên tố một lần và sử dụng lại chúng trong tất cả các trường hợp thử nghiệm. 

Việc tìm kiếm ngây thơ trên tất cả các ma trận có thể là vô vọng. Đối với một`5 × 5`ma trận, có`10^25`bài tập chữ số. Ngay cả khi chúng ta chỉ xem xét các hàng nguyên tố, vẫn có hàng nghìn ứng cử viên trên mỗi hàng, dẫn đến hàng nghìn tỷ kết hợp. 

Phần khó khăn là các ràng buộc về hàng và tính đối xứng tương tác theo cả hai hướng. Một DFS bất cẩn có thể dễ dàng đếm các ma trận không hợp lệ. 

Hãy xem xét đầu vào:```
1
11
```Câu trả lời là:```
4
```Các ma trận hợp lệ là:```
11
11
```

```
11
13
```

```
11
17
```

```
11
19
```Hàng thứ hai phải bắt đầu bằng`1`vì tính đối xứng. Bất kỳ số nguyên tố có hai chữ số nào bắt đầu bằng`1`hoạt động. 

Một sai lầm dễ mắc phải khác là quên rằng các số 0 đứng đầu được phép nằm ngoài hàng đầu tiên. 

Đối với đầu vào:```
1
401
```hàng`"023"`là hoàn toàn hợp pháp nếu`23`là nguyên tố. Việc xử lý các hàng dưới dạng chuỗi thập phân có chiều rộng cố định thay vì số nguyên sẽ từ chối các trường hợp đó một cách không chính xác. 

Điểm tinh tế thứ ba là mọi lựa chọn hàng sẽ hạn chế các hàng trong tương lai thông qua tính đối xứng. Giả sử hàng đầu tiên là`"239"`. 

Sau khi chọn hàng thứ hai`"307"`, lực đối xứng:```
2 3 9
3 ? ?
9 ? ?
```Hàng thứ ba phải bắt đầu bằng`"97"`bởi vì các ràng buộc về cột đã xác định những chữ số đó. Nếu chúng ta chỉ kiểm tra tính nguyên tố theo từng hàng mà không thực thi các tiền tố này thì chúng ta sẽ tính các ma trận không hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Giả sử kích thước ma trận là`n`. Chúng tôi sửa hàng đầu tiên từ đầu vào, sau đó thử mọi số nguyên tố có thể có cho hàng thứ hai, mọi số nguyên tố có thể có cho hàng thứ ba, v.v. Sau khi xây dựng ma trận đầy đủ, chúng tôi kiểm tra xem nó có đối xứng hay không. 

Điều này hoạt động hợp lý vì mọi ma trận ứng cử viên đều được kiểm tra trực tiếp. Vấn đề là yếu tố phân nhánh. Có khoảng`10^4`số nguyên tố dưới đây`100000`. Ngay cả việc hạn chế theo độ dài chữ số vẫn để lại hàng nghìn khả năng trên mỗi hàng. Vì`n = 5`, việc tìm kiếm trở nên gần như:```
3000^4 ≈ 8 × 10^13
```điều đó là hoàn toàn không thể. 

Quan sát quan trọng là tính đối xứng tạo ra các ràng buộc tiền tố. 

Khi chúng tôi chọn một số hàng, chúng tôi sẽ tự động xác định các phần của các hàng trong tương lai. Nếu hàng`0..k-1`đã được sửa rồi, sau đó là lần đầu tiên`k`chữ số của hàng`k`bị ép buộc bởi tính đối xứng. 

Ví dụ: sau khi chọn:```
239
307
```hàng thứ ba phải bắt đầu bằng`"97"`bởi vì:```
a[2][0] = a[0][2] = 9
a[2][1] = a[1][2] = 7
```Điều này thay đổi vấn đề hoàn toàn. Thay vì tự do chọn hàng, mỗi hàng phải thuộc tập hợp các số nguyên tố có tiền tố cụ thể. 

Từ`n ≤ 5`, chúng ta có thể xử lý trước tất cả các số nguyên tố được nhóm theo độ dài và tiền tố. Sau đó, việc tìm kiếm sẽ trở thành một DFS nhỏ được cắt tỉa tích cực. 

Ở độ sâu DFS`k`, chúng ta đã biết chính xác hàng tiền tố nào`k`phải có. Chúng tôi chỉ lặp lại các số nguyên tố phù hợp với tiền tố đó. Mọi lựa chọn hợp lệ đều mở rộng ma trận một cách nhất quán. 

Điều này biến việc tìm kiếm hàm mũ trên tất cả các bộ nguyên tố thành một không gian trạng thái nhỏ hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ, đại khái là O(P^(n-1)) | O(n²) | Quá chậm | 
| Tối ưu | O(số trạng thái DFS hợp lệ) | O(P) | Đã chấp nhận | 

Đây`P`là số số nguyên tố có nhiều nhất 5 chữ số. 

## Hướng dẫn thuật toán 

1. Tạo tất cả các số nguyên tố bên dưới`100000`dùng sàng. 

Chúng ta cần mọi số nguyên tố có độ dài`1`bởi vì`5`, vì các hàng sau hàng đầu tiên có thể chứa các số 0 đứng đầu. Ví dụ,`"023"`tương ứng với số nguyên tố`23`. 
2. Chuyển đổi mọi số nguyên tố thành các chuỗi có độ dài không đệm`1`bởi vì`5`. 

Ví dụ, số nguyên tố`23`trở thành:```
"23"   for length 2
"023"  for length 3
"00023" for length 5
```Điều này cho phép chúng ta xử lý các số 0 đứng đầu một cách tự nhiên. 
3. Nhóm các số nguyên tố theo`(length, prefix)`. 

Đối với mỗi chuỗi nguyên tố được đệm, hãy lưu nó vào một từ điển được khóa bằng tiền tố của nó. 

Ví dụ về chiều dài`3`:```
prefix "9"  -> ["907", "911", ...]
prefix "97" -> ["977", ...]
```Sau đó, khi một hàng buộc phải bắt đầu bằng`"97"`, chúng ta có thể truy xuất tất cả các số nguyên tố tương thích ngay lập tức. 
4. Đối với mỗi test, lưu hàng đầu tiên dưới dạng hàng`0`. 

Vì ma trận đối xứng nên cột đầu tiên cũng được cố định tự động. 
5. Chạy DFS từng hàng. 

Giả sử chúng ta đang chọn hàng`k`. 

Bởi vì hàng`0..k-1`đã được cố định, tính đối xứng xác định điểm đầu tiên`k`chữ số của hàng`k`. 

Cụ thể:```
row[k][j] = row[j][k]
```cho mọi`j < k`. 
6. Xây dựng tiền tố cần thiết cho hàng`k`. 

Nếu các hàng hiện tại là:```
239
307
```sau đó chèo`2`phải bắt đầu bằng:```
rows[0][2] + rows[1][2] = "97"
```7. Liệt kê mọi hàng nguyên tố phù hợp với tiền tố đó. 

Truy xuất tất cả các số nguyên tố có độ dài`n`với tiền tố được yêu cầu từ từ điển tiền xử lý. 
8. Nối thêm một hàng ứng cử viên và tiếp tục đệ quy. 

Mỗi hàng được chọn sẽ tự động duy trì tính đối xứng với tất cả các hàng trước đó vì tiền tố bắt buộc của nó đã khớp. 
9. Khi tất cả`n`các hàng được chọn, đếm một ma trận hợp lệ. 

### Tại sao nó hoạt động 

Ở độ sâu DFS`k`, hàng`0..k-1`đã thỏa mãn tính đối xứng giữa chúng. đầu tiên`k`chữ số của hàng`k`được xác định duy nhất bởi điều kiện:```
a[k][j] = a[j][k]
```Việc chọn bất kỳ số nguyên tố nào có chính xác tiền tố đó sẽ duy trì tính đối xứng với tất cả các hàng hiện có. Các hàng trong tương lai sau đó sẽ thực thi các vị trí đối xứng còn lại liên quan đến hàng`k`. 

Không có ma trận không hợp lệ nào được tính vì mọi ràng buộc đối xứng đều được thực thi vào thời điểm nó được xác định. Không có ma trận hợp lệ nào bị bỏ sót vì mọi hàng hợp lệ khớp với tiền tố bắt buộc đều được khám phá. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

LIMIT = 100000

# sieve
is_prime = [True] * LIMIT
is_prime[0] = is_prime[1] = False

for i in range(2, int(LIMIT ** 0.5) + 1):
    if is_prime[i]:
        step = i
        start = i * i
        for j in range(start, LIMIT, step):
            is_prime[j] = False

# prefix[length][prefix] = list of prime strings
prefix = [defaultdict(list) for _ in range(6)]

for x in range(2, LIMIT):
    if not is_prime[x]:
        continue

    s = str(x)

    for length in range(len(s), 6):
        padded = s.zfill(length)

        for k in range(length + 1):
            pref = padded[:k]
            prefix[length][pref].append(padded)

t = int(input())

for _ in range(t):
    first = input().strip()
    n = len(first)

    rows = [first]
    ans = 0

    def dfs(k):
        nonlocal ans

        if k == n:
            ans += 1
            return

        need = []

        for j in range(k):
            need.append(rows[j][k])

        need = ''.join(need)

        for candidate in prefix[n][need]:
            rows.append(candidate)
            dfs(k + 1)
            rows.pop()

    dfs(1)

    print(ans)
```Sàng tạo ra mọi số nguyên tố bên dưới`100000`một lần. Vì độ dài hàng tối đa chỉ là`5`, quá trình tiền xử lý này rất nhỏ. 

Cấu trúc tiền xử lý trung tâm là`prefix[length][pref]`. Nó lưu trữ mọi chuỗi nguyên tố có độ dài cố định được nhóm theo tiền tố của nó. Đây là điều làm cho DFS nhanh. Thay vì kiểm tra mọi số nguyên tố trong quá trình đệ quy, chúng tôi trực tiếp chuyển sang các ứng cử viên tương thích. 

Dòng:```
padded = s.zfill(length)
```là điều cần thiết. Không có phần đệm bằng 0, các hàng như`"023"`sẽ không bao giờ xuất hiện, mặc dù chúng đại diện cho số nguyên tố hợp lệ`23`. 

Trạng thái DFS chỉ lưu trữ các hàng đã được chọn. Khi chọn hàng`k`, tiền tố bắt buộc được xây dựng từ cột`k`của các hàng trước đó:```
rows[j][k]
```Điều này thực thi chính xác tính đối xứng. 

Độ sâu đệ quy tối đa là`5`, vì vậy đệ quy Python hoàn toàn an toàn ở đây. 

Một chi tiết triển khai tinh vi là chúng tôi không bao giờ kiểm tra tính đối xứng một cách rõ ràng ở phần cuối. Quá trình xây dựng đảm bảo điều đó theo từng bước. Mỗi hàng buộc phải khớp với tất cả các hàng trước đó ở các vị trí thích hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
11
```Kích thước ma trận là`2`. 

Trạng thái ban đầu:```
row 0 = "11"
```Bây giờ chúng ta xây dựng hàng`1`. 

Tiền tố bắt buộc đến từ cột`1`của các hàng trước đó. 

| k | Hàng hiện có | Tiền tố bắt buộc | Khớp các hàng Prime | 
| --- | --- | --- | --- | 
| 1 | ["11"] | "1" | 11, 13, 17, 19 | 

Mỗi ứng viên hoàn thành một ma trận đối xứng hợp lệ. 

Tổng số câu trả lời:```
4
```Ví dụ này cho thấy tính đối xứng chuyển đổi thành điều kiện tiền tố như thế nào. Khi hàng đầu tiên được sửa, hàng thứ hai chỉ cần bắt đầu bằng chữ số`'1'`. 

### Ví dụ 2 

đầu vào:```
1
239
```Bắt đầu với:```
row 0 = "239"
```Ở độ sâu`1`, tiền tố bắt buộc là`"3"`. 

| k | Hàng hiện có | Tiền tố bắt buộc | Một ứng cử viên | 
| --- | --- | --- | --- | 
| 1 | ["239"] | "3" | "307" | 

Bây giờ các hàng là:```
239
307
```Ở độ sâu`2`, tiền tố bắt buộc đến từ cột`2`:```
row[0][2] = 9
row[1][2] = 7
```Vì vậy, tiền tố bắt buộc là`"97"`. 

| k | Hàng hiện có | Tiền tố bắt buộc | Một ứng cử viên | 
| --- | --- | --- | --- | 
| 2 | ["239", "307"] | "97" | "977" | 

Ma trận đã hoàn thành:```
239
307
977
```Tất cả các hàng đều là số nguyên tố và ma trận đối xứng. 

Dấu vết này thể hiện sự bất biến cốt lõi. Các hàng trước đó hoàn toàn xác định điểm bắt đầu của mỗi hàng trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(số trạng thái DFS có thể truy cập) | Mỗi nhánh đệ quy tương ứng với một cấu trúc tương thích với tiền tố hợp lệ | 
| Không gian | O(P) | Lưu trữ cho tất cả các nhóm tiền tố và chuỗi nguyên tố được đệm | 

Quá trình tiền xử lý xử lý ít hơn`100000`số nguyên, đó là tầm thường. Độ sâu DFS tối đa là`5`và việc cắt bớt tiền tố sẽ loại bỏ gần như tất cả các nhánh ngay lập tức. Điều này dễ dàng phù hợp trong`3`giới hạn thứ hai. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    LIMIT = 100000

    is_prime = [True] * LIMIT
    is_prime[0] = is_prime[1] = False

    for i in range(2, int(LIMIT ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, LIMIT, i):
                is_prime[j] = False

    prefix = [defaultdict(list) for _ in range(6)]

    for x in range(2, LIMIT):
        if not is_prime[x]:
            continue

        s = str(x)

        for length in range(len(s), 6):
            padded = s.zfill(length)

            for k in range(length + 1):
                prefix[length][padded[:k]].append(padded)

    t = int(input())
    out = []

    for _ in range(t):
        first = input().strip()
        n = len(first)

        rows = [first]
        ans = 0

        def dfs(k):
            nonlocal ans

            if k == n:
                ans += 1
                return

            need = ''.join(rows[j][k] for j in range(k))

            for cand in prefix[n][need]:
                rows.append(cand)
                dfs(k + 1)
                rows.pop()

        dfs(1)
        out.append(str(ans))

    return '\n'.join(out)

# provided samples
assert run("4\n11\n239\n401\n9001\n") == "4\n28\n61\n2834", "sample"

# minimum size
assert run("1\n11\n") == "4", "two-digit smallest case"

# leading zeros inside rows
assert run("1\n401\n") == "61", "must allow rows like 023"

# repeated test cases
assert run("3\n11\n11\n11\n") == "4\n4\n4", "independent processing"

# boundary length 5
out = run("1\n99991\n")
assert out.isdigit(), "five-digit prime should work"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`11`|`4`| Kích thước ma trận nhỏ nhất | 
|`401`|`61`| Các hàng số 0 đứng đầu được xử lý | 
| lặp đi lặp lại`11`|`4`mỗi | Không có lỗi trạng thái chia sẻ giữa các trường hợp thử nghiệm | 
|`99991`| câu trả lời số | Độ dài chữ số tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
401
```Việc triển khai bất cẩn có thể từ chối các hàng như`"023"`bởi vì chúng bắt đầu bằng số 0. 

Giả sử trong DFS chúng ta cần một hàng có tiền tố`"02"`. Bước tiền xử lý bao gồm:```
padded = "023"
```bởi vì nguyên tố`23`được đệm bằng 0 theo chiều dài`3`. 

Thuật toán xử lý chính xác`"023"`như một hàng nguyên tố hợp lệ. 

Một trường hợp quan trọng khác là thực thi tính đối xứng tăng dần. 

đầu vào:```
1
239
```Sau khi chọn:```
239
307
```thuật toán tính toán:```
need = "97"
```cho hàng thứ ba. 

Chỉ các số nguyên tố bắt đầu bằng`"97"`được khám phá. Một hàng như`"911"`không bao giờ được xem xét vì nó sẽ vi phạm:```
a[1][2] = a[2][1]
```DFS không bao giờ xây dựng một ma trận từng phần không hợp lệ. 

Cuối cùng, hãy xem xét kích thước nhỏ nhất:```
1
11
```Quá trình đệ quy bắt đầu lúc`k = 1`. Tiền tố bắt buộc là`"1"`. Mọi số nguyên tố có hai chữ số bắt đầu bằng`1`trở thành hàng thứ hai hợp lệ. Một lần`k == n`, ma trận đã đầy đủ và được tính. 

Thuật toán xử lý việc này một cách tự nhiên mà không cần bất kỳ mã trường hợp đặc biệt nào.
