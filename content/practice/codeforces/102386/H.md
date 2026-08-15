---
title: "CF 102386H - ​​\u0421\u0432\u0435\u0442\u043e\u0444\u043e\u0440\u044b"
description: "Có hai đồng hồ đếm ngược đèn giao thông, ban đầu hiển thị (A) và (B). Sau mỗi giây, cả hai giá trị đều giảm đi một. Chúng ta chỉ quan tâm đến những khoảnh khắc mà cả hai bộ đếm vẫn dương, bởi vì ngay khi một bộ đếm về 0, thời gian đèn đỏ kết thúc."
date: "2026-08-15T18:48:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "H"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 730
verified: false
draft: false
---

[CF 102386H - \u0421\u0432\u0435\u0442\u043e\u0444\u043e\u0440\u044b](https://codeforces.com/problemset/problem/102386/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Có hai đồng hồ đếm ngược đèn giao thông, ban đầu hiển thị (A) và (B). Sau mỗi giây, cả hai giá trị đều giảm đi một. Chúng ta chỉ quan tâm đến những khoảnh khắc mà cả hai bộ đếm vẫn dương, bởi vì ngay khi một bộ đếm về 0, thời gian đèn đỏ kết thúc. 

Tại thời điểm đó, hai giá trị hiện tại được coi là tốt nếu một giá trị là bội số nguyên của giá trị kia. Chúng ta cần đếm tất cả những khoảnh khắc tốt đẹp trước khi một trong hai bộ đếm về 0. 

Giả sử (A \le B). Sau (t) giây, bộ đếm sẽ 

[ 
A-t,\qquad B-t. 
] 

Quá trình này hợp lệ trong (t=0,1,\ldots,A-1), do đó có thể có tới (10^9) khoảnh khắc. Do đó, một mô phỏng trực tiếp sẽ yêu cầu tới (10^9) lần lặp, vượt xa những gì một giải pháp lập trình cạnh tranh có thể thực hiện được. Bản thân các giá trị cũng vừa khít với số nguyên có dấu 32 bit, nhưng các tích số trung gian là không cần thiết và dù sao thì các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ mọi lo ngại về tràn. 

Các trường hợp cạnh chính được gây ra bởi sự bình đẳng và bởi thực tế là không được tính thời điểm bộ đếm trở thành 0. Ví dụ, với đầu vào`1 1`, khoảnh khắc đèn đỏ duy nhất là trạng thái ban đầu`(1, 1)`, vậy câu trả lời là`1`. Vòng lặp bắt đầu sau giây đầu tiên sẽ trả về 0 không chính xác. Với đầu vào`2 3`, các tiểu bang là`(2,3)`và sau đó`(1,2)`. Chỉ một`(1,2)`đủ điều kiện, vì vậy câu trả lời là`1`. Việc thực hiện bất cẩn cũng kiểm tra trạng thái`(0,1)`sẽ đếm không chính xác một trạng thái sau khi thời kỳ màu đỏ kết thúc. 

Bình đẳng là một trường hợp đặc biệt khác. Vì`5 5`, cả hai bộ đếm đều bằng nhau tại mọi thời điểm hợp lệ:`(5,5)`,`(4,4)`,`(3,3)`,`(2,2)`,`(1,1)`. Mọi người đều đủ điều kiện, vì vậy câu trả lời là`5`. Việc xử lý hiệu (B-A=0) giống như một bài toán đếm số chia thông thường sẽ thất bại vì mọi số nguyên dương đều chia hết cho 0 và thay vào đó, câu trả lời bị giới hạn bởi số đếm ngược ngắn hơn. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản mô phỏng từng giây. Tại thời điểm (t), nó tính hai giá trị còn lại và kiểm tra xem giá trị nào có chia hết cho giá trị kia hay không. Điều này đúng vì mọi khoảnh khắc đèn đỏ có thể đều được truy cập đúng một lần và phép thử tính chia hết chính xác là điều kiện của bài toán. 

Vấn đề là số lượng khoảnh khắc. Nếu một lần đếm ngược bắt đầu ở (1) và lần đếm ngược kia ở (10^9), thì chỉ cần kiểm tra một khoảnh khắc, nhưng nếu cả hai đều bắt đầu gần (10^9), thì có thể cần phải kiểm tra hầu hết (10^9). Trong trường hợp xấu nhất, chẳng hạn như`1000000000 1000000000`, quá trình mô phỏng thực hiện (10^9) lần lặp, quá chậm. 

Quan sát quan trọng là việc trừ đi cùng một số tiền từ cả hai quầy sẽ bảo toàn sự khác biệt của chúng. Giả sử (A \le B) và xác định 

[ 
d=B-A. 
] 

Sau (t) giây bộ đếm sẽ 

[ 
x=A-t,\qquad x+d=B-t. 
] 

Bộ đếm nhỏ hơn là (x) và bộ đếm lớn hơn là (x+d). Nếu (d>0), giá trị lớn hơn không bao giờ có thể chia hết giá trị dương nhỏ hơn. Như vậy điều kiện duy nhất có thể là 

[ 
x \giữa (x+d). 
] 

Vì (x\mid x), điều này tương đương với 

[ 
x\giữa d. 
] 

Điều này thay đổi vấn đề hoàn toàn. Thay vì kiểm tra từng giây, chúng ta chỉ cần đếm các ước dương của (d) có số đếm ngược ban đầu nhỏ hơn nhiều nhất. 

Hiệu (d) nhiều nhất là (10^9-1), vì vậy tất cả các ước của nó có thể được tính bằng cách kiểm tra các cặp ước số có thể có lên đến (\sqrt d). Có nhiều nhất khoảng (31623) ứng viên như vậy, con số này rất nhỏ so với (10^9). 

Khi (A=B), hiệu bằng 0 và mức giảm trước đó trở nên suy biến. Trong trường hợp đó, các bộ đếm đều bằng nhau tại mọi thời điểm hợp lệ, vì vậy mọi thời điểm đều đủ điều kiện và câu trả lời chỉ đơn giản là (A). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(\min(A,B))) | (O(1)) | Quá chậm | 
| Tối ưu | (O(\sqrt{ | A-B | })) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt (m=\min(A,B)) và (M=\max(A,B)). Bộ đếm nhỏ hơn xác định số lượng trạng thái dương còn lại trước khi một đèn chuyển sang màu xanh lục. 
2. Nếu (A=B), trả về (A). Hai bộ đếm giữ nguyên trong suốt thời gian đèn đỏ, vì vậy mọi trạng thái (A) đều hợp lệ. 
3. Ngược lại hãy tính hiệu dương (d=M-m). Tại bất kỳ thời điểm hợp lệ nào, hãy viết bộ đếm dòng điện nhỏ hơn là (x). Bộ đếm còn lại là (x+d). 
4. Một trạng thái hợp lệ yêu cầu một bộ đếm chia cho bộ đếm kia. Vì (x<x+d) nên chỉ có (x\mid x+d) mới có thể giữ được. Trừ (x) từ số lớn hơn sẽ cho điều kiện tương đương (x\mid d). 
5. Bộ đếm hiện tại nhỏ hơn lấy mọi giá trị nguyên từ (m) xuống (1). Do đó, câu trả lời chính xác là số ước dương của (d) không vượt quá (m). 
6. Đếm các số nguyên (i) từ (1) đến (\lfloor\sqrt d\rfloor). Bất cứ khi nào (i) chia (d), cả (i) và (d/i) đều là ước. Đếm từng số nếu nó lớn nhất là (m), chú ý không đếm cùng một ước số hai lần khi (i^2=d). 
7. In số lượng tích lũy. 

