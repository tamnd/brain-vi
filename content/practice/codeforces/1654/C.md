---
title: "CF 1654C - Alice và chiếc bánh"
description: "Chúng ta được cho biết trọng lượng cuối cùng của n miếng bánh. Những miếng này được tạo ra từ một chiếc bánh ban đầu bằng cách liên tục chọn một miếng có trọng lượng w và chia nó thành hai phần: - sàn(w / 2) - trần(w / 2) Chính xác là n - 1 lần cắt như vậy được thực hiện, thu được n miếng cuối cùng."
date: "2026-06-10T03:40:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1654
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 778 (Div. 1 + Div. 2, based on Technocup 2022 Final Round)"
rating: 1400
weight: 1654
solve_time_s: 117
verified: true
draft: false
---

[CF 1654C - Alice và chiếc bánh](https://codeforces.com/problemset/problem/1654/C) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp trọng lượng cuối cùng của`n`miếng bánh. Những miếng này được tạo ra từ một chiếc bánh ban đầu bằng cách liên tục chọn một miếng có trọng lượng`w`và chia nó thành hai phần: 

-`floor(w / 2)`-`ceil(w / 2)`Chính xác`n - 1`việc cắt giảm như vậy đã được thực hiện, dẫn đến`n`mảnh cuối cùng. Thứ tự của các phần cuối cùng không quan trọng. 

Nhiệm vụ là xác định xem mảng đã cho có thể là kết quả của một số chuỗi phân chia hợp lệ hay không. 

Một quan sát hữu ích là trọng lượng bánh ban đầu không được đưa ra một cách rõ ràng mà bị ép buộc. Mỗi lần chia đều bảo toàn tổng trọng lượng, do đó trọng lượng ban đầu phải bằng tổng của tất cả các phần cuối cùng. 

Những hạn chế là lớn. Trên tất cả các trường hợp thử nghiệm, tổng số phần nhiều nhất là`2 · 10^5`. Bất kỳ thuật toán nào cố gắng mô phỏng tất cả các lệnh cắt có thể sẽ bùng nổ theo cấp số nhân. Thậm chí một`O(n²)`cách tiếp cận này sẽ quá tốn kém trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần gũi`O(n log n)`. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1
3
2 3 3
```Tổng số tiền là`8`. Bắt đầu từ`8`, phép chia đầu tiên duy nhất có thể là`4 + 4`. Tiếp tục quá trình không bao giờ tạo ra chính xác`{2,3,3}`. Câu trả lời đúng là`NO`. 

Một trường hợp quan trọng khác là:```
1
6
1 1 1 1 1 1
```Câu trả lời là`YES`. Mặc dù mọi phần cuối cùng đều`1`, chúng ta có thể liên tục chia các phần lớn hơn cho đến khi có sáu phần xuất hiện. Một giải pháp dừng lại bất cứ khi nào nó thấy nhiều giá trị giống nhau có thể từ chối trường hợp này một cách không chính xác. 

Một ví dụ tinh tế thứ ba là:```
1
2
869 541
```Tổng số tiền là`1410`. Lần phân chia đầu tiên phải là`705 + 705`. Không bên nào ngang bằng`869`hoặc`541`và việc chia nhỏ hơn nữa chỉ làm cho các phần nhỏ hơn. Câu trả lời đúng là`NO`. 

Thách thức chính là xác định xem liệu một chuỗi phân chia hợp pháp có thể tạo ra chính xác nhiều trọng số mục tiêu hay không. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ bắt đầu từ tổng tổng và thử đệ quy mọi chuỗi phân chia có thể có, kiểm tra xem một trong các tập hợp lá kết quả có bằng với nhiều tập hợp mục tiêu hay không. 

Điều này đúng vì nó khám phá mọi công trình hợp lệ theo đúng nghĩa đen. Thật không may, số lượng cây chia tách có thể tăng theo cấp số nhân. Ngay cả đối với vài chục mảnh, việc tìm kiếm trở nên không khả thi, trong khi vấn đề này cho phép lên tới`2 · 10^5`miếng. 

Cấu trúc của hoạt động gợi ý nên xem xét quá trình ngược lại. 

Thay vì hỏi liệu các miếng mục tiêu có thể được tạo ra từ chiếc bánh ban đầu hay không, giả sử chúng ta bắt đầu từ trọng lượng chiếc bánh ban đầu, đơn giản là tổng của tất cả các giá trị mục tiêu. Bất cứ khi nào một mảnh khớp với một trong các giá trị mong muốn, chúng ta có thể "tiêu thụ" nó và ngừng chia nhánh đó. 

Nếu một phần không khớp với bất kỳ giá trị mục tiêu còn lại nào, điều duy nhất có thể xảy ra trong quá trình chuyển tiếp là phần này sẽ được chia thêm. Vì việc phân chia đã được xác định hoàn toàn nên chúng tôi biết chính xác cách tiếp tục:```
w -> floor(w/2), ceil(w/2)
```Điều này dẫn đến một chiến lược tham lam. Luôn lấy mảnh lớn nhất hiện có. Nếu nó khớp với một giá trị bắt buộc, hãy loại bỏ yêu cầu đó. Nếu không thì chia nó thành hai đứa con của nó. 

Tại sao việc lấy phần lớn nhất lại có ý nghĩa? Bất kỳ giá trị mục tiêu nào lớn hơn phần lớn nhất hiện có sẽ không bao giờ được tạo ra sau này, vì việc chia tách chỉ làm giảm trọng lượng. Việc khớp các giá trị lớn càng sớm càng tốt sẽ tránh lãng phí chúng do sự phân chia không cần thiết. 

Chúng ta có thể lưu trữ cả hai tập hợp theo thứ tự giảm dần. Một multiset chứa các giá trị đích. Phần còn lại chứa các phần hiện có sẵn trong khi đảo ngược quá trình. 

Bất cứ khi nào phần lớn nhất có sẵn bằng giá trị yêu cầu lớn nhất, chúng tôi sẽ khớp chúng. Nếu không, chúng tôi chia phần có sẵn. 

Một quy tắc cắt tỉa hữu ích xuất hiện một cách tự nhiên. Nếu phần lớn nhất hiện có trở nên nhỏ hơn mục tiêu lớn nhất được yêu cầu thì không thể thành công vì việc chia tách trong tương lai chỉ làm giảm giá trị hơn nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của mảng mục tiêu. Đây là trọng lượng bánh ban đầu duy nhất có thể. 
2. Lưu trữ tất cả các giá trị đích trong cấu trúc tối đa. Trong Python, vùng heap tối đa có thể được mô phỏng bằng các giá trị âm. 
3. Tạo một vùng heap tối đa khác chỉ chứa tổng số tiền. 
4. Lặp lại trong khi cả hai đống đều không trống. 
5. Hãy để`x`là phần lớn nhất hiện có và`y`là giá trị mục tiêu chưa từng có lớn nhất. 
6. Nếu`x == y`, loại bỏ cả hai. Điều này có nghĩa là chúng tôi đã xác định thành công một phần cuối cùng của nhiều mục tiêu. 
7. Nếu`x < y`, quay lại ngay`NO`. Phần lớn nhất hiện có đã quá nhỏ và các phần chia tách trong tương lai chỉ có thể làm cho các phần thậm chí còn nhỏ hơn. 
8. Nếu`x > y`, tách ra`x`vào trong:```
floor(x/2)
ceil(x/2)
```và chèn cả hai mảnh trở lại đống mảnh có sẵn. 
9. Nếu`x == 1`Và`x > y`, trở lại`NO`. Một phần trọng lượng`1`không thể chia nhỏ hơn nữa. 
10. Sau khi quá trình kết thúc, hãy quay lại`YES`nếu tất cả các giá trị mục tiêu đều khớp, nếu không thì trả về`NO`. 

### Tại sao nó hoạt động 

Điều bất biến là đống các mảnh có sẵn luôn đại diện cho một tập hợp các mảnh có thể tồn tại ở giai đoạn trung gian nào đó của quá trình cắt hợp lệ bắt đầu từ tổng. 

Bất cứ khi nào phần lớn nhất có sẵn bằng giá trị mục tiêu lớn nhất còn lại, phần đó cuối cùng phải xuất hiện không thay đổi trong bất kỳ công trình xây dựng thành công nào. Giữ nó và ngừng phân chia thêm trên nhánh đó không thể làm tổn hại đến một giải pháp hợp lệ. 

Khi phần lớn nhất có sẵn lớn hơn giá trị bắt buộc lớn nhất, phần đó không thể tương ứng trực tiếp với bất kỳ giá trị mục tiêu còn lại nào, do đó nó phải được tách ra trong mọi cấu trúc hợp lệ. Việc phân chia được xác định duy nhất, không để lại lựa chọn nào khác. 

Khi mảnh lớn nhất hiện có trở nên nhỏ hơn mục tiêu lớn nhất được yêu cầu, không hoạt động nào trong tương lai có thể tăng trọng lượng của nó. Việc đạt được giá trị yêu cầu là không thể, vì vậy việc từ chối là đúng. 

Những quan sát này buộc mỗi bước của quy trình tham lam, có nghĩa là thuật toán chấp nhận chính xác những tập hợp có thể được tạo ra bằng cách cắt hợp lệ. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        need = [-x for x in a]
        heapq.heapify(need)

        pieces = [-sum(a)]

        possible = True

        while need:
            if not pieces:
                possible = False
                break

            x = -heapq.heappop(pieces)
            y = -need[0]

            if x == y:
                heapq.heappop(need)
                continue

            if x < y:
                possible = False
                break

            if x == 1:
                possible = False
                break

            left = x // 2
            right = x - left

            heapq.heappush(pieces, -left)
            heapq.heappush(pieces, -right)

            if len(pieces) > len(need):
                possible = False
                break

        answers.append("YES" if possible else "NO")

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Heap đầu tiên lưu trữ tất cả các giá trị đích vẫn cần được khớp. Đống thứ hai lưu trữ các phần hiện có sẵn trong khi đảo ngược quá trình xây dựng. 

Thuật toán liên tục so sánh phần tử lớn nhất của mỗi vùng. Nếu chúng bằng nhau thì chúng ta đã tìm được mảnh cuối cùng và loại nó khỏi phần xem xét. 

Khi phần có sẵn lớn hơn, chúng tôi thực hiện hành động ngược lại về mặt pháp lý duy nhất, chia nó thành hai phần mà lẽ ra có thể tạo ra nó trong quá trình chuyển tiếp. 

điều kiện`x == 1`là quan trọng. Cân nặng`1`không thể phân chia thêm nên nếu vẫn không khớp với giá trị yêu cầu thì instance là không thể. 

Séc```
if len(pieces) > len(need):
```là một tối ưu hóa hữu ích được sử dụng trong nhiều giải pháp được chấp nhận. Một khi chúng ta có sẵn nhiều mảnh ghép hơn những mảnh mục tiêu còn lại, ngay cả sự kết hợp hoàn hảo cũng không thể ghép các mảnh lại với nhau, vì vậy thành công là không thể. 

Số nguyên Python dễ dàng xử lý tất cả các khoản tiền vì tổng trọng lượng tối đa nhiều nhất là:```
2 · 10^5 × 10^9 = 2 · 10^14
```phù hợp thoải mái với các số nguyên có độ chính xác tùy ý của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
2 3 1
```Đống mục tiêu chứa`{3,2,1}`. 

| Lớn nhất hiện có | Cần thiết lớn nhất | Hành động | 
| --- | --- | --- | 
| 6 | 3 | Chia thành 3,3 | 
| 3 | 3 | Trận đấu | 
| 3 | 2 | Chia thành 1,2 | 
| 2 | 2 | Trận đấu | 
| 1 | 1 | Trận đấu | 

Tất cả các mục tiêu đều phù hợp. 

Đầu ra:```
YES
```Ví dụ này cho thấy việc kết hợp một mục tiêu lớn sẽ ngay lập tức ngăn chặn sự phân chia không cần thiết như thế nào. 

### Ví dụ 2 

đầu vào:```
1
3
2 3 3
```Tổng số tiền là`8`. 

| Lớn nhất hiện có | Cần thiết lớn nhất | Hành động | 
| --- | --- | --- | 
| 8 | 3 | Chia thành 4,4 | 
| 4 | 3 | Chia thành 2,2 | 
| 4 | 3 | Chia thành 2,2 | 
| 2 | 3 | Không thể | 

Ở bước cuối cùng, phần lớn nhất có sẵn đã nhỏ hơn giá trị lớn nhất được yêu cầu. 

Đầu ra:```
NO
```Điều này chứng tỏ điều kiện bất khả thi quan trọng`x < y`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trận đấu sẽ loại bỏ một giá trị mục tiêu và mỗi phần chia sẽ tạo ra tối đa một phần bổ sung. Chi phí hoạt động của heap O(log n). | 
| Không gian | O(n) | Các đống cùng nhau lưu trữ các giá trị O(n). | 

Tổng số phần trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, Vì thế`O(n log n)`dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        need = [-x for x in a]
        heapq.heapify(need)

        pieces = [-sum(a)]

        ok = True

        while need:
            if not pieces:
                ok = False
                break

            x = -heapq.heappop(pieces)
            y = -need[0]

            if x == y:
                heapq.heappop(need)
                continue

            if x < y or x == 1:
                ok = False
                break

            l = x // 2
            r = x - l

            heapq.heappush(pieces, -l)
            heapq.heappush(pieces, -r)

            if len(pieces) > len(need):
                ok = False
                break

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

# provided sample subset
assert run(
"""2
1
327
2
869 541
"""
) == "YES\nNO"

# minimum size
assert run(
"""1
1
1
"""
) == "YES"

# simple valid split
assert run(
"""1
2
1 1
"""
) == "YES"

# impossible because largest target cannot appear
assert run(
"""1
3
2 3 3
"""
) == "NO"

# all ones
assert run(
"""1
6
1 1 1 1 1 1
"""
) == "YES"

# large equal values
assert run(
"""1
4
999999999 999999999 999999999 999999999
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 -> [1]`| CÓ | Bánh Đơn | 
|`2 -> [1,1]`| CÓ | Sự phân chia không cần thiết nhỏ nhất | 
|`3 -> [2,3,3]`| KHÔNG | Giá trị yêu cầu lớn nhất không thể truy cập được | 
|`6 -> all ones`| CÓ | Nhiều giá trị lặp lại | 
|`4 -> four 999999999s`| CÓ | Số nguyên lớn và phép chia lặp đi lặp lại | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
327
```Tổng số tiền đã có`327`. Mảnh lớn nhất hiện có bằng với mảnh mục tiêu lớn nhất ngay lập tức, vì vậy thuật toán sẽ khớp với mảnh đó và kết thúc. Câu trả lời là`YES`. Một giải pháp giả định rằng cần có ít nhất một lần phân chia sẽ không thành công ở đây. 

Coi như:```
1
6
1 1 1 1 1 1
```Thuật toán bắt đầu từ`6`, liên tục chia các phần lớn hơn và cuối cùng tạo ra sáu phần. Mỗi người đều được kết hợp riêng lẻ. Vì trọng lượng`1`chỉ được chấp nhận khi nó khớp với giá trị đích, quy trình vẫn đúng. 

Coi như:```
1
2
869 541
```Tổng số tiền là`1410`. Việc chia tách tạo ra`705`Và`705`. Sau đó, phần lớn nhất có sẵn là`705`, trong khi phần cần thiết lớn nhất là`869`. Từ`705 < 869`, thuật toán sẽ bác bỏ ngay lập tức. Không có chuỗi chia tách nào có thể đưa một mảnh trở lại`869`. 

Coi như:```
1
3
2 2 2
```Tổng số tiền là`6`. Chúng tôi chia thành`3`Và`3`, thì mỗi`3`vào trong`1`Và`2`. Các mảnh có sẵn trở thành`{2,2,1,1}`. Sau khi khớp hai mục tiêu, chỉ còn lại một mục tiêu trong khi vẫn cần có mục tiêu hai. điều kiện`x < y`phát hiện điều này và trả lại`NO`, điều này đúng vì không thể tạo ra ba số hai từ một chiếc bánh có trọng lượng sáu bằng cách sử dụng thao tác được phép.
