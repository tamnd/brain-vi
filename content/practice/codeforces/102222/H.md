---
title: "CF 102222H - Chiến đấu chống lại quái vật"
description: "Mỗi quái vật có giá trị máu HP và giá trị tấn công ATK. Trong mỗi giây, tất cả quái vật vẫn còn sống sẽ tấn công anh hùng trước, do đó anh hùng sẽ mất tổng giá trị tấn công của mình. Sau đó, người anh hùng chọn chính xác một con quái vật còn sống và tấn công nó."
date: "2026-08-17T22:12:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "H"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 138
verified: true
draft: false
---

[CF 102222H - Chiến đấu chống lại quái vật](https://codeforces.com/problemset/problem/102222/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi quái vật có một giá trị sức khỏe`HP`và giá trị tấn công`ATK`. Trong mỗi giây, tất cả quái vật vẫn còn sống sẽ tấn công anh hùng trước, do đó anh hùng sẽ mất tổng giá trị tấn công của mình. Sau đó, người anh hùng chọn chính xác một con quái vật còn sống và tấn công nó. 

Thiệt hại gây ra cho một quái vật cụ thể chỉ phụ thuộc vào số lần quái vật đó đã bị tấn công. Lần đầu tiên nó nhận được giao dịch thành công`1`, giao dịch thứ hai của nó`2`, giao dịch thứ ba của nó`3`, vân vân. Do đó, nếu một con quái vật cần`k`tấn công để chết, những cuộc tấn công đó phải gây ra 

[ 
1+2+\cdots+k=\frac{k(k+1)}2 
] 

tổng thiệt hại. Câu hỏi đặt ra là làm thế nào để chọn thứ tự tấn công sao cho tổng sát thương mà tướng nhận phải càng nhỏ càng tốt. 

Đầu vào chứa tối đa`10^3`trường hợp thử nghiệm, với nhiều nhất`10^5`quái vật trong một trường hợp thử nghiệm và nhiều nhất là`10^6`quái vật trên tất cả các trường hợp thử nghiệm. Giá trị sức khỏe và tấn công cao nhất`10^5`. Các giới hạn này loại trừ bất kỳ điều gì liên quan đến tập hợp con hoặc hoán vị của quái vật. Thậm chí một`O(n^2)`phương pháp sẽ quá tốn kém khi`n=10^5`, vì vậy giải pháp dự định phải giảm vấn đề về cơ bản là sắp xếp. 

Đối với mỗi quái vật, số lần tấn công mà nó yêu cầu là rất nhỏ mặc dù lượng máu tiềm tàng rất lớn. Với`HP <= 10^5`,`447`các cuộc tấn công đã được giải quyết`100128`sát thương, vì vậy mọi quái vật đều cần nhiều nhất`447`các cuộc tấn công. Các ràng buộc ban đầu và định dạng đầu ra được xác nhận bởi trang vấn đề chính thức. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai đơn giản trở nên sai lầm. 

Đối với một con quái vật, không có quyết định thứ tự nào cả. Ví dụ,```
1
1
1 1
```có câu trả lời`1`. Một công thức vô tình đếm số lượng quái vật còn sống thêm một giây sẽ tạo ra`2`, điều này là sai vì quái vật tấn công một lần và sau đó chết ngay sau đòn tấn công đầu tiên của anh hùng. 

Giá trị sức khỏe chính xác bằng số tam giác cũng cần xử lý chính xác. Vì```
1
1
3 2
```con quái vật cần chính xác hai đòn tấn công bởi vì`1 + 2 = 3`, vậy câu trả lời là`4`. Sử dụng một cách nghiêm ngặt`>`thay vì`>=`khi tìm đủ số đòn tấn công cần thiết sẽ yêu cầu tấn công thứ ba không chính xác. 

Tỷ lệ đặt hàng bằng nhau là một trường hợp ranh giới khác. Nếu hai con quái vật có`(k_1, ATK_1) = (1, 2)`Và`(k_2, ATK_2) = (2, 4)`, thì cả hai đều có tỉ số`k / ATK = 1/2`. Cả hai đơn hàng đều có tổng chi phí như nhau. Bộ so sánh phải coi đây là một sự ràng buộc thay vì áp đặt một thứ tự không nhất quán. 

Cuối cùng, câu trả lời có thể vượt quá số nguyên 32 bit. Vì`100000`quái vật với`HP=1`Và`ATK=1`, thời gian hoàn thành là`1,2,...,100000`, cho 

[ 
\frac{100000\cdot100001}{2}=5000050000. 
] 

Số nguyên 32 bit sẽ tràn. Số nguyên Python không gặp phải vấn đề này, nhưng nhu cầu triển khai C++ tương ứng`long long`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là thử mọi mệnh lệnh có thể để tiêu diệt hoàn toàn quái vật. Đối với một lệnh cố định, chúng ta có thể tính toán số lần tấn công cần thiết của mỗi quái vật và sau đó mô phỏng thời gian tích lũy. Điều này đúng vì mọi thứ tự có thể đều được xem xét, vì vậy thứ tự tốt nhất phải xuất hiện trong số các ứng cử viên. 

Vấn đề là số lượng ứng viên. Với`n`có quái vật ở đó`n!`hoán vị, và đánh giá một hoán vị mất`O(n)`thời gian. Do đó, công việc trong trường hợp xấu nhất là`Theta(n * n!)`. Thậm chí`20! * 20`là về`4.87 * 10^19`các bước xử lý đơn hàng cơ bản, vượt xa giới hạn thực tế. Vì`n=10^5`, việc liệt kê hoán vị là không khả thi từ xa. 

Quan sát hữu ích là tổng số lần tấn công cần thiết của quái vật được ấn định trước khi trận chiến bắt đầu. Nếu một con quái vật cần`k`tấn công, các cuộc tấn công thực tế có thể được coi là công việc xử lý thời gian`k`. Giá trị tấn công của nó`ATK`là cái giá phải trả cho mỗi giây mà con quái vật này còn sống. 

Giả sử tất cả quái vật có tổng giá trị tấn công`S`, và xem xét một con quái vật cần`k`các cuộc tấn công. Nếu nó bị giết sau`C`các cuộc tấn công anh hùng đã xảy ra, nó góp phần chính xác`ATK * C`vào tổng sát thương của anh hùng. Như vậy toàn bộ bài toán trở thành bài toán lập kế hoạch: mỗi con quái vật là một công việc có thời gian xử lý`k`và cân nặng`ATK`và chúng tôi muốn giảm thiểu tổng thời gian hoàn thành có trọng số. 

Thứ tự tối ưu có thể được rút ra bằng đối số trao đổi hai con quái vật. Hãy xem xét quái vật`A`Và`B`, với số lần tấn công cần thiết`k_A`,`k_B`và giá trị tấn công`w_A`,`w_B`. Bỏ qua mọi thứ trước hai con quái vật này và để thời gian hiện tại trôi qua`C`. 

Nếu như`A`bị giết đầu tiên, sự đóng góp của họ là 

[ 
w_A(C+k_A)+w_B(C+k_A+k_B). 
]

Nếu như`B`bị giết đầu tiên, sự đóng góp của họ là 
[ 
w B(C+k_B)+x_I(S+k_B+x_I). 
] 
Các điều khoản phổ biến bị hủy bỏ. Đặt`A`đầu tiên không tệ hơn chính xác là khi nào 
[ 
k_A w_B \le k_B x_I. 
] 
tương đương, 
[ 
\frac{k_A}{k_A}\le\frac{k_B}{w_B}. 
] 
Vì vậy quái vật nên được sắp xếp theo thứ tự tăng dần`required_attacks / ATK`. Đây là đối số trao đổi tương tự được sử dụng trong quy tắc lập kế hoạch thời gian hoàn thành có trọng số tiêu chuẩn và so sánh sản phẩm chéo tương tự xuất hiện trong các giải pháp hiện có cho vấn đề này. 

Lực lượng vũ phu hoạt động vì một thứ tự hoàn chỉnh sẽ xác định hoàn toàn thời gian hoàn thành của mọi quái vật, nhưng nó không thành công vì có rất nhiều thứ tự. Đối số trao đổi cho phép chúng ta loại bỏ tất cả các thứ tự có chứa sự đảo ngược, để lại một thứ tự được sắp xếp tối ưu toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · n!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi quái vật, hãy tính số lượng tối thiểu`k`các cuộc tấn công anh hùng cần thiết để giết nó. Chúng ta cần cái nhỏ nhất`k`thỏa mãn 

[ 
\frac{k(k+1)}2\ge HP. 
]

Từ`HP <= 10^5`,`k <= 447`, vì vậy có thể tìm thấy những giá trị này một cách hiệu quả bằng cách sử dụng bảng số tam giác được tính toán trước. 
2. Đối xử với mỗi quái vật như một công việc với thời gian xử lý`k`và cân nặng`ATK`. Thời gian xử lý có nghĩa là cần bao nhiêu lượt anh hùng trước khi quái vật chết, trong khi trọng lượng cho biết anh hùng nhận được bao nhiêu sát thương từ quái vật đó trong mỗi giây nó còn sống. 
3. Sắp xếp quái vật theo cấp độ tăng dần`k / ATK`. Sự so sánh có thể được hiểu mà không cần phân chia: quái vật`A`thuộc về quái vật`B`khi nào 

[ 
k_A ATK_B \le k_B ATK_A. 
] 

Dạng sản phẩm chéo này là sự so sánh toán học chính xác và tránh các vấn đề về thứ tự liên quan đến phép chia. 
4. Vượt qua các quái vật đã được sắp xếp. Duy trì`time`, tổng số đòn tấn công của anh hùng được sử dụng để tiêu diệt tất cả quái vật đã được xử lý trước đó. Đối với quái vật hiện tại, tăng`time`theo số lần tấn công cần thiết của nó`k`. Cái chết của nó xảy ra vào thời điểm mới này nên nó góp phần 

[ 
ATK \lần thời gian 
] 

làm tổn hại đến câu trả lời. 
5. Tính tổng các chi phí hoàn thành có trọng số này và in kết quả theo yêu cầu`Case #x: answer`định dạng. 

Điều bất biến là sau khi xử lý lần đầu tiên`i`quái vật theo thứ tự sắp xếp,`time`chính xác là số lần tấn công của anh hùng cần thiết để tiêu diệt những con quái vật đó và`answer`là mức đóng góp thiệt hại có trọng số tối thiểu có thể có trong số tất cả các lịch trình hoàn thành những quái vật giống nhau theo thứ tự đó. Đối số trao đổi cho thấy rằng bất cứ khi nào hai quái vật liền kề vi phạm thứ tự tỷ lệ, việc hoán đổi chúng không thể làm tăng kết quả. Việc loại bỏ nhiều lần mọi sự đảo ngược sẽ tạo ra thứ tự được sắp xếp, do đó không có lịch trình nào khác có thể có tổng chi phí nhỏ hơn. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

# The largest possible number of attacks is 447 because
# 447 * 448 // 2 = 100128 >= 100000.
MAX_HITS = 447

triangular = [0]
for k in range(1, MAX_HITS + 1):
    triangular.append(triangular[-1] + k)

def solve(inp=input):
    t = int(inp())
    out = []

    for case_id in range(1, t + 1):
        n = int(inp())
        monsters = []

        for _ in range(n):
            hp, atk = map(int, inp().split())

            # Smallest k such that k * (k + 1) / 2 >= hp.
            k = bisect_left(triangular, hp, 1)

            monsters.append((k, atk))

        # Under the given bounds, distinct rational ratios k / atk
        # are separated by at least 1 / 10^10, much more than the
        # floating-point precision around these values.
        monsters.sort(key=lambda x: x[0] / x[1])

        time = 0
        answer = 0

        for hits, atk in monsters:
            time += hits
            answer += time * atk

        out.append(f"Case #{case_id}: {answer}")

    return "\n".join(out)

if __name__ == "__main__":
    sys.stdout.write(solve())
```Mảng số tam giác được xây dựng một lần. Vì sức khỏe lớn nhất là`100000`, số lần tấn công cần thiết nhiều nhất là`447`.`bisect_left`tìm số hình tam giác đầu tiên đạt đến lượng máu của quái vật, đưa ra chính xác số lần tấn công cần thiết. 

Mỗi quái vật được lưu trữ dưới dạng`(hits, atk)`. Chìa khóa sắp xếp là tỷ lệ`hits / atk`, theo thứ tự tăng dần. Quy tắc toán học dựa trên phép nhân chéo chính xác, nhưng việc triển khai có thể sử dụng tỷ lệ dấu phẩy động một cách an toàn ở đây vì`hits <= 447`Và`atk <= 100000`. Hai phân số như vậy khác nhau ít nhất một`1 / 10^10`, trong khi độ chính xác gấp đôi có độ phân giải tuyệt đối tốt hơn nhiều trong khoảng thời gian từ`0`ĐẾN`447`. 

Ranh giới quan trọng là việc sử dụng`bisect_left`. Nếu sức khỏe chính xác là hình tam giác, chẳng hạn như`HP=3`, giá trị hợp lệ đầu tiên phải là`2`, không`3`. Tìm kiếm giới hạn dưới cung cấp chính xác điều đó. 

Trong lần di chuyển cuối cùng,`time`được cập nhật trước khi thêm phần đóng góp của quái thú hiện tại. Điều này phù hợp với thứ tự chiến đấu: quái vật tấn công trong mọi giây cho đến và kể cả giây mà anh hùng tiêu diệt nó. Do đó, một con quái vật đã hoàn thành vào thời điểm`C`đóng góp`ATK * C`. 

Câu trả lời có thể là vài tỷ hoặc nhiều hơn, do đó việc triển khai có chủ ý giữ phép tính ở dạng số nguyên có độ chính xác tùy ý của Python. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, quái vật là```
HP  ATK
1   1
2   2
3   3
```Số lần tấn công cần thiết của họ là`1`,`2`, Và`2`. 

| Quái vật | HP | ATK | Các cuộc tấn công bắt buộc | Tỷ lệ`hits / ATK`| Thời gian hoàn thành | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 3 | 3 | 2 |`2/3`| 2 | 6 | 
| 1 | 1 | 1 | 1 |`1`| 3 | 3 | 
| 2 | 2 | 2 | 2 |`1`| 5 | 10 | 

Thứ tự sắp xếp là quái vật`3`, quái vật`1`, quái vật`2`. Tổng cộng là`6 + 3 + 10 = 19`, phù hợp với đầu ra mẫu. Dấu vết cho thấy tại sao một quái vật có giá trị tấn công cao thường nên bị tiêu diệt sớm, ngay cả khi phải mất nhiều đòn tấn công của tướng hơn. 

Đối với Mẫu 2, quái vật là```
HP  ATK
3   1
2   2
1   3
```Số lần tấn công cần thiết là`2`,`2`, Và`1`. 

| Quái vật | HP | ATK | Các cuộc tấn công bắt buộc | Tỷ lệ`hits / ATK`| Thời gian hoàn thành | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 3 | 1 |`1/3`| 1 | 3 | 
| 2 | 2 | 2 | 2 |`1`| 3 | 6 | 
| 1 | 3 | 1 | 2 |`2`| 5 | 5 | 

Thứ tự tối ưu là quái vật`3`, rồi quái vật`2`, rồi quái vật`1`. Tổng cộng là`3 + 6 + 5 = 14`. 

Ví dụ này thể hiện rõ ràng quy tắc tỷ lệ. Quái vật`3`có giá trị tấn công lớn nhất và chỉ cần một đòn tấn công, vì vậy để nó sống sót dù chỉ trong thời gian ngắn cũng rất tốn kém. Tỷ lệ được sắp xếp tự động đặt nó lên hàng đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Tính toán các cuộc tấn công cần thiết`O(n log 447)`, sau đó sắp xếp chiếm ưu thế với`O(n log n)`| 
| Không gian |`O(n)`| Mảng quái vật lưu trữ một cặp cho mỗi quái vật | 

Trên tất cả các trường hợp thử nghiệm có nhiều nhất`10^6`quái vật, do đó tổng công việc sắp xếp bị giới hạn bởi tổng tương ứng của`n log n`điều khoản. Thuật toán không thực hiện liệt kê tập hợp con, mô phỏng theo từng giây hoặc theo cặp`O(n^2)`quá trình xử lý, làm cho nó phù hợp với yêu cầu đã nêu`10^5`mỗi bài kiểm tra và`10^6`tổng giới hạn. Tuyên bố chính thức đưa ra giới hạn thời gian 10 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`solve`chức năng từ giải pháp trên có sẵn.```python
import sys
import io

def run(inp: str) -> str:
    return solve(io.StringIO(inp).readline) + "\n"

# Sample 1
assert run(
    """1
3
1 1
2 2
3 3
"""
) == "Case #1: 19\n", "sample 1"

# Sample 2
assert run(
    """1
3
3 1
2 2
1 3
"""
) == "Case #1: 14\n", "sample 2"

# Minimum-size case
assert run(
    """1
1
1 1
"""
) == "Case #1: 1\n", "single monster"

# Exact triangular number, k = 2
assert run(
    """1
1
3 2
"""
) == "Case #1: 4\n", "exact triangular HP"

# Different ratios, catches incorrect attack-value-only sorting
assert run(
    """1
2
1 2
2 3
"""
) == "Case #1: 11\n", "ratio ordering"

# All values equal
assert run(
    """1
4
3 2
3 2
3 2
3 2
"""
) == "Case #1: 40\n", "equal ratios"

# Boundary HP = 100000, requiring 447 attacks
assert run(
    """1
1
100000 1
"""
) == "Case #1: 447\n", "maximum HP"

# Maximum number of monsters
max_input = "1\n100000\n" + "1 1\n" * 100000
assert run(max_input) == "Case #1: 5000050000\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1 1`|`Case #1: 1`| Đầu vào tối thiểu và tử vong ngay lập tức | 
|`1 / 1 / 3 2`|`Case #1: 4`| Ranh giới số tam giác chính xác | 
|`1 / 2 / 1 2 / 2 3`|`Case #1: 11`| Chính xác`hits / ATK`đặt hàng | 
| Bốn bản sao của`3 2`|`Case #1: 40`| Tỷ lệ bằng nhau và thứ tự hòa tùy ý | 
|`1 / 1 / 100000 1`|`Case #1: 447`| Giới hạn sức khỏe và số lần tấn công tối đa | 
|`100000`bản sao của`1 1`|`Case #1: 5000050000`| Tối đa`n`và câu trả lời lớn | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
1
1
1 1
```tìm kiếm số tam giác tìm thấy`k=1`. Mảng được sắp xếp chứa một quái vật, vì vậy`time`trở thành`1`và câu trả lời trở thành`1 * 1 = 1`. Quái vật tấn công một lần rồi chết, do đó không có thêm sát thương sau khi chết. 

Để có ranh giới tam giác chính xác```
1
1
3 2
```số tam giác bắt đầu`0, 1, 3, 6`.`bisect_left`tìm chỉ mục`2`, vậy là quái vật cần chính xác hai đòn tấn công. Thời gian hoàn thành của nó là`2`, cho`2 * 2 = 4`. Điều này tránh được sai lầm thường gặp khi coi sự bình đẳng là thiệt hại không đủ. 

Đối với trường hợp sắp xếp theo tỷ lệ```
1
2
1 2
2 3
```con quái vật đầu tiên yêu cầu`1`tấn công và có giá trị tấn công`2`, cho tỉ số`1/2`. Điều thứ hai yêu cầu`2`tấn công và có giá trị tấn công`3`, cho tỉ số`2/3`. Thuật toán tiêu diệt quái vật đầu tiên tại thời điểm`1`, đóng góp`2`, sau đó giết chết lần thứ hai`3`, đóng góp`9`, tổng cộng là`11`. Đảo ngược chúng sẽ cho`6 + 6 = 12`, do đó, chỉ sắp xếp theo giá trị tấn công hoặc chỉ theo các cuộc tấn công được yêu cầu sẽ bỏ lỡ mức tối ưu. 

Để có tỷ lệ bằng nhau, hãy xem xét```
1
2
1 2
2 4
```Cả hai quái vật đều có tỷ lệ`1/2`. Nếu việc đầu tiên được xử lý trước thì chi phí là`2*1 + 4*3 = 14`. Nếu cái thứ hai được xử lý trước thì chi phí là`4*2 + 2*3 = 14`. Điều kiện trao đổi trở nên bằng nhau, do đó thứ tự nào cũng là tối ưu. 

Để có sức khỏe tối đa,```
1
1
100000 1
```thuật toán tìm thấy`447`bởi vì 

[ 
\frac{446\cdot447}{2}=99681<100000 
] 

trong khi 

[ 
\frac{447\cdot448}{2}=100128\ge100000. 
] 

Do đó, quái vật chết trong cuộc tấn công của anh hùng thứ 447 và vì giá trị tấn công của nó là`1`, câu trả lời là`447`. 

Hộp đựng quái vật bằng nhau có kích thước tối đa chứa`100000`quái vật, mỗi quái vật yêu cầu một đòn tấn công và có giá trị tấn công`1`. Mọi thứ tự đều tương đương. Thời gian hoàn thành của họ là`1`bởi vì`100000`, vậy câu trả lời là 

[ 
1+2+\cdots+100000=5000050000. 
] 

Trường hợp này thực hiện cả`O(n log n)`việc thực hiện sắp xếp và sự cần thiết của một kiểu số nguyên có khả năng chứa một câu trả lời lớn hơn`2^32`.
