---
title: "CF 105055F - Festa Junina"
description: "Chúng ta được yêu cầu xây dựng một dãy gồm chính xác $N$ số nguyên dương sao cho dãy đó tăng chặt và trong số tất cả các dãy như vậy, chúng ta muốn dãy có tổng chi phí nhỏ nhất. Cơ cấu chi phí là phần quan trọng."
date: "2026-06-28T01:06:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "F"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 84
verified: true
draft: false
---

[CF 105055F - Festa Junina](https://codeforces.com/problemset/problem/105055/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một chuỗi chính xác$N$các số nguyên dương sao cho dãy tăng chặt, và trong số tất cả các dãy như vậy, chúng ta muốn dãy có tổng chi phí nhỏ nhất. 

Cơ cấu chi phí là phần quan trọng. Mỗi số nguyên được đánh giá thông qua biểu diễn nhị phân của nó. Mỗi vị trí bit$i$có chi phí cố định$C_i$. Nếu một số có bit$i$thiết lập, nó góp phần$C_i$với chi phí của con số đó. Vì vậy giá trị của một số chỉ là tổng giá trị của các vị trí bit mà nó chứa. Không có sự tương tác giữa các bit, không có hiệu ứng mang theo chi phí và không phụ thuộc vào giá trị số ngoài số bit hiện diện. 

Ràng buộc trình tự “tăng nghiêm ngặt các số nguyên dương” chỉ thực thi rằng tất cả các số nguyên được chọn là khác biệt và có thể được sắp xếp theo giá trị. Vì bất kỳ tập hợp số nguyên dương riêng biệt nào cũng có thể được sắp xếp nên ràng buộc thứ tự không hạn chế chúng ta có thể chọn bộ số nào, chỉ có điều chúng ta không thể lặp lại một số và chúng ta phải tránh số 0. 

Vì vậy, nhiệm vụ giảm xuống việc lựa chọn$N$mặt nạ nhị phân khác 0 khác biệt trên$M$bit, giảm thiểu tổng chi phí bit của chúng. 

Những hạn chế làm cho điều này trở nên thú vị.$M$có thể lớn tới 1000, do đó vũ trụ của các con số có thể là rất lớn, lên tới$2^M - 1$. Tuy nhiên,$N \le 100$, vì vậy chúng ta chỉ cần 100 đối tượng rẻ nhất trong một không gian có kích thước theo cấp số nhân trong$M$. Bất kỳ phương pháp nào cố gắng liệt kê tất cả các tập hợp con đều không thể thực hiện được. Ngay cả việc lập trình động trên tất cả các mặt nạ bit cũng không khả thi do$2^M$không gian trạng thái. 

Một nỗ lực ngây thơ có thể là nghĩ đến việc tạo ra tất cả các chi phí tập hợp con và chọn chi phí nhỏ nhất$N$. Điều đó thất bại ngay lập tức vì số lượng tập hợp con tăng theo cấp số nhân với$M$, và thậm chí việc lưu trữ chúng là không thể. 

Ý tưởng ngây thơ thứ hai là tham lam chọn các bit hoặc số có mô hình chi phí nhỏ nhất, nhưng điều đó không thành công vì các tập hợp con tương tác tổ hợp: việc kết hợp sớm các bit đắt hơn một chút có thể tạo ra các tập hợp con nhiều bit rẻ hơn sau đó, do đó các quyết định cục bộ không duy trì được tính tối ưu toàn cục. 

Trường hợp cạnh tinh tế là tập hợp con trống. Giá của nó bằng 0 và nó sẽ luôn là ứng cử viên tốt nhất, nhưng nó tương ứng với số 0, điều này không được phép vì tất cả các số nguyên phải dương. Bất kỳ giải pháp đúng nào cũng phải loại trừ nó một cách rõ ràng khỏi việc xem xét. 

## Phương pháp tiếp cận 

Cấu trúc của bài toán là: mỗi số là một tập hợp con gồm tối đa 1000 mục (bit) độc lập và giá của một tập hợp con là tổng trọng số của mục. Chúng tôi muốn$N$tổng tập hợp con nhỏ nhất, không bao gồm tập hợp con trống. 

Đây là bài toán cổ điển “k tổng tập con nhỏ nhất của các trọng số độc lập”. Cách tiếp cận brute-force liệt kê tất cả các tập hợp con của$M$bit, tính toán giá trị của chúng, sắp xếp chúng và chọn bit đầu tiên$N$. Điều này hoạt động về mặt khái niệm vì mỗi tập hợp con là độc lập và chi phí của nó dễ tính toán. Điểm thất bại là vụ nổ theo cấp số nhân:$2^{1000}$tập hợp con vượt xa mọi giới hạn. 

Quan sát quan trọng là chúng ta không cần tất cả các tập hợp con. Chúng ta chỉ cần cái đầu tiên$N$, Ở đâu$N \le 100$. This allows us to maintain only a small frontier of best candidates while scanning bits one by one. Each bit either appears in a subset or it does not, and adding a bit increases cost by$C_i$. Điều này tương đương với việc hợp nhất nhiều lần hai danh sách đã được sắp xếp: tổng tập hợp con hiện có và các tổng đó cộng với trọng số mới. 

Thay vì lưu trữ rõ ràng tất cả các tập hợp con, chúng tôi duy trì một danh sách các tập hợp con tốt nhất hiện tại.$N$tổng tập hợp con. Khi chúng tôi xử lý một bit mới với chi phí$w$, mọi tổng tập hợp con hiện có$x$có thể tạo ra một ứng cử viên mới$x + w$. Chúng tôi hợp nhất danh sách cũ và danh sách đã chuyển và chỉ giữ lại danh sách nhỏ nhất$N$kết quả. Lặp lại điều này cho tất cả các bit sẽ tạo ra kết quả tốt nhất$N$tổng thể các tập hợp con. 

Vì tất cả các chi phí đều không âm nên các tập hợp con sẽ tăng đều đều khi chúng ta mở rộng khả năng và cắt bớt để$N$các phần tử không bao giờ loại bỏ thứ gì đó mà sau này có thể trở thành một phần của kết quả nhỏ hơn. 

Cuối cùng, chúng ta loại bỏ tập con trống và lấy tập tiếp theo$N$các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các tập hợp con) |$O(2^M \cdot M)$|$O(2^M)$| Quá chậm | 
| Tập con lũy tiến k-tốt nhất DP |$O(M \cdot N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi vị trí bit là một mục độc lập có trọng số$C_i$. Mỗi tập hợp con tương ứng với việc chọn một số mục này và chi phí của nó là tổng của các trọng số đã chọn. 

## Hướng dẫn thuật toán 

1. Bắt đầu với một tập hợp con duy nhất đại diện cho “không chọn gì”, với chi phí bằng 0. Điều này đóng vai trò là trạng thái nhận dạng trước khi chúng tôi xem xét bất kỳ bit nào. 
2. Duy trì một danh sách`dp`chỉ lưu trữ những gì nhỏ nhất$N+1$tập hợp chi phí nhìn thấy cho đến nay. Chúng tôi giữ$N+1$thay vì$N$vì sau này chúng ta phải loại bỏ tập con trống. 
3. Đối với từng vị trí bit$i$, đọc giá của nó$C_i$. Hãy coi mục này như một mục có thể được bao gồm hoặc loại trừ trong bất kỳ tập hợp con nào mà chúng tôi đã tạo. 
4. Xây dựng danh sách ứng viên mới bằng cách lấy mọi giá trị trong`dp`và cũng thêm$C_i$đến nó. Điều này đại diện cho tất cả các tập hợp con không sử dụng bit này hoặc sử dụng nó. 
5. Hợp nhất bản gốc`dp`và danh sách đã dịch chuyển, sắp xếp chúng và chỉ giữ lại phần nhỏ nhất$N+1$các giá trị. Điều này đảm bảo chúng tôi chỉ giữ lại những chi phí tập hợp con hứa hẹn nhất. 
6. Lặp lại quá trình này cho tất cả các vị trí bit. 
7. Sau khi xử lý tất cả các bit, loại bỏ giá trị 0 (tương ứng với tập con trống) và tính tổng nhỏ nhất$N$các giá trị còn lại. 

Lý do việc hợp nhất hoạt động là sau khi xử lý lần đầu tiên$i$bit,`dp`chứa tổng tập hợp con nhỏ nhất có thể đạt được chỉ bằng cách sử dụng các bit đó. Khi chúng tôi giới thiệu một bit mới, mọi tập hợp con mới hợp lệ đều là tập hợp con cũ hoặc tập hợp con cũ cộng với bit này, do đó việc hợp nhất sẽ loại bỏ tất cả các khả năng mà không cần liệt kê tập hợp con rõ ràng. 

Bước cắt tỉa là an toàn vì mọi chi phí đều không âm. Bất kỳ tập hợp con nào không nằm trong số nhỏ nhất hiện tại$N+1$sau này không thể trở nên nhỏ hơn vì các phép toán trong tương lai chỉ cộng các giá trị không âm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    C = list(map(int, input().split()))

    dp = [0]

    for w in C:
        new = dp + [x + w for x in dp]
        new.sort()
        if len(new) > N + 1:
            new = new[:N + 1]
        dp = new

    # remove empty subset
    dp = [x for x in dp if x != 0]

    print(sum(dp[:N]))

if __name__ == "__main__":
    solve()
```Mã này duy trì một danh sách động các tổng tập hợp con tốt nhất. Danh sách luôn thể hiện chi phí nhỏ nhất có thể đạt được bằng cách sử dụng các bit được xử lý. Mỗi lần lặp lại sẽ sao chép trạng thái hiện tại với trọng số tăng thêm, hợp nhất cả hai khả năng và cắt bớt để chỉ giữ lại những ứng cử viên phù hợp nhất. 

Việc lọc số 0 ở cuối đảm bảo chúng tôi không vô tình đưa vào tập hợp con trống không hợp lệ. Vì số 0 luôn là phần tử nhỏ nhất ban đầu nên nó tự nhiên bị loại bỏ trước khi tính tổng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 1 1
```Chúng tôi bắt đầu với`dp = [0]`. 

Sau bit đầu tiên:```
dp = [0, 1]
```Sau bit thứ hai:```
dp = [0, 1, 1, 2] -> keep [0, 1, 1]
```Sau bit thứ ba:```
dp = [0, 1, 1, 1, 2, 2, 2, 3] -> keep [0, 1, 1, 1]
```Bỏ số 0, lấy 3 nhỏ nhất:```
[1, 1, 1]
sum = 3
```Điều này cho thấy nhiều tập hợp con khác nhau có chi phí giống nhau và thuật toán giữ các bản sao một cách chính xác khi chúng đại diện cho các tập hợp con riêng biệt. 

### Mẫu 2 

đầu vào:```
3 3
1 2 4
```Bắt đầu:```
dp = [0]
```Sau 1:```
[0, 1]
```Sau 2:```
[0, 1, 2, 3] -> keep [0, 1, 2]
```Sau 4:```
[0, 1, 2, 3, 4, 5, 6, 7] -> keep [0, 1, 2]
```Xóa số 0:```
[1, 2, 3]
```Tổng 3 số nhỏ nhất:```
6
```Dấu vết này cho thấy cách kết hợp các bit tạo ra tất cả các tổng tập hợp con một cách tự nhiên và việc cắt tỉa không làm mất các ứng cử viên tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \cdot N)$| Mỗi trong số$M$bit hợp nhất tối đa hai danh sách kích thước$N$, với sự sắp xếp giới hạn bởi nhỏ$N$| 
| Không gian |$O(N)$| Chúng tôi chỉ giữ những gì tốt nhất$N+1$tập hợp số tiền bất kỳ lúc nào | 

