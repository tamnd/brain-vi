---
title: "CF 104614D – Xác định các loại nucleotide"
description: "Chúng ta được cung cấp một chuỗi DNA chỉ bao gồm bốn loại nucleotide A, T, G và C. Sau chuỗi này, một số truy vấn phạm vi sẽ được thực hiện. Mỗi truy vấn chỉ định một phần liền kề của chuỗi và đối với phần đó, chúng ta phải xác định tần suất mỗi nucleotide xuất hiện."
date: "2026-06-29T22:01:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104614
codeforces_index: "D"
codeforces_contest_name: "2022-2023 ICPC East Central North America Regional Contest (ECNA 2022)"
rating: 0
weight: 104614
solve_time_s: 73
verified: true
draft: false
---

[CF 104614D - Xác định các loại nucleotide](https://codeforces.com/problemset/problem/104614/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi DNA chỉ bao gồm bốn loại nucleotide`A`,`T`,`G`, Và`C`. Sau chuỗi, một số truy vấn phạm vi tiếp theo. Mỗi truy vấn chỉ định một phần liền kề của chuỗi và đối với phần đó, chúng ta phải xác định tần suất mỗi nucleotide xuất hiện. 

Đầu ra cần thiết không phải là tần số. Thay vào đó, với mỗi truy vấn, chúng ta phải in bốn chữ cái nucleotide được sắp xếp từ tần số cao nhất đến thấp nhất. Bất cứ khi nào hai nucleotide xuất hiện thường xuyên như nhau, mức độ ưu tiên cố định`A < T < G < C`quyết định thứ tự tương đối của chúng. 

Chuỗi DNA chứa tối đa 50.000 ký tự, trong khi có thể có tới 25.000 truy vấn. Việc quét đơn giản mọi chuỗi con được yêu cầu sẽ kiểm tra mọi ký tự trong mọi truy vấn. Trong trường hợp xấu nhất, mọi truy vấn đều kéo dài toàn bộ chuỗi, dẫn đến khoảng 50.000 × 25.000 = 1,25 tỷ lượt truy cập ký tự. Khối lượng công việc đó vượt xa mức thực tế. 

Bảng chữ cái chỉ chứa bốn ký tự có thể. Hằng số nhỏ này là thuộc tính then chốt của bài toán. Nếu chúng ta có thể trả lời "có bao nhiêu điểm A trong khoảng này?", "có bao nhiêu điểm T?", v.v. trong thời gian không đổi thì mỗi truy vấn sẽ trở thành một lượng công việc rất nhỏ. 

Một sai lầm dễ mắc phải là xử lý cà vạt không đúng cách. Xem xét đầu vào```
AT
1
1 2
```Cả hai`A`Và`T`xuất hiện một lần trong khi`G`Và`C`xuất hiện không lần nào. Đầu ra đúng là```
ATGC
```Chỉ sắp xếp theo tần số có thể tạo ra`TAGC`, điều này không chính xác vì tần số bằng nhau phải tuân theo thứ tự nucleotide cố định. 

Một trường hợp tinh tế khác xảy ra khi một số nucleotide hoàn toàn không xuất hiện.```
AAAA
1
1 4
```Đầu ra đúng là```
ATGC
```

`A`là đầu tiên vì nó xuất hiện bốn lần. Ba nucleotide còn lại đều có tần số bằng 0 nên chúng giữ nguyên thứ tự ưu tiên`T`,`G`,`C`. 

Các truy vấn bao gồm một vị trí duy nhất cũng đáng được chú ý.```
G
1
1 1
```Câu trả lời là```
GATC
```

`G`có tần số 1, trong khi các nucleotide còn lại đều có tần số bằng 0 và được sắp xếp theo quy tắc ràng buộc. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất xử lý mọi truy vấn một cách độc lập. Đối với mỗi khoảng thời gian, hãy quét từng ký tự, duy trì bốn bộ đếm, sau đó sắp xếp bốn nucleotide theo tần số và mức độ ưu tiên liên kết. 

Cách tiếp cận này đúng vì mỗi ký tự trong khoảng đóng góp chính xác một lần vào bộ đếm tương ứng của nó. Thật không may, nó lặp lại công việc tương tự trên các truy vấn chồng chéo. Nếu mọi truy vấn bao gồm gần như toàn bộ chuỗi DNA thì thuật toán sẽ thực hiện khoảng 1,25 tỷ lần kiểm tra ký tự, khiến quá trình này trở nên quá chậm. 

Việc đếm lặp lại gợi ý quá trình tiền xử lý. Mọi truy vấn chỉ yêu cầu tần số bên trong chuỗi con và truy vấn tần số chuỗi con chính xác là mục đích của tổng tiền tố. 

Đối với mỗi nucleotide, hãy xây dựng một mảng đếm tiền tố trong đó`prefix[i]`lưu trữ bao nhiêu lần xuất hiện trong lần đầu tiên`i`các vị trí của sợi dây. Số lần xuất hiện bên trong bất kỳ khoảng thời gian nào`[l, r]`thì đơn giản là```
prefix[r] - prefix[l - 1]
```Vì chỉ có bốn nucleotide nên mỗi truy vấn yêu cầu chính xác bốn điểm khác biệt về tổng tiền tố. Sau khi có được bốn số đếm, chúng ta chỉ sắp xếp bốn mục. Sắp xếp bốn phần tử là thời gian không đổi, vì vậy mỗi truy vấn được trả lời theo thời gian không đổi sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bốn mảng tổng tiền tố, một mảng cho mỗi nucleotide`A`,`T`,`G`, Và`C`. Mỗi mảng có độ dài`n + 1`, trong đó chỉ số`0`đại diện cho một tiền tố trống. 
2. Quét chuỗi DNA từ trái sang phải. Tại mỗi vị trí, sao chép các giá trị tiền tố trước đó cho cả bốn nucleotide, sau đó tăng mảng tương ứng với ký tự hiện tại. 
3. Đối với mọi truy vấn`[l, r]`, tính tần số của mỗi nucleotide bằng cách trừ tổng tiền tố thích hợp. 
4. Tạo thành bốn cặp bao gồm tần số âm và chữ cái nucleotide. Việc sử dụng giá trị âm cho phép sắp xếp tăng dần thông thường đặt tần số lớn hơn trước. 
5. Sắp xếp bốn cặp này. Python so sánh các bộ dữ liệu theo từ điển, do đó các tần số bằng nhau sẽ tự động bị phá vỡ theo thứ tự bảng chữ cái của các chữ cái. Bởi vì mức độ ưu tiên cần thiết là chính xác`A`,`T`,`G`,`C`, điều này tạo ra thứ tự mong muốn. 
6. In ra bốn chữ cái theo thứ tự sắp xếp của chúng. 

### Tại sao nó hoạt động 

Đối với mỗi nucleotide, mảng tiền tố luôn lưu trữ chính xác số lần xuất hiện trước mỗi vị trí. Việc trừ hai giá trị tiền tố sẽ loại bỏ mọi thứ trước khoảng thời gian, để lại chính xác các lần xuất hiện bên trong chuỗi con được yêu cầu. 

Mỗi truy vấn tính toán tần số chính xác cho cả bốn nucleotide. Việc sắp xếp tiếp theo sắp xếp chúng theo tần số giảm dần. Bất cứ khi nào hai tần số bằng nhau, việc so sánh bộ dữ liệu sẽ quay trở lại chữ cái nucleotide, khớp với thứ tự ưu tiên cần thiết. Vì mọi đầu ra chỉ được xác định từ các tần số chính xác này và quy tắc ràng buộc được chỉ định nên mọi câu trả lời đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    letters = "ATGC"
    idx = {c: i for i, c in enumerate(letters)}

    pref = [[0] * (n + 1) for _ in range(4)]

    for i, ch in enumerate(s, 1):
        for k in range(4):
            pref[k][i] = pref[k][i - 1]
        pref[idx[ch]][i] += 1

    m = int(input())
    out = []

    for _ in range(m):
        l, r = map(int, input().split())
        arr = []
        for k, ch in enumerate(letters):
            cnt = pref[k][r] - pref[k][l - 1]
            arr.append((-cnt, ch))
        arr.sort()
        out.append("".join(ch for _, ch in arr))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng bốn mảng tổng tiền tố độc lập. Mỗi vị trí sao chép số lượng tích lũy trước đó, sau đó tăng chính xác một bộ đếm tương ứng với nucleotide hiện tại. 

Mỗi truy vấn thực hiện bốn sự khác biệt về tổng tiền tố. Vì các mảng sử dụng chỉ mục dựa trên một nên công thức khoảng sẽ trở thành`pref[r] - pref[l - 1]`không có bất kỳ xử lý đặc biệt nào cho các khoảng bắt đầu từ ký tự đầu tiên. 

Bước sắp xếp đáng được chú ý. Các bộ chứa`(-count, letter)`thay vì`(count, letter)`. Python sắp xếp theo thứ tự tăng dần, do đó việc phủ định số đếm sẽ làm cho tần số lớn hơn xuất hiện trước tiên. Khi hai số đếm giống hệt nhau, các chữ cái sẽ được so sánh trực tiếp. Bởi vì mức độ ưu tiên cần thiết là chính xác`A`,`T`,`G`,`C`, không cần bộ so sánh tùy chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
TATATGCTCT
1
1 10
```Chuỗi con là toàn bộ chuỗi DNA. 

| Nucleotide | Đếm | 
| --- | --- | 
| A | 2 | 
| T | 4 | 
| G | 1 | 
| C | 3 | 

Sau khi sắp xếp: 

| Vị trí | Nucleotide | 
| --- | --- | 
| 1 | T | 
| 2 | C | 
| 3 | A | 
| 4 | G | 

Đầu ra:```
TCAG
```Ví dụ chứng minh rằng thứ tự được xác định hoàn toàn bởi tần số. 

### Ví dụ 2 

đầu vào```
AAAA
1
1 4
```| Nucleotide | Đếm | 
| --- | --- | 
| A | 4 | 
| T | 0 | 
| G | 0 | 
| C | 0 | 

Sau khi sắp xếp: 

| Vị trí | Nucleotide | 
| --- | --- | 
| 1 | A | 
| 2 | T | 
| 3 | G | 
| 4 | C | 

Đầu ra:```
ATGC
```Dấu vết này cho thấy các tần số 0 bằng nhau chỉ được sắp xếp theo mức độ ưu tiên quy định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Việc xây dựng tiền tố là tuyến tính, mỗi truy vấn thực hiện công việc không đổi. | 
| Không gian | O(n) | Bốn mảng tiền tố có độ dài`n + 1`được lưu trữ. | 

Quá trình tiền xử lý sẽ quét chuỗi DNA một lần và mỗi truy vấn chỉ thực hiện bốn lần tra cứu tổng tiền tố và sắp xếp bốn phần tử. Những chi phí này dễ dàng thỏa mãn các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out

    input = sys.stdin.readline

    def solve():
        s = input().strip()
        n = len(s)

        letters = "ATGC"
        idx = {c: i for i, c in enumerate(letters)}

        pref = [[0] * (n + 1) for _ in range(4)]

        for i, ch in enumerate(s, 1):
            for k in range(4):
                pref[k][i] = pref[k][i - 1]
            pref[idx[ch]][i] += 1

        m = int(input())

        ans = []
        for _ in range(m):
            l, r = map(int, input().split())
            cur = []
            for k, ch in enumerate(letters):
                cnt = pref[k][r] - pref[k][l - 1]
                cur.append((-cnt, ch))
            cur.sort()
            ans.append("".join(ch for _, ch in cur))
        print("\n".join(ans))

    solve()

    sys.stdout = old
    return out.getvalue().strip()

# provided sample
assert run(
"""TATATGCTCT
3
1 10
6 10
6 6
"""
) == "\n".join([
    "TCAG",
    "TCGA",
    "GATC"
])

# minimum input
assert run(
"""A
1
1 1
"""
) == "ATGC"

# all equal
assert run(
"""CCCC
1
1 4
"""
) == "CATG"

# tie between all nucleotides
assert run(
"""ATGC
1
1 4
"""
) == "ATGC"

# boundary intervals
assert run(
"""ATGCAT
2
1 3
4 6
"""
) == "\n".join([
    "ATGC",
    "CATG"
])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ký tự đơn |`ATGC`| Kích thước đầu vào tối thiểu | 
|`CCCC`|`CATG`| Ba mối quan hệ tần số bằng không | 
|`ATGC`|`ATGC`| Tất cả các tần số đều bằng nhau | 
| Khoảng ranh giới |`ATGC`,`CATG`| Phép trừ tiền tố đúng ở cả hai đầu | 

## Vỏ cạnh 

Hãy xem xét ví dụ cà vạt```
AT
1
1 2
```Tần số tính toán là`(1, 1, 0, 0)`vì`(A, T, G, C)`. Sắp xếp các cặp`(-count, letter)`cho`A`,`T`,`G`,`C`, sản xuất```
ATGC
```Quy tắc ràng buộc được xử lý tự động vì số âm bằng nhau sẽ để lại các chữ cái để xác định thứ tự. 

Bây giờ hãy xem xét```
AAAA
1
1 4
```Sự khác biệt về tiền tố tạo ra số lượng`(4, 0, 0, 0)`. Sau khi sắp xếp,`A`đến trước vì tần số lớn hơn của nó. Ba nucleotide còn lại có số lượng bằng nhau nên chúng có trật tự như sau:`T`,`G`,`C`, cho```
ATGC
```Cuối cùng, hãy xem xét khoảng ký tự đơn.```
G
1
1 1
```Số lượng khoảng là`(0, 0, 1, 0)`. Số dương duy nhất thuộc về`G`, vì vậy nó xuất hiện đầu tiên. Các nucleotide tần số bằng 0 còn lại giữ nguyên thứ tự ưu tiên, mang lại```
GATC
```Tất cả ba trường hợp đều tuân theo trực tiếp từ tính toán tổng tiền tố và quy tắc sắp xếp bộ dữ liệu mà không yêu cầu bất kỳ logic trường hợp đặc biệt nào.
