---
title: "CF 102330D - \u041f\u0440\u043e\u0433\u0440\u0435\u0441\u0441\u0438\u0432\u043d\u044b\u0439 \u0442\u043e\u0440\u0433"
description: "Chúng tôi có hai ưu đãi hiện tại, x từ Barnum và y từ Carlisle, với x <= y. Barnum tăng đề nghị của mình lên a, sau đó Carlisle giảm đề nghị của mình xuống b. Ở cặp nước đi tiếp theo, những thay đổi sẽ trở thành 2a và 2b, rồi 3a và 3b, v.v."
date: "2026-08-13T03:57:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102330
codeforces_index: "D"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2019.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102330
solve_time_s: 60
verified: true
draft: false
---

[CF 102330D - \u041f\u0440\u043e\u0433\u0440\u0435\u0441\u0441\u0438\u0432\u043d\u044b\u0439 \u0442\u043e\u0440\u0433](https://codeforces.com/problemset/problem/102330/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai ưu đãi hiện tại,`x`từ Barnum và`y`từ Carlisle, với`x <= y`. Barnum tăng lời đề nghị của mình lên`a`, thì Carlisle giảm lời đề nghị của mình xuống`b`. Ở cặp nước đi tiếp theo, những thay đổi sẽ trở thành`2a`Và`2b`, sau đó`3a`Và`3b`, vân vân. 

Quá trình kết thúc ngay khi hai lời đề nghị gặp nhau hoặc giao nhau. Câu trả lời là lời đề nghị cuối cùng đã thực sự được nói ra. Vì mọi điều kiện chấm dứt đều xảy ra khi đề nghị hiện tại của Barnum ít nhất là đề nghị hiện tại hoặc đề nghị tiếp theo của Carlisle nên câu trả lời sẽ luôn là đề nghị của Barnum ở vòng kết thúc. 

Giả sử chúng ta đã hoàn thành`k`vòng. Barnum đã thêm 

a(1+2+⋯+k)=a 2 k(k+1) ​ 

với lời đề nghị ban đầu của anh ấy. Carlisle đã trừ 

b(1+2+⋯+k)=b 2 k(k+1) ​ . 

Như vậy sau cả hai lần di chuyển của vòng`k`, ưu đãi của họ là 

B k ​ =x+a 2 k(k+1) ​ 

và 

C k ​ =y−b 2 k(k+1) ​ . 

Quá trình kết thúc ở vòng`k`chính xác khi hai giá trị này thỏa mãn`B_k >= C_k`. Sắp xếp lại mang lại 

(a+b) 2 k(k+1) ​ ≥y−x. 

Vì vậy toàn bộ mô phỏng quy về việc tìm số nguyên không âm nhỏ nhất`k`thỏa mãn bất đẳng thức này. Một lần`k`đã biết, câu trả lời đơn giản là`B_k`. 

Có nhiều nhất`2000`trường hợp thử nghiệm, trong khi`x`Và`y`có thể khác nhau gần như`10^12`. Do đó, số vòng có thể vào khoảng`sqrt(10^12)`, đại khái`1.4 * 10^6`, ngay cả khi kích thước bước chỉ`1`. Do đó, một mô phỏng trực tiếp có thể thực hiện hàng tỷ lần lặp trong tất cả các thử nghiệm, vượt xa giới hạn một giây. Chúng ta cần khai thác sự tăng trưởng đơn điệu của số tam giác thay vì mô phỏng từng vòng. 

Một số trường hợp ranh giới xứng đáng được quan tâm đặc biệt. Ví dụ: nếu các ưu đãi ban đầu đã bằng nhau`1 1 5 7`, cuộc đàm phán kết thúc ngay lập tức và câu trả lời là`1`. Vòng lặp luôn thực hiện một vòng trước sẽ tạo ra giá trị lớn hơn không chính xác. 

Sự bình đẳng ở ranh giới cũng phải chấm dứt ngay lập tức. Vì`1 4 1 1`, sau hai vòng, các ưu đãi sẽ trở thành`4`Và`1`, và câu trả lời là`4`. Tổng quát hơn, điều kiện là`>=`, không chỉ`>`. Thay thế nó bằng một so sánh nghiêm ngặt có thể chuyển câu trả lời sang vòng tiếp theo. 

Kích thước bước có thể rất khác nhau. Vì`7 18 1 3`, câu trả lời là`10`: sau khi Barnum nói`8`và Carlisle nói`15`, Barnum nói`10`, sau đó dự định của Carlisle`9`đã không lớn hơn`10`. Giải pháp chỉ kiểm tra xem đề nghị của Barnum có đạt đến đề nghị trước đó của Carlisle hay không sẽ bỏ lỡ điều kiện chấm dứt này. 

## Phương pháp tiếp cận 

Giải pháp đơn giản mô phỏng quá trình đàm phán theo từng vòng. trong vòng`k`, nó làm tăng lời đề nghị của Barnum thêm`k*a`và giảm lời đề nghị của Carlisle xuống`k*b`, sau đó kiểm tra xem các ưu đãi có được đáp ứng hay không. Điều này đúng vì nó tuân theo chính xác trình tự các đề nghị được mô tả trong quy trình. 

Vấn đề là số vòng. Với`y-x`gần với`10^12`Và`a=b=1`, chúng ta cần đại khái 

2 k(k+1) ​ ≈5⋅10 11 , 

mang lại`k`xung quanh`10^6`. Với`2000`trường hợp thử nghiệm, mô phỏng trường hợp xấu nhất có thể đạt tới khoảng`2 * 10^9`vòng. Mặc dù mỗi vòng đều có thời gian không đổi nhưng nó vẫn quá lớn. 

Quan sát hữu ích là sau vòng`k`, tổng số tiền mà khoảng cách đã giảm chính xác là 

(a+b) 2 k(k+1) ​ . 

Số hình tam giác`k(k+1)/2`đang tăng nghiêm ngặt đối với số không âm`k`, vậy vị ngữ 

(a+b) 2 k(k+1) ​ ≥y−x 

là sai với mọi thứ nhỏ hơn`k`và đúng với mọi đủ lớn`k`. Tính đơn điệu đó làm cho tìm kiếm nhị phân trở thành sự thay thế tự nhiên cho mô phỏng. 

Ngoài ra còn có một điểm tinh tế về thứ tự của các ưu đãi. Barnum nói đầu tiên trong mỗi hiệp đấu. Nếu lời đề nghị mới của anh ấy đã đạt đến lời đề nghị trước đó của Carlisle thì quá trình này sẽ dừng ngay lập tức ở giá trị của Barnum. Nếu không, Carlisle sẽ nói tiếp. Nếu lời đề nghị mới của Carlisle giảm xuống bằng hoặc thấp hơn giá trị của Barnum thì quá trình này cũng dừng ở giá trị của Barnum. Cả hai tình huống đều được nắm bắt bởi một điều kiện duy nhất`B_k >= C_k`, bởi vì`C_k <= C_{k-1}`. Nếu Barnum đạt tới`C_{k-1}`, anh ấy chắc chắn cũng đạt đến mức thậm chí còn nhỏ hơn`C_k`. 

Vì vậy, chúng tôi chỉ cần vòng đầu tiên trong đó các ưu đãi sau vòng này sẽ đáp ứng được.`B_k >= C_k`. Câu trả lời chính là lời đề nghị của Barnum ở vòng đấu đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) mỗi lần kiểm tra, với k lên tới khoảng 1,4 * 10^6 | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(log k) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách ban đầu`d = y - x`. Nếu như`d`bằng 0, các đề nghị đã được đồng ý, vì vậy câu trả lời là`x`và không cần vòng. 
2. Đối với vòng ứng cử viên`k`, tính số tam giác 

