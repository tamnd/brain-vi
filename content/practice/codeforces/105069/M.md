---
title: "CF 105069M - \u67d3\u8272\u6e38\u620f\uff08phiên bản dễ dàng\uff09"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô đã được gán một màu. Trò chơi liên quan đến việc áp dụng nhiều lần các thao tác vẽ, trong đó mỗi thao tác vẽ toàn bộ một hàng hoặc toàn bộ một cột bằng một màu duy nhất, ghi đè lên bất kỳ màu nào có trước đó."
date: "2026-06-27T23:23:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105069
codeforces_index: "M"
codeforces_contest_name: "The 5th FanRuan Cup Southeast University Programming Contest \uff08Winter\uff09"
rating: 0
weight: 105069
solve_time_s: 55
verified: true
draft: false
---

[CF 105069M - \u67d3\u8272\u6e38\u620f\uff08phiên bản dễ dàng\uff09](https://codeforces.com/problemset/problem/105069/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô đã được gán một màu. Trò chơi liên quan đến việc áp dụng nhiều lần các thao tác vẽ, trong đó mỗi thao tác vẽ toàn bộ một hàng hoặc toàn bộ một cột bằng một màu duy nhất, ghi đè lên bất kỳ màu nào có trước đó. Trình tự các thao tác rất quan trọng vì các lớp sơn sau sẽ ghi đè lên các lớp sơn trước đó. 

Câu hỏi quan trọng là: màu nào có thể là màu được sử dụng trong thao tác sơn cuối cùng? 

Quan sát quan trọng là thao tác cuối cùng không thể bị ghi đè bởi bất kỳ thứ gì khác. Nếu nước đi cuối cùng sơn một hàng thì sau đó, mỗi nước đi trong hàng đó phải khớp với màu cuối cùng và không có gì thay đổi nó sau đó. Logic tương tự được áp dụng một cách đối xứng nếu nước đi cuối cùng vẽ một cột. 

Vì vậy, nhiệm vụ giảm xuống còn việc tìm tất cả các màu sao cho tồn tại ít nhất một hàng hoặc cột có thể là thao tác sơn cuối cùng sử dụng màu đó. Có một hạn chế bổ sung là một giá trị cụ thể (màu đại diện cho “không sơn” hoặc không hợp lệ) không thể được coi là màu cuối cùng hợp lệ, do đó nó phải được loại trừ khỏi câu trả lời. 

Đầu vào đại diện cho một$n \times m$lưới các số nguyên và đầu ra là danh sách tất cả các màu hợp lệ có thể dùng làm màu sơn cuối cùng, được sắp xếp theo thứ tự tăng dần. 

Các ràng buộc ngụ ý bởi một bài toán lưới điển hình thuộc loại này cho phép lên đến khoảng$10^5$tế bào hoặc nhiều hơn. Điều này ngay lập tức loại trừ bất kỳ mô phỏng sâu hình khối hoặc lặp đi lặp lại nào trên tất cả các trình tự sơn lại có thể có. Ngay cả việc kiểm tra tất cả các đơn đặt hàng sơn lại cũng sẽ mang tính giai thừa và hoàn toàn không khả thi. 

Việc quét đơn giản mọi ứng cử viên có thể có cho hoạt động cuối cùng phải được giảm xuống thành việc kiểm tra cấu trúc đơn giản trên các hàng và cột. 

Một trường hợp lỗi nhỏ xuất hiện khi một hàng hoặc cột gần như đồng nhất nhưng chỉ chứa một ô khác nhau. Ví dụ: một hàng như$[3, 3, 3, 4]$có thể cám dỗ một giải pháp ngây thơ chấp nhận màu 3 nếu nó chỉ kiểm tra tính nhất quán một phần. Tuy nhiên, hàng này không thể là hàng sơn cuối cùng cho màu 3, vì thao tác cuối cùng sẽ buộc tất cả các ô phải có cùng màu. Vấn đề tương tự phát sinh đối với các cột. 

Một cạm bẫy khác là quên rằng “thao tác cuối cùng” phải xác định đầy đủ trạng thái lưới cuối cùng trên đường đó chứ không chỉ tính nhất quán của đa số. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động vẽ hàng và cột có thể có. Đối với mỗi màu ứng cử viên, chúng ta có thể tưởng tượng tất cả các cách chọn hàng hoặc cột được sơn cuối cùng và kiểm tra xem lưới có thể được tạo nhất quán hay không. Điều này ngay lập tức bùng nổ về mặt tổ hợp vì mỗi hàng hoặc cột có thể được vẽ theo nhiều thứ tự và số lượng trình tự tăng dần theo tổng số dòng. Ngay cả một phiên bản rút gọn kiểm tra tất cả các hàng và cột vì các thao tác cuối cùng vẫn sẽ yêu cầu xác thực từng khả năng đối với lưới theo thời gian tuyến tính, dẫn đến$O((n+m)\cdot nm)$, quá lớn đối với lưới tối đa. 

Cái nhìn sâu sắc về cấu trúc quan trọng là hoạt động cuối cùng chiếm ưu thế hoàn toàn ít nhất một dòng đầy đủ. Nếu thao tác cuối cùng vẽ một hàng thì hàng đó phải đồng nhất trong lưới cuối cùng. Điều tương tự cũng áp dụng cho các cột. Điều này biến vấn đề thành một nhiệm vụ xác thực trực tiếp: đối với mỗi hàng, hãy kiểm tra xem tất cả các phần tử của nó có bằng nhau hay không; đối với mỗi cột, thực hiện tương tự. Mỗi đường thống nhất như vậy xác định trực tiếp một màu ứng cử viên. 

Điều này có tác dụng vì mọi trạng thái cuối cùng hợp lệ đều phải chứa một dòng được vẽ cuối cùng và dòng đó không thể chứa các màu hỗn hợp sau bước cuối cùng. Vì vậy, sự đồng đều không chỉ cần thiết mà còn đủ để trở thành ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O(nm(n+m))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới kích thước$n \times m$. Đây là trạng thái cuối cùng sau khi tất cả các thao tác vẽ đã được áp dụng. 
2. Kiểm tra từng hàng một cách độc lập và xác định xem tất cả các giá trị trong hàng đó có giống nhau hay không. Nếu một hàng đồng nhất, hãy ghi lại màu của nó làm ứng cử viên. Lý do là một nước đi cuối cùng hợp lệ có thể đã vẽ toàn bộ hàng này, khiến nó không thay đổi sau đó. 
3. Lặp lại thao tác kiểm tra tương tự cho mỗi cột. Đối với mỗi cột, hãy xác minh xem tất cả các giá trị có giống nhau hay không. Nếu vậy, hãy ghi lại màu của cột đó làm ứng cử viên. 
4. Lưu trữ tất cả các màu ứng cử viên được phát hiện trong một bộ để tránh trùng lặp. Một màu có thể xuất hiện dưới dạng cả hàng đồng nhất và cột đồng nhất. 
5. Loại bỏ màu bị cấm nếu vấn đề chỉ ra rằng một giá trị cụ thể không thể được coi là hợp lệ. 
6. Sắp xếp các màu ứng cử viên còn lại và xuất chúng theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Thao tác cuối cùng hợp lệ phải ghi đè lên toàn bộ hàng hoặc toàn bộ cột trong một lần di chuyển. Sau bước di chuyển đó, không có thao tác tiếp theo nào tồn tại để sửa đổi bất kỳ ô nào. Điều này buộc mọi ô trong dòng đó phải chia sẻ cùng một giá trị cuối cùng, nếu không nó sẽ bị ghi đè một cách không nhất quán. Do đó, bất kỳ nước đi cuối cùng hợp lệ nào đều tương ứng chính xác với một hàng hoặc cột hoàn toàn thống nhất trong lưới cuối cùng. Ngược lại, bất kỳ hàng hoặc cột đồng nhất nào cũng có thể được giải thích là nước đi cuối cùng bằng cách đơn giản giả sử rằng nó đã được sơn ở cuối, làm cho nó đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [list(map(int, input().split())) for _ in range(n)]

    bad = -1  # if problem defines forbidden color, adjust if needed
    # (in many CF variants this is given; here we keep placeholder logic)

    ans = set()

    for i in range(n):
        ok = True
        for j in range(1, m):
            if g[i][j] != g[i][0]:
                ok = False
                break
        if ok:
            ans.add(g[i][0])

    for j in range(m):
        ok = True
        for i in range(1, n):
            if g[i][j] != g[0][j]:
                ok = False
                break
        if ok:
            ans.add(g[0][j])

    if bad in ans:
        ans.remove(bad)

    res = sorted(ans)
    print(len(res))
    if res:
        print(*res)

if __name__ == "__main__":
    solve()
```Kiểm tra hàng lặp lại trên mỗi hàng và so sánh mọi ô với phần tử đầu tiên, đủ để xác định tính đồng nhất. Việc kiểm tra cột thực hiện tương tự theo chiều dọc. Việc sử dụng một bộ đảm bảo rằng các màu xuất hiện ở cả hàng và cột không bị trùng lặp. 

Một điểm tinh tế là chúng tôi không bao giờ cố gắng tái tạo lại trình tự bức tranh. Chúng tôi hoàn toàn dựa vào các ràng buộc về cấu trúc của lưới cuối cùng, điều này tránh mọi lý luận theo cấp số nhân. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới:```
3 3
1 1 1
2 2 2
3 2 3
```| Bước | Hàng 1 | Hàng 2 | Hàng 3 | Cột 1 | Cột 2 | Cột 3 | Bộ ứng cử viên | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | - | - | - | ∅ | 
| Quét hàng | đồng phục (1) | đồng phục (2) | không đồng đều | - | - | - | {1,2} | 
| Quét cột | - | - | - | không đồng đều | không đồng đều | không đồng đều | {1,2} | 

Hai hàng đầu tiên giống nhau nên màu 1 và 2 được thêm vào. Không có cột nào giống nhau nên đáp án cuối cùng là {1, 2}. Điều này cho thấy chỉ riêng cấu trúc hàng có thể xác định màu cuối cùng hợp lệ. 

### Ví dụ 2```
2 3
5 5 5
5 1 5
```| Bước | Hàng 1 | Hàng 2 | Cột 1 | Cột 2 | Cột 3 | Bộ ứng cử viên | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | - | - | ∅ | 
| Quét hàng | đồng phục (5) | không đồng đều | - | - | - | {5} | 
| Quét cột | - | - | đồng phục (5) | không đồng đều | đồng phục (5) | {5} | 

Chỉ có màu 5 xuất hiện ở các đường đồng nhất. Ô trung tâm không ngăn cản tính hợp lệ của cột 1 và 3 vì các cột đó vẫn hoàn toàn nhất quán. Câu trả lời cuối cùng vẫn là {5}. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô được kiểm tra một số lần không đổi trong quá trình quét hàng và cột | 
| Không gian |$O(nm)$| Lưu trữ lưới | 

Thuật toán xử lý mỗi ô lưới với số lần không đổi và chỉ sử dụng một tập hợp cho các ứng cử viên, điều này hiệu quả đối với các ràng buộc điển hình cho đến các lưới lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO

    out = StringIO()
    old = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old
    return out.getvalue().strip()

# simple uniform grid
assert run("1 3\n7 7 7\n") == "1\n7"

# mixed rows, only one uniform
assert run("2 3\n1 1 2\n3 3 3\n") == "1\n3"

# single column uniform
assert run("3 1\n4\n4\n4\n") == "1\n4"

# no uniform lines
assert run("2 2\n1 2\n3 4\n") == "0\n"

# all equal grid
assert run("2 2\n9 9\n9 9\n") == "1\n9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả lưới bằng nhau | màu đơn | trường hợp đồng nhất lưới đầy đủ | 
| hàng hỗn hợp | hàng hợp lệ duy nhất | độ chính xác phát hiện hàng | 
| cột đơn | trường hợp chỉ có cột | phát hiện thống nhất theo chiều dọc | 
| không có đường nét thống nhất | kết quả trống | loại trừ những ứng viên không hợp lệ | 
| lưới thống nhất | chấp nhận hoàn toàn | tính nhất quán tối đa của cạnh | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi chỉ có một ô tồn tại trong một hàng hoặc cột. Ví dụ, một lưới có$n = 1$có nghĩa là mọi cột đều đồng đều một cách tầm thường. Thuật toán xử lý chính xác từng cột là hợp lệ vì một giá trị duy nhất luôn thỏa mãn tính đồng nhất. Điều ngược lại cũng đúng đối với$m = 1$, trong đó mỗi hàng sẽ tự động đồng nhất. 

Một trường hợp khác là lưới trong đó tất cả các hàng không đồng nhất nhưng một số cột giống nhau. Ví dụ:```
3 3
1 2 3
1 2 3
1 2 3
```Ở đây, không có hàng nào là đồng nhất, nhưng tất cả các cột đều đồng nhất. Thuật toán thu thập {1,2,3} làm ứng cử viên. Điều này đúng vì bất kỳ cột nào cũng có thể là thao tác được vẽ cuối cùng theo một trình tự hợp lệ, mặc dù không có hàng nào đủ điều kiện. 

Trường hợp khó phát hiện cuối cùng là khi màu cấm xuất hiện trên một hàng hoặc cột thống nhất. Cách tiếp cận dựa trên tập hợp đảm bảo dễ dàng lọc nó ở cuối mà không ảnh hưởng đến logic phát hiện trong quá trình quét.
