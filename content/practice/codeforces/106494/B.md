---
title: "CF 106494B - Điểm nghỉ"
description: "Chúng ta được cung cấp một số khoảng độc lập và từ mỗi khoảng chúng ta phải chọn một giá trị duy nhất. Sau khi chọn một giá trị cho mỗi khoảng, chúng tôi coi các giá trị đó là độ lớn của vectơ."
date: "2026-06-19T17:33:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106494
codeforces_index: "B"
codeforces_contest_name: "MEPhI Spring Cup 2026"
rating: 0
weight: 106494
solve_time_s: 51
verified: true
draft: false
---

[CF 106494B - Điểm nghỉ](https://codeforces.com/problemset/problem/106494/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số khoảng độc lập và từ mỗi khoảng chúng ta phải chọn một giá trị duy nhất. Sau khi chọn một giá trị cho mỗi khoảng, chúng tôi coi các giá trị đó là độ lớn của vectơ. Câu hỏi quan trọng là liệu chúng ta có thể gán hướng cho các vectơ này sao cho tổng vectơ của chúng chính xác bằng 0 hay không. 

Hình học đằng sau bài toán quy mọi thứ về một điều kiện duy nhất trên các con số. Nếu một giá trị được chọn quá lớn so với tổng của tất cả những giá trị khác thì cho dù chúng ta định hướng hướng như thế nào, vectơ lớn nhất đó cũng không thể bị phần còn lại triệt tiêu hoàn toàn. Nếu không tồn tại sự mất cân bằng như vậy thì một định hướng hợp lệ luôn tồn tại trong một chiều, bởi vì chúng ta luôn có thể liên tục điều chỉnh các hướng để cân bằng hệ thống tại một số điểm. 

Mỗi khoảng thời gian góp phần linh hoạt, nhưng chỉ trong giới hạn của nó. Quyết định thực sự là làm thế nào để chọn một giá trị từ mỗi khoảng sao cho tập hợp nhiều giá trị được chọn có thể thỏa mãn điều kiện cân bằng. 

Các ràng buộc ngụ ý rằng chúng ta cần một giải pháp O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Bất kỳ cách tiếp cận nào cố gắng khám phá sự kết hợp của các lựa chọn trong các khoảng thời gian, thậm chí ở dạng rút gọn, sẽ ngay lập tức trở thành cấp số nhân và không thể sử dụng được khi n tăng vượt quá vài chục. 

Một trường hợp lỗi tinh vi xuất hiện khi khoảng thời gian lớn nhất không cố định tại điểm cuối tối đa của nó. Ví dụ, hãy xem xét các khoảng [5, 10], [1, 2], [1, 2]. Một cách tiếp cận ngây thơ có thể cho rằng việc chọn tất cả các mức tối đa luôn là tối ưu, nhưng điều đó có thể làm tăng giá trị lớn nhất một cách không cần thiết và từ chối một cấu hình khả thi một cách không chính xác. Lý do đúng phải xem xét việc điều chỉnh khoảng thời gian lớn nhất xuống dưới đồng thời tôn trọng tính khả thi. 

Một trường hợp cạnh khác phát sinh khi nhiều khoảng liên kết với giới hạn dưới tối đa hoặc khi giảm phần tử lớn nhất thay đổi thì khoảng đó trở nên chiếm ưu thế. Việc triển khai bất cẩn nhằm sửa giá trị tối đa theo giá trị r thay vì giá trị l sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ thử tất cả các lựa chọn có một giá trị trên mỗi khoảng và với mỗi lựa chọn, hãy kiểm tra xem liệu chúng ta có thể gán các dấu hiệu để làm cho tổng bằng 0 hay không. Việc kiểm tra một lựa chọn cố định sẽ giảm xuống việc xác minh xem có phần tử nào vượt quá một nửa tổng số hay không, là tuyến tính. Tuy nhiên, số lượng lựa chọn là tích của các độ dài khoảng, theo cấp số nhân tính bằng n, khiến điều này không thể thực hiện được ngoài những đầu vào rất nhỏ. 

Quan sát quan trọng là chúng ta thực sự không cần phải liệt kê các lựa chọn. Chúng tôi chỉ quan tâm đến việc liệu có tồn tại lựa chọn tránh được “yếu tố chiếm ưu thế” vượt quá tổng của tất cả những yếu tố khác hay không. Điều này biến vấn đề thành việc xây dựng một tập hợp nhiều giá trị được chọn hợp lệ. 

Chúng tôi xác định khoảng có giới hạn dưới lớn nhất vì khoảng này là khoảng khó giảm nhất. Chúng tôi tạm thời sửa một giá trị ứng cử viên từ nó và sau đó cố gắng đảm bảo tất cả các giá trị được chọn khác có thể hỗ trợ giá trị đó mà không vi phạm điều kiện cân bằng. 

Nếu chúng ta chọn một giá trị x từ khoảng tới hạn thì tất cả các khoảng khác đóng góp các giá trị tối đa vào giới hạn đã chọn của chúng và chúng ta muốn tổng của các khoảng khác ít nhất là x. Điều này chuyển vấn đề thành việc kiểm tra xem x có thể được hỗ trợ bởi các khoảng còn lại dưới các ràng buộc của chúng hay không. Chiến lược tham lam là tối đa hóa đóng góp từ các khoảng khác mà không vượt quá x, vì bất kỳ giá trị nào trên x đều không giúp cân bằng điều kiện hơn nữa. 

Điều này dẫn đến một cấu trúc trực tiếp: chọn khoảng có giới hạn dưới tối đa l_i làm điểm neo, đặt giá trị của nó thành l_i và với mỗi khoảng khác, hãy chọn min(r_j, l_i). Điều này tối đa hóa sự đóng góp vào việc hỗ trợ mỏ neo trong khi vẫn giữ được tính nhất quán với các giới hạn khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi tập trung vào việc xây dựng một lựa chọn khả thi hơn là tìm kiếm. 

### 1. Xác định khoảng quan trọng 

Chúng tôi tìm thấy khoảng thời gian với điểm cuối bên trái lớn nhất. Khoảng này là hạn chế nhất vì nó buộc bất kỳ giải pháp hợp lệ nào cũng phải có khả năng hỗ trợ ít nhất độ lớn đó. 

### 2. Tạm thời sửa giá trị đã chọn 

Chúng tôi đặt giá trị đã chọn cho khoảng này ở giới hạn dưới của nó. Chúng tôi không thử các giá trị lớn hơn vì việc tăng nó chỉ khiến việc cân bằng trở nên khó khăn hơn. 

### 3. Gán giá trị cho tất cả các khoảng khác một cách tham lam 

Đối với mọi khoảng thời gian khác, chúng tôi chọn giá trị tối đa không vượt quá giá trị tới hạn. Cụ thể, chúng tôi chọn min(r_j, l_max). Điều này đảm bảo mỗi khoảng thời gian đóng góp nhiều nhất có thể vào việc hỗ trợ giá trị tới hạn mà không phá vỡ tính khả thi. 

### 4. Kiểm tra tính khả thi thông qua so sánh tổng 

Chúng tôi tính tổng tổng của tất cả các giá trị đã chọn. Nếu tổng này ít nhất gấp đôi giá trị tới hạn đã chọn thì các giá trị còn lại có thể đối trọng với nó, nghĩa là có thể có cấu hình vectơ bằng 0. 

Nếu tổng nhỏ hơn thì không có cấu trúc hợp lệ nào tồn tại. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào việc chuyển đổi điều kiện hủy vectơ thành một ràng buộc thống trị duy nhất. Một cấu hình khả thi khi và chỉ khi không có phần tử nào vượt quá tổng của tất cả các phần tử khác sau khi chọn giá trị. Bằng cách neo khoảng thời gian giới hạn dưới lớn nhất và tối đa hóa tất cả những đóng góp khác cho khoảng thời gian neo đó, chúng tôi xây dựng môi trường hỗ trợ tốt nhất có thể cho khoảng thời gian đó. Nếu ngay cả mức hỗ trợ tối đa này cũng không thành công thì bất kỳ lựa chọn nào khác cũng sẽ chỉ làm giảm mức hỗ trợ hơn nữa, khiến cho tính khả thi là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    seg = [tuple(map(int, input().split())) for _ in range(n)]

    # find interval with maximum left endpoint
    i_max = 0
    for i in range(1, n):
        if seg[i][0] > seg[i_max][0]:
            i_max = i

    x = seg[i_max][0]

    total = 0
    for i, (l, r) in enumerate(seg):
        if i == i_max:
            total += x
        else:
            if r < x:
                total += r
            else:
                total += x

    # feasibility condition
    print("YES" if total >= 2 * x else "NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã xác định khoảng có giới hạn dưới lớn nhất, vì khoảng đó xác định ràng buộc khó đáp ứng nhất. Sau đó, nó sửa giá trị đã chọn ở giới hạn dưới đó. Đối với mỗi khoảng thời gian khác, nó đóng góp nhiều nhất có thể vào việc hỗ trợ mỏ neo này, nhưng không bao giờ vượt quá nó. 

Kiểm tra cuối cùng so sánh tổng mức đóng góp với hai lần neo, mã hóa xem các vectơ còn lại có thể đối trọng hoàn toàn với nó hay không. 

Một điểm tinh tế là chúng tôi không bao giờ tính toán rõ ràng “tổng của những người khác” một cách riêng biệt; thay vào đó, chúng tôi đưa điểm cố định vào tổng số và so sánh với 2x. Điều này tránh được những sai sót riêng lẻ và giữ cho việc triển khai được tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét các khoảng: 

[3, 5], [1, 2], [2, 4] 

Điểm cuối bên trái tối đa là 3, vì vậy x = 3. 

| Khoảng thời gian | Giá trị được chọn | Đóng góp | 
| --- | --- | --- | 
| [3,5] | 3 | 3 | 
| [1,2] | 2 | 2 | 
| [2,4] | 3 | 3 | 

Tổng = 8 và 2x = 6, vì vậy điều kiện đúng. 

Điều này cho thấy rằng mặc dù một số khoảng không thể đạt tới 3 nhưng mức đóng góp tối đa của chúng vẫn đủ để cân bằng mỏ neo. 

### Ví dụ 2 

Hãy xem xét các khoảng: 

[10, 10], [1, 2], [1, 2] 

Ở đây x = 10. 

| Khoảng thời gian | Giá trị được chọn | Đóng góp | 
| --- | --- | --- | 
| [10,10] | 10 | 10 | 
| [1,2] | 2 | 2 | 
| [1,2] | 2 | 2 | 

Tổng = 14 và 2x = 20, do đó điều kiện không thành công. 

Điều này thể hiện dạng lỗi trong đó một khoảng quá lớn so với tất cả các khoảng khác, ngay cả khi được hỗ trợ tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để tìm điểm cuối bên trái tối đa và một lần để tính tổng | 
| Không gian | O(1) | chỉ các biến phụ không đổi bên cạnh bộ nhớ đầu vào | 

Giải pháp này dễ dàng phù hợp trong giới hạn vì mỗi trường hợp thử nghiệm chỉ yêu cầu quét tuyến tính theo các khoảng thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input())
    seg = [tuple(map(int, input().split())) for _ in range(n)]

    i_max = 0
    for i in range(1, n):
        if seg[i][0] > seg[i_max][0]:
            i_max = i

    x = seg[i_max][0]

    total = 0
    for i, (l, r) in enumerate(seg):
        total += x if (i == i_max and l <= x <= r) else min(r, x)

    print("YES" if total >= 2 * x else "NO")

# sample-like cases
assert run("3\n3 5\n1 2\n2 4\n") == "YES"
assert run("3\n10 10\n1 2\n1 2\n") == "NO"

# minimum case
assert run("1\n0 5\n") == "YES"

# tight boundary case
assert run("2\n5 5\n5 5\n") == "YES"

# impossible spread
assert run("2\n10 10\n1 1\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 quãng hỗn hợp | CÓ | trường hợp khả thi tiêu chuẩn | 
| khoảng lớn chiếm ưu thế | KHÔNG | phát hiện không thể | 
| khoảng đơn | CÓ | trường hợp cân bằng tầm thường | 
| khoảng cách chặt chẽ bằng nhau | CÓ | xử lý bình đẳng ranh giới | 
| mất cân bằng cực độ | KHÔNG | từ chối trong trường hợp xấu nhất | 

## Vỏ cạnh 

Trường hợp khoảng đơn là đơn giản nhất. Đối với đầu vào [0, 5], thuật toán chọn x = 0 và thỏa mãn ngay điều kiện vì không có ràng buộc nào khác. Tổng được tính bằng 0 và 2x cũng bằng 0, vì vậy câu trả lời là CÓ. 

Trường hợp khoảng bằng nhau bị ràng buộc chặt chẽ, chẳng hạn như [5,5], [5,5] đặt x = 5. Cả hai khoảng đều đóng góp 5, cho tổng 10 bằng 2x, tạo ra CÓ. Điều này xác nhận việc xử lý bình đẳng là đúng và không yêu cầu bất bình đẳng nghiêm ngặt. 

Trường hợp mất cân bằng mạnh như [10,10], [1,1] đặt x = 10. Khoảng thứ hai chỉ đóng góp 1, tạo thành tổng 11, nhỏ hơn nhiều so với 20, do đó thuật toán sẽ loại bỏ nó một cách chính xác. Điều này cho thấy ngay cả khi các khoảng thời gian nhỏ hơn được tối đa hóa, chúng cũng không thể bù đắp cho khoảng thời gian neo quá lớn.
