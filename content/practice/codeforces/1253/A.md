---
problem: 1253A
contest_id: 1253
problem_index: A
name: "Single Push"
contest_name: "Codeforces Round 600 (Div. 2)"
rating: 1000
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 297
date: 2026-06-13
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d70a2-c08c-83ec-a0ab-b70b1d4d82e3
---

#CF 1253A - Đẩy đơn 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4 phút 57 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d70a2-c08c-83ec-a0ab-b70b1d4d82e3 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có độ dài bằng nhau và được phép thực hiện một loại sửa đổi rất cụ thể: nhiều nhất một lần, chúng tôi có thể chọn một phân đoạn liền kề và thêm cùng một số nguyên dương cho mọi phần tử trong phân đoạn đó. Nhiệm vụ là xác định xem liệu chúng ta có thể chuyển đổi mảng đầu tiên thành mảng thứ hai bằng cách không sử dụng thao tác nào hay sử dụng chính xác một phép cộng phân đoạn như vậy hay không. 

Hạn chế chính là việc sửa đổi được thống nhất trong một khoảng thời gian. Điều đó có nghĩa là mọi chỉ mục bên ngoài phân đoạn đã chọn phải khớp giữa các mảng và bên trong phân đoạn, sự khác biệt giữa các phần tử tương ứng phải không đổi. 

Tổng kích thước đầu vào lên tới 100.000 cho mỗi trường hợp thử nghiệm, điều này loại trừ mọi hoạt động kiểm tra bậc hai đối với tất cả các phân đoạn có thể một cách rõ ràng. Bất kỳ giải pháp nào cũng phải quét từng mảng theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể chọn nhiều phân khúc “tốt” rời rạc trong đó sự khác biệt là như nhau. Điều đó không thành công vì chỉ cho phép một phân đoạn liên tục. Ví dụ: nếu sự khác biệt trông giống như`[0, 2, 0, 2, 0]`, người ta có thể nghĩ mỗi`2`có thể được cố định một cách độc lập, nhưng một thao tác đơn lẻ không thể bao gồm nhiều khoảng thời gian riêng biệt. 

Một trường hợp khó phát hiện khác là khi tất cả sự khác biệt bằng 0 ngoại trừ một vị trí trong đó sự khác biệt là âm. Vì chúng ta chỉ có thể cộng một giá trị dương nên hiệu số âm ngay lập tức khiến câu trả lời là không thể. 

Ví dụ trường hợp thất bại: 

đầu vào:```
3
1 2 3
2 1 3
```Ở đây sự khác biệt là`[+1, -1, 0]`. Mặc dù hầu hết các chỉ số đều có vẻ ổn, nhưng sự khác biệt tiêu cực duy nhất khiến điều đó là không thể. 

Đầu ra đúng:`NO`. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi phân khúc có thể`[l, r]`và tính toán xem có tồn tại một giá trị dương hay không`k`như vậy việc thêm`k`đến phân khúc đó sẽ chuyển đổi`a`vào trong`b`. Đối với một phân khúc cố định, chúng tôi sẽ tính toán tất cả sự khác biệt`b[i] - a[i]`bên trong phân khúc và xác minh chúng giống hệt nhau và khẳng định, đồng thời đảm bảo tất cả các yếu tố bên ngoài đã khớp. 

Kiểm tra chi phí một phân khúc`O(n)`trong trường hợp xấu nhất, và có`O(n^2)`phân đoạn, dẫn đến`O(n^3)`tổng thời gian, quá chậm. 

Quan sát quan trọng là hoạt động xác định một cấu trúc rất cứng nhắc trên mảng khác biệt`d[i] = b[i] - a[i]`. Sau nhiều nhất một thao tác, tất cả các giá trị khác 0 trong`d`phải nằm bên trong một phân đoạn liền kề và trong phân đoạn đó tất cả các giá trị phải giống hệt nhau và dương. Mọi nơi khác phải chính xác bằng không. 

