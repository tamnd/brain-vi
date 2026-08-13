---
title: "CF 102297D - Quầy nước chanh"
description: "Chúng tôi có một chuỗi ngày bán hàng. Vào ngày (i), chính xác (ci) cốc được yêu cầu. Mỗi cốc tiêu thụ (x) quả chanh và (s) ounce đường. Có thể mua chanh riêng lẻ, trong khi đường chỉ được bán trong túi 5 pound."
date: "2026-08-13T08:24:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102297
codeforces_index: "D"
codeforces_contest_name: "UCF Locals 2015"
rating: 0
weight: 102297
solve_time_s: 153
verified: true
draft: false
---

[CF 102297D - Quầy bán nước chanh](https://codeforces.com/problemset/problem/102297/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi ngày bán hàng. Vào ngày (i), chính xác (c_i) cốc được yêu cầu. Mỗi cốc tiêu thụ (x) quả chanh và (s) ounce đường. Có thể mua chanh riêng lẻ, trong khi đường chỉ được bán trong túi 5 pound. Vì một pound chứa 16 ounce nên một túi đường chứa chính xác 80 ounce. 

Cả hai nguyên liệu đều có thể được mua vào buổi sáng trước khi khách hàng của ngày hôm đó đến và những nguyên liệu chưa sử dụng vẫn có sẵn vào những ngày sau đó. Giá một quả chanh và giá một túi đường có thể thay đổi theo từng ngày. Chúng tôi biết trước toàn bộ trình tự và có số vốn không giới hạn, vì vậy câu hỏi duy nhất là mua khi nào và bao nhiêu. 

Đối với mỗi trường hợp thử nghiệm, đầu ra là tổng số xu tối thiểu cần thiết để mua đủ chanh và đường cho nhu cầu hàng ngày. 

Các ràng buộc đủ nhỏ để thậm chí (O(d^2)) có thể vừa vặn thoải mái vì (d\le1000), nhưng cấu trúc cho phép giải pháp (O(d)). Có nhiều nhất 100 trường hợp thử nghiệm, do đó, cách tiếp cận (O(d^2)) có thể thực hiện khoảng (100\times1000^2=10^8) số lần lặp vòng lặp bên trong ở mức tối đa theo lý thuyết, gần với giới hạn trong Python một cách không cần thiết. Quét tuyến tính an toàn hơn nhiều. Kho lưu trữ chính thức của Codeforces liệt kê giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Trường hợp tinh vi đầu tiên là đường được bán nguyên túi. Coi như```
1
1 1 10
1 1 7
```Một cốc cần 10 ounce đường, vì vậy cần có cả một túi 80 ounce. Quả chanh có giá 1 xu và túi đường có giá 7 xu, cho kết quả là 8. Một giải pháp bất cẩn nhân 10 ounce cần thiết với giá của một túi sẽ tạo ra 71 xu, trong khi một giải pháp làm tròn nhu cầu mỗi ngày một cách độc lập sau một số biến đổi cũng có thể xử lý sai 70 ounce chưa sử dụng. 

Trường hợp cạnh thứ hai xảy ra khi nhiều ngày gộp lại vẫn nhét vừa một túi đường. Coi như```
1
2 1 10
1 10 100
7 20 99
```Ngày đầu tiên cần 10 ounce và ngày thứ hai cần 70 ounce, vì vậy tổng cộng cần chính xác là 80 ounce. Một túi mua ngày thứ nhất đủ dùng cho cả hai ngày, gồm 10 xu chanh và 100 xu đường, tổng cộng là 110 xu. Một giải pháp đáp ứng riêng nhu cầu đường mỗi ngày sẽ mua hai túi và thay vào đó nhận được 209 xu. 

Trường hợp thứ ba là khi giá rẻ hơn xuất hiện muộn hơn. Coi như```
1
2 1 1
1 50 500
80 1 1
```Ngày đầu tiên buộc phải mua một quả chanh và một túi đường theo giá ngày đầu tiên. Vào ngày thứ 2, giá rẻ hơn có thể được sử dụng cho các yêu cầu bổ sung. Câu trả lời là 631 xu. Một chiến lược mua nhiều túi sớm chỉ vì ngày đầu tiên rẻ so với tương lai sẽ bỏ lỡ thực tế là việc chờ đợi được cho phép và giá sau đó có thể còn thấp hơn nữa. 

## Phương pháp tiếp cận 

Một giải pháp cưỡng bức trực tiếp có thể liệt kê mọi lịch trình mua hàng có thể có. Đối với đường, gọi (B) là tổng số bao cần thiết trong tất cả các ngày. Về nguyên tắc, mỗi túi có thể được chỉ định cho một trong (d) ngày mua, sau đó chúng tôi kiểm tra xem lịch trình kết quả có đủ đường mỗi ngày hay không và tính toán chi phí của nó. Tổng số bài tập tối đa là (d^B). Vì tổng nhu cầu có thể đạt (1000\cdot1000\cdot10=10^7) ounce nên (B) có thể đạt (\lceil10^7/80\rceil=125000). Với (d=1000), điều này mang lại tối đa (1000^{125000}) bài tập cho riêng đường. Bạo lực là đúng vì nó xem xét rõ ràng mọi lịch trình pháp lý, nhưng nó sẽ trở nên vô dụng rất lâu trước những hạn chế tối đa. 

Quan sát hữu ích là nguồn cung cấp cùng một thành phần có thể thay thế cho nhau. Đối với chanh, không có hạn chế nào về đóng gói. Giả sử ngày (i) yêu cầu (q_i=c_i x) chanh mới. Những quả chanh (q_i) cụ thể đó phải được mua không muộn hơn ngày (i), vì vậy giá rẻ nhất có thể cho mỗi quả chỉ đơn giản là giá chanh tối thiểu trong số các ngày (1) đến (i). Chúng ta có thể mua chúng vào bất kỳ ngày nào trước đó có mức giá tối thiểu đó. 

Đường gần như giống nhau, ngoại trừ túi có sức chứa 80. Gọi (S_i) là tổng số ounce cần thiết trong ngày (i). Vào đầu ngày (i), ít nhất 

[ 
B_i=\left\lceil\frac{S_i}{80}\right\rceil 
] 

túi chắc chắn đã được mua. Ngày hôm trước chỉ có (B_{i-1}) túi bị ép buộc. Do đó, chính xác (B_i-B_{i-1}) túi bổ sung trở nên cần thiết vào ngày (i). Mỗi túi trong số đó có thể được mua vào bất kỳ ngày nào từ ngày 1 đến (i), vì vậy giá rẻ nhất có thể của chúng là giá túi đường tối thiểu được thấy cho đến nay. 

Điểm mấu chốt là chúng tôi không làm tròn nhu cầu đường mỗi ngày một cách riêng biệt. Chúng tôi làm tròn nhu cầu tích lũy. Điều này cho thấy thực tế là số ounce chưa sử dụng trong túi sẽ được sử dụng vào những ngày sau đó. 

Hai thành phần này độc lập nên chúng ta có thể xử lý đồng thời trong một lần quét. Đối với chanh, hãy cộng số chanh cần thiết trong ngày nhân với giá chanh rẻ nhất cho đến nay. Đối với đường, hãy cập nhật số ounce tích lũy, tính số lượng bao mới được yêu cầu và chỉ tính phí cho những bao mới được yêu cầu với giá đường rẻ nhất từng thấy cho đến nay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(d^B)) cho đường | (O(B)) | Quá chậm | 
| Tối ưu | (O(d)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo giá chanh tối thiểu và giá túi đường tối thiểu thành giá trị lớn hơn mọi giá đầu vào có thể có. Đồng thời khởi tạo nhu cầu đường tích lũy và số lượng túi đường đã được yêu cầu về 0. 
2. Đọc từng ngày một. Đối với ngày (i), cập nhật giá chanh tối thiểu thành giá chanh rẻ nhất trong số các ngày (1) đến (i) và thực hiện tương tự đối với túi đường. Nguồn cung cấp cần thiết vào ngày (i) có thể đã được mua vào bất kỳ ngày nào trước đó, vì vậy các tiền tố tối thiểu này mô tả giá mua hợp pháp rẻ nhất. 
3. Tính số chanh mới cần có trong ngày này là (c_i x). Nhân số lượng đó với giá chanh tối thiểu hiện tại và cộng nó vào câu trả lời. Lemon là các đơn vị riêng lẻ nên không có sự phức tạp về làm tròn hoặc trạng thái tồn kho. 
4. Thêm (c_i s) vào nhu cầu đường tích lũy. Chuyển đổi số tiền tích lũy đó thành số lượng túi tối thiểu bằng cách sử dụng 
[ 
B_i=\left\lceil\frac{S_i}{80}\right\rceil. 
] 
Trong số học số nguyên đây là`(S_i + 79) // 80`. 
5. Tính toán`new_bags = B_i - B_previous`. Chỉ những chiếc túi mới được yêu cầu này mới thêm chi phí. Nhân chúng với giá túi đường rẻ nhất được thấy cho đến nay và cộng số tiền đó vào câu trả lời. 
6. Lưu (B_i) như số túi trước đó và tiếp tục cho ngày hôm sau. Sau ngày cuối cùng, câu trả lời tích lũy là chi phí tối thiểu cho toàn bộ test case. 

### Tại sao nó hoạt động 

Đối với chanh, mỗi quả chanh mới được yêu cầu vào ngày (i) phải được mua theo ngày (i), và tất cả những ngày mua trước đó đều không bị giới hạn về số lượng. Do đó, giá tối thiểu có thể có của nó chính xác là giá chanh tối thiểu ở tiền tố kết thúc tại (i). Mua số lượng cần thiết ở mức giá đó vừa khả thi vừa tối ưu. 

Đối với đường, sau ngày (i) tổng nhu cầu là (S_i) ounce nên mỗi lịch trình khả thi đều phải sở hữu ít nhất bao (\lceil S_i/80\rceil). Sự khác biệt (B_i-B_{i-1}) chính xác là số túi bổ sung mà bất kỳ lịch trình khả thi nào cũng phải có được trong ngày (i). Mỗi túi như vậy có thể được mua vào bất kỳ ngày nào trong tiền tố, vì vậy chi phí rẻ nhất có thể của nó là giá đường tối thiểu ở tiền tố. Mua những chiếc túi đó vào một ngày đạt mức tối thiểu đó là khả thi vì nguồn cung cấp chuyển tiếp và việc lưu trữ là không giới hạn. Do đó, mỗi túi được tính phí sẽ có mức giá hợp pháp nhỏ nhất cho nó, trong khi mọi túi cần thiết sẽ được tính phí đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30
BAG_OUNCES = 80

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        d, x, s = map(int, input().split())

        min_lemon_price = INF
        min_sugar_price = INF

        cumulative_sugar = 0
        previous_bags = 0
        answer = 0

        for _ in range(d):
            c, pl, ps = map(int, input().split())

            min_lemon_price = min(min_lemon_price, pl)
            min_sugar_price = min(min_sugar_price, ps)

            lemons_needed = c * x
            answer += lemons_needed * min_lemon_price

            cumulative_sugar += c * s
            required_bags = (cumulative_sugar + BAG_OUNCES - 1) // BAG_OUNCES

            new_bags = required_bags - previous_bags
            answer += new_bags * min_sugar_price

            previous_bags = required_bags

        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào xử lý chính xác một ngày cho mỗi lần lặp, do đó thuật toán không bao giờ cần lưu trữ toàn bộ chuỗi.`min_lemon_price`Và`min_sugar_price`đại diện cho cơ hội mua hàng rẻ nhất hiện có trong ngày hiện tại. 

Việc tính toán quả chanh sử dụng`c * x`bởi vì mỗi cốc tiêu thụ chính xác`x`chanh riêng lẻ. Không có lý do gì để theo dõi riêng số chanh còn sót lại. Bất kỳ quả chanh nào được mua với giá rẻ vào ngày hôm trước đều có thể vẫn có sẵn và đối số tiền tố tối thiểu đã giải thích cho điều đó. 

Đối với đường,`cumulative_sugar`là trạng thái quan trọng. biểu thức`(cumulative_sugar + 79) // 80`thực hiện phép chia trần cho 80. Sử dụng`c * s`trực tiếp trong phép tính làm tròn sẽ không chính xác vì túi được sử dụng một phần có thể đáp ứng nhu cầu sau này.`new_bags`là sự khác biệt giữa yêu cầu tích lũy hiện tại và yêu cầu tích lũy trước đó. Điều này ngăn cản chúng tôi tính phí lại cho những túi đã có sẵn lượng đường chưa sử dụng. Nó cũng xử lý chính xác bội số của 80. Nếu nhu cầu tích lũy thay đổi từ 79 đến 80 ounce, số lượng túi yêu cầu vẫn ở mức 1, do đó không có túi mới nào được mua. 

Số nguyên Python có độ chính xác tùy ý, do đó, ngay cả tổng chi phí lớn nhất có thể cũng được xử lý mà không bị tràn. Các giá trị lớn nhất vẫn thấp hơn nhiều so với bất kỳ mối quan tâm nào về bộ nhớ thực tế và thuật toán chỉ giữ một số lượng biến số nguyên không đổi. 

## Ví dụ đã hoạt động 

Tài liệu chính thức của cuộc thi đưa ra kết quả mẫu là 31977 và 1347. 

Đối với mẫu 1,```
3 3 2
200 10 399
300 8 499
400 12 499
```nhà nước phát triển như sau. 

| Ngày | Cốc | Giá chanh | Giá chanh tối thiểu | Giá đường | Giá đường tối thiểu | Đường tích lũy | Túi bắt buộc | Túi mới | Chi phí hàng ngày | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 200 | 10 | 10 | 399 | 399 | 400 | 5 | 5 | 7995 | 
| 2 | 300 | 8 | 8 | 499 | 399 | 1000 | 13 | 8 | 10392 | 
| 3 | 400 | 12 | 8 | 499 | 399 | 1800 | 23 | 10 | 13590 | 

Giá chanh lần lượt là 6000, 7200 và 9600 xu. Giá đường là 1995, 3192 và 3990 cent. Tổng cộng của họ là (7995+10392+13590=31977). Giá chanh rẻ hơn của ngày thứ hai ảnh hưởng đến tất cả chanh mới được yêu cầu từ ngày đó trở đi, trong khi giá đường 399 vẫn là mức tối thiểu trong suốt quá trình thử nghiệm. 

Đối với mẫu 2,```
2 5 10
9 10 199
8 20 99
```dấu vết là: 

| Ngày | Cốc | Giá chanh tối thiểu | Đường tích lũy | Giá đường tối thiểu | Túi bắt buộc | Túi mới | Chi phí hàng ngày | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 9 | 10 | 90 | 199 | 2 | 2 | 848 | 
| 2 | 8 | 10 | 170 | 99 | 3 | 1 | 499 | 

Ngày đầu tiên cần 45 quả chanh, giá 450 xu và 90 ounce đường, cần hai túi với giá 199 xu mỗi túi. Vào ngày thứ hai, giá chanh thấp hơn nên tiền tố tối thiểu vẫn là 10. Giá đường giảm xuống 99 nên một túi bổ sung chỉ có giá 99 xu. Tổng số là (848+499=1347). 

Ví dụ này chứng minh tại sao thuật toán theo dõi lượng đường tích lũy thay vì làm tròn độc lập mỗi ngày. Hai túi đầu tiên đã đủ để chứa 160 ounce đầu tiên, vì vậy chỉ cần thêm một túi nữa sau khi đã bao gồm nhu cầu của ngày thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(d)) cho mỗi trường hợp thử nghiệm | Mỗi ngày được xử lý một lần với công việc liên tục | 
| Không gian | (O(1)) | Chỉ có giá tiền tố, nhu cầu tích lũy và một số bộ đếm được lưu trữ | 

