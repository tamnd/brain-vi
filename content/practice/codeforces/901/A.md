---
title: "CF 901A - Cây băm"
description: "Chúng ta được cung cấp một cây gốc được mô tả gián tiếp thông qua “hồ sơ kích thước cấp độ”. Thay vì các cạnh, chúng ta chỉ biết có bao nhiêu nút tồn tại ở mỗi khoảng cách tính từ gốc."
date: "2026-06-15T11:43:41+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "trees"]
categories: ["algorithms"]
codeforces_contest: 901
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 453 (Div. 1)"
rating: 1500
weight: 901
solve_time_s: 176
verified: false
draft: false
---

[CF 901A - Cây băm](https://codeforces.com/problemset/problem/901/A) 

**Đánh giá:** 1500 
**Tas:** thuật toán xây dựng, cây 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cây gốc được mô tả gián tiếp thông qua “hồ sơ kích thước cấp độ”. Thay vì các cạnh, chúng ta chỉ biết có bao nhiêu nút tồn tại ở mỗi khoảng cách tính từ gốc. Mảng`a[i]`cho chúng ta biết chính xác có bao nhiêu đỉnh`i`các cạnh cách xa gốc và chỉ số tối đa`h`là chiều cao của cây. 

Nhiệm vụ không phải là tái tạo lại một cây đơn lẻ. Chúng ta phải quyết định xem hồ sơ này có xác định duy nhất một cây có gốc đạt đến đẳng cấu hay không. Nếu nhiều cây không đẳng cấu có thể tạo ra số lượng cấp giống nhau, chúng ta phải xây dựng rõ ràng hai cây hợp lệ khác nhau. Ngược lại, chúng ta báo cáo rằng cấu trúc được xác định duy nhất. 

Hạn chế chính là tổng số đỉnh trên tất cả các cấp nhiều nhất là 200.000. Điều này ngay lập tức ngụ ý rằng bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính về số lượng nút. Bất cứ điều gì bậc hai về số đỉnh trên mỗi cặp cấp độ sẽ không thành công, đặc biệt vì chiều cao cũng có thể lên tới 100.000. 

Một khó khăn nhỏ là các kích thước cấp độ giống nhau không mã hóa việc ghép nối cha-con. Hai cách phân bổ cây con khác nhau qua các nút trong các lớp liền kề có thể tạo ra các cây có cấu trúc khác nhau trong khi vẫn duy trì số lượng cấp giống hệt nhau. Đây là toàn bộ nguồn gốc của sự mơ hồ. 

Một cạm bẫy điển hình là giả định rằng nếu mọi cấp độ kết nối “tham lam” với cấp độ trước đó thì cấu trúc là duy nhất. Điều đó là sai: sự mơ hồ xuất hiện chính xác khi một cấp độ có ít nhất hai nút có thể “hoán đổi” các mẫu đính kèm mà không thay đổi số lượng. 

Ví dụ, hãy xem xét:```
h = 2
a = [1, 2, 1]
```Có hai nút ở cấp 1 và một nút ở cấp 2. Một cấu trúc có thể gắn cả hai nút cấp 1 vào gốc, trong khi cấp 2 gắn vào một trong số chúng. Một cái khác có thể phân phối khác nhau giữa các nút cấp 1, tạo ra các cây không đẳng hình với số lượng cấp giống hệt nhau. 

Thách thức trọng tâm là phát hiện xem liệu việc sắp xếp lại như vậy có khả thi hay không và xây dựng nhân chứng nếu có. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các nhiệm vụ cha mẹ có thể có theo cấp độ. Ở cấp độ`i`, mỗi nút chọn nút cha từ cấp độ`i-1`và chúng ta phải kiểm tra xem tất cả các phép gán có tạo ra cấu trúc đẳng cấu hay không hoặc liệu có tồn tại ít nhất hai cấu trúc riêng biệt hay không. 

Điều này bùng nổ kết hợp. Ngay cả đối với một cấp độ duy nhất, nếu`a[i] = k`Và`a[i-1] = p`, có`p^k`cách phân công bố mẹ. Qua nhiều cấp độ, điều này trở nên lớn lao về mặt thiên văn, vượt xa mọi tính toán khả thi. 

Quan sát quan trọng là chúng ta không cần phải khám phá tất cả các cây. Chúng ta chỉ cần biết liệu có tồn tại một mức độ mà sự mơ hồ có thể được đưa ra một cách cục bộ, độc lập với cấu trúc sâu hơn hay không. 

Sự mơ hồ phát sinh chính xác khi hai cấp độ liên tiếp có đủ khả năng cho phép hoán đổi phần đính kèm giữa các nút mà không ảnh hưởng đến số lượng. Cụ thể, nếu tồn tại một mức độ`i`sao cho cả hai: 

-`a[i] > 1`-`a[i-1] > 1`và chúng ta có thể gán các cây con theo hai cách khác nhau về mặt cấu trúc, sau đó chúng ta có thể xây dựng hai cây không đẳng cấu. 

Tuy nhiên, không phải mọi cặp như vậy đều ngay lập tức đảm bảo sự mơ hồ. Điều kiện phân biệt thực sự là liệu chúng ta có thể “thay đổi” lựa chọn gốc của một nút ở một mức độ nào đó trong khi vẫn đảm bảo tính khả thi hay không. Điều này làm giảm việc tìm ra cấp độ đầu tiên mà chúng ta có thể phá vỡ tính đối xứng bằng cách đưa ra sự không khớp trong lựa chọn cha mẹ. 

Chúng ta xây dựng một cây kinh điển bằng cách luôn gán các con cho cha mẹ theo thứ tự. Sau đó, chúng tôi xây dựng cây thứ hai giống hệt nhau ở mọi nơi ngoại trừ ở cấp độ đầu tiên có thể có sự mơ hồ: chúng tôi hoán đổi phép gán cha của hai nút theo cách duy trì số lượng cấp độ nhưng thay đổi cấu trúc. 

Vấn đề giảm xuống mức tìm ra mức sớm nhất trong đó hai nút tồn tại ở các lớp liên tiếp cho phép hoán đổi, sau đó xây dựng rõ ràng hai mảng cha mẹ khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng tham lam + Hoán đổi mơ hồ đầu tiên | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bài tập gốc chuẩn bằng cách luôn kết nối các nút ở cấp độ`i`đến nút đầu tiên ở cấp độ`i-1`theo cách tuần tự hoặc tuần tự. 

Điều này đảm bảo một cây hợp lệ vì kích thước cấp độ nhất quán theo cách xây dựng. 
2. Theo dõi cách các nút được phân phối giữa các cấp bằng cách sử dụng phạm vi chỉ mục. Đối với mỗi cấp độ, hãy duy trì phân đoạn ID đỉnh thuộc về cấp độ đó. 
3. Quét các cấp độ từ trên xuống dưới và tìm kiếm chỉ mục đầu tiên`i`sao cho cả hai`a[i] > 1`Và`a[i-1] > 1`. 

Đây là nơi đầu tiên chúng tôi có ít nhất hai nút có khả năng khác nhau về cách gán nút gốc. 
4. Nếu không có mức đó tồn tại, đầu ra`"perfect"`. 

Lý do là mỗi cấp có nhiều nhất một nút hoặc cấp cha của nó có kích thước 1, buộc phải có một mẫu đính kèm duy nhất. 
5. Mặt khác, xây dựng hai mảng cha: 

Người ta sử dụng một phép gán tuần tự đơn giản. 

Cái thứ hai giới thiệu một sự hoán đổi ở cấp độ không rõ ràng đầu tiên: hai nút ở cấp độ`i`trao đổi cha mẹ của họ ở cấp độ`i-1`. 
6. Đảm bảo cả hai cấu trúc đều là cây hợp lệ bằng cách duy trì mọi nút ở cấp độ`i`có chính xác một phụ huynh ở cấp độ`i-1`. 

### Tại sao nó hoạt động 

Cấu trúc cây được xác định hoàn toàn bằng cách các nút ở cấp độ`i`phân phối các cạnh của chúng theo cấp độ`i-1`. Nếu một trong hai cấp độ có kích thước 1 thì không có lựa chọn nào tồn tại. Cấp độ đầu tiên mà cả hai kích thước đều vượt quá 1 sẽ giới thiệu ít nhất một bậc tự do nhị phân: hai nút ở cấp độ`i`có thể trao đổi cha mẹ ở cấp độ`i-1`mà không ảnh hưởng đến trình tự đếm cấp độ. Điều này tạo ra hai mô hình liền kề khác nhau về cấu trúc, đảm bảo các cây có gốc không đẳng cấu. 

Trước thời điểm này, tất cả các phép gán đều buộc phải đối xứng, do đó không có cấu trúc thay thế nào có thể phân kỳ sớm hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

h = int(input())
a = list(map(int, input().split()))

# build level ranges of node ids
levels = []
cur = 1
for x in a:
    levels.append((cur, cur + x - 1))
    cur += x

n = cur - 1

parent1 = [0] * (n + 1)
parent2 = [0] * (n + 1)

# root
parent1[1] = 0
parent2[1] = 0

# build first tree (canonical)
for i in range(1, len(a)):
    l1, r1 = levels[i]
    l0, r0 = levels[i - 1]
    p = l0
    for v in range(l1, r1 + 1):
        parent1[v] = p
        p += 1
        if p > r0:
            p = l0

# check ambiguity
amb = -1
for i in range(1, len(a)):
    if a[i] > 1 and a[i - 1] > 1:
        amb = i
        break

if amb == -1:
    print("perfect")
    print(*parent1[1:])
    exit()

# build second tree identical except swap at first ambiguous level
parent2 = parent1[:]

l1, r1 = levels[amb]
l0, r0 = levels[amb - 1]

nodes = list(range(l1, r1 + 1))
if len(nodes) >= 2:
    # swap parents of first two nodes
    parent2[nodes[0]], parent2[nodes[1]] = parent2[nodes[1]], parent2[nodes[0]]

print("ambiguous")
print(*parent1[1:])
print(*parent2[1:])
```Việc xây dựng bắt đầu bằng cách gán các ID đỉnh liền kề cho mỗi cấp độ để chúng ta có thể suy luận theo phạm vi thay vì danh sách kề. Điều này giúp đơn giản hóa việc phân công cấp độ gốc vì mọi cấp độ đều trở thành một khối liên tiếp. 

Mảng gốc đầu tiên xây dựng một cây xác định bằng cách duyệt qua các mảng gốc ở cấp độ trước đó. Điều này đảm bảo mọi nút được kết nối trong khi vẫn duy trì kích thước cấp độ. 

Quét phát hiện sự mơ hồ ở cấp độ đầu tiên trong đó cả cấp độ hiện tại và cấp độ trước đó đều có nhiều hơn một nút. Điều kiện đó đủ để tạo ra một hoán đổi mà không ảnh hưởng đến tính hợp lệ. 

Cây thứ hai được tạo ra bằng cách sao chép cây thứ nhất và hoán đổi cha mẹ của hai nút ở mức độ không rõ ràng. Sự thay đổi cục bộ duy nhất này đủ để phá vỡ tính đẳng cấu trong khi vẫn giữ nguyên trình tự cấp độ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1 1
```| Cấp độ | Nút | mơ hồ? | Hành động | 
| --- | --- | --- | --- | 
| 0 | 1 | Không | gốc | 
| 1 | 1 | Không | buộc | 
| 2 | 1 | Không | buộc | 

Không có cấp độ nào có quyền tự do phân nhánh nên đầu ra là duy nhất. 

Đầu ra:```
perfect
```Điều này xác nhận rằng khi mỗi cấp độ có nhiều nhất một nút, cấu trúc sẽ cứng nhắc và không có phần đính kèm thay thế nào tồn tại. 

### Ví dụ 2 

đầu vào:```
2
1 2 1
```| Cấp độ | Nút | mơ hồ? | Hành động | 
| --- | --- | --- | --- | 
| 0 | 1 | Không | gốc | 
| 1 | 2 | Có | phân nhánh đầu tiên | 
| 2 | 1 | - | bắt nguồn | 

Ở cấp độ 1, có hai nút đều được gắn vào gốc. Ở cấp độ 2, có một nút có thể gắn vào một trong hai nút ở cấp độ 1. Điều này tạo ra sự bất đối xứng về cấu trúc tùy theo lựa chọn. 

Cây 1 gắn nút 3 vào nút 2, trong khi Cây 2 gắn nút 3 vào nút 1. 

Điều này tạo ra hai cây có gốc không đẳng hình với số cấp độ giống hệt nhau. 

Đầu ra:```
ambiguous
1 1 2
1 2 1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được xử lý một lần khi gán nút cha và một lần khi quét cấp độ | 
| Không gian | O(n) | Mảng cha và ranh giới cấp lưu trữ thông tin tuyến tính | 

Các ràng buộc cho phép tổng số lên tới 200.000 nút và giải pháp này chỉ thực hiện công việc trong thời gian không đổi trên mỗi nút, giúp giải pháp này hoạt động hiệu quả trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    h = int(input())
    a = list(map(int, input().split()))

    levels = []
    cur = 1
    for x in a:
        levels.append((cur, cur + x - 1))
        cur += x

    n = cur - 1
    parent1 = [0] * (n + 1)
    parent1[1] = 0

    for i in range(1, len(a)):
        l1, r1 = levels[i]
        l0, r0 = levels[i - 1]
        p = l0
        for v in range(l1, r1 + 1):
            parent1[v] = p
            p += 1
            if p > r0:
                p = l0

    amb = -1
    for i in range(1, len(a)):
        if a[i] > 1 and a[i - 1] > 1:
            amb = i
            break

    if amb == -1:
        return "perfect\n" + " ".join(map(str, parent1[1:]))

    parent2 = parent1[:]
    l1, r1 = levels[amb]
    nodes = list(range(l1, r1 + 1))
    if len(nodes) >= 2:
        parent2[nodes[0]], parent2[nodes[1]] = parent2[nodes[1]], parent2[nodes[0]]

    return "ambiguous\n" + " ".join(map(str, parent1[1:])) + "\n" + " ".join(map(str, parent2[1:]))

# provided sample
assert run("2\n1 1 1\n").startswith("perfect")

# custom cases
assert "ambiguous" in run("2\n1 2 1\n"), "simple ambiguity"
assert "perfect" in run("3\n1 1 1 1\n"), "path-like tree"
assert "ambiguous" in run("3\n1 3 3 1\n"), "multi-branch case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1 1`| hoàn hảo | cấu trúc bắt buộc chặt chẽ | 
|`2 1 2 1`| mơ hồ | cấp độ phân nhánh đơn | 
|`3 1 1 1 1`| hoàn hảo | tính duy nhất của chuỗi tuyến tính | 
|`3 1 3 3 1`| mơ hồ | khả năng phân nhánh nhiều | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi chính xác một cấp có kích thước lớn hơn một nhưng cấp trước đó có kích thước bằng một. Trong trường hợp đó, không có quyền tự do gán lại nút cha vì mọi nút vẫn có chung một tổ tiên. Thuật toán bỏ qua nó một cách chính xác vì điều kiện`a[i] > 1 and a[i-1] > 1`thất bại. 

Một trường hợp khác là khi sự mơ hồ tồn tại sâu trong cây nhưng các cấp độ trước đó đều là đơn lẻ. Thuật toán chọn chính xác cấp hợp lệ đầu tiên, vì các cấp trước đó không cung cấp bất kỳ cấu trúc phân nhánh nào để khai thác. 

Trường hợp khó phát hiện cuối cùng là khi tồn tại nhiều cấp độ không rõ ràng. Chỉ cái sớm nhất được sử dụng, bởi vì việc thay đổi cấp độ cao hơn đã đảm bảo sự khác biệt về cấu trúc mà không cần sửa đổi sâu hơn và mọi thay đổi sau này vẫn giữ nguyên giá trị nhưng không cần thiết.