Vì vậy, thay vì tìm kiếm một đoạn, chúng ta phân tích trực tiếp cấu trúc của mảng sai phân. Chúng tôi chỉ cần kiểm tra xem tất cả các mục khác 0 có tạo thành chính xác một khối liên tục hay không và liệu tất cả các giá trị trong khối đó có bằng nhau và dương hay không. 

Điều này làm giảm vấn đề từ “chọn một phân đoạn” thành “xác minh một mẫu trong một lần”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định mảng khác biệt ngầm là`d[i] = b[i] - a[i]`. 

1. Tính chỉ số đầu tiên trong đó`d[i]`là khác không. Nếu không tồn tại thì các mảng đã bằng nhau và chúng ta trả về CÓ ngay lập tức. Điều này xử lý trường hợp không cần thao tác. 
2. Từ vị trí khác 0 đầu tiên này, ta xác định giá trị`k = d[start]`. Đây là giá trị khả thi duy nhất mà chúng ta có thể thêm vào trong phép toán, vì phép toán thực thi một mức tăng không đổi. 
3. Chúng tôi quét về phía trước từ`start`và đảm bảo mọi vị trí có chênh lệch khác 0 đều có giá trị chính xác`k`. Nếu chúng ta từng thấy một giá trị khác 0, điều đó có nghĩa là không có một bước tăng thống nhất nào có thể giải thích được sự biến đổi. 
4. Chúng tôi đảm bảo tất cả các khác biệt khác 0 đều xuất hiện trong một khối liền kề. Khi chúng ta rời khỏi khối có giá trị khác 0, sau này chúng ta không được gặp giá trị khác 0 nữa. Nếu chúng tôi làm như vậy, thao tác được yêu cầu sẽ cần hai phân đoạn riêng biệt, điều này không được phép. 
5. Chúng tôi cũng đảm bảo`k > 0`. Không thể đạt được giá trị không dương vì thao tác chỉ cho phép cộng các số nguyên dương. 

Tại sao nó hoạt động: phép toán thay đổi chính xác một khoảng bằng một độ lệch không đổi, do đó mảng sai phân sau một phép toán hợp lệ phải bằng 0 ở mọi nơi ngoại trừ có thể là một khoảng liền kề, trong đó tất cả các giá trị đều bằng cùng một hằng số dương. Thuật toán kiểm tra chính xác thuộc tính cấu trúc này, vì vậy bất kỳ sai lệch nào cũng có nghĩa là không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    i = 0
    while i < n and a[i] == b[i]:
        i += 1

    if i == n:
        print("YES")
        return

    k = b[i] - a[i]
    if k <= 0:
        print("NO")
        return

    while i < n and a[i] != b[i]:
        if b[i] - a[i] != k:
            print("NO")
            return
        i += 1

    while i < n:
        if a[i] != b[i]:
            print("NO")
            return
        i += 1

    print("YES")

t = int(input())
for _ in range(t):
    solve()
```Giải pháp đầu tiên là xác định vị trí bắt đầu của vùng không khớp. Chỉ số đó xác định giá trị gia tăng khả thi duy nhất. Sau đó, nó xác nhận rằng toàn bộ vùng không khớp là nhất quán và liền kề, đồng thời cuối cùng đảm bảo không có vùng không khớp nào xuất hiện sau này. 

Một cạm bẫy triển khai phổ biến là quên thực thi tính liền kề, dẫn đến việc chấp nhận các trường hợp xuất hiện sự không khớp trong nhiều khối riêng biệt. Một vấn đề tế nhị khác là không thể bác bỏ số âm hoặc số 0`k`, điều này không hợp lệ vì phép toán yêu cầu phép cộng hoàn toàn dương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
3 7 1 4 1 2
3 7 3 6 3 2
```Mảng khác biệt:`[0, 0, 2, 2, 2, 0]`| Bước | tôi | a[i] vs b[i] | k | Trạng thái | 
| --- | --- | --- | --- | --- | 
| Tìm bắt đầu | 2 | 1 → 3 | 2 | bắt đầu tìm thấy | 
| Khối quét | 2 | 2 | 2 | được | 
| Khối quét | 3 | 2 | 2 | được | 
| Khối quét | 4 | 2 | 2 | được | 
| Khối thoát | 5 | 0 | 2 | được | 

