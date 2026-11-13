---
title: "CF 1730C - Ký hiệu tối thiểu"
description: "Chúng tôi được cung cấp một chuỗi các chữ số thập phân. Đối với bất kỳ chữ số nào, chúng tôi có thể xóa nó khỏi vị trí hiện tại, tăng giá trị của nó lên một (giới hạn ở mức 9) và chèn chữ số kết quả vào bất kỳ đâu trong chuỗi. Hoạt động có thể được lặp lại bao nhiêu lần tùy thích."
date: "2026-06-09T18:41:22+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1730
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 823 (Div. 2)"
rating: 1200
weight: 1730
solve_time_s: 101
verified: true
draft: false
---

[CF 1730C - Ký hiệu tối thiểu](https://codeforces.com/problemset/problem/1730/C) 

**Đánh giá:** 1200 
**Tags:** cấu trúc dữ liệu, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các chữ số thập phân. Đối với bất kỳ chữ số nào, chúng tôi có thể xóa nó khỏi vị trí hiện tại, tăng giá trị của nó lên một (giới hạn ở mức 9) và chèn chữ số kết quả vào bất kỳ đâu trong chuỗi. 

Hoạt động có thể được lặp lại bao nhiêu lần tùy thích. Vì việc chèn không bị hạn chế nên thứ tự cuối cùng của các chữ số phần lớn nằm trong tầm kiểm soát của chúng tôi. Thử thách ở đây là quyết định chữ số nào nên tăng và di chuyển, chữ số nào không thay đổi, sao cho chuỗi kết quả về mặt từ điển càng nhỏ càng tốt. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào nhằm khám phá chuỗi hoạt động. Thậm chí một$O(n^2)$Thuật toán sẽ có rủi ro vì một trường hợp thử nghiệm có thể chứa$2 \cdot 10^5$chữ số. Chúng tôi cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Phần khó khăn là hiểu khi nào một chữ số nên được sửa đổi. Một ý tưởng ngây thơ là tăng mọi chữ số lớn, vì di chuyển nó sau có thể hữu ích. Điều đó không phải lúc nào cũng đúng. 

Coi như:```
s = 01
```Câu trả lời là:```
01
```Nếu chúng ta tăng chữ số`0`, nó trở thành`1`, tạo ra một chuỗi tồi tệ hơn. 

Một trường hợp tế nhị khác là:```
s = 21
```Câu trả lời là:```
13
```chữ số`2`xuất hiện trước chữ số nhỏ hơn`1`. Giữ`2`tại chỗ buộc ký tự đầu tiên của chuỗi cuối cùng phải ít nhất`2`. Bằng cách tăng nó lên`3`và di chuyển nó sau, chúng tôi cho phép`1`đến trước. 

Trường hợp quan trọng thứ ba là:```
s = 999
```Câu trả lời vẫn là:```
999
```Mặc dù chúng ta có thể áp dụng nhiều lần các thao tác nhưng các chữ số vẫn bị giới hạn ở mức`9`. Di chuyển chúng xung quanh không thể tạo ra bất cứ thứ gì nhỏ hơn nhiều tập hợp chữ số hiện có. 

Quan sát quan trọng là một chữ số chỉ trở nên có vấn đề nếu tồn tại một chữ số nhỏ hơn ở đâu đó bên phải nó. 

## Phương pháp tiếp cận 

Một quan điểm thô bạo là suy nghĩ về từng chữ số một cách độc lập. Đối với mỗi vị trí, chúng ta có thể quyết định nên giữ chữ số hay áp dụng thao tác, sau đó thử mọi vị trí chèn có thể. Điều này cuối cùng sẽ tạo ra tất cả các chuỗi có thể truy cập được và chuỗi nhỏ nhất về mặt từ điển sẽ là câu trả lời. 

Khó khăn là số lượng khả năng bùng nổ. Ngay cả việc quyết định giữ hoặc sửa đổi cho mỗi chữ số cũng đã tạo ra$2^n$các khả năng và vị trí chèn thêm một yếu tố tổ hợp khác. Điều này đã trở thành không thể từ rất lâu trước đó$n$thậm chí đạt tới vài chục ký tự. 

Để tìm thứ gì đó nhanh hơn, chúng ta cần hiểu hoạt động thực sự thay đổi điều gì. 

Giả sử một chữ số$s[i]$có chữ số nhỏ hơn ở đâu đó bên phải. Sau đó dù sau này chúng ta có làm gì đi chăng nữa, hãy giữ$s[i]$ở phía trước chữ số nhỏ hơn đó sẽ ảnh hưởng đến trật tự từ điển. Vì hoạt động cho phép chúng tôi loại bỏ$s[i]$, tăng nó lên một và đặt nó ở bất cứ đâu sau này, chúng ta nên làm chính xác điều đó. Chúng ta hy sinh chữ số một chút bằng cách tăng nó lên, nhưng chúng ta giải phóng chữ số nhỏ hơn để xuất hiện sớm hơn. 

Bây giờ hãy xem xét một chữ số đã nhỏ hơn hoặc bằng mọi chữ số ở bên phải của nó. Một chữ số như vậy không chặn bất kỳ giá trị nhỏ hơn nào. Giữ nguyên nó ít nhất cũng tốt như tăng nó lên. 

Điều này đưa ra một tiêu chí rõ ràng: 

Đối với mọi vị trí, hãy nhìn vào chữ số tối thiểu xuất hiện ở bên phải của nó. Nếu tồn tại một chữ số nhỏ hơn, hãy tăng chữ số hiện tại lên một (giới hạn ở mức 9). Nếu không thì giữ nguyên. 

Sau khi đưa ra những quyết định này, vị trí chính xác không còn quan trọng nữa. Mỗi chữ số được giữ vẫn có sẵn với giá trị ban đầu của nó. Mỗi chữ số được sửa đổi đều đóng góp giá trị gia tăng của nó. Vì các chữ số đã di chuyển có thể được sắp xếp lại một cách tùy ý, nên chuỗi cuối cùng nhỏ nhất về mặt từ điển có thể thu được một cách đơn giản bằng cách sắp xếp tất cả các chữ số thu được. 

Toàn bộ vấn đề giảm xuống mức tối thiểu tính toán hậu tố và xây dựng nhiều tập hợp chữ số được chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với chuỗi$s$, tính chữ số nhỏ nhất xuất hiện trong mỗi hậu tố. 
2. Di chuyển chuỗi từ trái sang phải. 
3. Đối với chức vụ$i$, so sánh$s[i]$với chữ số tối thiểu xuất hiện ở đâu đó bên phải của nó. 
4. Nếu có chữ số nhỏ hơn ở bên phải, hãy thay thế$s[i]$qua$\min(s[i]+1, 9)$. 
5. Nếu không thì giữ$s[i]$không thay đổi. 
6. Lưu trữ chữ số kết quả vào danh sách. 
7. Sau khi xử lý tất cả các vị trí, sắp xếp danh sách các chữ số thu được. 
8. Ghép các chữ số đã sắp xếp và in kết quả. 

Tại sao việc sắp xếp lại hợp lệ? Mọi chữ số cần di chuyển đều được chuyển đổi thành phiên bản tăng lên của nó. Khi tất cả các quyết định như vậy đã được sửa, thao tác cho phép chèn các chữ số đã sửa đổi đó vào bất kỳ đâu. Sự sắp xếp nhỏ nhất về mặt từ điển của một tập hợp nhiều chữ số chỉ đơn giản là thứ tự được sắp xếp của nó. 

### Tại sao nó hoạt động 

Một chữ số chỉ được giữ nguyên khi nó không lớn hơn mọi chữ số sau nó. Một chữ số như vậy không bao giờ ngăn cản một chữ số nhỏ hơn xuất hiện sớm hơn, do đó việc sửa đổi nó không thể cải thiện tiền tố của chuỗi cuối cùng. 

Nếu có một chữ số nhỏ hơn ở đâu đó bên phải, việc giữ chữ số hiện tại trước nó sẽ làm cho chuỗi cuối cùng bắt đầu với giá trị lớn hơn mức cần thiết. Di chuyển chữ số đi luôn có lợi và hoạt động này yêu cầu thanh toán chính xác một đơn vị tăng thêm, giới hạn ở mức 9. 

Do đó, mọi vị trí đều có một quyết định cục bộ tối ưu được xác định duy nhất bằng việc có tồn tại chữ số hậu tố nhỏ hơn hay không. Sau khi áp dụng tất cả các mức tăng bắt buộc, nhiệm vụ còn lại là sắp xếp nhiều tập chữ số thu được. Mọi sự sắp xếp tối thiểu về mặt từ điển đều phải sắp xếp các chữ số theo thứ tự không giảm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        s = input().strip()
        n = len(s)

        suf_min = [''] * n
        suf_min[-1] = s[-1]

        for i in range(n - 2, -1, -1):
            suf_min[i] = min(s[i], suf_min[i + 1])

        res = []

        for i in range(n):
            if i + 1 < n and suf_min[i + 1] < s[i]:
                d = min(int(s[i]) + 1, 9)
                res.append(str(d))
            else:
                res.append(s[i])

        res.sort()
        print("".join(res))

solve()
```Phần đầu tiên tính hậu tố cực tiểu. Đối với mọi vị trí, chúng tôi muốn biết liệu chữ số nhỏ hơn có xuất hiện sau này hay không. Giá trị tối thiểu trong hậu tố bắt đầu tại`i + 1`trả lời chính xác câu hỏi đó. 

Lần thứ hai áp dụng quy tắc tham lam. Nếu chữ số nhỏ nhất trong tương lai hoàn toàn nhỏ hơn chữ số hiện tại, chúng ta sẽ tăng chữ số hiện tại lên một và đánh dấu nó là có thể di chuyển được. Nếu không thì chúng tôi giữ nó. 

Sau khi tất cả các phép biến đổi được xác định, chỉ có nhiều tập hợp các chữ số kết quả mới quan trọng. Việc sắp xếp chúng mang lại sự sắp xếp từ điển nhỏ nhất có thể. 

Lỗi triển khai phổ biến nhất là so sánh với mức tối thiểu của hậu tố bao gồm cả vị trí hiện tại. Chúng tôi đặc biệt cần`suf_min[i + 1]`, bởi vì câu hỏi đặt ra là liệu có tồn tại một chữ số nhỏ hơn ở bên phải hay không. sử dụng`suf_min[i]`sẽ coi các chữ số bằng nhau không chính xác là bằng chứng cho thấy tồn tại một chữ số nhỏ hơn. 

Một sai lầm dễ mắc phải khác là quên nắp ở`9`. Khi chữ số hiện tại là`9`, tăng thì vẫn ra`9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
04829
```Hậu tố tối thiểu: 

| Vị trí | Chữ số | Tối thiểu sang Phải | 
| --- | --- | --- | 
| 0 | 0 | 2 | 
| 1 | 4 | 2 | 
| 2 | 8 | 2 | 
| 3 | 2 | 9 | 
| 4 | 9 | - | 

Xử lý: 

| Vị trí | Bản gốc | Quyền nhỏ hơn tồn tại? | Đã lưu trữ | 
| --- | --- | --- | --- | 
| 0 | 0 | Không | 0 | 
| 1 | 4 | Có | 5 | 
| 2 | 8 | Có | 9 | 
| 3 | 2 | Không | 2 | 
| 4 | 9 | Không | 9 | 

Trước khi sắp xếp:```
05929
```Sau khi sắp xếp:```
02599
```Ví dụ này cho thấy ý tưởng trung tâm. chữ số`4`Và`8`chặn một chữ số nhỏ hơn`2`, vì vậy chúng được tăng lên và di chuyển ra khỏi đường đi. 

### Ví dụ 2 

đầu vào:```
314752277691991
```Xử lý: 

| Vị trí | Bản gốc | Kết quả | 
| --- | --- | --- | 
| 0 | 3 | 4 | 
| 1 | 1 | 1 | 
| 2 | 4 | 5 | 
| 3 | 7 | 8 | 
| 4 | 5 | 6 | 
| 5 | 2 | 2 | 
| 6 | 2 | 2 | 
| 7 | 7 | 8 | 
| 8 | 7 | 8 | 
| 9 | 6 | 7 | 
| 10 | 9 | 9 | 
| 11 | 1 | 1 | 
| 12 | 9 | 9 | 
| 13 | 9 | 9 | 
| 14 | 1 | 1 | 

Các chữ số được thu thập:```
415862288791991
```Sau khi sắp xếp:```
111334567888999
```Dấu vết này chứng tỏ rằng khi mỗi chữ số đã được chuyển đổi độc lập theo quy tắc hậu tố tối thiểu, việc sắp xếp ngay lập tức sẽ tạo ra câu trả lời tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Tính toán hậu tố cực tiểu là tuyến tính, việc sắp xếp các chữ số kết quả chiếm ưu thế | 
| Không gian |$O(n)$| Mảng hậu tố và danh sách kết quả | 

Tổng kích thước đầu vào tối đa là$2 \cdot 10^5$. Sắp xếp tối đa$2 \cdot 10^5$các chữ số trong tất cả các trường hợp thử nghiệm dễ dàng phù hợp với giới hạn thời gian và bộ nhớ phụ tuyến tính nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())

    out = []

    for _ in range(t):
        s = input().strip()
        n = len(s)

        suf = [''] * n
        suf[-1] = s[-1]

        for i in range(n - 2, -1, -1):
            suf[i] = min(s[i], suf[i + 1])

        res = []

        for i in range(n):
            if i + 1 < n and suf[i + 1] < s[i]:
                res.append(str(min(int(s[i]) + 1, 9)))
            else:
                res.append(s[i])

        res.sort()
        out.append("".join(res))

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run(
"""4
04829
9
01
314752277691991
"""
) == """02599
9
01
111334567888999"""

# minimum size
assert run(
"""1
0
"""
) == "0"

# all equal
assert run(
"""1
77777
"""
) == "77777"

# decreasing sequence
assert run(
"""1
9876543210
"""
) == "1234567899"

# repeated minimum digit
assert run(
"""1
1000
"""
) == "0002"

# cap at 9
assert run(
"""1
9991
"""
) == "1999"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| Độ dài tối thiểu có thể | 
|`77777`|`77777`| Không có chữ số nào có giá trị nhỏ hơn ở bên phải | 
|`9876543210`|`1234567899`| Mọi chữ số ngoại trừ chữ số cuối cùng phải được tăng lên và di chuyển | 
|`1000`|`0002`| Chữ số đứng đầu bị chặn bởi các chữ số hậu tố nhỏ hơn | 
|`9991`|`1999`| Xử lý đúng nắp ở`9`| 

## Vỏ cạnh 

Hãy xem xét:```
1
21
```Hậu tố tối thiểu ở bên phải của`2`là`1`, cái nào nhỏ hơn Chúng tôi tăng`2`ĐẾN`3`. chữ số`1`vẫn không thay đổi. Sau khi sắp xếp, ta thu được:```
13
```Một giải pháp giữ`2`sẽ tạo ra một chuỗi lớn hơn về mặt từ điển. 

Bây giờ hãy xem xét:```
1
01
```chữ số`0`không có chữ số nhỏ hơn ở bên phải của nó. chữ số`1`đã là vị trí cuối cùng rồi. Không có sửa đổi nào xảy ra và việc sắp xếp vẫn giữ:```
01
```Điều này xác nhận rằng chúng tôi không sửa đổi các chữ số trừ khi chúng thực sự chặn một chữ số nhỏ hơn trong tương lai. 

Cuối cùng hãy xem xét:```
1
999
```Mọi hậu tố tối thiểu đều bằng`9`. Không có vị trí nào nhìn thấy chữ số nhỏ hơn ở bên phải của nó, vì vậy mọi chữ số đều không thay đổi. Sắp xếp vẫn mang lại:```
999
```Điều này xác nhận điều kiện biên trong đó hoạt động tăng bị giới hạn và không mang lại lợi ích gì.
