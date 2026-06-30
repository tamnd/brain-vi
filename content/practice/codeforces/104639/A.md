---
title: "CF 104639A - Quy tắc xếp hạng vòng loại"
description: "Chúng tôi nhận được kết quả theo thứ tự của hai cuộc thi lập trình riêng biệt. Mỗi cuộc thi đều xếp hạng các đội riêng lẻ, nhưng điều quan trọng cuối cùng là thành tích của các trường đại học chứ không phải các đội riêng lẻ."
date: "2026-06-29T16:55:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104639
codeforces_index: "A"
codeforces_contest_name: "The 2023 ICPC Asia EC Regionals Online Contest (I)"
rating: 0
weight: 104639
solve_time_s: 56
verified: true
draft: false
---

[CF 104639A - Quy tắc xếp hạng người đủ điều kiện](https://codeforces.com/problemset/problem/104639/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được kết quả theo thứ tự của hai cuộc thi lập trình riêng biệt. Mỗi cuộc thi đều xếp hạng các đội riêng lẻ, nhưng điều quan trọng cuối cùng là thành tích của các trường đại học chứ không phải các đội riêng lẻ. 

Đối với mỗi cuộc thi, một trường đại học được đại diện bởi nhiều đội, nhưng chỉ có đội có thành tích tốt nhất mới quan trọng. Vì vậy, bước đầu tiên trong cả hai cuộc thi là nén thứ hạng cấp độ nhóm thành thứ hạng cấp đại học bằng cách giữ cho mỗi trường đại học vị trí tốt nhất (sớm nhất) xuất hiện trong danh sách của cuộc thi đó. 

Khi chúng ta có hai bảng xếp hạng các trường đại học, mỗi bảng xếp hạng cho một cuộc thi, chúng ta cần hợp nhất chúng thành một thứ tự cuối cùng duy nhất. Quy tắc hợp nhất hoạt động giống như một sự hợp nhất ổn định của hai danh sách được sắp xếp: chúng tôi liên tục so sánh các trường đại học dựa trên thứ hạng của họ trong mỗi cuộc thi. Nếu một trường đại học xuất hiện sớm hơn trong bảng xếp hạng cuộc thi đầu tiên so với trường đại học khác thì được coi là tốt hơn; nếu họ có cùng thứ hạng trong các cuộc thi thì vị trí của cuộc thi đầu tiên sẽ được ưu tiên. 

Sau khi hợp nhất, các trường trùng lặp sẽ xuất hiện do các trường đại học xuất hiện trong cả hai danh sách. Chúng tôi chỉ giữ lại lần xuất hiện đầu tiên của mỗi trường đại học trong trình tự đã hợp nhất, giữ nguyên trật tự. 

Đầu ra là bảng xếp hạng cuối cùng được loại bỏ trùng lặp của các trường đại học. 

Các ràng buộc cho phép tối đa 10^4 đội trong mỗi cuộc thi, do đó, việc so sánh O(n^2) đơn giản hoặc quét lặp lại để tìm thứ hạng tốt nhất đã là giới hạn nhưng vẫn có thể được chấp nhận trong Python. Tuy nhiên, bước hợp nhất phải tuyến tính sau khi xử lý trước, nếu không, nó sẽ TLE nếu được thực hiện bằng cách xóa danh sách lặp đi lặp lại hoặc kiểm tra tư cách thành viên lặp lại trong danh sách. 

Một trường hợp thất bại tinh vi sẽ xuất hiện nếu một người cố gắng hợp nhất trực tiếp danh sách nhóm mà không bị tụt xuống thứ hạng đại học trước. Điều đó sẽ coi nhiều đội của cùng một trường đại học là những đối thủ riêng biệt. 

Một trường hợp khác nảy sinh khi một trường đại học chỉ xuất hiện trong một cuộc thi. Nó vẫn phải xuất hiện trong bảng xếp hạng cuối cùng và phải được hợp nhất một cách chính xác mà không bị phạt giả tạo. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng toàn bộ quá trình theo đúng nghĩa đen. Đối với mỗi cuộc thi, hãy quét danh sách xếp hạng và đối với mỗi trường đại học, hãy lưu chỉ số xuất hiện đầu tiên làm điểm số. Điều này tạo ra hai từ điển ánh xạ trường đại học để xếp hạng. 

Sau đó, chúng tôi liên tục xây dựng một chuỗi đã hợp nhất. Ở mỗi bước, chúng tôi so sánh tất cả các trường đại học còn lại chưa được xếp hạng và chọn trường tốt nhất theo quy tắc so sánh, bổ sung và loại bỏ trường đó khỏi quá trình xem xét. Điều này tương đương với việc liên tục chọn mức tối thiểu theo thứ tự tùy chỉnh. 

Vấn đề là bước lựa chọn này rất tốn kém. Nếu có U trường đại học, việc chọn trường tốt nhất tiếp theo yêu cầu quét các ứng viên O(U), lặp lại O(U) lần, dẫn đến O(U^2). Với U tối đa 10^4, việc này trở thành khoảng 10^8 thao tác, quá chậm trong Python. 

Điều quan trọng là sau khi chuyển đổi từng cuộc thi thành danh sách xếp hạng các trường đại học duy nhất, cả hai danh sách đều đã được sắp xếp theo thứ tự mong muốn. Quy tắc cuối cùng chính xác là sự hợp nhất ổn định của hai chuỗi đã được sắp xếp, trong đó việc so sánh mang tính từ điển dựa trên các vị trí xếp hạng trong mỗi danh sách, với điểm phân chia ưu tiên cho danh sách đầu tiên. 

Điều này làm giảm vấn đề khi hợp nhất hai danh sách được sắp xếp trước bằng cách sử dụng hai con trỏ, sau đó là loại bỏ trùng lặp bằng cách sử dụng một tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lựa chọn vũ phu | O(U^2) | O(U) | Quá chậm | 
| Hợp nhất hai con trỏ | O(n + m) | O(U) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén danh sách đội của mỗi cuộc thi thành một bảng xếp hạng trường đại học duy nhất.

1. Đọc danh sách cuộc thi đầu tiên từ trên xuống dưới. Đối với mỗi trường đại học, nếu chưa từng thấy trước đây, hãy ghi lại theo thứ tự. Điều này tạo ra danh sách xếp hạng đầu tiên. Điều tương tự cũng được thực hiện cho cuộc thi thứ hai. Bước này hợp lệ vì chỉ có đội giỏi nhất ở mỗi trường đại học mới quan trọng và lần xuất hiện sớm nhất tương ứng với thứ hạng tốt nhất. 
2. Hãy coi hai danh sách này là những chuỗi có thứ tự. Bây giờ chúng ta hợp nhất chúng bằng cách sử dụng hai con trỏ, một con trỏ trỏ vào mỗi danh sách. Ở mỗi bước, chúng tôi so sánh các ứng cử viên hiện tại từ cả hai danh sách. 
3. Nếu cả hai con trỏ đều trỏ đến các trường đại học khác nhau, chúng tôi sẽ thêm trường xuất hiện sớm hơn vào thứ tự cuối cùng. Quy tắc đặt hàng là vị trí trước đó trong danh sách đầu tiên sẽ chiếm ưu thế; nếu không chúng ta sẽ so sánh các vị trí trong danh sách thứ hai. Trên thực tế, điều này tương đương với việc hợp nhất dựa trên cặp (rank_in_first, Rank_in_second), với các giá trị bị thiếu được coi là vô cùng. 
4. Nếu một danh sách đã hết, chúng tôi sẽ nối thêm các phần tử còn lại từ danh sách kia. 
5. Trong quá trình sáp nhập, chúng tôi duy trì một nhóm các trường đại học đã có đầu ra. Nếu một trường đại học đã được xuất ra, chúng tôi sẽ bỏ qua nó. Điều này là cần thiết vì một trường đại học có thể xuất hiện trong cả hai danh sách và các trường trùng lặp phải được loại bỏ ở đầu ra cuối cùng. 
6. Tiếp tục cho đến khi cả hai danh sách được xử lý đầy đủ. 

Sau các bước này, danh sách đầu ra chứa mỗi trường đại học đúng một lần theo đúng thứ tự đã hợp nhất. 

### Tại sao nó hoạt động 

Mỗi danh sách cuộc thi đã được sắp xếp theo thứ hạng tăng dần. Quy tắc hợp nhất xác định thứ tự nhất quán giữa hai trường đại học bất kỳ dựa trên vị trí tương đối của chúng trong hai chuỗi được sắp xếp này. Bởi vì sự so sánh mang tính bắc cầu và nhất quán với thứ tự từ điển của các cặp thứ hạng, nên chuỗi hợp nhất được tạo ra bởi quy trình hai con trỏ sẽ duy trì thứ tự chung. Việc loại bỏ trùng lặp không ảnh hưởng đến tính chính xác vì một khi một trường đại học xuất hiện theo thứ tự được hợp nhất, bất kỳ sự xuất hiện nào sau đó đều thể hiện vị trí tồi tệ hơn và có thể được bỏ qua một cách an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_ranking(arr):
    seen = set()
    res = []
    for x in arr:
        if x not in seen:
            seen.add(x)
            res.append(x)
    return res

def solve():
    n, m = map(int, input().split())
    a = [input().strip() for _ in range(n)]
    b = [input().strip() for _ in range(m)]

    r1 = build_ranking(a)
    r2 = build_ranking(b)

    pos1 = {u: i for i, u in enumerate(r1)}
    pos2 = {u: i for i, u in enumerate(r2)}

    i = j = 0
    used = set()
    out = []

    while i < len(r1) or j < len(r2):
        while i < len(r1) and r1[i] in used:
            i += 1
        while j < len(r2) and r2[j] in used:
            j += 1

        if i >= len(r1) and j >= len(r2):
            break
        if i >= len(r1):
            u = r2[j]
            j += 1
        elif j >= len(r2):
            u = r1[i]
            i += 1
        else:
            u1 = r1[i]
            u2 = r2[j]

            if pos1.get(u1, float('inf')) < pos1.get(u2, float('inf')):
                u = u1
                i += 1
            elif pos1.get(u1, float('inf')) > pos1.get(u2, float('inf')):
                u = u2
                j += 1
            else:
                if pos2.get(u1, float('inf')) <= pos2.get(u2, float('inf')):
                    u = u1
                    i += 1
                else:
                    u = u2
                    j += 1

        if u not in used:
            used.add(u)
            out.append(u)

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ nén các trường đại học trùng lặp trong mỗi danh sách cuộc thi, chỉ giữ lại những lần xuất hiện đầu tiên vì những trường đó đại diện cho thứ hạng của đội tốt nhất. Sau đó, nó xây dựng bản đồ vị trí để có thể thực hiện so sánh trong thời gian không đổi. Vòng lặp hợp nhất nâng cao con trỏ một cách cẩn thận trong khi bỏ qua các trường đại học đã được sử dụng để tránh xử lý dư thừa. Điều kiện ràng buộc đảm bảo hành vi xác định khi các trường đại học có thứ tự tương đối ngang nhau. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ: 

đầu vào:```
5 4
A
B
A
C
D
B
C
E
C
```Thứ hạng duy nhất của cuộc thi đầu tiên trở thành A, B, C, D. Thứ hai trở thành B, C, E. 

| Bước | tôi con trỏ | con trỏ j | đã chọn | đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | A | B | A | A | 
| 2 | B | B | B (hòa giải được xử lý theo ưu tiên danh sách đầu tiên) | A B | 
| 3 | C | C | C | A B C | 
| 4 | D | E | D | A B C D | 
| 5 | - | E | E | A B C D E | 

Điều này xác nhận rằng việc hợp nhất tôn trọng cả thứ hạng và duy trì sự ổn định trong các tình huống hòa. 

Một trường hợp khác: 

đầu vào:```
3 3
X
Y
Z
Z
Y
X
```Xếp hạng thứ nhất: X, Y, Z. Xếp hạng thứ hai: Z, Y, X. 

| Bước | r1 | r2 | đã chọn | đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | X | Z | X | X | 
| 2 | Y | Z | Y | X Y | 
| 3 | Z | Z | Z | X Y Z | 

Điều này cho thấy ngay cả khi thứ hạng bị đảo ngược thì việc hợp nhất vẫn tạo ra một thứ tự xác định nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi danh sách được quét một lần để xây dựng thứ hạng và một lần trong quá trình hợp nhất | 
| Không gian | O(u) | Lưu trữ bản đồ vị trí và danh sách đầu ra của các trường đại học độc đáo | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong giới hạn 10^4 mục nhập cho mỗi cuộc thi và các thao tác từ điển đảm bảo so sánh theo thời gian liên tục trong quá trình hợp nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case
assert run("""5 4
A
B
A
C
D
B
C
E
C
""") == "A\nB\nC\nD\nE"

# reverse ordering
assert run("""3 3
X
Y
Z
Z
Y
X
""") == "X\nY\nZ"

# single university
assert run("""3 2
UNI
UNI
UNI
UNI
UNI
""") == "UNI"

# disjoint sets
assert run("""3 3
A
B
C
D
E
F
""") == "A\nB\nC\nD\nE\nF"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường đại học lặp lại duy nhất | ĐƠN VỊ | tính đúng đắn của sự trùng lặp | 
| đảo ngược thứ hạng | X Y Z | sự ổn định ràng buộc | 
| bộ rời rạc | đơn hàng hợp nhất | trường hợp nối đơn giản | 
| đầu vào nặng trùng lặp | sửa đầu ra duy nhất | nén lần đầu tiên | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một trường đại học xuất hiện nhiều lần trong một danh sách cuộc thi. Ví dụ: nếu đầu vào là`A A A A B`, bước nén chỉ đảm bảo`A B`vẫn còn. Trong quá trình thực thi, thuật toán sẽ bỏ qua những lần xuất hiện sau đó vì`seen`đặt bộ lọc chúng ngay lập tức. Nếu không có bước này, logic hợp nhất sẽ xử lý không chính xác các lần xuất hiện lặp lại dưới dạng các mục nhập riêng biệt và tạo ra các bản sao. 

Một trường hợp khác xảy ra khi một trường đại học chỉ xuất hiện trong một cuộc thi. Ví dụ, nếu`X`ở vị trí thứ nhất nhưng không ở vị trí thứ hai, vị trí của nó trong`pos2`mặc định là vô cùng. Trong quá trình so sánh, điều này đảm bảo nó được đặt hoàn toàn theo thứ tự cuộc thi đầu tiên, phù hợp với quy tắc đã định rằng việc vắng mặt trong cuộc thi sẽ không bị phạt. 

Trường hợp cuối cùng là sự đảo ngược hoàn toàn giữa các cuộc thi. Ngay cả khi hai thứ hạng đối lập nhau, quy tắc so sánh theo cặp vẫn đảm bảo thứ tự xác định vì mọi so sánh đều được giải quyết dựa trên thứ tự từ điển nhất quán của các cặp thứ hạng và mối quan hệ bị phá vỡ do ưu tiên cuộc thi đầu tiên.