### Tại sao nó hoạt động 

Tại mọi thời điểm hợp lệ với (A\ne B), gọi (x) là bộ đếm nhỏ hơn còn lại. Bộ đếm còn lại chính xác là (x+d), trong đó (d=|A-B|) không bao giờ thay đổi. Vì (x<x+d) nên điều kiện chia hết chỉ có thể là (x\mid x+d), tương đương với (x\mid d). Trong thời gian đèn đỏ, (x) lấy mọi số nguyên dương từ (\min(A,B)) xuống (1), đúng một lần. Do đó, có sự tương ứng một-một giữa các khoảnh khắc hợp lệ và các ước số của (d) tối đa là (\min(A,B)). Phép liệt kê số chia đếm chính xác các giá trị đó nên kết quả là chính xác. 

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
            if i <= m:
                ans += 1

            other = d // i
            if other != i and other <= m:
                ans += 1

        i += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Ba biến đầu tiên giảm đầu vào xuống mức đếm ngược nhỏ hơn (m) và chênh lệch cố định (d). sử dụng`abs(A - B)`làm cho mã không phụ thuộc vào bộ đếm gốc nào lớn hơn. 

Trường hợp đẳng thức được xử lý trước khi liệt kê số chia. Khi`d == 0`, mọi trạng thái dương đều có số đếm bằng nhau, do đó trả về`m`trực tiếp tránh coi số 0 như thể nó có một tập hữu hạn các ước số thông thường. 