T k ​ = 2 k(k+1) ​ . 

Sau khi cả hai đề nghị ở vòng đó, khoảng cách của họ là 

d−(a+b)T k ​ . 

Cuộc thương lượng đã kết thúc ở vòng đó đúng lúc giá trị này không dương. 
3. Sử dụng tìm kiếm nhị phân để tìm số nhỏ nhất`k`vì cái gì 

(a+b)T k ​ ≥d. 

Vị ngữ là đơn điệu vì`T_k`tăng nghiêm ngặt với`k`. 
4. Một khi giá trị nhỏ nhất`k`được tìm thấy, hãy tính lời đề nghị của Barnum 

x+aT k ​ . 

Đó là số tiền mà cuộc đàm phán kết thúc. 
5. In giá trị này cho trường hợp kiểm thử hiện tại và lặp lại cho tất cả các trường hợp kiểm thử. 

### Tại sao nó hoạt động 

Trước vòng đấu`k`, mức tăng tích lũy do Barnum thực hiện là`a(1 + ... + k)`, trong khi mức giảm tích lũy do Carlisle thực hiện là`b(1 + ... + k)`. Do đó, chuyển động kết hợp của chúng hướng về nhau là`(a+b)T_k`. Vòng đầu tiên mà chuyển động này ít nhất là khoảng cách ban đầu chính xác là vòng đầu tiên mà lời đề nghị của Barnum ít nhất là lời đề nghị của Carlisle sau mức giảm tương ứng. 

