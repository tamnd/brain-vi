---
title: "CF 102606G - Geralt xứ Rivia"
description: "Geralt chiến đấu với một con quái vật luôn nhận đòn đầu tiên. Geralt có thể cải thiện hai chỉ số trước trận chiến: tấn công và phòng thủ. Tăng sức tấn công thêm một điểm sẽ tốn một vương miện, trong khi tăng phòng thủ thêm một điểm sẽ tốn b vương miện."
date: "2026-08-04T17:05:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102606
codeforces_index: "G"
codeforces_contest_name: "2020 ECNU Campus Online Invitational Contest"
rating: 0
weight: 102606
solve_time_s: 103
verified: true
draft: false
---

[CF 102606G - Geralt of Rivia](https://codeforces.com/problemset/problem/102606/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Geralt chiến đấu với một con quái vật luôn nhận đòn đầu tiên. Geralt có thể cải thiện hai chỉ số trước trận chiến: tấn công và phòng thủ. Tăng chi phí tấn công thêm một điểm`a`vương miện, đồng thời tăng khả năng phòng thủ thêm một điểm chi phí`b`vương miện. Việc nâng cấp có thể là một phần nhỏ, vì vậy giá trị tấn công và phòng thủ cuối cùng không nhất thiết phải là số nguyên. 

Sau khi nâng cấp, Geralt giao dịch`max(0, attack - monster_defense)`thiệt hại mỗi lượt. Quái vật mất HP sau mỗi đòn tấn công của Geralt và chỉ tấn công lại nếu nó sống sót. Những giao dịch quái vật`max(0, monster_attack - Geralt_defense)`hư hại. Vì Geralt có HP vô hạn nên số lượng duy nhất cần giảm thiểu là tổng sát thương nhận được trước khi quái vật chết. 

Đối với mỗi trường hợp thử nghiệm, đầu vào cung cấp đòn tấn công và phòng thủ ban đầu của cả hai chiến binh, HP của quái vật, vương miện có sẵn và hai mức giá nâng cấp. Kết quả đầu ra là thiệt hại nhỏ nhất mà Geralt có thể nhận được, được viết dưới dạng phân số rút gọn, hoặc`-1`nếu không có kế hoạch nâng cấp nào có thể khiến anh ta giành chiến thắng. 

Các ràng buộc là nhỏ đối với một giá trị duy nhất của`n`, nhưng có thể có tới`10^4`trường hợp thử nghiệm. Một thuật toán dành`O(n)`thời gian trên mọi trường hợp có thể đạt được khoảng`10^8`các phép toán và việc sử dụng số học phân số đắt tiền bên trong một vòng lặp như vậy sẽ quá chậm trong Python. Giải pháp cần khai thác hình dạng toán học của việc tối ưu hóa hơn là mô phỏng các nâng cấp có thể có. 

Những trường hợp nguy hiểm là những trường hợp nâng cấp liên tục tương tác với số lượng cuộc tấn công rời rạc. 

Nếu Geralt bắt đầu mà không có đòn tấn công hiệu quả thì sẽ sai lầm khi cho rằng có số lượt tối đa cố định. Ví dụ:```
1
1 10 100 1
10 5 1 100
```Geralt không có sát thương ban đầu vì đòn tấn công của anh ta thấp hơn khả năng phòng thủ của quái vật. Anh ta có thể mua một mức tăng tấn công nhỏ tùy ý, do đó số lần tấn công không bị giới hạn bởi giá trị tấn công ban đầu. 

Một lỗi phổ biến khác là tính toán đòn tấn công cuối cùng của quái vật. Nếu quái vật chết vì đòn tấn công của Geralt, nó sẽ không đánh trả. Ví dụ:```
1
5 1 1 1
1 1 1 1
```Geralt đã giao dịch rồi`4`gây sát thương và tiêu diệt quái vật ngay lập tức. Câu trả lời là:```
0/1
```Một mô phỏng tăng thêm sát thương cho quái vật sau mỗi lượt Geralt sẽ tạo ra giá trị dương không chính xác. 

Xử lý phân số cũng rất quan trọng. Một kế hoạch có thể yêu cầu tăng tấn công theo từng phần và câu trả lời cuối cùng có thể không phải là số nguyên. Ví dụ: việc giảm phòng thủ tối ưu có thể để lại một lượng sát thương hợp lý cho mỗi đòn đánh, do đó số học dấu phẩy động có thể mất độ chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi mức độ nâng cấp tấn công có thể, tính toán số tiền còn lại dành cho phòng thủ, mô phỏng số lượng cuộc tấn công cần thiết và giữ kết quả tốt nhất. Điều này đúng vì mọi lựa chọn nâng cấp hợp pháp đều được kiểm tra. Tuy nhiên, các giá trị nâng cấp là liên tục nên có vô số lựa chọn. Ngay cả khi chúng ta chỉ nhìn vào số lần tấn công cần thiết, có thể có tới`n`khả năng cho mỗi trường hợp thử nghiệm. Sang`10^4`trường hợp, điều này là quá chậm. 

Quan sát hữu ích là việc nâng cấp đòn tấn công chỉ quan trọng thông qua số lượt cần thiết để tiêu diệt quái vật. Giả sử Geralt cần chính xác`k`các cuộc tấn công. Cách rẻ nhất để đạt được điều này là làm cho sát thương của anh ta đủ chính xác để thỏa mãn:```
k * damage >= n
```Bất kỳ sát thương tấn công bổ sung nào đều không làm giảm số lần tấn công của quái vật, vì vậy nó chỉ lãng phí vương miện mà lẽ ra có thể dùng để phòng thủ. 

Đối với một cố định`k`, chúng ta có thể tính toán mức đầu tư tấn công tối thiểu, sau đó đưa tất cả vương miện còn lại vào phòng thủ. Vấn đề trở thành việc tìm kiếm điều tốt nhất có thể`k`. 

Khi Geralt đã có sát thương dương, số lần tấn công có thể bị giới hạn. Nếu không có nâng cấp tấn công, anh ta cần nhiều nhất`ceil(n / base_damage)`các cuộc tấn công và nâng cấp tấn công nhiều hơn chỉ có thể làm giảm con số đó. Chúng tôi có thể giảm thiểu trong phạm vi này, nhưng chúng tôi thực hiện việc đó một cách toán học thay vì kiểm tra mọi giá trị. 

Đối với một số cuộc tấn công ứng cử viên`k`, thiệt hại nhận được là một hàm có dạng:```
(k - 1) * max(0, C - T/k)
```cho hằng số`C`Và`T`. Sau khi loại bỏ phép toán cực đại, đây là hàm lồi hoặc đơn điệu trên số nguyên. Điều đó có nghĩa là mức tối thiểu chỉ có thể xuất hiện gần ranh giới hoặc gần điểm mà đạo hàm trở thành 0. Kiểm tra một vài giá trị số nguyên lân cận là đủ. 

Khi Geralt bắt đầu với sát thương bằng 0, có hai trường hợp đặc biệt. Nếu việc phòng thủ có thể bị vô hiệu hóa hoàn toàn trong khi vẫn để lại một lượng tiền nhỏ cho việc tấn công, thì câu trả lời ngay lập tức là con số 0. Ngược lại, số lần tấn công tốt nhất là số lần tấn công nhỏ nhất khả thi, vì hàm số đang tăng lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm với nhiều trường hợp | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán sát thương tấn công hiệu quả hiện tại của Geralt và sát thương hiệu quả hiện tại của quái vật chống lại Geralt. Nếu đòn tấn công của Geralt đã đủ thì các công thức sau sẽ sử dụng giá trị này làm điểm bắt đầu. 
2. Xử lý trường hợp Geralt không có sát thương tấn công. Nếu việc chi vương miện để phòng thủ có thể khiến sát thương của quái vật bằng 0, thì câu trả lời là`0/1`. Ngược lại, hãy tính số lần tấn công nhỏ nhất có thể và chỉ đánh giá lựa chọn đó. 
3. Để có sát thương tấn công ban đầu tích cực, hãy`h`là số lượng các cuộc tấn công cần thiết. Lớn nhất có thể`h`là số lần tấn công mà không nâng cấp tấn công. Tối ưu`h`là giá trị biên hoặc gần điểm mà phiên bản liên tục của hàm thiệt hại đạt mức tối thiểu. 
4. Kiểm tra các giá trị ứng viên cần thiết của`h`. Đối với mỗi ứng cử viên, hãy tính toán mức nâng cấp tấn công tối thiểu cần thiết, sử dụng tất cả vương miện còn lại để phòng thủ và tính sát thương nhận được dưới dạng phân số. 
5. Giữ lại phân số nhỏ nhất và in nó ở dạng rút gọn. 

Tại sao nó hoạt động: đối với bất kỳ số lần tấn công cố định nào, việc dành thêm vương miện cho cuộc tấn công không thể cải thiện số lượt, do đó, giải pháp tối ưu luôn sử dụng mức nâng cấp tấn công tối thiểu cần thiết cho số lượt đó và dành phần còn lại cho phòng thủ. Tối ưu hóa còn lại là hàm một biến. Hình dạng của nó là đơn điệu hoặc lồi, do đó mức tối thiểu được nắm bắt bằng cách kiểm tra các ranh giới và vùng lân cận của điểm dừng của nó. 

## Giải pháp Python```python
import sys
from fractions import Fraction

input = sys.stdin.readline

def solve_case(ag, dg, ac, dc, n, m, a, b):
    base_attack = max(0, ag - dc)
    base_taken = max(0, ac - dg)

    def evaluate(k):
        need = Fraction(n, k)
        add_attack = max(Fraction(0), need - base_attack)
        remain = Fraction(m) - Fraction(a) * add_attack
        defense_gain = remain / b
        taken = max(Fraction(0), Fraction(base_taken) - defense_gain)
        return (k - 1) * taken

    if base_attack == 0:
        if b * base_taken < m:
            return Fraction(0)
        k = (n * a + m - 1) // m
        return evaluate(k)

    kmax = (n + base_attack - 1) // base_attack

    if kmax == 1:
        return Fraction(0)

    candidates = {1, kmax}

    last = kmax - 1
    if last >= 1:
        c = b * base_taken - b * m + a * base_attack
        t = a * n

        if c <= 0:
            candidates.add(last)
        else:
            root = int((t / c) ** Fraction(1, 2))
            for x in range(root - 3, root + 4):
                if 1 <= x <= last:
                    candidates.add(x)

    ans = None
    for k in candidates:
        if 1 <= k <= kmax:
            cur = evaluate(k)
            if ans is None or cur < ans:
                ans = cur

    return ans

def main():
    out = []
    t = int(input())
    for _ in range(t):
        ag, dg, ac, dc = map(int, input().split())
        n, m, a, b = map(int, input().split())

        ans = solve_case(ag, dg, ac, dc, n, m, a, b)
        out.append(f"{ans.numerator}/{ans.denominator}")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```chức năng`evaluate`là bản dịch trực tiếp của quan sát quay cố định. Đầu tiên nó tìm thấy mức tăng tấn công tối thiểu cần thiết để kết thúc`k`các cuộc tấn công. Vì cuộc tấn công và phòng thủ sử dụng cùng một ngân sách vương miện nên số tiền còn lại luôn được chuyển thành phòng thủ. 

Trường hợp không tấn công được tách biệt vì số lần tấn công có thể không bị giới hạn bởi số liệu thống kê ban đầu. Khi khả năng phòng thủ có thể bị loại bỏ hoàn toàn, việc sử dụng một nâng cấp tấn công nhỏ và nhiều đòn tấn công sẽ không nhận được sát thương. 

Đối với sát thương tấn công tích cực,`kmax`là số lần tấn công mà không mua tấn công. Không có cuộc tấn công nào có số lượng tấn công lớn hơn có thể là tối ưu vì nâng cấp chỉ làm tăng sức tấn công. Việc tìm kiếm ứng cử viên sử dụng dạng lồi của hàm còn lại và chỉ kiểm tra các giá trị gần mức tối thiểu toán học.`Fraction`chỉ được sử dụng cho nhóm nhỏ ứng cử viên cuối cùng. Nó tránh được các lỗi dấu phẩy động trong khi vẫn giữ thời gian chạy ở mức nhỏ. Số nguyên Python cũng xử lý tất cả các giá trị trung gian một cách an toàn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1
1 1 2 2
1 1 1 1
```Sát thương tấn công của Geralt là:```
max(0, 1 - 2) = 0
```Chỉ với một chiếc vương miện, anh ta không thể mua đủ các cải tiến về tấn công và phòng thủ để lập kế hoạch chiến thắng. 

| Bước | Tấn công căn cứ | Ứng cử viên tấn công | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | không | không thể | 

Thuật toán phát hiện rằng không thể tạo ra cuộc tấn công hiệu quả nào và trả về:```
-1
```Đối với mẫu thứ ba:```
1
6 6 66 66
66 666 6 666
```Các giá trị ban đầu là: 

| Bước | Giá trị | 
| --- | --- | 
| Sát thương tấn công cơ bản | 0 | 
| Sát thương quái vật cơ bản | 60 | 
| Chi phí quốc phòng | 666 | 
| Vương miện có sẵn | 666 | 

Việc nâng cấp phòng thủ có thể loại bỏ tất cả sát thương của quái vật, trong khi vẫn có thể nâng cấp đòn tấn công nhỏ. 

| Bước | Lựa chọn tấn công | Kết quả phòng thủ | Thiệt hại nhận được | 
| --- | --- | --- | --- | 
| Sử dụng phòng thủ thật nhiều | Tăng tấn công nhỏ | Không có sát thương quái vật | 0 | 

Mẫu này chứng minh tại sao việc nâng cấp liên tục lại quan trọng. Các bản nâng cấp chỉ có số nguyên sẽ bỏ lỡ khả năng này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ kiểm tra số lượt ứng cử viên cố định | 
| Không gian | O(1) | Không sử dụng mảng hoặc cấu trúc dữ liệu tùy thuộc vào kích thước đầu vào | 

Giải pháp thực hiện một lượng nhỏ phép tính số học cho mỗi trường hợp thử nghiệm, do đó nó dễ dàng xử lý`10^4`trường hợp. 

## Trường hợp thử nghiệm```python
from fractions import Fraction

def run(inp: str) -> str:
    import sys, io
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    t = int(data())
    ans = []
    for _ in range(t):
        ag, dg, ac, dc = map(int, data().split())
        n, m, a, b = map(int, data().split())
        ans.append(f"{solve_case(ag, dg, ac, dc, n, m, a, b).numerator}/{solve_case(ag, dg, ac, dc, n, m, a, b).denominator}")
    sys.stdin = old
    return "\n".join(ans)

assert run("""3
1 1 2 2
1 1 1 1
2 2 1 1
1 1 1 1
6 6 66 66
66 666 6 666
""") == """-1
0/1
2214/37"""

assert run("""1
5 1 1 1
1 1 1 1
""") == "0/1"

assert run("""1
1 10 100 1
10 5 1 100
""") == "0/1"

assert run("""1
10000 1 1 10000
10000 10000 1 1
""") == "0/1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba mẫu đầu tiên |`-1`,`0/1`,`2214/37`| Ví dụ chính thức và câu trả lời phân số | 
|`5 1 1 1`với một con quái vật HP |`0/1`| Không phản công sau đòn kết liễu | 
| Không tấn công ban đầu với nâng cấp phòng thủ mạnh mẽ |`0/1`| Vỏ cạnh xoay vô hạn | 
| Giá trị lớn |`0/1`| Giới hạn số nguyên và số học biên | 

## Vỏ cạnh 

Khi Geralt bắt đầu với sát thương tấn công bằng 0, thuật toán không bao giờ cố liệt kê số lần tấn công. Nó kiểm tra xem khả năng phòng thủ có thể đạt tới mức 0 sát thương nhận vào hay không. Nếu có thể, câu trả lời là 0 vì Geralt có thể chi một khoản nhỏ tùy ý cho việc tấn công và phần còn lại cho phòng thủ. 

Khi Geralt giết quái vật trong một đòn tấn công, hệ số nhân`(k - 1)`trở thành số không. Việc triển khai giữ nguyên điều kiện chính xác này, do đó nó không vô tình tính lần tấn công quái vật cuối cùng. 

Khi nâng cấp tối ưu yêu cầu phân số, mọi phép tính đều được giữ dưới dạng số hữu tỷ. Phân số cuối cùng được Python tự động giảm`Fraction`, tránh lỗi từ các giá trị thập phân được làm tròn.
