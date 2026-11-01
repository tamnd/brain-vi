---
title: "CF 106403B - Tạo đồng hồ"
description: "Chúng tôi đang thiết kế một chiếc đồng hồ được làm từ nhiều bánh răng độc lập. Một bánh răng có x răng hoàn thành một chu kỳ đầy đủ cứ sau x giây vì nó di chuyển một răng trong mỗi giây."
date: "2026-06-25T10:07:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106403
codeforces_index: "B"
codeforces_contest_name: "Bay Area Programming Contest 2026 Novice Division"
rating: 0
weight: 106403
solve_time_s: 39
verified: true
draft: false
---

[CF 106403B - Tạo đồng hồ](https://codeforces.com/problemset/problem/106403/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang thiết kế một chiếc đồng hồ được làm từ nhiều bánh răng độc lập. Một thiết bị với`x`răng hoàn thành một chu kỳ đầy đủ mỗi`x`giây vì nó di chuyển một chiếc răng mỗi giây. Tất cả các bánh răng bắt đầu thẳng hàng tại thời điểm 0 và chuyển động cùng nhau, vì vậy thời điểm đầu tiên khi mọi bánh răng quay trở lại vị trí ban đầu là bội số chung nhỏ nhất của tất cả các kích cỡ bánh răng. 

Nhiệm vụ là chọn kích thước bánh răng sao cho sự quay trở lại đồng thời đầu tiên này xảy ra chính xác sau`k`giây. Trong số tất cả các lựa chọn có thể, chúng ta cần kết cấu có tổng số răng nhỏ nhất. 

Đầu vào chứa một số giá trị của`k`. Đối với mỗi cái, đầu ra phải mô tả một bộ kích thước bánh răng tối ưu. Số lượng bánh răng và số răng của mỗi bánh răng phải được in. 

Giá trị của`k`nhiều nhất là`100000`, và có thể có tới`10000`trường hợp thử nghiệm. Điều này có nghĩa là chúng tôi không đủ khả năng cho các thuật toán thử nhiều tổ hợp bánh răng. Việc tìm kiếm trên các tập hợp con hoặc phân vùng có thể có của các yếu tố sẽ phát triển quá nhanh. Chúng ta cần sử dụng cấu trúc toán học của bội chung nhỏ nhất và phân tích nhân tử. 

Các trường hợp cạnh chính xuất phát từ thực tế là bội số chung nhỏ nhất chỉ quan tâm đến lũy thừa lớn nhất của mỗi số nguyên tố xuất hiện. 

Ví dụ: nếu đầu vào là:```
1
16
```đầu ra đúng là:```
1
16
```Một giải pháp bất cẩn có thể tạo ra`2 8`, bởi vì`lcm(2, 8) = 8`, điều đó là không đủ. Toàn bộ sức mạnh cơ bản`2^4`phải xuất hiện ở đâu đó. 

Một trường hợp khác là:```
1
12
```Cách xây dựng đúng là:```
2
4 3
```sử dụng`2 2 3`cũng sẽ làm cho bội số chung nhỏ nhất bằng`12`, nhưng tổng số răng lớn hơn:`7`thay vì`7`trong trường hợp cụ thể này, nó có liên quan, trong khi đối với các giá trị khác, các yếu tố lặp lại có thể tệ hơn. Việc xây dựng tối ưu phải tránh sự trùng lặp không cần thiết và giữ chính xác các công suất cần thiết. 

Trường hợp biên cuối cùng là số nguyên tố:```
1
97
```Việc xây dựng hữu ích duy nhất là:```
1
97
```Việc chia một số nguyên tố thành các bánh răng nhỏ hơn là không thể vì không có yếu tố nào nhỏ hơn có thể tạo ra số nguyên tố trong LCM. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử các bộ sưu tập bánh răng khác nhau và kiểm tra xem bội số chung nhỏ nhất của chúng có phải là`k`. Điều này đúng vì cấu trúc hợp lệ chính xác là một bộ sưu tập có LCM khớp với thời gian trả về được yêu cầu. Vấn đề là số lượng bộ sưu tập có thể. Thậm chí hạn chế bản thân chúng ta ở các ước số của`k`, số lượng kết hợp tăng theo cấp số nhân với số lượng yếu tố, do đó phương pháp này trở nên không thể sử dụng được. 

Quan sát quan trọng là mọi số nguyên đều có thể được viết dưới dạng tích của lũy thừa nguyên tố. Giả định:`k = p1^a1 * p2^a2 * ... * pm^am`Để LCM của bánh răng chứa`pi^ai`, ít nhất một thiết bị phải chứa công suất cao nhất chính xác của`pi`. Nếu mỗi bánh răng có công suất nhỏ hơn`pi`, LCM sẽ bỏ lỡ yếu tố đó. Mặt khác, việc thêm một thiết bị chứa nhiều lũy thừa chỉ có thể làm tăng tổng số tiền so với việc giữ riêng các lũy thừa đó. 

Ví dụ, thay vì sử dụng một bánh răng duy nhất`12`, sử dụng bánh răng`4`Và`3`đưa ra cùng một LCM với số tiền nhỏ hơn hoặc bằng nhau. Do đó, cách xây dựng rẻ nhất là tạo ra một bánh răng cho mọi lũy thừa nguyên tố trong hệ số của`k`. 

Cách tiếp cận bạo lực thất bại vì nó bỏ qua sự đóng góp độc lập của các cường quốc. Một khi chúng ta nhận ra rằng mỗi lũy thừa nguyên tố có thể được xử lý riêng biệt thì bài toán sẽ trở thành phép phân tích nhân tử đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng các yếu tố | Phụ thuộc vào độ sâu tìm kiếm | Quá chậm | 
| Tối ưu | O(sqrt(k)) cho mỗi trường hợp thử nghiệm | O(số thừa số nguyên tố) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Yếu tố`k`vào các quyền lực hàng đầu. Bắt đầu với ước số nhỏ nhất có thể và liên tục loại bỏ các thừa số. Bất cứ khi nào tìm được một thừa số nguyên tố, hãy tiếp tục nhân nó cho đến khi rút hết lũy thừa của số nguyên tố đó. 

Lý do chúng tôi giữ toàn bộ lũy thừa là vì LCM cần số mũ tối đa của mọi số nguyên tố, không chỉ riêng số nguyên tố đó. 

1. Lưu trữ mỗi công suất chính được trích xuất dưới dạng một kích cỡ bánh răng. 

Ví dụ,`72 = 2^3 * 3^2`, do đó các giá trị được trích xuất là`8`Và`9`. 

1. Nếu giá trị còn lại sau khi chia thử lớn hơn 1 thì đó là thừa số nguyên tố lớn hơn căn bậc hai của số ban đầu. Thêm nó như là thiết bị của riêng nó. 
2. Xuất ra số lũy thừa nguyên tố được trích và danh sách các cỡ bánh răng. 

Tại sao nó hoạt động: 

Mọi lũy thừa nguyên tố trong phân tích nhân tử của`k`xuất hiện ở ít nhất một bánh răng đã chọn, do đó LCM của tất cả các bánh răng được chọn chứa mọi hệ số được yêu cầu bởi`k`. Vì mọi thiết bị được chọn đều chia`k`, LCM không thể chứa bất cứ thứ gì bổ sung, vì vậy nó chính xác là`k`. 

Để đạt được sự tối ưu, hãy xem xét mọi cách xây dựng hợp lệ. Đối với mỗi số nguyên tố`p`TRONG`k`, một số thiết bị phải chứa đầy đủ công suất cần thiết của`p`, nếu không thì LCM sẽ thiếu nguồn điện đó. Cách rẻ nhất để đáp ứng từng yêu cầu là sử dụng chính xác các công suất chính đó làm các bánh răng riêng biệt. Việc kết hợp hai lũy thừa nguyên tố cần thiết vào một bánh răng sẽ thay thế tổng của chúng bằng tích của chúng và đối với các số nguyên lớn hơn một thì tích không bao giờ nhỏ hơn tổng trong cài đặt này. Do đó, việc xây dựng công suất nguyên tố riêng biệt giúp giảm thiểu tổng số răng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(k):
    ans = []
    x = k
    d = 2
    while d * d <= x:
        if x % d == 0:
            power = 1
            while x % d == 0:
                x //= d
                power *= d
            ans.append(power)
        d += 1 if d == 2 else 2

    if x > 1:
        ans.append(x)

    return ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        k = int(input())
        ans = solve_case(k)
        out.append(str(len(ans)))
        out.append(" ".join(map(str, ans)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Vòng lặp nhân tử thử các ước số cho đến khi bình phương của số chia vượt quá giá trị còn lại. Khi điều đó xảy ra, mọi giá trị còn lại phải là số nguyên tố, vì một số tổng hợp sẽ có hệ số không lớn hơn căn bậc hai của nó. 

Khi tìm thấy số chia, vòng lặp bên trong sẽ loại bỏ tất cả các bản sao của số nguyên tố đó. Biến`power`tích lũy toàn bộ sức mạnh cơ bản, chẳng hạn như biến các yếu tố lặp đi lặp lại của`2`vào trong`8`vì`72`. 

Sự gia tăng bỏ qua ngay cả các ứng cử viên sau khi kiểm tra`2`, giúp tránh những công việc không cần thiết. Từ`k`chỉ là`100000`, phép chia thử đơn giản đã đủ nhanh rồi. 

Đầu ra luôn chứa ít nhất một bánh răng vì`k`ít nhất là`2`. Đầu vào chính rời khỏi đầu vào chính`x`và giá trị cuối cùng đó được xử lý bằng quá trình kiểm tra hệ số còn lại. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
72
```việc thực hiện là: 

| Bước | Giá trị còn lại | Số chia | Thiết bị chiết xuất | Bánh răng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 72 | 2 | 8 | 8 | 
| Sau khi xóa 2 | 9 | 3 | 9 | 8, 9 | 
| Sau khi xóa 3 | 1 | không | không | 8, 9 | 

Đầu ra là:```
2
8 9
```Dấu vết cho thấy thuật toán bảo toàn lũy thừa cao nhất của mọi số nguyên tố, cho`lcm(8,9)=72`. 

Đối với đầu vào:```
1
210
```việc thực hiện là: 

| Bước | Giá trị còn lại | Số chia | Thiết bị chiết xuất | Bánh răng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 210 | 2 | 2 | 2 | 
| Sau khi xóa 2 | 105 | 3 | 3 | 2, 3 | 
| Sau khi xóa 3 | 35 | 5 | 5 | 2, 3, 5 | 
| Sau khi xóa 5 | 7 | 7 | 7 | 2, 3, 5, 7 | 

Đầu ra là:```
4
2 3 5 7
```Điều này chứng tỏ trường hợp mọi thừa số đều khác biệt và không cần đến lũy thừa nguyên tố nào lớn hơn một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(k)) cho mỗi trường hợp thử nghiệm | Phép chia thử chỉ kiểm tra các thừa số có thể có đến căn bậc hai của giá trị còn lại | 
| Không gian | O(số thừa số nguyên tố) | Chỉ có kích thước bánh răng kết quả được lưu trữ | 

Với`k <= 100000`, căn bậc hai nhiều nhất là khoảng`317`, do đó, thậm chí hàng nghìn trường hợp thử nghiệm đều nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(k):
    ans = []
    x = k
    d = 2
    while d * d <= x:
        if x % d == 0:
            power = 1
            while x % d == 0:
                x //= d
                power *= d
            ans.append(power)
        d += 1 if d == 2 else 2
    if x > 1:
        ans.append(x)
    return ans

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    t = data[0]
    res = []
    idx = 1
    for _ in range(t):
        ans = solve_case(data[idx])
        idx += 1
        res.append(str(len(ans)))
        res.append(" ".join(map(str, ans)))
    return "\n".join(res)

assert run("""4
2
12
72
210
""") == """1
2
2
4 3
2
8 9
4
2 3 5 7""", "samples"

assert run("""1
97
""") == """1
97""", "prime input"

assert run("""1
16
""") == """1
16""", "single prime power"

assert run("""1
100000
""") == """2
32 3125""", "large composite with powers"

assert run("""1
36
""") == """2
4 9""", "multiple repeated factors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`| Một bánh răng có kích thước`2`| Ranh giới giá trị tối thiểu | 
|`97`| Một bánh răng có kích thước`97`| Xử lý chính | 
|`16`| Một bánh răng có kích thước`16`| Khai thác năng lượng nguyên tố đầy đủ | 
|`100000`|`32 3125`| Phân tích nhân tử lớn với lũy thừa lặp lại | 
|`36`|`4 9`| Phân chia chính xác các lũy thừa khác nhau | 

## Vỏ cạnh 

cho`k = 16`, thuật toán trích xuất hệ số`2`bốn lần và kết hợp nó thành một giá trị bánh răng:```
16 = 2^4
```Công trình được sản xuất là:```
1
16
```LCM chính xác là`16`, trong khi việc sử dụng các bánh răng nhỏ hơn sẽ thất bại trừ khi LCM kết hợp của chúng chứa toàn bộ lũy thừa thứ tư của hai. 

Vì`k = 12`, hệ số hóa là:```
12 = 2^2 * 3
```Các đầu ra thuật toán:```
2
4 3
```LCM là`12`. Một công trình chỉ chứa`2`Và`3`sẽ sản xuất`6`, vì vậy giữ toàn bộ sức mạnh`4`là cần thiết. 

Vì`k = 97`, giá trị còn lại sau khi chia thử vẫn là`97`, vì vậy nó được thêm trực tiếp:```
1
97
```Một số nguyên tố không thể được lắp ráp từ các ước số nhỏ hơn, vì vậy bánh răng đơn là cấu trúc tối ưu duy nhất có thể có.
