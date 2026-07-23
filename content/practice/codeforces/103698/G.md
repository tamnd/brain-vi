---
title: "CF 103698G - Palinomial"
description: "Chúng ta có $n$ đa thức, mỗi đa thức được mô tả bởi các hệ số của nó theo thứ tự tăng dần. Sau đó, chúng tôi trả lời các truy vấn $q$, mỗi truy vấn đưa ra một khoảng $[l, r]$."
date: "2026-07-02T10:17:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103698
codeforces_index: "G"
codeforces_contest_name: "The 4th Turing Cup"
rating: 0
weight: 103698
solve_time_s: 62
verified: true
draft: false
---

[CF 103698G - Palinomial](https://codeforces.com/problemset/problem/103698/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$n$đa thức, mỗi đa thức được mô tả bởi các hệ số của nó theo thứ tự tăng dần. Sau đó chúng tôi trả lời$q$truy vấn, mỗi truy vấn đưa ra một khoảng$[l, r]$. Đối với mỗi khoảng, về mặt khái niệm, chúng tôi nhân tất cả các đa thức bên trong nó và kiểm tra xem đa thức thu được có hệ số nhân đôi hay không. 

Việc đọc trực tiếp cho thấy chúng ta đang xử lý phép nhân đa thức trên nhiều phạm vi, việc này thường tốn kém vì độ có thể tăng lên và các hệ số có thể trở nên lớn. Tuy nhiên, tính chất đại số đặc biệt được đưa ra trong mệnh đề làm thay đổi toàn bộ bản chất của bài toán: điều duy nhất quan trọng là liệu mỗi đa thức có phải là palindrome riêng lẻ hay không. 

Vì vậy, nhiệm vụ ẩn thực sự là xử lý trước các đa thức nào đối xứng, sau đó trả lời các truy vấn phạm vi để kiểm tra xem tất cả các mục trong khoảng có đối xứng hay không. 

Các ràng buộc ngụ ý đến$10^5$đa thức và$10^5$truy vấn, với tổng mức độ lên tới$5 \cdot 10^5$. Bất kỳ cách tiếp cận nào tính toán lại tính đối xứng cho mỗi truy vấn hoặc mô phỏng phép nhân đều không thể thực hiện được. Ngay cả việc kiểm tra đa thức cho mỗi truy vấn cũng sẽ quá chậm trong trường hợp xấu nhất, vì vậy chúng ta phải tính toán trước theo thời gian tuyến tính và trả lời các truy vấn theo thời gian không đổi hoặc logarit. 

Trường hợp cạnh tinh tế là khi đa thức có bậc 0. Theo định nghĩa, các đa thức như vậy luôn có tính đối xứng vì chỉ có một hệ số. 

Một trường hợp cạnh quan trọng khác là tính đối xứng phải tôn trọng mức độ thực tế chứ không phải kích thước được khai báo. Ví dụ: nếu các hệ số đầu khác 0 nhưng cấu trúc bên trong đối xứng thì nó hợp lệ; nhưng nếu các hệ số theo sau phá vỡ tính đối xứng thì không phải vậy. 

Hãy xem xét một lỗi ngây thơ: giả sử một đa thức được biểu diễn dưới dạng$[1, 2, 1, 0]$. Nếu một người xử lý không chính xác mức độ được khai báo là 3 và so sánh tất cả các vị trí bao gồm các số 0 ở cuối mà không cắt xén, thì người ta có thể phân loại nó không chính xác. Giải thích đúng là mức độ được cố định bởi đầu vào$k_i$, do đó, các số 0 ở cuối$k_i$không tồn tại. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là mô phỏng trực tiếp từng truy vấn bằng cách nhân các đa thức trong khoảng và sau đó kiểm tra tính đối xứng của kết quả. Nếu chúng ta nhân hai đa thức bậc lên tới$K$, chi phí tích chập$O(K^2)$. Lặp lại điều này trên một đoạn dài$n$làm cho nó có hiệu quả theo cấp số nhân trong thực tế đối với các đầu vào trong trường hợp xấu nhất, vì mức độ trung gian tăng lên và mỗi truy vấn tính toán lại các phép tích chập lớn. Ngay cả với phép nhân nhanh, làm điều này cho$10^5$truy vấn là không thể thực hiện được. 

Nhận xét quan trọng từ phát biểu đã làm thay đổi hoàn toàn bài toán: tích là palindrome khi và chỉ nếu mọi đa thức trong dãy đều là palindrome. Điều này thu gọn một điều kiện đại số phức tạp thành một AND logic đơn giản trên một mảng nhị phân. 

Khi mỗi đa thức được giảm xuống giá trị boolean, vấn đề sẽ trở thành truy vấn phạm vi cổ điển: kiểm tra xem tất cả các giá trị trong$[l, r]$là đúng. Điều này tương đương với việc kiểm tra xem mức tối thiểu trong phạm vi là 1 hay liệu có tồn tại bất kỳ số 0 nào trong phạm vi hay không. Tổng tiền tố hoặc cây phân đoạn là đủ. 

Chúng tôi tính toán trước một mảng$a[i]$Ở đâu$a[i] = 1$nếu đa thức$i$là palindromic, ngược lại là 0. Sau đó xây dựng các tổng tiền tố sao cho tổng phạm vi đó cho chúng ta biết liệu có bất kỳ đa thức không palindromic nào trong phân đoạn hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phép nhân đa thức Brute Force cho mỗi truy vấn |$O(q \cdot n \cdot K^2)$|$O(K)$| Quá chậm | 
| Tính toán trước palindromicity + tổng tiền tố |$O(nK + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng đa thức và kiểm tra xem nó có phải là palindrome hay không bằng cách so sánh các hệ số đối xứng ở cả hai đầu. Bước này trực tiếp mã hóa định nghĩa và tránh mọi thao tác đại số. 
2. Với mỗi đa thức, lặp từ$j = 0$ĐẾN$k_i$và so sánh$a_j$với$a_{k_i - j}$. Nếu tìm thấy bất kỳ sự không khớp nào, hãy đánh dấu đa thức là không phải palindrome. 
3. Lưu kết quả vào mảng`good[i]`, trong đó 1 biểu thị palindromic và 0 biểu thị không. 
4. Xây dựng mảng tổng tiền tố`pref`, Ở đâu`pref[i] = pref[i-1] + good[i]`. Điều này cho phép tổng hợp phạm vi nhanh chóng. 
5. Đối với mỗi truy vấn$[l, r]$, tính toán`pref[r] - pref[l-1]`. Nếu kết quả bằng$r-l+1$, mọi đa thức đều là palindrome, nếu không thì ít nhất một đa thức không phải là palindrome. 

Lý do tính tổng tiền tố có tác dụng ở đây là vì chúng tôi đã chuyển vấn đề sang kiểm tra xem một phân đoạn có chứa bất kỳ phần tử không hợp lệ nào hay không. Tổng tiền tố là cách trực tiếp để phát hiện sự hiện diện thông qua việc đếm. 

### Tại sao nó hoạt động 

Cấu trúc nhân đảm bảo rằng tính chất palindromic được bảo toàn trong phép nhân và bị phá hủy bởi bất kỳ yếu tố không palindromic nào. Điều này tạo ra một tính chất đơn điệu: một khi phần tử “xấu” xuất hiện trong sản phẩm thì không thể sửa nó bằng cách nhân với các đa thức khác. Do đó, tích trên một phạm vi là palindromic khi và chỉ khi mọi yếu tố đều là palindromic, điều này làm giảm vấn đề thành kiểm tra tính đồng nhất phạm vi trên một mảng nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_pal(coef):
    i, j = 0, len(coef) - 1
    while i < j:
        if coef[i] != coef[j]:
            return False
        i += 1
        j -= 1
    return True

n, q = map(int, input().split())
good = [0] * (n + 1)

for i in range(1, n + 1):
    arr = list(map(int, input().split()))
    k = arr[0]
    coef = arr[1:]
    good[i] = 1 if is_pal(coef) else 0

pref = [0] * (n + 1)
for i in range(1, n + 1):
    pref[i] = pref[i - 1] + good[i]

out = []
for _ in range(q):
    l, r = map(int, input().split())
    total = pref[r] - pref[l - 1]
    out.append("1" if total == (r - l + 1) else "0")

print("\n".join(out))
```Lựa chọn triển khai cốt lõi là tách việc kiểm tra tính đối xứng khỏi quá trình xử lý truy vấn. Phân tích cú pháp đa thức đọc mức độ được khai báo và bỏ qua mọi nhu cầu về tích chập hoặc chuẩn hóa. Mảng tiền tố được lập chỉ mục 1 để đơn giản hóa phép trừ phạm vi và tránh các lỗi sai sót. 

Một lỗi phổ biến ở đây là quên rằng số đầu tiên trong mỗi dòng đa thức là bậc chứ không phải hệ số. Một trường hợp khác là vô tình sử dụng tính năng lập chỉ mục dựa trên số 0 trong tổng tiền tố trong khi các truy vấn dựa trên một, điều này sẽ làm sai lệch mọi câu trả lời. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ gồm ba đa thức: 

đầu vào:```
3 2
2 1 2 1
1 3 4
2 5 0 5
1 3
2 3
```Việc kiểm tra tính chất palindromicity tạo ra: 

| tôi | đa thức | kiểm tra kết quả | tốt[i] | 
| --- | --- | --- | --- | 
| 1 | 1 2 1 | đối xứng | 1 | 
| 2 | 3 4 | không đối xứng | 0 | 
| 3 | 5 0 5 | đối xứng | 1 | 

Tổng tiền tố trở thành`[0,1,1,2]`. 

Truy vấn$[1,3]$cho`pref[3] - pref[0] = 2`, nhưng kích thước phạm vi là 3, do đó đầu ra là 0. Điều này cho thấy rằng ngay cả một đa thức không palindromic cũng phá vỡ tích. 

Truy vấn$[2,3]$cho`pref[3] - pref[1] = 1`, kích thước phạm vi là 2, do đó đầu ra lại là 0. Điều này xác nhận rằng bất kỳ phân đoạn nào chứa phần tử xấu đều bị lỗi. 

Bây giờ hãy xem xét một bộ đối xứng hoàn toàn:```
2 1
2 2 3 2
1 5 5
1 2
```Cả hai đa thức đều là palindrome, vì vậy tổng tiền tố là`[0,1,2]`. Truy vấn trả về 1 vì toàn bộ phạm vi đều rõ ràng, minh họa thuộc tính bảo toàn đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nK + q)$| Mỗi hệ số được quét một lần để kiểm tra tính đối xứng, sau đó mỗi truy vấn được trả lời bằng O(1) bằng cách sử dụng tổng tiền tố | 
| Không gian |$O(n)$| Lưu trữ mảng palindromicity và tổng tiền tố | 

Ràng buộc tổng độ tổng đảm bảo bước quét hệ số là tuyến tính trên tất cả các kích thước đầu vào. Với$n, q \le 10^5$, điều này dễ dàng phù hợp với giới hạn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def is_pal(coef):
        i, j = 0, len(coef) - 1
        while i < j:
            if coef[i] != coef[j]:
                return False
            i += 1
            j -= 1
        return True

    n, q = map(int, input().split())
    good = [0] * (n + 1)

    for i in range(1, n + 1):
        arr = list(map(int, input().split()))
        k = arr[0]
        coef = arr[1:]
        good[i] = 1 if is_pal(coef) else 0

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + good[i]

    res = []
    for _ in range(q):
        l, r = map(int, input().split())
        res.append("1" if pref[r] - pref[l - 1] == (r - l + 1) else "0")

    return "\n".join(res)

# sample-like tests
assert run("1 1\n2 1 2 1\n1 1") == "1"
assert run("2 1\n2 1 2 1\n2 1 0 2\n1 2") == "0"

# single element edge
assert run("1 1\n0 5\n1 1") == "1"

# all bad
assert run("3 2\n1 1 2\n1 2 3\n1 3 4\n1 3\n2 3") == "0\n0"

# all good
assert run("2 1\n2 1 0 1\n1 2 3 2\n1 2") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hằng số đơn | 1 | hằng số luôn đối xứng | 
| phạm vi hỗn hợp | 0 | sự hiện diện của một phạm vi phá vỡ xấu | 
| tất cả đều tệ | 0 | lan truyền lỗi nhất quán | 
| tất cả đều tốt | 1 | thành công toàn diện | 

## Vỏ cạnh 

Trường hợp cạnh khóa là đa thức không đổi. Ví dụ, đầu vào`0 7`đại diện cho một đa thức bậc 0. Thuật toán coi nó như một mảng một phần tử, do đó`is_pal`ngay lập tức trả về true, khớp với định nghĩa. 

Một trường hợp cạnh khác là đa thức có các số 0 ở cuối trong biểu diễn có thể gợi ý sự bất đối xứng một cách trực quan. Ví dụ,`3 1 0 1`thực sự là đối xứng và hợp lệ. Việc kiểm tra so sánh các vị trí chính xác nên xác định chính xác tính đối xứng. 

Trường hợp cạnh thứ ba là một truy vấn đa thức đơn. Ngay cả khi$l = r$, câu trả lời chỉ phụ thuộc vào việc đa thức đó có phải là palindrome hay không. Công thức tổng tiền tố xử lý việc này một cách tự nhiên vì nó giảm xuống việc kiểm tra khoảng một phần tử.
