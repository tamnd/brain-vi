---
title: "CF 302A - Eugeny và mảng"
description: "Chúng ta được cung cấp một mảng chỉ chứa 1 và -1. Đối với mỗi truy vấn [l, r], chúng ta xem xét mảng con từ chỉ mục l đến r."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 302
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 182 (Div. 2)"
rating: 800
weight: 302
solve_time_s: 92
verified: true
draft: false
---

[CF 302A - Eugeny và Mảng](https://codeforces.com/problemset/problem/302/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng chỉ chứa`1`Và`-1`. Đối với mỗi truy vấn`[l, r]`, chúng ta nhìn vào mảng con từ chỉ mục`l`ĐẾN`r`. Chúng ta được phép sắp xếp lại các phần tử bên trong mảng con đó theo bất kỳ thứ tự nào và chúng ta phải quyết định xem liệu tổng của mảng con đó có thể bằng 0 hay không. 

Thứ tự của các số không thực sự quan trọng đối với tổng. Sắp xếp lại chỉ thay đổi vị trí chứ không thay đổi bao nhiêu`1`cát`-1`s tồn tại. Câu hỏi thực sự là liệu đoạn được chọn có chứa cùng số lượng`1`cát`-1`S. 

Nếu một mảng con có độ dài`k`, thì có tổng`0`có nghĩa là chính xác một nửa số phần tử là`1`và nửa còn lại là`-1`. Điều đó ngay lập tức cho chúng ta biết hai điều. Đầu tiên, độ dài đoạn phải chẵn. Thứ hai, số lượng`1`s phải bằng số`-1`S. 

Các ràng buộc đủ lớn để chúng tôi không thể kiểm tra từng phần tử truy vấn. Cả hai`n`Và`m`có thể đạt được`2 * 10^5`. Việc quét đơn giản từng truy vấn sẽ mất`O(length of segment)`thời gian, điều này trở nên đại khái`4 * 10^10`hoạt động trong trường hợp xấu nhất. Điều đó vượt xa những gì phù hợp với giới hạn thời gian 1 giây. 

Một số trường hợp khó xử lý. 

Xét một đoạn có độ dài lẻ:```
3 1
1 -1 1
1 3
```Câu trả lời đúng là:```
0
```Mặc dù số lượng bằng nhau, một đoạn có độ dài lẻ không bao giờ có thể chia đều thành các số bằng nhau.`1`Và`-1`. 

Một trường hợp tinh tế khác là một đoạn có độ dài chẵn nhưng số lượng không cân bằng:```
4 1
1 1 -1 -1
1 2
```Phân đoạn`[1, 2]`chứa hai`1`s và không`-1`S. Sắp xếp lại không có gì thay đổi nên đáp án vẫn là`0`. 

Việc triển khai bất cẩn cũng có thể làm vấn đề trở nên phức tạp hơn khi cố gắng mô phỏng sự sắp xếp lại. Ví dụ:```
4 1
1 -1 1 -1
1 4
```Tổng số tiền đã có rồi`0`, vậy câu trả lời là`1`. Không cần logic sắp xếp lại thực tế. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý mọi truy vấn một cách độc lập. Đối với mỗi phân đoạn`[l, r]`, chúng tôi đếm có bao nhiêu`1`cát`-1`s xuất hiện. Nếu số đếm khớp nhau, chúng tôi in`1`; nếu không chúng tôi sẽ in`0`. 

Điều này hoạt động vì điều kiện để đạt được tổng`0`chính xác là sự bằng nhau giữa số đếm dương và âm. Vấn đề là tốc độ. Trong trường hợp xấu nhất, mỗi truy vấn có thể quét gần như toàn bộ mảng. Với`2 * 10^5`truy vấn và`2 * 10^5`phần tử thì tổng công sẽ trở thành bậc hai. 

Quan sát quan trọng là các giá trị thực tế chỉ`1`Và`-1`. Một đoạn có thể có tổng`0`khi và chỉ khi độ dài của nó là số chẵn và số lượng`1`s bằng số lượng`-1`S. Thay vì mỗi lần đếm cả hai một cách riêng biệt, chúng ta chỉ có thể đếm một danh mục trên toàn cầu. 

Giả sử chúng ta tính toán trước tổng số`1`s trong toàn bộ mảng. Cho phép:```
ones = number of elements equal to 1
minus = number of elements equal to -1
```Đối với một đoạn truy vấn có độ dài`len`:```
len = r - l + 1
```Để làm tổng bằng 0, chúng ta cần:```
len / 2 <= ones
len / 2 <= minus
```Tại sao? Bởi vì phân đoạn phải chứa chính xác`len / 2`bản sao của mỗi giá trị. Vì việc sắp xếp lại không bị hạn chế nên chỉ tính sẵn có mới là quan trọng. 

Điều này làm giảm mọi truy vấn để kiểm tra thời gian liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và đếm xem có bao nhiêu phần tử bằng`1`. 
2. Tính xem có bao nhiêu phần tử bằng nhau`-1`. 
3. Với mỗi truy vấn, hãy tính độ dài đoạn:```
len = r - l + 1
```4. Nếu chiều dài là số lẻ, hãy in`0`. 

Tổng bằng 0 đòi hỏi số lượng bằng nhau`1`Và`-1`, điều này là không thể với số phần tử lẻ. 
5. Ngược lại, hãy tính:```
need = len // 2
```Đây là bao nhiêu`1`cát`-1`s phân đoạn phải chứa. 
6. Nếu cả hai tổng số đều thỏa mãn:```
ones >= need
minus >= need
```in`1`. Nếu không thì in`0`. 

Lý do điều này có tác dụng là vì việc sắp xếp lại sẽ loại bỏ tất cả các ràng buộc về vị trí. Chúng tôi chỉ quan tâm liệu có đủ bản sao của từng giá trị để lấp đầy một phân đoạn có kích thước được yêu cầu hay không. 

### Tại sao nó hoạt động 

Một đoạn có độ dài`len`có tổng`0`chính xác khi nó chứa cùng một số lượng`1`cát`-1`S. Vì mọi phần tử đều là`1`hoặc`-1`, số lượng bằng nhau ngụ ý:```
count(1) = count(-1) = len / 2
```Nếu như`len`thật kỳ lạ, điều này là không thể ngay lập tức. 

Nếu như`len`là số chẵn thì câu hỏi duy nhất còn lại là liệu mảng đó có chứa ít nhất`len / 2`bản sao của mỗi giá trị. Bởi vì việc sắp xếp lại được cho phép tự do nên các vị trí không quan trọng chút nào. Thuật toán kiểm tra chính xác các điều kiện cần và đủ này nên mọi câu trả lời đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    ones = arr.count(1)
    minus = n - ones

    ans = []

    for _ in range(m):
        l, r = map(int, input().split())

        length = r - l + 1

        if length % 2 == 1:
            ans.append("0")
            continue

        need = length // 2

        if ones >= need and minus >= need:
            ans.append("1")
        else:
            ans.append("0")

    print("\n".join(ans))

solve()
```Phần đầu đếm xem có bao nhiêu`1`s tồn tại trên toàn cầu. Vì mọi giá trị khác phải là`-1`, số lượng âm chỉ đơn giản là`n - ones`. 

Mỗi truy vấn chỉ phụ thuộc vào độ dài phân đoạn. Các vị trí thực tế`l`Và`r`không liên quan ngoài việc xác định độ dài đó. Đó là sự đơn giản hóa trung tâm của vấn đề. 

Việc kiểm tra độ dài lẻ được thực hiện trước tiên vì nó ngay lập tức loại bỏ các trường hợp không thể thực hiện được. Sau đó,`need = length // 2`đại diện cho số lượng yêu cầu của mỗi giá trị. 

Một lỗi phổ biến là cố gắng phân tích chính xác nội dung của mảng con. Câu lệnh cho phép sắp xếp lại bất kỳ phần tử nào trong phạm vi đã chọn, do đó chỉ có số lượng mới quan trọng. 

Một lỗi dễ mắc phải khác là tính toán độ dài không chính xác. Vì cả hai điểm cuối đều bao gồm nên công thức đúng là:```
r - l + 1
```## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 -1
1 1
1 2
2 2
```Số lượng toàn cầu:```
ones = 1
minus = 1
```| Truy vấn | Chiều dài | Số lẻ? | Cần | 1 giây đủ chưa? | Đủ -1s? | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| [1, 1] | 1 | Có | - | - | - | 0 | 
| [1, 2] | 2 | Không | 1 | Có | Có | 1 | 
| [2, 2] | 1 | Có | - | - | - | 0 | 

Truy vấn thứ nhất và thứ ba không thành công ngay lập tức vì các đoạn có độ dài lẻ không thể chia đều thành các số lượng bằng nhau. Truy vấn ở giữa thành công vì mảng có một`1`và một`-1`. 

### Ví dụ 2 

đầu vào:```
6 4
1 1 1 -1 -1 -1
1 2
1 4
2 5
1 6
```Số lượng toàn cầu:```
ones = 3
minus = 3
```| Truy vấn | Chiều dài | Số lẻ? | Cần | 1 giây đủ chưa? | Đủ -1s? | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| [1, 2] | 2 | Không | 1 | Có | Có | 1 | 
| [1, 4] | 4 | Không | 2 | Có | Có | 1 | 
| [2, 5] | 4 | Không | 2 | Có | Có | 1 | 
| [1, 6] | 6 | Không | 3 | Có | Có | 1 | 

Dấu vết này chứng tỏ rằng nội dung khoảng thời gian chính xác không quan trọng. Mọi truy vấn có độ dài chẵn đều thành công vì mảng trên toàn cầu chứa đủ bản sao của cả hai giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Đếm các giá trị lấy O(n), mỗi truy vấn là O(1) | 
| Không gian | O(1) | Chỉ có một số bộ đếm và biến được lưu trữ | 

Giải pháp dễ dàng phù hợp với giới hạn. Với nhiều nhất`2 * 10^5`truy vấn, công việc liên tục cho mỗi truy vấn là cần thiết. Thuật toán chỉ thực hiện các phép tính và so sánh đơn giản sau bước đếm đầu tiên. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    ones = arr.count(1)
    minus = n - ones

    ans = []

    for _ in range(m):
        l, r = map(int, input().split())

        length = r - l + 1

        if length % 2 == 1:
            ans.append("0")
            continue

        need = length // 2

        if ones >= need and minus >= need:
            ans.append("1")
        else:
            ans.append("0")

    print("\n".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""2 3
1 -1
1 1
1 2
2 2
"""
) == "0\n1\n0\n", "sample 1"

# minimum size
assert run(
"""1 1
1
1 1
"""
) == "0\n", "single odd segment"

# all equal values
assert run(
"""4 2
1 1 1 1
1 2
1 4
"""
) == "0\n0\n", "no -1 values available"

# balanced array
assert run(
"""6 3
1 1 1 -1 -1 -1
1 2
1 4
1 6
"""
) == "1\n1\n1\n", "all even lengths possible"

# off-by-one boundary checks
assert run(
"""5 3
1 -1 1 -1 1
1 4
2 5
1 5
"""
) == "1\n1\n0\n", "inclusive range lengths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn |`0`| Không thể phân đoạn có độ dài lẻ | 
| Tất cả các phần tử đều bằng nhau | Tất cả`0`| Cần cả hai`1`Và`-1`có sẵn | 
| Mảng cân bằng hoàn toàn | Tất cả`1`| Độ dài thậm chí còn thành công khi số lượng đủ | 
| Kiểm tra độ dài ranh giới | Câu trả lời hỗn hợp | Xác nhận đúng`r - l + 1`xử lý | 

## Vỏ cạnh 

Hãy xem xét trường hợp độ dài lẻ:```
3 1
1 -1 1
1 3
```Thuật toán tính toán:```
length = 3
```Vì độ dài là số lẻ nên nó in ngay`0`. Tổng số bằng 0 sẽ yêu cầu`1.5`bản sao của từng giá trị, điều này là không thể. 

Bây giờ hãy xem xét một phân đoạn chẵn không có đủ số âm:```
4 1
1 1 1 -1
1 4
```Thuật toán tính toán:```
length = 4
need = 2
ones = 3
minus = 1
```Có đủ`1`s nhưng không đủ`-1`là. Điều kiện không thành công nên đầu ra là:```
0
```Điều này đúng vì một đoạn có tổng bằng 0 có độ dài`4`phải chứa chính xác hai trong số mỗi giá trị. 

Cuối cùng, hãy xem xét một trường hợp cân bằng:```
6 1
1 1 -1 -1 1 -1
2 5
```Độ dài đoạn là`4`, Vì thế:```
need = 2
```Số lượng toàn cầu là:```
ones = 3
minus = 3
```Cả hai điều kiện đều thành công nên thuật toán in ra`1`. 

Mặc dù bản thân khoảng thời gian được truy vấn có thể chưa có tổng bằng 0, việc sắp xếp lại cho phép chúng ta đặt hai`1`s và hai`-1`s bên trong phân khúc, đạt được số tiền cần thiết.
