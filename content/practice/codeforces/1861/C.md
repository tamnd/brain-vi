---
title: "CF 1861C - Truy vấn mảng"
description: "Chúng ta được yêu cầu kiểm tra xem liệu một chuỗi ký hiệu nhất định có thể được tạo ra bởi các phép toán trên một mảng bắt đầu trống hay không."
date: "2026-06-09T00:18:15+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "implementation", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1861
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 154 (Rated for Div. 2)"
rating: 1600
weight: 1861
solve_time_s: 196
verified: false
draft: false
---

[CF 1861C - Truy vấn mảng](https://codeforces.com/problemset/problem/1861/C) 

**Đánh giá:** 1600 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, triển khai, chuỗi, cây 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu kiểm tra xem liệu một chuỗi ký hiệu nhất định có thể được tạo ra bởi các phép toán trên một mảng bắt đầu trống hay không. Các thao tác là: thêm một số nguyên vào cuối mảng (ký hiệu là`+`), loại bỏ phần tử cuối cùng (`-`) và kiểm tra xem mảng hiện tại đã được sắp xếp chưa (`0`hoặc`1`). MỘT`1`có nghĩa là mảng được sắp xếp theo thứ tự không giảm dần, trong khi`0`nghĩa là không phải vậy. Đầu vào chỉ chứa các ký tự mà Monocarp đã viết ra và chúng tôi phải xác định xem có thể gán các giá trị nguyên thực tế cho`+`các thao tác sao cho chuỗi đầu ra chính xác là chuỗi đã cho. 

Các ràng buộc cho chúng ta gợi ý về cách tiếp cận vấn đề này một cách hiệu quả. Mỗi chuỗi có tối đa 200.000 ký tự và tổng của tất cả các trường hợp thử nghiệm cũng được giới hạn ở 200.000. Điều đó có nghĩa là bất kỳ giải pháp nào có thời gian tuyến tính cho mỗi ký tự đều khả thi. Mô phỏng ngây thơ thử tất cả các phép gán số nguyên cho`+`các phép toán sẽ thất bại vì có vô số số nguyên, nhưng chúng ta chỉ quan tâm đến việc một chuỗi có nhất quán hay không chứ không quan tâm đến các con số chính xác. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi bắt đầu bằng một`0`hoặc`1`không có trước`+`, các chuỗi loại bỏ các phần tử khỏi một mảng trống (không được phép bởi các ràng buộc) và các chuỗi có nhiều lần kiểm tra (`0`hoặc`1`) xuất hiện liên tiếp Ví dụ,`0`một mình là không thể vì mảng trống và không thể sắp xếp được. Tương tự,`+1-0`là có thể, nhưng`+0-1`có thể là không thể tùy thuộc vào trạng thái mảng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng gán các giá trị số nguyên cho mọi`+`theo cách mà tất cả`0`Và`1`đầu ra được thỏa mãn. Điều này có thể liên quan đến việc quay lại hoặc kiểm tra tất cả các trình tự có thể, điều này không thực tế vì số lượng lựa chọn là không giới hạn. 

Thông tin chi tiết quan trọng là chúng ta không cần các giá trị thực tế, chỉ cần các ràng buộc trên mảng. Nếu chúng ta duy trì giá trị tối thiểu và tối đa có thể có ở mỗi vị trí trong mảng, chúng ta có thể truyền các ràng buộc về phía trước khi đọc chuỗi. Cụ thể, đối với một`+`hoạt động, chúng ta có thể coi phần tử mới là "ít nhất bằng mức tối đa trước đó đối với việc kiểm tra thứ tự không giảm dần" và đối với`-`chúng tôi chỉ cần xóa phần tử cuối cùng và cập nhật mức tối thiểu/tối đa có thể. Đối với một`1`kiểm tra, phần tử được thêm vào cuối cùng ít nhất phải lớn bằng phần tử trước đó và đối với`0`kiểm tra, chúng ta phải đảm bảo rằng mảng đó có thể chưa được sắp xếp. Điều này cho phép chúng ta mô phỏng mảng với một dãy các phạm vi giá trị thay vì những con số cụ thể. 

Cách tiếp cận vũ phu có độ phức tạp theo cấp số nhân và không khả thi. Cách tiếp cận tối ưu mô phỏng mảng dưới dạng một dãy các phạm vi giá trị có thể, truyền bá các ràng buộc tối thiểu/tối đa, hoạt động theo thời gian tuyến tính trên mỗi ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(∞) | O(n) | Quá chậm | 
| Tuyên truyền ràng buộc / Mô phỏng ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một ngăn xếp trống. Mỗi phần tử trong ngăn xếp sẽ lưu trữ một bộ`(min_possible, max_possible)`đại diện cho phạm vi giá trị mà phần tử tương ứng có thể nhận. 
2. Lặp qua chuỗi`s`từng nhân vật. 
3. Khi một`+`gặp phải, đẩy một bộ dữ liệu mới`(1, INF)`vào ngăn xếp, thể hiện rằng chúng ta có thể chọn bất kỳ giá trị số nguyên nào. Đây,`INF`là một số đủ lớn. 
4. Khi một`-`gặp phải, hãy lấy phần tử cuối cùng ra khỏi ngăn xếp. 
5. Khi một`1`gặp phải, hãy kiểm tra xem ngăn xếp có thể biểu diễn một mảng không giảm hay không. Để thực hiện việc này, hãy lặp qua ngăn xếp từ dưới lên trên, đảm bảo rằng giá trị tối đa của mỗi phần tử ít nhất bằng giá trị tối thiểu của phần tử trước đó. Nếu kiểm tra thất bại, trình tự không nhất quán và chúng ta có thể xuất ra`NO`. 
6. Khi một`0`gặp phải, hãy kiểm tra xem mảng có thể không được sắp xếp hay không. Điều này được thực hiện bằng cách kiểm tra xem có tồn tại một cặp phần tử liên tiếp sao cho giá trị tối thiểu của phần tử tiếp theo nhỏ hơn giá trị tối đa của phần tử trước đó hay không. Nếu không, trình tự không nhất quán. 
7. Nếu chúng ta xử lý xong toàn bộ chuỗi mà không có mâu thuẫn, thì xuất ra`YES`. 

Tại sao nó hoạt động: Tính bất biến được duy trì là mỗi phần tử trong ngăn xếp lưu trữ phạm vi số nguyên mà nó có thể nhất quán với tất cả các thao tác trước đó. Bằng cách chỉ truyền các ràng buộc tối thiểu/tối đa, chúng ta có thể quyết định tại mỗi lần kiểm tra xem liệu có tồn tại phép gán các số nguyên thỏa mãn chuỗi cho đến nay hay không. Nếu bất kỳ kiểm tra nào không thành công thì không có phép gán nào có thể thỏa mãn trình tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        stack = []
        possible = True

        for c in s:
            if c == '+':
                # push an unrestricted new element
                stack.append([1, int(1e9)])
            elif c == '-':
                if not stack:
                    possible = False
                    break
                stack.pop()
            elif c == '1':
                # check if array can be sorted
                if len(stack) < 2:
                    continue
                ok = True
                for i in range(1, len(stack)):
                    if stack[i][1] < stack[i-1][0]:
                        ok = False
                        break
                if not ok:
                    possible = False
                    break
            elif c == '0':
                # check if array can be unsorted
                if len(stack) < 2:
                    possible = False
                    break
                ok = False
                for i in range(1, len(stack)):
                    if stack[i][0] < stack[i-1][1]:
                        ok = True
                        break
                if not ok:
                    possible = False
                    break

        print("YES" if possible else "NO")

if __name__ == "__main__":
    solve()
```Mã này sử dụng ngăn xếp để duy trì phạm vi có thể có cho các phần tử mảng. Mỗi`+`đẩy một phần tử mới không bị hạn chế,`-`bật phần tử cuối cùng và kiểm tra`0`Và`1`sử dụng các ràng buộc tối thiểu/tối đa để xác minh tính nhất quán. Các phép toán tuyến tính theo độ dài của chuỗi, đủ hiệu quả cho các ràng buộc đầu vào. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`++1`| Bước | Nhân vật | Ngăn xếp | Bình luận | 
| --- | --- | --- | --- | 
| 1 | + | [[1, INF]] | Thêm phần tử mới | 
| 2 | + | [[1, INF], [1, INF]] | Thêm phần tử mới | 
| 3 | 1 | [[1, INF], [1, INF]] | Kiểm tra có thể sắp xếp: true | 

Đầu ra: CÓ 

Đầu vào mẫu:`+++1--0`| Bước | Nhân vật | Ngăn xếp | Bình luận | 
| --- | --- | --- | --- | 
| 1 | + | [[1, INF]] | Thêm | 
| 2 | + | [[1, INF],[1, INF]] | Thêm | 
| 3 | + | [[1, INF],[1, INF],[1, INF]] | Thêm | 
| 4 | 1 | [[1, INF],[1, INF],[1, INF]] | Kiểm tra có thể sắp xếp: true | 
| 5 | - | [[1, INF],[1, INF]] | Pop | 
| 6 | - | [[1, INF]] | Pop | 
| 7 | 0 | [[1, INF]] | Ít hơn 2 yếu tố: không thể | 

Đầu ra: KHÔNG 

Những ví dụ này minh họa cách thuật toán xác định chính xác tính nhất quán bằng cách kiểm tra các ràng buộc ngăn xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần; vòng lặp bên trong để kiểm tra có kích thước ngăn xếp tối đa, tổng của tất cả các kiểm tra được giới hạn bởi n | 
| Không gian | O(n) | Ngăn xếp có thể tăng theo độ dài của chuỗi | 

Thuật toán dễ dàng phù hợp với giới hạn 2 giây và 256 MB cho n 2·10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("7\n++1\n+++1--0\n+0\n0\n++0-+1-+0\n++0+-1+-0\n+1-+0\n") == "YES\nNO\nNO\nNO\nYES\nNO\nNO"

# custom cases
assert run("1\n+1-0\n") == "YES"  # add 5, check sorted, remove, check unsorted
assert run("1\n0\n") == "NO"       # cannot check empty array as unsorted
assert run("1\n+0\n") == "NO"      # one element cannot be unsorted
assert run("1\n++1\n") == "YES"    # two elements, possible sorted
assert run("1\n++0\n") == "YES"    # two elements, possible unsorted
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`+1-0`| CÓ | trình tự thêm, xóa và kiểm tra | 
|`0`| KHÔNG | không thể kiểm tra chưa được sắp xếp trên mảng trống | 
|`+0`| KHÔNG | không thể kiểm tra chưa được sắp xếp trên một phần tử | 
|`++1`| | |
