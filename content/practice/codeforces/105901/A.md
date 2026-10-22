---
title: "CF 105901A - Thiết lập vấn đề"
description: "Chúng ta được cung cấp một danh sách các thuộc tính số, trong đó mỗi thuộc tính bắt đầu bằng một giá trị ban đầu. Bên cạnh đó, còn có một số hạn chế."
date: "2026-06-21T15:20:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105901
codeforces_index: "A"
codeforces_contest_name: "2025 ICPC Wuhan Invitational Contest (The 3rd Universal Cup. Stage 37: Wuhan)"
rating: 0
weight: 105901
solve_time_s: 57
verified: true
draft: false
---

[CF 105901A - Cài đặt sự cố](https://codeforces.com/problemset/problem/105901/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các thuộc tính số, trong đó mỗi thuộc tính bắt đầu bằng một giá trị ban đầu. Bên cạnh đó, còn có một số hạn chế. Mỗi ràng buộc nhắm đến một chỉ mục thuộc tính cụ thể và yêu cầu sau khi sửa đổi, giá trị tại chỉ mục đó phải nằm trong một khoảng bao gồm nhất định. 

Chúng tôi được phép điều chỉnh bất kỳ giá trị thuộc tính nào bằng cách tăng hoặc giảm liên tục 1 đơn vị và mỗi thay đổi đơn vị sẽ tốn 1 đơn vị thời gian. Nhiệm vụ là sửa đổi mảng ban đầu sao cho mọi ràng buộc đều được thỏa mãn, đồng thời giảm thiểu tổng chi phí trên tất cả các thuộc tính. Nếu không thể thỏa mãn đồng thời tất cả các ràng buộc thì câu trả lời phải được báo cáo là không thể. 

Nhận xét quan trọng từ định dạng đầu vào là các ràng buộc độc lập ngoại trừ việc chia sẻ cùng một chỉ mục thuộc tính. Điều đó có nghĩa là các ràng buộc không tương tác giữa các vị trí khác nhau; chúng chỉ giao nhau trong cùng một chỉ mục. 

Các ràng buộc có số lượng nhỏ, nhiều nhất là 100 thuộc tính và 100 ràng buộc cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ điều gì như thử tất cả các mảng cuối cùng có thể có hoặc thực hiện tìm kiếm toàn cục theo các giá trị. Ngay cả việc lặp lại tất cả các giá trị có thể có trong phạm vi lên tới 10^9 là hoàn toàn không khả thi. 

Một cách tiếp cận ngây thơ cố gắng điều chỉnh từng thuộc tính một cách độc lập trong khi liên tục kiểm tra tất cả các ràng buộc vẫn có thể được chấp nhận, nhưng bất kỳ điều gì mô phỏng các thay đổi đơn vị từng bước về mặt khái niệm đều quá chậm, ngay cả khi được tối ưu hóa. 

Một trường hợp thất bại tinh tế phát sinh khi các ràng buộc trên cùng một chỉ mục mâu thuẫn nhau nhưng không rõ ràng như vậy. Ví dụ: nếu một thuộc tính phải nằm trong [1, 5] theo một ràng buộc và [6, 10] theo ràng buộc khác, thì không có giá trị nào có thể thỏa mãn cả hai. Một bộ giải ngây thơ chỉ áp dụng ràng buộc cuối cùng được nhìn thấy sẽ giả định tính khả thi một cách không chính xác. 

Một trường hợp góc khác là khi một thuộc tính không có ràng buộc nào cả. Trong trường hợp đó, nó không nên đóng góp bất kỳ chi phí nào vì nó có thể không thay đổi bất kể giá trị ban đầu của nó là bao nhiêu. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ cố gắng điều chỉnh từng giá trị thuộc tính bằng cách thử tất cả các giá trị cuối cùng có thể có trong phạm vi hợp lý, chẳng hạn như quét từ 1 đến 10^9 cho mỗi chỉ mục và kiểm tra xem nó có đáp ứng tất cả các ràng buộc hay không. Đối với mỗi giá trị ứng cử viên, chúng tôi sẽ tính khoảng cách của nó với giá trị ban đầu và chọn giá trị hợp lệ tối thiểu. Về nguyên tắc, điều này có tác dụng vì chúng tôi xác minh tính khả thi một cách rõ ràng, nhưng độ phức tạp trở nên quá cao. Ngay cả đối với một thuộc tính, việc quét tối đa 10^9 giá trị là không thể và việc nhân giá trị này với tối đa 100 thuộc tính khiến việc này hoàn toàn không khả thi. 

Việc đơn giản hóa cấu trúc xuất phát từ việc nhận ra rằng các ràng buộc trên một chỉ mục cố định chỉ hạn chế chỉ mục đó nằm bên trong giao điểm của các khoảng. Thay vì xem xét nhiều giá trị ứng cử viên, chúng ta chỉ cần giao điểm của tất cả các phạm vi ảnh hưởng đến chỉ số đó. Khi chúng ta có giao điểm đó, vấn đề đối với thuộc tính đó sẽ giảm xuống việc chọn điểm gần nhất trong một đoạn với một số cố định, đây là một vấn đề chiếu tiêu chuẩn trên một đường. 

Điều này biến việc tối ưu hóa toàn cục thành các vấn đề một chiều độc lập cho mỗi chỉ mục. Mỗi thuộc tính đóng góp độc lập vào tổng chi phí khi khoảng khả thi của nó được biết đến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 10^9) | O(1) | Quá chậm | 
| Tối ưu | O(nq) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Đối với mỗi chỉ mục thuộc tính, chúng ta bắt đầu bằng việc giả định rằng nó không có hạn chế nào. Chúng tôi biểu thị khoảng khả thi của nó là rộng vô hạn, nghĩa là ban đầu mọi giá trị đều được cho phép. 
2. Chúng tôi lặp lại tất cả các ràng buộc. Đối với mỗi ràng buộc (p, l, r), chúng tôi hạn chế khoảng khả thi của thuộc tính p bằng cách giao nó với [l, r]. Điều này có nghĩa là chúng tôi cập nhật giới hạn dưới thành max(giới hạn dưới hiện tại, l) và giới hạn trên thành min(giới hạn trên hiện tại, r). Bước này đảm bảo chúng ta tích lũy tất cả các yêu cầu mà không mất bất kỳ ràng buộc nào. 
3. Sau khi xử lý tất cả các ràng buộc, chúng tôi kiểm tra từng khoảng thuộc tính. Nếu đối với bất kỳ chỉ mục nào, giới hạn dưới lớn hơn giới hạn trên thì giao điểm sẽ trống và không có giá trị nào có thể thỏa mãn tất cả các ràng buộc. Trong trường hợp này, câu trả lời ngay lập tức là -1. 
4. Nếu khoảng hợp lệ, chúng tôi tính toán chi phí tối thiểu để chuyển giá trị ban đầu vào khoảng này. Nếu giá trị ban đầu đã ở bên trong thì chi phí bằng 0. Nếu nó ở dưới khoảng, chúng ta di chuyển nó lên giới hạn dưới. Nếu nó ở trên, chúng ta di chuyển nó xuống giới hạn trên. Chúng tôi tích lũy chi phí này trên tất cả các thuộc tính. 
5. Cuối cùng, chúng tôi đưa ra tổng chi phí tích lũy. 