Đối với trường hợp không bằng nhau, vòng lặp chỉ diễn ra trong khi`i * i <= d`. Mỗi ước số bên dưới căn bậc hai đều có một ước số bổ sung ở trên nó, vì vậy việc kiểm tra một bên sẽ tìm thấy cả hai. điều kiện`other != i`ngăn không cho số chia bình phương được đếm hai lần. 

Sự so sánh với`m`là cần thiết vì không phải mọi ước số của hiệu đều tương ứng với trạng thái thực sự xảy ra. Bộ đếm dòng điện nhỏ hơn không bao giờ vượt quá giá trị ban đầu của nó, do đó chỉ có tối đa các ước số`m`có thể đại diện cho những khoảnh khắc hợp lệ. 

Không có vấn đề riêng lẻ nào liên quan đến số 0 vì bộ đếm nhỏ hơn hiện tại nằm trong khoảng từ`m`xuống tới`1`. Trạng thái có giá trị bằng 0 không bao giờ được biểu thị bằng phép liệt kê số chia. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`3 30`. Ở đây (m=3) và (d=27). Các giá trị bộ đếm nhỏ hơn có thể là (3,2,1). Chỉ những phép chia (27) mới tương ứng với những khoảnh khắc hợp lệ. 

| (i) | (i^2 \le d) | (i\giữa d) | Cặp chia số | Đếm theo cặp | 
| --- | --- | --- | --- | --- | 
| 1 | Có | Có | (1,27) | 1 | 
| 2 | Có | Không | Không có | 1 | 
| 3 | Có | Có | (3,9) | 2 | 
| 4 | Không | Chưa được kiểm tra | Chưa được kiểm tra | 2 | 

Các ước không lớn hơn (3) là (1) và (3), cho kết quả`2`. Xét về trạng thái đếm ngược thực tế, đây là`(1,28)`Và`(3,30)`. Nhà nước`(2,29)`không đủ điều kiện. 

Đối với Mẫu 2, đầu vào là`16 4`. Ở đây (m=4) và (d=12). Bộ đếm nhỏ hơn lấy các giá trị (4,3,2,1) và cả bốn đều chia (12). 

| (i) | (i^2 \le d) | (i\giữa d) | Cặp chia số | Đếm theo cặp | 
| --- | --- | --- | --- | --- | 
| 1 | Có | Có | (1,12) | 2 | 
| 2 | Có | Có | (2,6) | 4 | 
| 3 | Có | Có | (3,4) | 6 | 
| 4 | Không | Chưa được kiểm tra | Chưa được kiểm tra | 6 | 