Vùng không khớp là một phân đoạn liên tục có chênh lệch không đổi 2, do đó, một lần đẩy sẽ hoạt động. 

### Ví dụ 2 

đầu vào:```
5
1 1 1 1 1
1 2 1 3 1
```Mảng khác biệt:`[0, 1, 0, 2, 0]`| Bước | tôi | a[i] vs b[i] | k | Trạng thái | 
| --- | --- | --- | --- | --- | 
| Tìm bắt đầu | 1 | 1 → 2 | 1 | k=1 | 
| Khối quét | 1 | 1 | 1 | được | 
| Sự không phù hợp tiếp theo | 3 | 1 → 3 | dự kiến ​​1 | không khớp | 

Tại chỉ số 3, chúng tôi tìm thấy một chênh lệch khác 0 (2 thay vì 1), do đó không có phân đoạn đơn lẻ nào có mức tăng đồng đều có thể giải thích được phép biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | truyền qua các mảng một lần để tìm và xác thực phân đoạn không khớp | 
| Không gian | O(1) | không cần mảng phụ trợ | 

Quét tuyến tính là đủ vì mỗi phần tử được xử lý tối đa một số lần không đổi. Với tổng số`n`trên các trường hợp thử nghiệm lên tới 100.000, điều này dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        i = 0
        while i < n and a[i] == b[i]:
            i += 1

        if i == n:
            out.append("YES")
            return

        k = b[i] - a[i]
        if k <= 0:
            out.append("NO")
            return

        while i < n and a[i] != b[i]:
            if b[i] - a[i] != k:
                out.append("NO")
                return
            i += 1

        while i < n:
            if a[i] != b[i]:
                out.append("NO")
                return
            i += 1

        out.append("YES")

    for _ in range(t):
        solve()

    return "\n".join(out)

# provided samples
assert run("""4
6
3 7 1 4 1 2
3 7 3 6 3 2
5
1 1 1 1 1
1 2 1 3 1
2
42 42
42 42
1
7
6
""") == """YES
NO
YES
NO"""

# custom cases
assert run("""1
3
1 2 3
1 2 3
""") == "YES", "already equal"

assert run("""1
3
1 2 3
2 2 3
""") == "NO", "multiple mismatches"

assert run("""1
4
1 1 1 1
2 1 2 1
""") == "NO", "non-contiguous differences"

assert run("""1
3
5 5 5
4 4 4
""") == "NO", "negative k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã bằng nhau | CÓ | trường hợp không hoạt động | 
| 1 2 3 → 2 2 3 | KHÔNG | sự khác biệt không nhất quán | 
| 1 1 1 1 → 2 1 2 1 | KHÔNG | không phù hợp liền kề | 
| 5 5 5 → 4 4 4 | KHÔNG | bác bỏ k âm | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các mảng đã giống hệt nhau. Thuật toán phát hiện điều này khi không tìm thấy chỉ số không khớp và ngay lập tức trả về CÓ mà không cần cố gắng tính toán`k`. 

Một trường hợp khác là khi có sự không phù hợp nhưng không liền kề nhau. Đối với đầu vào`a = [1,1,1,1]`,`b = [2,1,2,1]`, mảng khác biệt là`[1,0,1,0]`. Quá trình quét tìm thấy một điểm không khớp, sau đó trở về 0, rồi sau đó tìm thấy một điểm không khớp khác, vi phạm yêu cầu một phân đoạn, tạo ra NO một cách chính xác. 

Trường hợp cạnh cuối cùng là khi sự không khớp đầu tiên đã vi phạm tính tích cực. Nếu như`b[i] < a[i]`tại chỉ số khác nhau đầu tiên,`k`trở nên âm và thuật toán từ chối ngay lập tức, vì thao tác chỉ có thể tăng giá trị.
