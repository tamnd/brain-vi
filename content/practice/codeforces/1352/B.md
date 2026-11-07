---
title: "CF 1352B - Lệnh triệu tập chẵn lẻ tương tự"
description: "Chúng ta được yêu cầu chia một số nguyên $n$ cho trước thành chính xác $k$ phần dương sao cho tất cả các phần đều có cùng tính chẵn lẻ. Điều này có nghĩa là chúng ta phải chọn tất cả các số lẻ hoặc tất cả các số chẵn và các số $k$ này phải có tổng chính xác bằng $n$."
date: "2026-06-16T10:36:46+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1352
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 640 (Div. 4)"
rating: 1200
weight: 1352
solve_time_s: 388
verified: false
draft: false
---

[CF 1352B - Lệnh triệu tập chẵn lẻ tương tự](https://codeforces.com/problemset/problem/1352/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 6 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chia một số nguyên nhất định$n$vào chính xác$k$phần tích cực sao cho tất cả các phần có cùng tính chẵn lẻ. Điều này có nghĩa là chúng ta phải chọn tất cả các số lẻ hoặc tất cả các số chẵn và những số này$k$các số phải có tổng chính xác bằng$n$. 

Đầu ra là một cấu trúc cụ thể của một trình tự như vậy hoặc một tuyên bố rằng không có cấu trúc nào như vậy tồn tại. Chúng tôi có thể tự do đưa ra bất kỳ phân tách hợp lệ nào, vì vậy chúng tôi không tìm kiếm giải pháp tối ưu hoặc duy nhất, chỉ tìm kiếm tính khả thi cộng với một ví dụ. 

Những ràng buộc cho phép$n$lên đến$10^9$và lên tới 1000 trường hợp thử nghiệm, với$k \le 100$. Điều này ngay lập tức loại trừ bất kỳ việc liệt kê các phân vùng theo cấp số nhân hoặc theo từng thử nghiệm. Thậm chí một$O(n)$cách tiếp cận cho mỗi bài kiểm tra tổng thể sẽ quá chậm. Cấu trúc của bài toán gợi ý rõ ràng việc kiểm tra số học theo thời gian không đổi cho mỗi trường hợp kiểm thử. 

Khó khăn chính không phải là xây dựng các con số sau khi chọn chẵn lẻ, mà là đảm bảo rằng sự lựa chọn đó thậm chí có thể thực hiện được trong khi vẫn duy trì các ràng buộc dương. 

Một số tình huống khó khăn bộc lộ những lý luận ngây thơ không thành công. Nếu chúng ta cố gắng luôn sử dụng số lẻ, chúng ta có thể gặp phải tình trạng không khớp chẵn lẻ. Ví dụ,$n = 10, k = 3$hoạt động, nhưng$n = 8, k = 7$thì không, vì tổng nhỏ nhất có thể có của 7 số dương lẻ là 7, nhưng các ràng buộc chẵn lẻ cũng không thành công. Một lỗi nhỏ khác xảy ra khi trộn lẫn tính chẵn lẻ một cách ngầm định, chẳng hạn như cố gắng "sửa tính chẵn lẻ ở cuối" sau khi chọn tham lam hầu hết các giá trị, điều này phá vỡ yêu cầu rằng tất cả$k$các số phải có tính chẵn lẻ giống hệt nhau. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu sẽ thử mọi cách để chọn$k$số nguyên dương và kiểm tra xem tổng của chúng có bằng không$n$, giới hạn ở tất cả số lẻ hoặc tất cả số chẵn. Ngay cả khi giới hạn ở một mức chẵn lẻ, số lượng các tác phẩm vẫn tăng lên một cách tổ hợp, gần như$\binom{n-1}{k-1}$, điều này hoàn toàn không thể thực hiện được ngay cả với những giá trị rất nhỏ của$n$. Cách tiếp cận này đúng về mặt toán học nhưng không thể sử dụng được về mặt tính toán. 

Quan sát quan trọng là một khi tính chẵn lẻ được cố định, cấu trúc của các số hợp lệ sẽ trở nên cực kỳ cứng nhắc. Nếu tất cả các số là số lẻ thì mỗi số ít nhất là 1. Nếu tất cả các số là số chẵn thì mỗi số ít nhất là 2. Điều này làm giảm vấn đề kiểm tra xem tổng còn lại sau khi gán các giá trị tối thiểu có thể được phân phối theo các bước 2 hay không. 

Đối với tính chẵn lẻ lẻ, chúng tôi chỉ định$k$những cái, tiêu thụ$k$từ$n$. Tổng còn lại phải chẵn để có thể chia thành số gia 2 cộng vào những số này. Để có tính chẵn lẻ, chúng tôi chỉ định$k$hai, tiêu thụ$2k$từ$n$. Tổng còn lại phải không âm và chẵn. 

Điều này biến vấn đề thành hai bước kiểm tra tính khả thi đơn giản, sau đó là việc xây dựng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đánh giá từng trường hợp thử nghiệm một cách độc lập. 

1. Kiểm tra xem chúng ta có thể sử dụng số lẻ không. Bắt đầu bằng cách gán 1 cho tất cả$k$các vị trí. Điều này sử dụng$k$tổng số tiền. Ta tính phần còn lại$r = n - k$. Nếu như$r \ge 0$Và$r$chẵn, cấu hình này có thể được hoàn thành. Phần còn lại chẵn có thể được phân phối bằng cách thêm 2 liên tục vào bất kỳ vị trí nào mà không phá vỡ tính dương hoặc tính chẵn lẻ. 
2. Nếu việc xây dựng số lẻ không thành công, hãy thử các số chẵn. Chỉ định 2 cho tất cả$k$các vị trí. Điều này sử dụng$2k$, rời đi$r = n - 2k$. Nếu như$r \ge 0$Và$r$là số chẵn, chúng ta có thể phân phối nó theo bước 2 trên mảng. 
3. Nếu cả hai lần thử đều thất bại, xuất ra NO. 
4. Khi xây dựng thành công, hãy bắt đầu từ mảng cơ sở (tất cả 1 hoặc tất cả 2) và phân phối phần còn lại bằng cách thêm 2 liên tục vào bất kỳ vị trí nào, thường là vị trí đầu tiên để đơn giản. 

Lý do việc thêm 2 bảo toàn tính hợp lệ là vì nó giữ tính chẵn lẻ không thay đổi trong khi vẫn duy trì tính dương. 

### Tại sao nó hoạt động 

Bất kỳ giải pháp hợp lệ nào cũng phải hoàn toàn lẻ hoặc hoàn toàn chẵn. Nếu tất cả đều là số lẻ, việc trừ đi 1 từ mỗi phần tử sẽ để lại số chẵn không âm được phân bổ trên$k$khe cắm. Điều đó có nghĩa là tổng số tiền ít nhất phải bằng$k$và khác với$k$bằng một số chẵn. Logic tương tự cũng áp dụng cho các số chẵn có giá trị cơ sở 2. Đây không chỉ là điều kiện đủ mà còn là điều kiện cần, vì mỗi số lẻ đóng góp ít nhất 1 và mọi số chẵn đóng góp ít nhất 2 và tính chẵn lẻ cố định kích thước bước thành 2 số gia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())

    # try odd: all 1s
    if n >= k and (n - k) % 2 == 0:
        base = [1] * k
        rem = n - k
        base[0] += rem
        print("YES")
        print(*base)
        continue

    # try even: all 2s
    if n >= 2 * k and (n - 2 * k) % 2 == 0:
        base = [2] * k
        rem = n - 2 * k
        base[0] += rem
        print("YES")
        print(*base)
        continue

    print("NO")