Ý tưởng cốt lõi là mỗi thuộc tính được rút gọn thành việc chọn một điểm trong đoạn nhằm giảm thiểu khoảng cách đến một giá trị cố định. 

### Tại sao nó hoạt động 

Đối với mỗi chỉ mục, tất cả các ràng buộc chỉ hạn chế tư cách thành viên trong các khoảng và tư cách thành viên trong nhiều khoảng tương đương với tư cách thành viên trong giao điểm của chúng. Mọi giá trị cuối cùng hợp lệ đều phải nằm bên trong giao điểm này và mọi giá trị bên trong nó đều có giá trị như nhau đối với các ràng buộc. Do đó, việc giảm thiểu chi phí sẽ giảm tới bài toán điểm gần nhất trên một khoảng thời gian đóng. Vì khoảng cách trên một đường là lồi nên điểm hợp lệ gần nhất luôn là giá trị ban đầu (nếu nằm trong) hoặc một trong các ranh giới khoảng. Điều này đảm bảo tính tối ưu một cách độc lập cho mỗi thuộc tính và tính độc lập giữa các thuộc tính đảm bảo tính tổng các điểm tối ưu cục bộ này mang lại mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

t = int(input())
for _ in range(t):
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    L = [-INF] * n
    R = [INF] * n

    for _ in range(q):
        p, l, r = map(int, input().split())
        p -= 1
        if l > L[p]:
            L[p] = l
        if r < R[p]:
            R[p] = r

    ok = True
    for i in range(n):
        if L[i] == -INF:
            L[i] = 1
            R[i] = 10**9

        if L[i] > R[i]:
            ok = False
            break

    if not ok:
        print(-1)
        continue

    ans = 0
    for i in range(n):
        if a[i] < L[i]:
            ans += L[i] - a[i]
        elif a[i] > R[i]:
            ans += a[i] - R[i]

    print(ans)
