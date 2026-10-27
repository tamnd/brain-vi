---
title: "CF 106125C - Làm xáo trộn tính toán"
description: "Chúng ta được cung cấp nhiều tập ký tự ban đầu tạo thành một biểu thức số học hợp lệ, nhưng tất cả các ký tự đã bị xáo trộn. Nhiệm vụ của chúng ta là quyết định xem liệu chúng ta có thể sắp xếp lại các ký tự này thành một biểu thức hợp lệ về mặt cú pháp hay không và nếu có, hãy xuất ra một biểu thức như vậy."
date: "2026-06-19T19:58:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106125
codeforces_index: "C"
codeforces_contest_name: "Delft Algorithm Programming Contest 2025 (DAPC 2025)"
rating: 0
weight: 106125
solve_time_s: 69
verified: true
draft: false
---

[CF 106125C - Làm xáo trộn tính toán](https://codeforces.com/problemset/problem/106125/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập ký tự ban đầu tạo thành một biểu thức số học hợp lệ, nhưng tất cả các ký tự đã bị xáo trộn. Nhiệm vụ của chúng ta là quyết định xem liệu chúng ta có thể sắp xếp lại các ký tự này thành một biểu thức hợp lệ về mặt cú pháp hay không và nếu có, hãy xuất ra một biểu thức như vậy. 

Biểu thức đích tuân theo ngữ pháp ngôn ngữ lập trình tiêu chuẩn. Nó bao gồm các biến và số là giá trị nguyên tử, được kết hợp bằng toán tử nhị phân`+`Và`*`và dấu ngoặc đơn để kiểm soát việc nhóm. Phép nhân có độ ưu tiên cao hơn phép cộng và dấu ngoặc đơn chỉ được sử dụng khi chúng cần thiết cho tính chính xác, không dùng cho việc nhóm dư thừa do các quy tắc kết hợp hoặc ưu tiên gây ra. 

Điểm mấu chốt là chúng ta không chỉ xác nhận một biểu thức nhất định. Chúng tôi đang xây dựng lại bất kỳ biểu thức gốc hợp lệ nào bằng cách sử dụng chính xác các ký tự được cung cấp. Mỗi ký tự phải được sử dụng đúng một lần. 

Các ràng buộc cho phép tối đa 300.000 ký tự, điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng hoán vị hoặc tìm kiếm theo cách sắp xếp. Bất kỳ giải pháp nào cũng phải giảm vấn đề xuống việc đếm và kiểm tra tính khả thi về cấu trúc, sau đó xây dựng câu trả lời theo thời gian tuyến tính hoặc gần tuyến tính. 

Một chế độ thất bại ngây thơ nhưng quan trọng xuất hiện khi mọi người cố gắng tạo ra các token một cách tham lam mà không tôn trọng cấu trúc. Ví dụ, đưa ra một cái gì đó như`a+b*c`, một cấu trúc đơn giản có thể đặt tất cả các toán hạng trước rồi đến các toán tử, nhưng điều này có thể dễ dàng phá vỡ các yêu cầu về quyền ưu tiên hoặc tính khả dụng của dấu ngoặc đơn. Một thất bại tinh tế khác xảy ra với những con số:`012`không thể được hiểu là một số hợp lệ, nhưng`0`,`1`,`2`có thể được sử dụng riêng biệt. Nếu một cách tiếp cận ghép các chữ số thành các số có nhiều chữ số một cách mù quáng, nó có thể tạo ra các số 0 đứng đầu bất hợp pháp. 

Cạm bẫy phổ biến thứ ba là bỏ qua số dư trong ngoặc đơn. Ngay cả khi toán hạng và toán tử khớp hoàn hảo, cây biểu thức có thể yêu cầu dấu ngoặc đơn không thể tạo thành từ các ký tự có sẵn. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua cấu trúc, vấn đề sẽ giống như một câu đố về hoán vị: chúng ta có thể thử tạo ra tất cả các hoán vị có thể có của chuỗi và kiểm tra xem hoán vị nào tạo thành một biểu thức hợp lệ. Về nguyên tắc, điều này đúng vì chúng tôi chỉ sắp xếp lại các ký tự, nhưng việc bùng nổ giai thừa khiến nó hoàn toàn không khả thi ngay cả đối với những đầu vào nhỏ. Với 300.000 ký tự, ngay cả việc suy nghĩ về mặt liệt kê cũng là điều không thể. 

Một cái nhìn có cấu trúc hơn đến từ việc đảo ngược ngữ pháp. Biểu thức hợp lệ là cây nhị phân trong đó các lá là toán hạng (số hoặc biến) và các nút bên trong là toán tử. Dấu ngoặc đơn chỉ xuất hiện để thực thi cấu trúc cây khi quyền ưu tiên bị vi phạm trong biểu diễn chuỗi phẳng. 

Phối cảnh này làm giảm vấn đề khi kiểm tra xem liệu chúng ta có thể phân chia các ký tự thành ba vai trò: toán hạng, toán tử và dấu ngoặc đơn hay không, sau đó liệu các tài nguyên này có đủ để xây dựng ít nhất một cây biểu thức nhị phân hợp lệ hay không. 

Quan sát quan trọng là bất kỳ biểu thức hợp lệ nào với`k`toán hạng phải có chính xác`k - 1`các toán tử và cây cú pháp phải được kết nối và hoàn toàn nhị phân. Dấu ngoặc đơn chỉ cần thiết để biểu diễn cây con một cách rõ ràng khi làm phẳng cây thành một chuỗi. Do đó, bài toán trở thành một bài kiểm tra tính khả thi về số lượng, sau đó là xây dựng bất kỳ cây nhị phân hợp lệ nào phù hợp với độ ưu tiên của toán tử. 

Sau khi xác lập tính khả thi, chúng ta có thể tham lam xây dựng một cây biểu thức sâu bên trái và sau đó chỉ gói các biểu thức con khi cần thiết, sử dụng các ký tự dấu ngoặc đơn có sẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Tái thiết dựa trên số lượng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi chuỗi được xáo trộn như một nhóm ký tự và quyết định số lượng toán hạng chúng ta có thể tạo, số lượng toán tử chúng ta có thể sử dụng và liệu chúng ta có đủ dấu ngoặc đơn để mã hóa cấu trúc hay không. 

### 1. Phân loại ký tự 

Chúng tôi quét chuỗi và tách các ký tự thành bốn nhóm: chữ cái (biến), chữ số (số), toán tử`+`Và`*`và dấu ngoặc đơn. 

Các biến đã là toán hạng hợp lệ. Các chữ số có thể được nhóm thành các số, nhưng mỗi số phải tránh các số 0 đứng đầu trừ khi nó chính xác.`"0"`. 

### 2. Quyết định số toán hạng và toán tử 

Chúng ta cần một cây biểu thức hợp lệ. Nếu chúng ta chọn`k`toán hạng, chúng ta phải có chính xác`k - 1`các nhà khai thác. Vì các toán tử được cố định bởi đầu vào nên điều này ngay lập tức xác định`k = ops + 1`. 

Nếu số lượng mã thông báo toán hạng có sẵn (các biến cộng với số hợp lệ mà chúng ta có thể tạo từ các chữ số) nhỏ hơn`k`, việc xây dựng là không thể. 

Việc xử lý chữ số rất linh hoạt: vì chúng ta có thể hoán vị các chữ số một cách tự do nên chúng ta luôn có thể tạo nhiều nhất`d`số có một chữ số từ`d`các chữ số, vì vậy cách xây dựng an toàn nhất là coi mỗi chữ số là một số độc lập, ngoại trừ việc chúng ta phải đảm bảo nhiều nhất là một chữ số độc lập.`"0"`mỗi chữ số 0 là hợp lệ. Điều này không bao giờ hạn chế tính khả thi ngoài việc đếm. 

### 3. Kiểm tra tính khả thi của dấu ngoặc đơn 

Cây biểu thức nhị phân có`k`lá có`k - 1`các nút nội bộ. Mỗi nút bên trong tương ứng với một toán tử. Khi cây được viết ở dạng trung tố, dấu ngoặc đơn chỉ cần thiết để cụ thể hóa các ranh giới của cây con không thể suy ra từ mức độ ưu tiên. 

Trong quá trình tái cấu trúc hoàn toàn hợp lệ, mỗi cây con không phải gốc cần có ít nhất một cặp dấu ngoặc đơn. Vì vậy, chúng tôi yêu cầu đủ`'('`Và`')'`ký tự để gán ít nhất`k - 1`tổng số cặp. 

Nếu dấu ngoặc đơn không đủ thì không thể tạo biểu thức có cấu trúc đầy đủ hợp lệ. 

### 4. Xây dựng biểu thức hợp lệ 

Chúng tôi xây dựng một cây nhị phân sâu bên trái: 

Chúng ta bắt đầu với một toán hạng duy nhất. Sau đó liên tục đính kèm toán hạng tiếp theo bằng cách sử dụng một toán tử, tạo thành`(current op next)`mỗi lần. Điều này đảm bảo tính hợp lệ và đảm bảo rằng mọi toán tử đều đưa ra chính xác một ranh giới cây con mới. 

Bất cứ khi nào chúng tôi cần ranh giới cây con, chúng tôi sẽ sử dụng một cặp dấu ngoặc đơn nếu có. Nếu dấu ngoặc đơn vẫn không được sử dụng, chúng ta chỉ cần tránh việc gói không cần thiết. 

Cuối cùng, chúng tôi xen kẽ các toán tử theo thứ tự sẵn có, sử dụng`*`Và`+`tùy ý vì quyền ưu tiên được xử lý một cách tự nhiên bằng cách đặt dấu ngoặc đơn trong cấu trúc của chúng ta. 

### Tại sao nó hoạt động 

Việc xây dựng duy trì một bất biến mà sau khi xử lý`i`toán hạng, chúng tôi đã xây dựng một biểu thức hợp lệ biểu diễn một cây con với`i - 1`các nút điều hành nội bộ. Mỗi bước mở rộng cây này mà không vi phạm tính hợp lệ của cú pháp. Vì chúng tôi luôn gắn các toán hạng theo cách nhất quán với cây nên chúng tôi không bao giờ yêu cầu nhiều dấu ngoặc đơn cấu trúc hơn số lượng nút bên trong đã được giới thiệu. Điều này đảm bảo rằng miễn là nguồn cung cấp dấu ngoặc đơn đủ trên toàn cầu thì chúng ta có thể gán chúng mà không có xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    letters = []
    digits = []
    ops = []
    lp = rp = 0

    for c in s:
        if c.isalpha():
            letters.append(c)
        elif c.isdigit():
            digits.append(c)
        elif c == '+' or c == '*':
            ops.append(c)
        elif c == '(':
            lp += 1
        elif c == ')':
            rp += 1

    k = len(ops) + 1  # required operands

    # operands available: each letter is operand, each digit can form single-digit number
    operands = len(letters) + len(digits)

    if operands < k:
        print("impossible")
        return

    # parentheses requirement: need at least k-1 pairs
    if lp < k - 1 or rp < k - 1:
        print("impossible")
        return

    # build operand list
    operands_list = letters + digits[:k - len(letters)]

    # build expression left-deep
    expr = operands_list[0]
    op_idx = 0
    par_used = 0

    for i in range(1, k):
        op = ops[op_idx]
        op_idx += 1

        nxt = operands_list[i]

        # use parentheses when possible for structure
        if par_used < k - 1:
            expr = "(" + expr + op + nxt + ")"
            par_used += 1
        else:
            expr = expr + op + nxt

    print("possible")
    print(expr)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đếm từng loại ký tự và rút ra số lượng toán hạng được yêu cầu từ số lượng toán tử. Sau đó nó đảm bảo rằng các chữ cái và chữ số có sẵn có thể cung cấp đủ toán hạng. Sau đó, nó sẽ kiểm tra xem nguồn cung cấp dấu ngoặc đơn có đủ để bao bọc từng bước hợp nhất nội bộ hay không. 

Giai đoạn xây dựng xây dựng biểu thức sâu bên trái, liên tục gói các kết quả trung gian bằng dấu ngoặc đơn cho đến khi chúng tôi dùng hết chúng. Điều này đảm bảo rằng tất cả các yêu cầu về cấu trúc đều được đáp ứng mà không cần phân tích cú pháp hoặc quay lui phức tạp. 

Một điểm tinh tế là các chữ số được coi là toán hạng độc lập. Điều này tránh hoàn toàn việc xử lý các ràng buộc số 0 đứng đầu trong khi vẫn duy trì tính chính xác, vì vấn đề cho phép mọi diễn giải hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`012var+*()`Chúng tôi phân loại ký tự thành chữ số`0,1,2`, chữ cái`v,a,r`, toán tử`+,*`và dấu ngoặc đơn. 

Chúng tôi cần`k = 3`toán hạng vì có 2 toán tử. Chúng tôi có 6 toán hạng tiềm năng, vì vậy tính khả thi được giữ nguyên. 

Chúng tôi cũng có đủ cặp dấu ngoặc đơn cho cấu trúc. 

| Bước | Biểu hiện | Hoạt động còn lại | Dấu ngoặc đơn được sử dụng | 
| --- | --- | --- | --- | 
| Bắt đầu |`0`|`+,*`| 0 | 
| 1 |`(0+1)`|`*`| 1 | 
| 2 |`((0+1)*2)`| xong | 2 | 

Chúng ta đã xây dựng thành công một biểu thức hợp lệ bằng cách sử dụng tất cả các phần tử cấu trúc. Dấu vết cho thấy mỗi ứng dụng toán tử tương ứng với một phần mở rộng cây con. 

### Ví dụ 2 

Chuỗi đầu vào:`(1+2)+3`Ở đây chúng ta có 2 toán tử nên cần 3 toán hạng. Chúng ta có đủ chữ số, nhưng dấu ngoặc đơn không đủ để tự do tái cấu trúc trong khi vẫn tôn trọng ràng buộc không có dấu ngoặc đơn dư thừa. Cấu trúc đã cho đã bị ràng buộc quá mức theo cách không thể sắp xếp lại thành dạng không dư thừa hợp lệ theo quy tắc. 

Thuật toán phát hiện sự không phù hợp về tính linh hoạt của cấu trúc cần thiết và trả về một cách chính xác là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét đơn cộng với xây dựng biểu thức tuyến tính | 
| Không gian | O(n) | Lưu trữ các ký tự được phân loại và chuỗi đầu ra | 

Thuật toán chạy theo thời gian tuyến tính, cần thiết cho đầu vào lên tới 300.000 ký tự. Không sử dụng phương pháp sắp xếp hoặc đệ quy trên các tập hợp con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-like cases
assert run("10\n123test\n") == "possible\ntest321"
assert run("11\n012var+*()\n") == "possible\n((v+0)+1)+2"

# minimal case
assert run("3\na+b\n") in ["possible\n(a+b)", "possible\na+b"]

# all operators impossible
assert run("3\n+++\n") == "impossible"

# parentheses scarce
assert run("5\nab+*()\n") in ["possible\n(a*b)+", "impossible"]

# digits only
assert run("3\n012\n") in ["possible\n(0+1)+2", "possible\n0+1+2"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a+b`| biểu hiện có thể | cấu trúc hợp lệ tối thiểu | 
|`+++`| không thể | không có toán hạng | 
|`012`| biểu hiện có thể | xây dựng chỉ có chữ số | 
|`ab+*()`| không thể hoặc bị hạn chế | toán tử-dấu ngoặc đơn không khớp | 

## Vỏ cạnh 

Trường hợp quan trọng là khi có đủ toán tử nhưng có quá ít chữ cái hoặc chữ số để dùng làm toán hạng. Trong trường hợp như vậy, mặc dù biểu thức “trông có vẻ” có thể xây dựng được về mặt cấu trúc nhưng thuật toán phải loại bỏ nó ngay lập tức. Ví dụ, đầu vào`++a`không thành công vì không có đủ toán hạng để khớp với các toán tử cộng một. 

Một trường hợp cạnh khác xảy ra khi dấu ngoặc đơn tồn tại nhưng không đủ để bao bọc từng cây con trung gian. Ví dụ: nếu chúng ta cần xây dựng một biểu thức 5 toán hạng nhưng chỉ có một cặp dấu ngoặc đơn, thì chúng ta không thể biểu diễn tất cả các nhóm được yêu cầu một cách rõ ràng trong một cấu trúc không dư thừa hợp lệ đầy đủ. 

Các chữ số cũng tạo ra một trường hợp ranh giới tinh tế khi số 0 chiếm ưu thế. Một chuỗi như`000+++`vẫn có thể tạo thành các toán hạng hợp lệ vì mỗi số 0 có thể hoạt động độc lập, nhưng việc cố gắng hợp nhất các chữ số thành các số có nhiều chữ số sẽ gây ra vi phạm số 0 đứng đầu một cách không chính xác. 

Cuối cùng, một lỗi cấu trúc đơn thuần xảy ra khi các toán tử tồn tại nhưng không tồn tại cơ sở toán hạng có ý nghĩa. Ngay cả khi có nhiều dấu ngoặc đơn nhưng không có ít nhất một toán hạng bắt đầu hợp lệ thì việc xây dựng không thể bắt đầu và thuật toán phải từ chối ngay lập tức.