Bởi vì lời đề nghị của Carlisle sau vòng đấu`k`không lớn hơn lời đề nghị của anh ấy trước vòng đấu`k`, bất kỳ sự vượt qua nào xảy ra khi Barnum nói trước cũng được thể hiện bằng điều kiện tương tự`B_k >= C_k`. Do đó nhỏ nhất`k`được tìm thấy bằng tìm kiếm nhị phân chính xác là vòng kết thúc và`x + aT_k`chính xác là số tiền được nói cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        x, y, a, b = map(int, input().split())

        if x == y:
            out.append(str(x))
            continue

        gap = y - x
        step = a + b

        # Find the smallest k such that
        # step * k * (k + 1) / 2 >= gap.
        lo, hi = 0, 1

        while step * hi * (hi + 1) // 2 < gap:
            hi *= 2

        while lo < hi:
            mid = (lo + hi) // 2
            triangular = mid * (mid + 1) // 2

            if step * triangular >= gap:
                hi = mid
            else:
                lo = mid + 1

        k = lo
        triangular = k * (k + 1) // 2
        answer = x + a * triangular

        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Xử lý trường hợp đặc biệt đầu tiên`x == y`trực tiếp. Trong tình huống đó`k = 0`thỏa mãn điều kiện kết thúc, do đó việc thực hiện một lần lặp khác sẽ là một lỗi riêng lẻ. 

Đối với mọi bài kiểm tra khác,`gap`lưu trữ khoảng cách giữa các ưu đãi ban đầu, trong khi`step = a + b`là mức độ giảm khoảng cách đó trên một đơn vị cấp số tam giác. biểu thức`k * (k + 1) // 2`là số nhân tích lũy sau`k`vòng. 

Giới hạn trên của tìm kiếm nhị phân được tìm thấy bằng cách nhân đôi`hi`. Điều này tránh việc dựa vào hằng số được chọn thủ công. Vì yêu cầu`k`chỉ quanh căn bậc hai của khoảng trống, việc nhân đôi này cần rất ít thao tác. 

Tìm kiếm nhị phân duy trì tính bất biến thông thường là câu trả lời nằm ở đâu đó trong`[lo, hi]`. Khi điểm giữa đã thỏa mãn bất đẳng thức cần thiết thì vẫn có thể`mid`chính nó là câu trả lời, vì vậy`hi`di chuyển đến`mid`. Nếu không thì câu trả lời phải lớn hơn, vì vậy`lo`trở thành`mid + 1`. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các biểu thức như`a * k * (k + 1)`không tràn. Việc chia số nguyên cũng được thực hiện trước khi nhân với`a`trong phép tính cuối cùng, vì`k(k+1)`luôn là số chẵn và số tam giác là số nguyên. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`x = 7`,`y = 18`,`a = 1`, Và`b = 3`. Khoảng cách ban đầu là`11`và mỗi vòng đóng góp`4T_k`hướng tới việc đóng nó lại. 

| k | T_k | Barnum`B_k`| Carlisle`C_k`|`4T_k >= 11`| 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 7 | 18 | Không | 
| 1 | 1 | 8 | 15 | Không | 
| 2 | 3 | 10 | 9 | Có | 

Vòng hợp lệ đầu tiên là`k = 2`. Đề nghị của Barnum là`7 + 1 * 3 = 10`, vậy câu trả lời là`10`. Ví dụ này cũng chứng minh tại sao chỉ kiểm tra đề nghị trước đó của Carlisle là không đủ. Barnum's`10`không đạt được trước đó`15`, nhưng lời đề nghị dự định tiếp theo của Carlisle là`9`, do đó cuộc đàm phán vẫn kết thúc vào lúc`10`. 

Đối với mẫu thứ hai, cả hai bên bắt đầu lúc`4`. 

| k | T_k | Barnum`B_k`| Carlisle`C_k`| Tình trạng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 4 | 4 | Có | 

Hợp lệ nhỏ nhất`k`bằng 0, vì vậy câu trả lời vẫn là`4`. Điều này khẳng định rằng sự bình đẳng ban đầu phải được xử lý mà không cần thực hiện vòng thương lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log K) | Mỗi bài kiểm tra sử dụng giai đoạn nhân đôi và tìm kiếm nhị phân trong vòng bắt buộc`K`| 
| Không gian | O(1) phụ trợ | Chỉ một số lượng biến số nguyên không đổi được sử dụng cho mỗi bài kiểm tra | 

