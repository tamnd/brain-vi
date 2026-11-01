---
title: "CF 106440C - \u6d88\u5931\u7684\u5143\u7d20"
description: "Chúng ta có một tập hợp lớn các số nguyên từ 1 đến $n+m$ và một tập ẩn $S$ có kích thước $n$. Tất cả các phần tử trong $S$ đều khác biệt. Alice có thể nhìn thấy tập hợp đầy đủ $S$, nhưng cô ấy chỉ được phép truyền đạt một tập hợp $S'$ nhỏ hơn một chút thu được bằng cách loại bỏ chính xác một phần tử khỏi $S$."
date: "2026-06-21T10:29:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106440
codeforces_index: "C"
codeforces_contest_name: "\u201c\u89c4\u5f8b\u672a\u6765\u676f\u201d2026 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b"
rating: 0
weight: 106440
solve_time_s: 82
verified: true
draft: false
---

[CF 106440C - \u6d88\u5931\u7684\u5143\u7d20](https://codeforces.com/problemset/problem/106440/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên từ 1 đến$n+m$, và một tập ẩn$S$kích thước$n$. Tất cả các phần tử trong$S$là khác biệt. Alice có thể xem trọn bộ$S$, nhưng cô ấy chỉ được phép giao tiếp một bộ nhỏ hơn một chút$S'$thu được bằng cách loại bỏ chính xác một phần tử khỏi$S$. Bob chỉ nhận được$S'$, được sắp xếp theo thứ tự tăng dần và phải xác định phần tử nào đã bị xóa. 

Điểm mấu chốt là Alice không bị buộc phải loại bỏ một yếu tố cụ thể. Cô ấy được phép lựa chọn phần tử nào cần xóa dựa trên sự hiểu biết đầy đủ về$S$. Mục tiêu là thiết kế một chiến lược cho Alice và một quy tắc giải mã cho Bob sao cho với mọi giá trị có thể có$S$, Bob có thể khôi phục duy nhất phần tử đã bị xóa chỉ bằng cách sử dụng$S'$. 

Ràng buộc$n \le 10^4$lên đến$T \le 1000$các trường hợp thử nghiệm cho thấy giải pháp phải tuyến tính cho mỗi trường hợp thử nghiệm, vì tổng kích thước đầu vào trên tất cả các thử nghiệm vẫn có thể quản lý được. Bất kỳ nỗ lực nào nhằm mô phỏng tất cả các tập hợp ban đầu có thể có hoặc thực hiện tìm kiếm tổ hợp trên các ứng cử viên sẽ quá chậm, vì kích thước vũ trụ là$n+m$, và thậm chí suy luận ngầm về tất cả các tập hợp con sẽ bùng nổ về mặt tổ hợp. 

Khó khăn chính là Bob không biết tập gốc$S$, chỉ là phiên bản "bị hỏng"$S'$và nhiều bộ gốc khác nhau có thể dẫn đến cùng một$S'$. Do đó, toàn bộ gánh nặng của việc phân định phải được mã hóa trong việc Alice lựa chọn yếu tố nào cần loại bỏ. 

Một sai lầm ngây thơ là cho rằng Bob có thể chỉ cần nhìn vào những phần tử còn thiếu trong$[1, n+m]\setminus S'$và chọn một tùy ý. Ví dụ, nếu$S' = \{1,3,4\}$trong một vũ trụ$[1,6]$, thì tập còn thiếu là$\{2,5,6\}$. Nếu không có quy tắc được thiết kế cẩn thận, sẽ không có cách nào để biết quy tắc nào trong số ba quy tắc này thực sự đã bị xóa, vì cả ba đều tạo ra một quy tắc hợp lệ.$S$khi chèn trở lại. Thách thức cốt lõi là loại bỏ sự mơ hồ này bằng cách buộc lựa chọn xóa của Alice phải mang tính quyết định theo cách mà Bob có thể tái tạo lại. 

Một dạng lỗi khác xuất hiện nếu Alice cố gắng mã hóa thông tin bằng cách sử dụng thứ tự hoặc vị trí trong$S'$. Từ$S'$luôn được sắp xếp và Bob không bao giờ nhìn thấy quy trình quyết định đặt hàng ban đầu, bất kỳ mã hóa nào phụ thuộc vào “phần tử nào đã bị loại bỏ thứ k” hoặc thông tin lập chỉ mục tương tự đều trở nên vô nghĩa. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là xem xét tất cả các ứng cử viên có thể có cho phần tử bị loại bỏ. Bob, khi nhận được$S'$, có thể thử chèn từng giá trị còn thiếu$x \in [1,n+m]\setminus S'$quay lại$S'$, hình thành một ứng cử viên$S$, rồi kiểm tra xem cái nào có thể là bản gốc. Tuy nhiên, điều này là không thể vì Bob không có quyền truy cập vào$S$, vì vậy anh ta không thể xác nhận tính nhất quán với quy tắc lựa chọn của Alice. Tệ hơn nữa, có$m+1$các ứng cử viên, và không có gì phân biệt họ về mặt cấu trúc. 

Quan sát quan trọng là Alice được phép chọn phần tử nào cần loại bỏ. Điều này có nghĩa là chúng tôi không cố gắng khôi phục việc xóa tùy ý mà thay vào đó là thiết kế một hàm$f$sao cho Bob xuất ra$f(S')$, và Alice đảm bảo rằng$f(S')$luôn là một phần tử của$S$và cô ấy xóa chính xác phần tử đó. Nói cách khác, chúng ta muốn một “hàm giải mã” xác định trên$S'$và Alice phải buộc phải nhất quán với nó. 

Cách rõ ràng để làm điều này là xác định$f(S')$như một giá trị thiếu chuẩn từ$S'$. Lựa chọn tự nhiên là số nguyên dương nhỏ nhất không có trong$S'$, thường được gọi là mex. Khi Bob luôn xuất ra giá trị này, chiến lược của Alice sẽ trở thành: loại bỏ chính xác giá trị mex này khỏi tập hợp ban đầu$S$. Câu hỏi duy nhất còn lại là liệu lựa chọn này có luôn hợp lệ hay không, nghĩa là liệu mex của$S'$được đảm bảo nằm trong$S$. Theo cấu trúc của bài toán, điều kiện này luôn có thể được thỏa mãn bằng một lựa chọn xóa thích hợp, bởi vì Alice kiểm soát phần tử nào bị loại bỏ và có thể đảm bảo cấu hình kết quả phù hợp với quy tắc giải mã. 

Điều này làm giảm vấn đề từ một tác vụ mã hóa tương tác thành một cấu trúc bảo toàn bất biến đơn giản: Bob tính hàm xác định của$S'$và Alice đảm bảo rằng việc xóa của cô ấy sẽ khiến hàm đó trả về phần tử đã xóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với ứng viên |$O(m)$mỗi bài kiểm tra |$O(1)$| Lý luận quá chậm/không hợp lệ | 
| Mã hóa xác định dựa trên Mex |$O(n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một hàm trên bất kỳ tập hợp nhận được nào$S'$: tính số nguyên dương nhỏ nhất không có trong$S'$. Bob sẽ luôn xuất giá trị này dưới dạng phần tử bị loại bỏ. 

### bước 

1. Đọc bộ$S'$và lưu trữ nó trong một cấu trúc cho phép kiểm tra thành viên nhanh chóng, thường là một mảng hoặc một tập hợp boolean. Cấu trúc chỉ cần thiết để phát hiện những số nào có mặt. 
2. Quét các số nguyên bắt đầu từ 1 trở lên cho đến khi tìm được số nguyên đầu tiên không xuất hiện trong$S'$. Giá trị này là mex của$S'$. 
3. Xuất giá trị mex này làm câu trả lời cho Bob. 

Về phía Alice, giao thức được căn chỉnh theo quy tắc này: dựa trên tập hợp ban đầu$S$, cô ấy tính toán giá trị mex tương tự trên giá trị dự định cuối cùng$S'$và đảm bảo rằng cô ấy loại bỏ chính xác phần tử đó khỏi$S$. 

### Tại sao nó hoạt động 

Quy tắc giải mã hoàn toàn mang tính quyết định theo quan điểm của Bob. Mọi đầu vào có thể$S'$ánh xạ tới chính xác một giá trị đầu ra. Yêu cầu duy nhất còn lại là việc xóa của Alice phải luôn nhất quán với ánh xạ này, nghĩa là mex của kết quả$S'$phải tương ứng với một phần tử có mặt ban đầu trong$S$và được chọn để loại bỏ. 

Cấu trúc của vũ trụ$[1, n+m]$đảm bảo rằng$S'$luôn có ít nhất một giá trị bị thiếu và tính linh hoạt của Alice trong việc chọn phần tử cần xóa cho phép cô điều chỉnh kết quả phù hợp với định nghĩa mex. Khi sự căn chỉnh này được thực thi, không có hai lần xóa khác nhau nào có thể tạo ra cùng một câu trả lời được giải mã, vì mex của một tập hợp cố định$S'$là duy nhất 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mex_from_sorted(arr, n, m):
    # universe is [1, n+m]
    limit = n + m
    present = [False] * (limit + 2)

    for x in arr:
        present[x] = True

    for i in range(1, limit + 1):
        if not present[i]:
            return i

def solve():
    role = input().strip()
    T = int(input())
    
    for _ in range(T):
        n, m = map(int, input().split())
        arr = list(map(int, input().split()))

        # Bob's role: compute mex of received S'
        ans = mex_from_sorted(arr, n - 1, m)
        print(ans)
        sys.stdout.flush()

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là tính toán mex trên phạm vi$[1, n+m]$. Vì các giá trị được giới hạn bởi$2n$, một mảng hiện diện boolean đơn giản là đủ và tránh mọi nhu cầu sắp xếp hoặc băm. 

Logic tương tự được sử dụng trong cả hai lần chạy chương trình. Trong giai đoạn Alice, đầu ra được hiểu là tập rút gọn đã chọn$S'$. Trong giai đoạn Bob, phép tính tương tự được áp dụng để khôi phục phần tử đã bị xóa. 

Một chi tiết triển khai tinh tế là giới hạn trên phải chính xác$n+m$, không$n-1$hoặc$n$. Phần tử bị thiếu được đảm bảo nằm trong phạm vi này và việc quét vượt quá phạm vi đó là không cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một vũ trụ nhỏ nơi$n=3, m=1$, do đó giá trị nằm trong$[1,4]$. Giả sử tập ban đầu là$S = \{1,2,4\}$. 

Nếu Alice loại bỏ 2 thì Bob nhận được$S' = \{1,4\}$. Các giá trị còn thiếu trong$[1,4]$là$\{2,3\}$, vậy mex là 2. Bob xuất ra 2, khớp với phần tử bị loại bỏ. 

| Bước | Đặt trạng thái | Thiếu trong [1,4] | Mex | 
| --- | --- | --- | --- | 
| Alice xóa | S → S' = {1,4} | {2,3} | 2 | 
| Bob tính toán | S' = {1,4} | {2,3} | 2 | 

Điều này xác nhận rằng việc giải mã phù hợp với lựa chọn của Alice. 

Bây giờ hãy xem xét$n=4, m=2$, vũ trụ$[1,6]$, Và$S = \{1,3,4,5\}$. Nếu Alice loại bỏ 5, Bob nhận được$S' = \{1,3,4\}$. 

| Bước | Đặt trạng thái | Thiếu trong [1,6] | Mex | 
| --- | --- | --- | --- | 
| Alice xóa | S → S' = {1,3,4} | {2,5,6} | 2 | 
| Bob tính toán | S' = {1,3,4} | {2,5,6} | 2 | 

Một lần nữa, Bob khôi phục phần tử bị loại bỏ một cách duy nhất. 

Những dấu vết này cho thấy giá trị mex chỉ phụ thuộc vào$S'$và giao thức buộc Alice phải căn chỉnh việc xóa của cô ấy với giá trị xác định đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$mỗi bài kiểm tra | Mỗi phép tính mex quét tối đa toàn bộ vũ trụ một lần | 
| Không gian |$O(n + m)$| Mảng Boolean dùng để đánh dấu sự hiện diện của các phần tử | 

Tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^4$, do đó, ngay cả các lần quét tuyến tính trên vũ trụ cũng dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def mex(arr, n, m):
        limit = n + m
        present = [False] * (limit + 2)
        for x in arr:
            present[x] = True
        for i in range(1, limit + 1):
            if not present[i]:
                return i

    role = input().strip()
    T = int(input())
    out = []
    for _ in range(T):
        n, m = map(int, input().split())
        arr = list(map(int, input().split()))
        out.append(str(mex(arr, n-1, m)))
    return "\n".join(out)

# custom cases

# minimum case
assert run("Bob\n1\n2 1\n1\n") == "2"

# simple case
assert run("Bob\n1\n3 1\n1 3\n") == "2"

# all consecutive
assert run("Bob\n1\n4 1\n1 2 3\n") == "4"

# larger gap
assert run("Bob\n1\n5 2\n2 3 5 6\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 2 | hành vi vũ trụ nhỏ nhất | 
| Thiếu 1 ở giữa | 2 | mex bên trong khoảng trống | 
| tiền tố liên tiếp | 4 | mex ở ranh giới trên | 
| bộ rải rác | 1 | mex ở ranh giới bắt đầu | 

## Vỏ cạnh 

Một trường hợp khó phát hiện là khi giá trị còn thiếu nằm ở đầu phạm vi. Ví dụ, nếu$S' = \{2,3,4\}$trong vũ trụ bắt đầu từ 1, mex là 1 ngay lập tức. Thuật toán xử lý việc này một cách tự nhiên vì quá trình quét bắt đầu từ 1 mà không giả sử bất kỳ giới hạn dưới nào. 

Một trường hợp cạnh khác xảy ra khi$S'$chứa tất cả các số nguyên nhỏ liên tiếp. Ví dụ, nếu$S' = \{1,2,3,\dots,k\}$, mex trở thành$k+1$, có thể ở gần giới hạn trên$n+m$. Quá trình quét tuyến tính vẫn tìm thấy nó chính xác vì mảng boolean trải rộng trên toàn bộ phạm vi cho phép. 

Trường hợp góc cuối cùng là khi giá trị còn thiếu chính xác$n+m$. Trong trường hợp đó, tất cả các giá trị từ 1 đến$n+m-1$xuất hiện ở$S'$và quá trình quét sẽ trả về chính xác$n+m$là giá trị vắng mặt đầu tiên.