Giới hạn$M \le 1000$Và$N \le 100$làm điều này nhanh chóng thoải mái. Thuật toán thực hiện tối đa khoảng$1000 \times 200 \log 200$hoạt động trong thực tế, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # assume solve() is defined above in same file
    return main_capture(inp)

def main_capture(inp):
    import sys
    from io import StringIO
    backup = sys.stdin
    sys.stdin = StringIO(inp)

    import sys
    from contextlib import redirect_stdout
    out = StringIO()
    with redirect_stdout(out):
        solve()

    sys.stdin = backup
    return out.getvalue().strip()

# provided samples
assert run("3 3\n1 1 1\n") == "3"
assert run("3 3\n1 2 4\n") == "6"
assert run("3 3\n4 2 1\n") == "6"

# custom: minimum case
assert run("1 1\n5\n") == "5"

# custom: all equal costs
assert run("4 4\n1 1 1 1\n") == "4"

# custom: increasing costs
assert run("2 5\n5 4 3 2 1\n") == "3"

# custom: large N small M
assert run("3 3\n10 100 1000\n") == "120"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | trường hợp cơ sở đúng đắn | 
| trọng lượng bằng nhau | 4 | xử lý tập hợp con trùng lặp | 
| giảm cân | 3 | tham lam không áp dụng được | 
| trọng lượng lớn thưa thớt | 120 | sự kết hợp đúng đắn | 

## Vỏ cạnh 

Tập con trống là trường hợp cạnh cấu trúc chính. Nó luôn xuất hiện ở trạng thái ban đầu với chi phí 0 và luôn là giá trị nhỏ nhất trong quá trình xây dựng. Thuật toán cố tình đưa nó qua các trạng thái trung gian vì nó cần phải tạo ra các tổ hợp siêu tập hợp chính xác nhưng sẽ loại bỏ nó trước khi đưa ra câu trả lời cuối cùng. Ví dụ, nếu$M = 1$Và$C = [7]$, trạng thái trung gian trở thành$[0, 7]$. Sau khi loại bỏ số 0, phần tử chuỗi hợp lệ duy nhất là 7, khớp với số nguyên dương duy nhất có thể. 

Một trường hợp khó phát hiện khác là khi nhiều tập hợp con riêng biệt tạo ra cùng một mức chi phí. Điều này xảy ra thường xuyên khi một số bit có cùng$C_i$. Thuật toán giữ các bản sao trong`dp`, điều này là cần thiết vì mỗi cái đại diện cho một tập hợp con khác nhau và chúng ta có thể cần nhiều chi phí giống nhau để đạt được$N$số phân biệt hợp lệ.