Với (d\le1000), quá trình quét tuyến tính thực hiện tối đa 1000 lần lặp cho mỗi trường hợp kiểm thử. Ngay cả với 100 trường hợp thử nghiệm, đó chỉ là bản ghi 100000 ngày, vì vậy giải pháp vẫn nằm trong giới hạn đã nêu và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`lemonade_solution.py`và phơi bày`solve()`như được hiển thị ở trên.```python
# helper: run solution on input string, return output string
import sys
import io
from lemonade_solution import solve

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

# Provided samples
assert run(
    """2
3 3 2
200 10 399
300 8 499
400 12 499
2 5 10
9 10 199
8 20 99
"""
) == "31977\n1347", "provided samples"

# Minimum-size input
assert run(
    """1
1 1 1
1 7 1
"""
) == "8", "minimum-size case"

# All values equal
assert run(
    """1
2 2 3
10 5 100
10 5 100
"""
) == "300", "all-equal values"

# Exactly 80 ounces across two days, so only one sugar bag is needed
assert run(
    """1
2 1 10
1 10 100
7 20 99
"""
) == "180", "exact bag boundary"

# A cheaper later price must be used for later requirements
assert run(
    """1
2 1 1
1 50 500
80 1 1
"""
) == "631", "later cheaper prices"

# Maximum-size dimensions and values.
# 1000 days, 1000 cups/day, 10 lemons/cup, 10 ounces/cup.
# Lemon cost = 10,000,000 * 50 = 500,000,000.
# Sugar demand = 10,000,000 ounces = 125,000 bags.
# Sugar cost = 125,000 * 500 = 62,500,000.
# Total = 562,500,000.
max_case = ["1", "1000 10 10"]
max_case.extend(["1000 50 500"] * 1000)

assert run("\n".join(max_case) + "\n") == "562500000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 1 / 1 7 1`|`8`| Đầu vào tối thiểu và một túi đường được sử dụng một phần | 
|`2 2 3`với hai ngày giống hệt nhau |`300`| Giá bằng nhau và hàng tồn kho tích lũy | 
|`2 1 10 / 1 10 100 / 7 20 99`|`180`| Giới hạn chính xác 80 ounce và không có túi thứ hai không cần thiết | 
|`2 1 1 / 1 50 500 / 80 1 1`|`631`| Sau này giá rẻ hơn và chờ mua thêm vật tư | 
| 1000 ngày có giá trị tối đa giống hệt nhau |`562500000`| Số ngày tối đa, nhu cầu, yêu cầu về thành phần và giá | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một túi đường được lấp đầy một phần. Vì```
1
1 1 10
1 1 7
```nhu cầu đường tích lũy là 10 ounce. Thuật toán tính toán ((10+79)//80=1), do đó, một túi được tính giá 7 xu. Giá quả chanh là 1 xu, được 8 xu. 70 ounce chưa sử dụng vẫn có sẵn, nhưng không có nhu cầu sau này, vì vậy chúng chỉ đơn giản là không được sử dụng. 

Trường hợp cạnh thứ hai là ranh giới túi chính xác:```
1
2 1 10
1 10 100
7 20 99
```Sau ngày thứ nhất, nhu cầu tích lũy là 10 ounce, do đó cần có một túi. Sau ngày thứ 2, nhu cầu tích lũy trở thành chính xác 80 ounce và số lượng túi yêu cầu vẫn là một. Như vậy`new_bags`bằng 0 vào ngày thứ 2, mặc dù có nhu cầu mới về 70 ounce. Một túi hiện có đã chứa đủ lượng đường. Giá chanh lần lượt là 10 xu và 70 xu, vì giá chanh rẻ nhất cho đến nay là 10 xu và giá đường chỉ là 100 xu của ngày đầu tiên. Tổng số là 180. 

Trường hợp cạnh thứ ba kiểm tra việc giảm giá trong tương lai:```
1
2 1 1
1 50 500
80 1 1
```Sau ngày thứ nhất, không thể tránh khỏi một quả chanh và một túi đường, giá 50 xu và 500 xu. Sau ngày thứ 2, người ta đã yêu cầu 81 ounce đường, vì vậy tổng cộng cần có hai túi. Chỉ cần một túi mới và giá đường tối thiểu hiện tại là 1 xu. 80 quả chanh của ngày thứ hai cũng sử dụng tiền tố mới giá chanh tối thiểu là 1 xu mỗi quả. Tổng số là (50+500+80+1=631). Đây chính xác là tình huống mà việc mua thêm túi vào ngày đầu tiên sẽ rất lãng phí. 

Trường hợp ranh giới cuối cùng là kích thước đầu vào lớn nhất có thể. Với 1000 ngày có 1000 cốc, (x=s=10), mỗi ngày cần 10000 quả chanh và 10000 ounce đường. Trong toàn bộ thời gian, con số này là 10.000.000 quả chanh và 10.000.000 ounce đường. Loại thứ hai yêu cầu chính xác 125.000 túi vì (10.000.000/80=125.000). Với mức giá cố định là 50 xu một quả chanh và 500 xu một túi, câu trả lời là 500.000.000 cộng với 62.500.000, hay 562.500.000 xu. Thuật toán đạt được kết quả này chỉ với một lần vượt qua và không bao giờ tạo mảng kiểm kê.