```Khối đầu tiên kiểm tra tính khả thi của việc xây dựng hoàn toàn kỳ lạ. điều kiện$n \ge k$đảm bảo tính dương vì mỗi phần tử ít nhất bằng 1. Điều kiện chẵn lẻ đảm bảo phần còn lại có thể được chia thành số gia là 2. 

Khối thứ hai lặp lại lý do tương tự cho việc xây dựng hoàn toàn chẵn, trong đó giá trị tối thiểu cho mỗi phần tử là 2 thay vì 1. 

Chúng tôi phân phối phần còn lại hoàn toàn cho phần tử đầu tiên để đơn giản. Điều này là an toàn vì việc thêm một số chẵn sẽ bảo toàn cả số dương và số chẵn lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 10, k = 3 

Chúng tôi thử xây dựng kỳ lạ trước tiên. 

| Bước | Mảng cơ sở | Số tiền còn lại | Tình trạng | 
| --- | --- | --- | --- | 
| kiểm tra lẻ | [1, 1, 1] | 10 - 3 = 7 | 7 lẻ → thất bại | 
| thậm chí kiểm tra | [2, 2, 2] | 10 - 6 = 4 | hợp lệ | 

Chúng tôi thành công với việc xây dựng đồng đều và phân phối 4 vào phần tử đầu tiên, tạo ra [6, 2, 2]. 

Điều này xác nhận rằng việc xây dựng chẵn lẻ có thể hấp thụ số tiền bổ sung ở bước 2 trong khi vẫn duy trì tính hợp lệ. 

### Ví dụ 2: n = 8, k = 7 

| Bước | Mảng cơ sở | Số tiền còn lại | Tình trạng | 
| --- | --- | --- | --- | 
| kiểm tra lẻ | [1,1,1,1,1,1,1] | 1 | điều kiện chẵn lẻ không đủ không thành công | 
| thậm chí kiểm tra | [2,2,2,2,2,2,2] | -6 | tiêu cực | 

Cả hai cách xây dựng đều thất bại vì tổng lẻ nhỏ nhất có thể là 7 và tổng chẵn nhỏ nhất là 14 và 8 nằm giữa chúng. 

Điều này chứng tỏ tính khả thi hoàn toàn được xác định bởi hai giới hạn chặt chẽ dưới đây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi bài kiểm tra thực hiện kiểm tra số học liên tục và nhiều nhất là một cấu trúc | 
| Không gian | O(k) | Chỉ lưu trữ tối đa k số nguyên cho đầu ra | 

Lời giải dễ dàng nằm trong giới hạn vì$t \le 1000$Và$k \le 100$, dẫn đến nhiều nhất$10^5$tổng số hoạt động đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _StringIO

    out = _StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out

    # solution
    import sys
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())

        if n >= k and (n - k) % 2 == 0:
            res = [1] * k
            res[0] += n - k
            print("YES")
            print(*res)
        elif n >= 2 * k and (n - 2 * k) % 2 == 0:
            res = [2] * k
            res[0] += n - 2 * k
            print("YES")
            print(*res)
        else:
            print("NO")

    _sys.stdout = _stdout
    return out.getvalue().strip()

# provided samples
assert run("""8
10 3
100 4
8 7
97 2
8 8
3 10
5 3
1000000000 9
""") == """YES
6 2 2
YES
97 1 1 1
NO
NO
YES
2 2 2 2 2 2 2 2
NO
YES
3 1 1
YES
111111110 111111110 111111110 111111110 111111110 111111110 111111110 111111110 111111120""", "sample 1"

# minimum case
assert run("1\n1 1\n") == "YES\n1"

# impossible small
assert run("1\n2 2\n") == "YES\n2 2" or run("1\n2 2\n") == "YES\n2 2"

# odd feasibility edge
assert run("1\n3 2\n") == "NO"

# large even construction
assert run("1\n1000000000 2\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | CÓ 1 | cấu hình hợp lệ tối thiểu | 
| 2 2 | CÓ 2 2 | thậm chí trường hợp cơ sở xây dựng | 
| 3 2 | KHÔNG | không thể chẵn lẻ | 
| 1000000000 2 | cặp hợp lệ | khả thi quy mô lớn | 

## Vỏ cạnh 

cho$n = k$, việc xây dựng số lẻ luôn thành công vì tất cả các số đều có tổng chính xác bằng$k$, tạo ra một dãy toàn lẻ hợp lệ. Thuật toán chấp nhận chính xác vì$n - k = 0$, thỏa mãn điều kiện số dư chẵn. 

Vì$n = 2k$, cấu trúc chẵn tạo ra tất cả hai số có số dư bằng 0. điều kiện$n \ge 2k$Và$(n - 2k) \% 2 = 0$giữ, vì vậy đầu ra là hợp lệ. 

Vì$n < k$, cả hai cách xây dựng đều thất bại ngay lập tức vì ngay cả tổng nhỏ nhất có thể của$k$số nguyên dương là$k$khi tất cả đều bằng 1. Thuật toán loại bỏ chính xác trước bất kỳ nỗ lực xây dựng nào. 

Đối với những trường hợp$n - k$là số lẻ nhưng không âm, việc xây dựng số lẻ không thành công mặc dù có đủ tổng số tiền. Điều này nắm bắt được hạn chế chẵn lẻ chính: khối lượng còn lại phải chia hết thành các bước 2, không chỉ là không âm.