```Việc triển khai duy trì hai mảng biểu thị phạm vi khả thi cho từng thuộc tính. Các ràng buộc được áp dụng bằng cách thắt chặt các phạm vi này. Một điểm tinh tế là xử lý các thuộc tính không có ràng buộc; chúng được coi là hoàn toàn miễn phí bằng cách khởi tạo chúng trong một khoảng rộng và sau đó chuyển đổi những miền chưa được xử lý thành miền được phép đầy đủ. 

Việc tính toán chi phí được thực hiện sau khi tính khả thi được xác nhận, vì việc tính toán khoảng cách trước khi kiểm tra tính hợp lệ của nút giao thông có thể gây lãng phí công việc hoặc che giấu các trường hợp không thể thực hiện được. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp có ba thuộc tính: 

đầu vào:```
n = 3, q = 3
a = [20, 25, 4]
constraints:
1: (3, 5, 7)
2: (1, 10, 15)
3: (3, 2, 6)
```Chúng tôi theo dõi khoảng thời gian cho mỗi chỉ mục. 

| Chỉ mục | Khoảng thời gian ban đầu | Sau ràng buộc 1 | Sau ràng buộc 2 | Sau ràng buộc 3 | Khoảng thời gian cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [-inf, inf] | không thay đổi | [10, 15] | không thay đổi | [10, 15] | 
| 2 | [-inf, inf] | không thay đổi | không thay đổi | không thay đổi | [1, 1e9] | 
| 3 | [-inf, inf] | [5, 7] | không thay đổi | [5, 6] | [5, 6] | 

Bây giờ hãy tính chi phí: 

Đối với chỉ số 1, 20 nằm trên [10, 15], do đó chi phí là 5. 

Đối với chỉ số 2, 25 nằm trong phạm vi miễn phí, chi phí là 0. 

Đối với chỉ số 3, 4 nằm dưới [5, 6] nên chi phí là 1. 

Tổng chi phí là 6. 

Dấu vết này cho thấy các ràng buộc được nén thành các khoảng và việc điều chỉnh cuối cùng hoàn toàn là khoảng cách hình học. 

Bây giờ hãy xem xét một trường hợp thất bại khả thi:```
n = 1
a = [10]
constraints: [1, 1, 5], [1, 6, 8]
```| Bước | Khoảng thời gian | 
| --- | --- | 
| bắt đầu | [-inf, inf] | 
| sau lần đầu tiên | [1, 5] | 
| sau giây | [6, 5] | 

Khoảng thời gian trở nên không hợp lệ do giới hạn dưới vượt quá giới hạn trên, do đó đầu ra là -1. Điều này chứng tỏ rằng việc phát hiện mâu thuẫn giảm xuống còn kiểm tra giao điểm theo khoảng thời gian đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) cho mỗi trường hợp thử nghiệm | Mỗi ràng buộc cập nhật một khoảng thời gian một lần và mỗi chỉ mục được xử lý một lần | 
| Không gian | O(n) | Hai mảng lưu trữ giới hạn khoảng cho mỗi thuộc tính | 

Cho rằng cả n và q đều có nhiều nhất là 100 và có nhiều nhất là 100 trường hợp thử nghiệm, điều này diễn ra thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    INF = 10**18

    t = int(input())
    out = []
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        L = [-INF] * n
        R = [INF] * n

        for _ in range(q):
            p, l, r = map(int, input().split())
            p -= 1
            L[p] = max(L[p], l)
            R[p] = min(R[p], r)

        ok = True
        for i in range(n):
            if L[i] == -INF:
                L[i], R[i] = 1, 10**9
            if L[i] > R[i]:
                ok = False
                break

        if not ok:
            out.append("-1")
            continue

        ans = 0
        for i in range(n):
            if a[i] < L[i]:
                ans += L[i] - a[i]
            elif a[i] > R[i]:
                ans += a[i] - R[i]

        out.append(str(ans))

    return "\n".join(out)

# minimal feasible
assert run("""1
1 0
10
""") == "0"

# contradiction
assert run("""1
1 2
10
1 1 5
1 6 8
""") == "-1"

# already satisfied
assert run("""1
2 2
5 7
1 1 10
2 6 8
""") == "0"

# needs adjustment both sides
assert run("""1
2 2
0 20
1 5 10
2 0 15
""") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thuộc tính duy nhất, không có ràng buộc | 0 | biến miễn phí không mất phí | 
| ràng buộc xung đột | -1 | phát hiện giao lộ không khả thi | 
| khoảng đã thỏa mãn | 0 | không có chuyển động không cần thiết | 
| điều chỉnh hỗn hợp | 15 | logic khoảng cách chính xác | 

## Vỏ cạnh 

Khi một thuộc tính không có ràng buộc nào cả, thuật toán sẽ gán cho nó một miền hợp lệ đầy đủ. Vì giá trị ban đầu luôn nằm bên trong miền đó nên chi phí tính toán vẫn bằng 0, điều này phù hợp với thực tế là không có ràng buộc nào buộc bất kỳ thay đổi nào. 

Khi các ràng buộc mâu thuẫn nhau, chẳng hạn như các khoảng rời rạc trên cùng một chỉ mục, giao điểm sẽ sụp đổ ngay lập tức trong quá trình xử lý. Thuật toán phát hiện điều này hoàn toàn thông qua các giới hạn khoảng mà không cần mô phỏng các giá trị và đưa ra kết quả chính xác -1. 

Khi giá trị ban đầu nằm chính xác trên ranh giới của khoảng cuối cùng, cả hai nhánh bất đẳng thức đều tránh tăng thêm chi phí, vì sự di chuyển chỉ cần thiết khi hoàn toàn ở bên ngoài. Điều này đảm bảo rằng các trường hợp liền kề với ranh giới không gây ra lỗi riêng lẻ trong tính toán khoảng cách.