Bảng liệt kê cặp tìm thấy tổng cộng sáu ước số, nhưng chỉ có nhiều nhất (1,2,3,4) (m=4). Như vậy câu trả lời là`4`. Các trạng thái hợp lệ tương ứng là`(16,4)`,`(15,3)`,`(14,2)`, Và`(13,1)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\sqrt{ | A-B | })) | Tối đa (\lfloor\sqrt{ | A-B | }\rfloor) ứng cử viên chia được kiểm tra. | 
| Không gian | (O(1)) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Với (A,B\le10^9), vòng lặp thực hiện ít hơn (31623) lần lặp trong trường hợp không bằng kém nhất. Đó là mức nhỏ thoải mái, trong khi phương pháp vũ phu có thể yêu cầu số lần lặp gần (10^9). 

## Trường hợp thử nghiệm```python
# helper: run the core solution on an input string
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        A, B = map(int, input().split())

        m = min(A, B)
        d = abs(A - B)

        if d == 0:
            print(m)
            return sys.stdout.getvalue()

        ans = 0
        i = 1

        while i * i <= d:
            if d % i == 0:
                if i <= m:
                    ans += 1

                other = d // i
                if other != i and other <= m:
                    ans += 1

            i += 1

        print(ans)
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Provided samples
assert run("3 30\n") == "2", "sample 1"
assert run("16 4\n") == "4", "sample 2"

# Minimum-size input
assert run("1 1\n") == "1", "both counters start at the minimum"

# Equal values
assert run("5 5\n") == "5", "every equal state is valid"

# Difference is 1, so only divisor 1 can contribute
assert run("2 3\n") == "1", "off-by-one boundary around zero"

# A square difference, checking the i*i == d case
assert run("5 14\n") == "2", "difference 9 has divisors 1, 3, 9, only 1 and 3 fit"

# Maximum-size equal input
assert run("1000000000 1000000000\n") == "1000000000", "maximum equal values"

# Large asymmetric input, only divisor 1 is within the smaller countdown
assert run("1 1000000000\n") == "1", "only the initial state can qualify"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Giá trị tối thiểu và trường hợp đặc biệt bộ đếm bằng | 
|`5 5`|`5`| Mọi khoảnh khắc hợp lệ đều đủ điều kiện khi các bộ đếm bằng nhau | 
|`2 3`|`1`| Sự khác biệt (1) và loại trừ trạng thái 0 | 
|`5 14`|`2`| Xử lý số chia bình phương và giới hạn trên (x\le m) | 
|`1000000000 1000000000`|`1000000000`| Giá trị đầu vào tối đa không cần mô phỏng | 
|`1 1000000000`|`1`| Sự khác biệt lớn chỉ với một ước số có thể sử dụng được | 

## Vỏ cạnh 

cho`1 1`, thuật toán tính toán (m=1) và (d=0). Nó ngay lập tức trả về (m=1), khớp với trạng thái hợp lệ duy nhất`(1,1)`. Điều này xử lý cả kích thước đầu vào tối thiểu và ý nghĩa đặc biệt của chênh lệch bằng 0. 

Vì`2 3`, ta có (m=2) và (d=1). Ước số dương duy nhất của (1) là`1`, và nhiều nhất là`2`, vậy câu trả lời là`1`. Các trạng thái thực tế là`(2,3)`Và`(1,2)`và chỉ có trạng thái thứ hai có một bộ đếm chia cho bộ đếm kia. Trạng thái sau đây sẽ là`(0,1)`, nhưng nằm ngoài thời gian đèn đỏ và không bao giờ được xem xét. 

Vì`5 5`, chênh lệch bằng 0, do đó thuật toán trả về`5`ngay lập tức. Năm tiểu bang`(5,5)`,`(4,4)`,`(3,3)`,`(2,2)`, Và`(1,1)`tất cả đều hợp lệ. Đây là lý do tại sao trường hợp bằng nhau không thể được chuyển qua công thức đếm số chia thông thường. 

Vì`5 14`, hiệu là (9), có ước số là (1,3,9). Bộ đếm nhỏ hơn chỉ có thể là (5,4,3,2,1), nên chỉ`1`Và`3`tương ứng với những khoảnh khắc thực tế. Câu trả lời là`2`. Trường hợp này cũng kiểm tra logic cặp số chia vì (9) là số chính phương và số chia`3`phải được tính đúng một lần. 

Vì`1000000000 1000000000`, sự khác biệt là bằng 0 và câu trả lời là chính xác`1000000000`. Thuật toán tạo ra điều này ngay lập tức thay vì cố gắng mô phỏng hàng tỷ bước, điều này chứng tỏ tại sao trường hợp tương tự cần được xử lý trực tiếp.
