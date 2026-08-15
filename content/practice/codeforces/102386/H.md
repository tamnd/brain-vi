---
title: "CF 102386H - ​​\u0421\u0432\u0435\u0442\u043e\u0444\u043e\u0440\u044b"
description: "Tại giao lộ có hai đồng hồ đếm ngược, ban đầu hiển thị (A) và (B). Mỗi giây, cả hai giá trị đều giảm đi một. Chúng ta chỉ xét các khoảnh khắc trong khi cả hai đèn vẫn đỏ, do đó cả hai bộ đếm đều chưa đạt đến 0."
date: "2026-08-15T07:41:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "H"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 161
verified: false
draft: false
---

[CF 102386H - \u0421\u0432\u0435\u0442\u043e\u0444\u043e\u0440\u044b](https://codeforces.com/problemset/problem/102386/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Tại giao lộ có hai đồng hồ đếm ngược, ban đầu hiển thị (A) và (B). Mỗi giây, cả hai giá trị đều giảm đi một. Chúng ta chỉ xét các khoảnh khắc trong khi cả hai đèn vẫn đỏ, do đó cả hai bộ đếm đều chưa đạt đến 0. 

Tại một thời điểm cụ thể (t), bộ đếm hiển thị 

[ 
A-t,\qquad B-t. 
] 

Chúng ta cần đếm xem có bao nhiêu khoảnh khắc như vậy có đặc tính mà một số được hiển thị là bội số nguyên của số kia. Bao gồm thời điểm ban đầu (t=0). 

Vì (A,B\le 10^9), việc mô phỏng mỗi giây có thể yêu cầu tối đa (10^9) lần lặp. Điều đó vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Chúng ta cần khai thác thực tế là cả hai quầy đều giảm một lượng bằng nhau, do đó hiệu của chúng không bao giờ thay đổi. 

Có một số trường hợp nhỏ trong đó việc triển khai trực tiếp có thể gặp trục trặc. Với (A=B=1), khoảnh khắc màu đỏ duy nhất là ((1,1)), nên đáp án là (1). Một giải pháp chỉ bắt đầu kiểm tra sau lần giảm đầu tiên sẽ trả về không chính xác (0). Với (A=1,B=2), thời điểm hợp lệ duy nhất là ((1,2)), nên đáp án cũng là (1). Không được bao gồm thời điểm bộ đếm nhỏ hơn trở thành số 0. Cuối cùng, với (A=B=5), mọi trạng thái ((5,5),(4,4),\ldots,(1,1)) đủ điều kiện, cho (5), vì vậy đẳng thức cần được xử lý riêng thay vì cố gắng tìm các ước số của hiệu bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng việc đếm ngược. Tại mỗi giây, đặt giá trị hiện tại là (x) và (y), kiểm tra xem (x) chia (y) hay (y) chia (x), sau đó giảm cả hai bộ đếm. Điều này đúng vì nó kiểm tra mọi trạng thái màu đỏ có thể có chính xác một lần. Tuy nhiên, nếu một bộ đếm bắt đầu ở (10^9), mô phỏng sẽ thực hiện gần như (10^9) lần lặp, tốc độ này quá chậm. 

Cấu trúc hữu ích xuất hiện khi chúng ta viết trạng thái sau (t) giây dưới dạng (A-t) và (B-t). Sự khác biệt của họ luôn 

[ 
(B-t)-(A-t)=B-A. 
] 

Giả sử tại thời điểm đó (A\le B). Đặt (x=A-t). Bộ đếm còn lại là (x+(B-A)). Điều kiện hai số khác nhau một thừa số nguyên tương đương với 

[ 
x+(B-A)\equiv 0\pmod{x}. 
] 

Vì (x\equiv0\pmod{x}), điều này giảm xuống còn 

[ 
B-A\equiv0\pmod{x}. 
] 

Vì vậy, mọi thời điểm hợp lệ đều tương ứng chính xác với ước số dương (x) của chênh lệch cố định (B-A). Trong khoảng thời gian màu đỏ, (x) lấy mọi giá trị nguyên từ (A) xuống (1). Do đó, chúng ta chỉ cần đếm các ước của (|A-B|) lớn nhất là (\min(A,B)). 

Nếu (A=B), hiệu bằng 0 và mọi trạng thái đều có bộ đếm bằng nhau, vì vậy câu trả lời đơn giản là (A). Mặt khác, chúng ta có thể liệt kê các ước của (|A-B|) trong thời gian (O(\sqrt{|A-B|})). Bất cứ khi nào (i) chia hiệu, cả (i) và (|A-B|/i) đều là ước số và chúng tôi đếm từng ước số nếu nó lớn nhất là (\min(A,B)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(\min(A,B))) | (O(1)) | Quá chậm | 
| Tối ưu | (O(\sqrt{ | A-B | })) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt (m=\min(A,B)) và (d=|A-B|). Bộ đếm nhỏ hơn bắt đầu ở (m) và giảm dần qua mọi giá trị (m,m-1,\ldots,1) trong khi cả hai đèn vẫn có màu đỏ. 
2. Nếu (d=0), trả về (m). Các bộ đếm bằng nhau tại mọi thời điểm màu đỏ và các số dương bằng nhau là bội số nguyên của nhau với tỷ lệ (1). 
3. Ngược lại, liệt kê các số nguyên (i) từ (1) đến (\lfloor\sqrt d\rfloor). Bất cứ khi nào (i) chia (d), (i) là một ước số và (d/i) là ước số ghép của nó. 
4. Đếm (i) khi (i\le m). Cũng tính (d/i) khi (d/i\le m), với điều kiện hai ước số khác nhau. 
5. Xuất ra số đếm tích lũy. Mỗi ước số được đếm biểu thị chính xác một giá trị của bộ đếm nhỏ hơn, do đó có chính xác một thời điểm hợp lệ.

Bất biến chính là bộ đếm nhỏ hơn lấy mỗi giá trị dương từ (m) xuống (1) đúng một lần. Đối với giá trị (x) như vậy, bộ đếm lớn hơn là (x+d) và (x) chia (x+d) chính xác khi (x) chia (d). Do đó, thuật toán đếm chính xác các trạng thái hợp lệ và không có trạng thái nào khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B = map(int, input().split())

    m = min(A, B)
    d = abs(A - B)

    if d == 0:
        print(m)
        return

    ans = 0
    i = 1

    while i * i <= d:
        if d % i == 0:
            j = d // i

            if i <= m:
                ans += 1

            if j != i and j <= m:
                ans += 1

        i += 1

    print(ans)

solve()
```Ba biến đầu tiên giảm số đếm ngược ban đầu xuống còn hai đại lượng quan trọng: giá trị lớn nhất có thể có của bộ đếm nhỏ hơn, (m) và chênh lệch cố định (d). 

Nhánh (d=0) là cần thiết vì số 0 không có tập hữu hạn các ước số dương. Quan trọng hơn, khi các bộ đếm bằng nhau, mọi trạng thái màu đỏ đều hợp lệ, do đó việc xử lý trực tiếp đẳng thức vừa đơn giản vừa chính xác về mặt toán học. 

Đối với (d>0), vòng lặp chỉ đạt tới (\sqrt d). Khi (i) chia (d), ước số ghép đôi (j=d/i) sẽ thu được ngay lập tức, do đó không cần phải quét tất cả các giá trị bộ đếm có thể có. 

điều kiện`j != i`ngăn không cho một số bình phương được đếm hai lần. Ví dụ: nếu (d=16) và (i=4), cả hai biểu thức ước số đều tạo ra (4), nhưng nó chỉ biểu thị một giá trị truy cập có thể có. 

Các số nguyên Python không bị tràn và (i*i) nhiều nhất là khoảng (10^9) bên trong vòng lặp, vì vậy phép tính số học là an toàn. Ranh giới trên cũng đúng: nếu bộ đếm nhỏ hơn là (m), trạng thái ban đầu đó vẫn có màu đỏ và phải được xem xét, trong khi trạng thái sau (m) giây có bộ đếm bằng 0 và không được xem xét. 

## Ví dụ đã hoạt động 

Đối với (A=3,B=30), chênh lệch cố định là (27) và bộ đếm nhỏ hơn nằm trong khoảng từ (3) đến (1). 

| (i) | (d/i) | Số chia (i\le m) | Số chia (d/i\le m) | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 27 | Có | Không | 1 | 
| 2 | không phải là số chia | Không | Không | 1 | 
| 3 | 9 | Có | Không | 2 | 
| 4 | kết thúc vòng lặp | | | 2 | 

Các ước số liên quan của (27) là (1,3,9,27), nhưng chỉ có (1) và (3) nhiều nhất là (m=3). Chúng tương ứng với các trạng thái ((1,28)) và ((3,30)). Câu trả lời là (2). 

Đối với (A=16,B=4), chênh lệch cố định là (12) và bộ đếm nhỏ hơn nằm trong khoảng từ (4) xuống (1). 

| (i) | (d/i) | Số chia (i\le m) | Số chia (d/i\le m) | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 12 | Có | Không | 1 | 
| 2 | 6 | Có | Không | 2 | 
| 3 | 4 | Có | Có | 4 | 

Các ước của (12) không vượt quá (4) là (1,2,3,4). Chúng tương ứng với bốn trạng thái hợp lệ trong đó bộ đếm nhỏ hơn là (1,2,3) hoặc (4). Câu trả lời là (4). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\sqrt{ | A-B | })) | Chúng tôi chỉ kiểm tra khả năng chia hết đến căn bậc hai của hiệu khác 0. | 
| Không gian | (O(1)) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Với (A,B\le10^9), vòng lặp thực hiện tối đa khoảng (31623) lần lặp. Điều đó thay thế khả năng mô phỏng hàng tỷ bước bằng vài chục nghìn thao tác liên tục trong thời gian sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    A, B = map(int, input().split())

    m = min(A, B)
    d = abs(A - B)

    if d == 0:
        print(m)
    else:
        ans = 0
        i = 1

        while i * i <= d:
            if d % i == 0:
                j = d // i

                if i <= m:
                    ans += 1

                if j != i and j <= m:
                    ans += 1

            i += 1

        print(ans)

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

def run(inp: str) -> str:
    return solve_data(inp)

assert run("3 30\n") == "2\n", "sample 1"
assert run("16 4\n") == "4\n", "sample 2"

assert run("1 1\n") == "1\n", "minimum equal values"
assert run("1 2\n") == "1\n", "only initial state is valid"
assert run("5 5\n") == "5\n", "all states are equal"
assert run("4 7\n") == "2\n", "divisor boundary"
assert run("2 1000000000\n") == "2\n", "large difference"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Giá trị tối thiểu và nhánh sai phân bằng 0 | 
|`1 2`|`1`| Trạng thái ban đầu được bao gồm, trạng thái 0 bị loại trừ | 
|`5 5`|`5`| Mọi trạng thái đều hợp lệ khi các bộ đếm bằng nhau | 
|`4 7`|`2`| Các ước số ở biên (x\le\min(A,B)) | 
|`2 1000000000`|`2`| Giá trị lớn không cần mô phỏng tuyến tính | 

## Vỏ cạnh 

cho`1 1`, ta có (m=1) và (d=0). Nhánh đẳng thức ngay lập tức trả về (m=1). Trạng thái màu đỏ duy nhất là`(1, 1)`, vậy kết quả đúng 

Vì`1 2`, ta có (m=1) và (d=1). Ước duy nhất của (d) là (1) và nó thỏa mãn (1\le m), nên đáp án là (1). Trạng thái sau một giây sẽ chứa số 0 và nằm ngoài khoảng thời gian màu đỏ, do đó không có trạng thái bổ sung nào được tính. 

Vì`5 5`, (d=0), do đó thuật toán trả về (5). Năm trạng thái màu đỏ là`(5,5)`,`(4,4)`,`(3,3)`,`(2,2)`, Và`(1,1)`. Việc coi số 0 như một ước số thông thường sẽ không có giá trị về mặt toán học và cũng sẽ bỏ sót một thực tế là mọi tiểu bang đều đủ điều kiện. 

Vì`4 7`, (m=4) và (d=3). Các ước dương của (3) là (1) và (3), cả hai đều nhiều nhất là (4), nên đáp án là (2). Chúng tương ứng với các trạng thái`(4,7)`Và`(1,4)`. Số chia (3) được ghép với (1) và cả hai đều phải được tính vì cả hai giá trị đều xuất hiện trong số các trạng thái bộ đếm nhỏ hơn có thể có. 

Vì`2 1000000000`, chênh lệch là (999999998), nhưng bộ đếm nhỏ hơn chỉ có thể là (2) hoặc (1). Các ước số liên quan đến các trạng thái đó là (1) và (2), và cả hai đều chia hiệu, cho câu trả lời (2). Thuật toán tìm thấy điều này mà không cần thử bất kỳ điều gì gần với (10^9) bước đếm ngược.