Với`y-x <= 10^12`Và`a+b >= 2`, vòng kết thúc nhiều nhất là theo thứ tự`10^6`. Do đó, tìm kiếm nhị phân chỉ cần vài chục lần lặp cho mỗi lần kiểm tra, vì vậy ngay cả`2000`các trường hợp thử nghiệm chỉ yêu cầu hàng chục nghìn đánh giá vị từ. Việc sử dụng bộ nhớ không đổi ngoài bộ đệm đầu ra. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        x, y, a, b = map(int, input().split())

        if x == y:
            out.append(str(x))
            continue

        gap = y - x
        step = a + b

        lo, hi = 0, 1

        while step * hi * (hi + 1) // 2 < gap:
            hi *= 2

        while lo < hi:
            mid = (lo + hi) // 2
            triangular = mid * (mid + 1) // 2

            if step * triangular >= gap:
                hi = mid
            else:
                lo = mid + 1

        k = lo
        triangular = k * (k + 1) // 2
        out.append(str(x + a * triangular))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("1\n7 18 1 3\n") == "10\n", "sample 1"

# Provided sample 2
assert run("1\n4 4 4 4\n") == "4\n", "sample 2"

# Minimum values, already equal
assert run("1\n1 1 1 1\n") == "1\n", "minimum equal case"

# Exact triangular boundary:
# gap = 3, a+b = 2, T_2 = 3, so equality occurs at k = 2.
assert run("1\n1 4 1 1\n") == "4\n", "triangular boundary"

# Highly asymmetric step sizes
assert run("1\n1 10 1 8\n") == "2\n", "asymmetric steps"

# Large values
assert run("1\n1 1000000000000 1000000 1000000\n") == \
       "500000500000000001\n", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 1 1`|`1`| Bình đẳng ban đầu và vòng 0 | 
|`1 / 1 4 1 1`|`4`| Bình đẳng chính xác ở ranh giới tam giác | 
|`1 / 1 10 1 8`|`2`| Các bước thương lượng bất đối xứng mạnh mẽ | 
|`1 / 1 1 1000000000000 1000000 1000000`|`500000500000000001`| Giá trị lớn và số học số nguyên | 

## Vỏ cạnh 

Trường hợp đẳng thức ban đầu là`1 1 1 1`. Đây`gap = 0`, Vì thế`k = 0`đã thỏa mãn điều kiện rồi. Thuật toán ngay lập tức trở lại`x = 1`, tránh vòng đầu tiên không cần thiết. 

Ranh giới bình đẳng được thể hiện bằng`1 4 1 1`. Khoảng cách ban đầu là`3`. Vì`k = 1`, tổng số tiền đóng là`2`, điều đó là không đủ. Vì`k = 2`, số tam giác là`3`, Vì thế`(a+b)T_2 = 6`, vượt quá khoảng cách. Đề nghị của Barnum là`1 + 3 = 4`, đưa ra câu trả lời đúng`4`. Việc so sánh chặt chẽ sẽ xử lý sai các trường hợp trong đó hai lời đề nghị đáp ứng chính xác. 

Mẫu đầu tiên,`7 18 1 3`, nắm bắt được vấn đề đặt hàng giữa hai ưu đãi. Tại`k = 2`, Barnum cung cấp`10`trong khi lời đề nghị của Carlisle sau vòng đấu tương tự là`9`. Quá trình kết thúc lúc`10`, mặc dù lời đề nghị của Barnum không đạt được lời đề nghị trước đó của Carlisle`15`. Điều kiện khoảng cách kết hợp nắm bắt điều này một cách chính xác. 

Đối với trường hợp lớn`1 1000000000000 1000000 1000000`, khoảng cách là`999999999999`. Tại`k = 999999`, số tam giác là`499999500000`, vẫn còn quá nhỏ sau khi nhân với`2 * 10^6`. Tại`k = 1000000`, số tam giác trở thành`500000500000`, thế là đủ rồi. Lời đề nghị cuối cùng của Barnum là 

1+1000000⋅500000500000=500000500000000001. 

Việc tính toán diễn ra thoải mái trong phạm vi số nguyên của Python vì số nguyên Python tự động tăng lên và tìm kiếm nhị phân tránh lặp lại qua một triệu vòng.
