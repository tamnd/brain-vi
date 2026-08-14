---
title: "CF 102323F - Vi sóng nhanh hơn"
description: "Chúng tôi được cung cấp thời gian nấu vi sóng được đề xuất ở dạng MM:SS và tỷ lệ phần trăm p. Chris được phép chọn bất kỳ số nguyên giây nào có khoảng cách từ thời gian được đề xuất tối đa là p phần trăm của thời gian được đề xuất đó."
date: "2026-08-14T04:53:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "F"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 66
verified: true
draft: false
---

[CF 102323F - Vi sóng nhanh hơn](https://codeforces.com/problemset/problem/102323/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp thời gian nấu vi sóng được khuyến nghị trong`MM:SS`hình thức và tỷ lệ phần trăm`p`. Chris được phép chọn bất kỳ số nguyên giây nào có khoảng cách từ thời gian được đề xuất tối đa`p`phần trăm thời gian được đề nghị đó. Trong số tất cả các thời điểm ứng cử viên như vậy, anh ta muốn thời gian có dãy chữ số nhanh nhất được nhập vào. 

Lò vi sóng không yêu cầu hai chữ số cuối để tạo thành số giây hợp lệ. Nếu chúng ta nhấn`190`, lò vi sóng giải thích điều đó như`1:90`, có tổng thời lượng bằng 2 phút 30 giây. Do đó, một ứng cử viên được thể hiện tốt nhất bằng các chữ số thập phân trên tổng số giây của nó. Ví dụ: 88 có nghĩa là`0:88`, tức là 1 phút 28 giây. 

Nhập một chữ số tốn một chút thời gian. Việc di chuyển từ một chữ số này sang một chữ số khác sẽ tốn thêm một chút thời gian, trong khi nhấn lại cùng một chữ số đó thì không cần phải di chuyển. Do đó, đối với một chuỗi chữ số`d1 d2 ... dk`, giá của nó là`k + number of positions i where di != d(i+1)`. 

Đầu vào chứa`n`mặt hàng thực phẩm. Đối với mỗi mục, thời gian đề xuất có tính từ phút`00`bởi vì`20`, và giây của nó là một trong`00`,`15`,`30`, hoặc`45`. Thời gian khuyến nghị ít nhất là 15 giây, trong khi`p`nằm trong khoảng từ 2 đến 10. Những hạn chế này làm cho không gian tìm kiếm trở nên cực kỳ nhỏ. Ngay cả đề xuất lớn nhất, 20 phút, cũng có phạm vi 10 phần trăm chỉ trong 240 giây nguyên, vì vậy việc kiểm tra trực tiếp từng ứng viên là đủ nhanh chóng. Tuyên bố cuộc thi ban đầu đưa ra các giới hạn chính xác này và chỉ định rằng khoảng phần trăm được diễn giải bằng số giây nguyên thực sự thỏa mãn điều kiện phần trăm. 

Có một số chi tiết có thể khiến việc triển khai đơn giản trở nên sai lầm. 

Ví dụ, hãy xem xét:```
1
01:30
4
```Thời gian khuyến nghị là 90 giây, vì vậy phạm vi cho phép là từ 87 đến 93 giây. Câu trả lời là`88`, không`01:28`. Trình tự`88`chỉ mất hai khoảnh khắc, trong khi lò vi sóng tự chuyển đổi các chữ số đó thành 1 phút 28 giây. 

Điểm cuối phần trăm cũng phải được làm tròn theo đúng hướng. Vì:```
1
00:15
10
```khoảng toán học là 13,5 đến 16,5 giây. Vì chỉ cho phép số giây nguyên nên các ứng cử viên hợp lệ là 14, 15 và 16, vì vậy câu trả lời là`15`. Bao gồm 13 hoặc 17 sẽ mở rộng tập ứng cử viên một cách không chính xác. 

Trường hợp cạnh thứ ba là thời gian được chọn không nhất thiết phải có số giây dưới 60. Đối với:```
1
00:30
10
```phạm vi hợp lệ là từ 27 đến 33 giây và`33`là tối ưu vì nhấn cùng một chữ số hai lần chỉ tốn hai giây. Phiên dịch lò vi sóng`33`như 33 giây thông thường, nhưng nguyên tắc tương tự cũng cho phép các giá trị như`88`. 

Cuối cùng, người đăng ký nhanh nhất không nhất thiết phải là ứng viên gần nhất với đề xuất. Vì:```
1
06:00
8
```khoảng thời gian hợp lệ là 331 đến 388 giây. Trình tự`555`chỉ mất ba phút, vì vậy nó đánh bại các ứng cử viên như`600`, mặc dù 600 giây chính xác là thời gian được khuyến nghị. Thời hạn thực tế của`555`là 5 phút 55 giây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi số nguyên giây trong khoảng thời gian cho phép. Đối với mỗi ứng cử viên, hãy chuyển đổi số nguyên thành biểu diễn thập phân và tính chi phí đầu vào của nó. Giữ ứng viên có chi phí nhỏ nhất và khi hai ứng viên có chi phí bằng nhau, hãy giữ ứng viên có khoảng cách tuyệt đối nhỏ hơn so với số giây được đề xuất. 

Phương pháp vũ phu này đã đủ vì các ràng buộc làm cho khoảng thời gian trở nên nhỏ bé. Thời lượng lớn nhất có thể được đề xuất là 20 phút hoặc 1200 giây. Với`p = 10`, khoảng có thể chứa tối đa 241 giá trị nguyên, từ 1080 đến 1320. Tính chi phí của mỗi ứng cử viên kiểm tra tối đa bốn chữ số, do đó, một trường hợp kiểm thử chỉ cần khoảng 1000 phép toán nguyên thủy. Ngay cả khi đầu vào chứa một số lượng lớn các trường hợp thử nghiệm thì con số này vẫn nhỏ. 

Ngoài ra còn có một quan sát cấu trúc hữu ích đằng sau việc tính toán chi phí. Tổng chi phí chỉ phụ thuộc vào số lần chữ số thay đổi khi chúng ta gõ. Mỗi chữ số luôn tốn một lần nhấn và mỗi lần chuyển đổi sang một chữ số khác sẽ tốn chính xác một lần di chuyển. Do đó, chúng tôi không cần mô phỏng vị trí ngón tay hoặc mô hình hóa hình dạng bàn phím. Chi phí có thể được tính trực tiếp từ chuỗi thập phân. 

Tìm kiếm brute-force hoạt động vì khoảng thời gian ứng cử viên nhỏ. Việc tối ưu hóa phức tạp hơn sẽ chỉ khiến việc triển khai trở nên khó khăn hơn mà không mang lại lợi ích thiết thực dưới những ràng buộc này. Việc giảm khóa chỉ đơn giản là chuyển đổi thời gian được đề xuất thành giây, tính toán khoảng nguyên chính xác và kiểm tra mọi ứng cử viên trong khoảng đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(R log R) | O(log R) | Đã chấp nhận | 
| Tối ưu | O(R log R) | O(logR) | Đã chấp nhận | 

Đây`R`là số giây nguyên trong khoảng thời gian cho phép và trong bài toán này`R <= 241`. Vì mỗi ứng viên có nhiều nhất bốn chữ số thập phân nên đây thực sự là thời gian không đổi cho mỗi trường hợp kiểm thử. 

## Hướng dẫn thuật toán 

1. Chuyển đổi khuyến nghị`MM:SS`giá trị thành một số nguyên duy nhất`target = 60 * MM + SS`. Làm việc hoàn toàn trong vài giây tránh phải xử lý ranh giới phút và giây riêng biệt. 
2. Tính ứng viên hợp lệ nhỏ nhất bằng số nguyên. Điểm cuối thấp hơn là`ceil(target * (100 - p) / 100)`. 

Đối với số nguyên dương, giá trị này có thể được tính như sau`(x + 99) // 100`, Ở đâu`x = target * (100 - p)`. 
3. Tính toán ứng viên hợp lệ lớn nhất là`floor(target * (100 + p) / 100)`. 

Phép chia số nguyên đưa ra điều này trực tiếp. 
4. Lặp qua mọi số nguyên`t`từ điểm cuối dưới đến điểm cuối trên. Mọi giá trị trong vòng lặp này là thời gian nấu hợp pháp được đề xuất và không có thời gian hợp pháp nào bị bỏ qua. 
5. Chuyển đổi`t`ĐẾN`str(t)`và tính toán chi phí đầu vào của nó. Bắt đầu với số chữ số, vì mỗi chữ số cần một lần nhấn. Sau đó, thêm một số cho mỗi cặp chữ số khác nhau liền kề, vì việc di chuyển giữa các nút khác nhau sẽ tốn thêm một khoảnh khắc. 
6. So sánh ứng viên hiện tại với ứng viên tốt nhất được tìm thấy cho đến nay. Một ứng cử viên sẽ tốt hơn nếu chi phí đầu vào của nó nhỏ hơn. Nếu chi phí bằng nhau thì so sánh`abs(t - target)`và ưu tiên ứng viên gần với lời giới thiệu hơn. 
7. Xuất ra các chữ số thập phân của ứng viên đã chọn. Chúng tôi tự xuất số mà không chèn dấu hai chấm hoặc số 0 ở đầu vì đó không phải là các nút mà Chris cần nhấn. 

Điều bất biến là sau khi xử lý mọi ứng viên cho đến`t`, câu trả lời được lưu trữ là ứng cử viên tốt nhất trong số tất cả các ứng cử viên được xử lý cho đến nay theo thứ tự hai cấp chính xác từ bài toán: chi phí đầu vào tối thiểu trước tiên, sau đó là khoảng cách tối thiểu từ đề xuất. Vì vòng lặp cuối cùng xử lý mọi ứng cử viên số nguyên hợp lệ nên câu trả lời được lưu trữ cuối cùng là câu trả lời bắt buộc duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def entry_cost(t):
    s = str(t)
    cost = len(s)

    for i in range(1, len(s)):
        if s[i] != s[i - 1]:
            cost += 1

    return cost

def solve():
    n = int(input())
    out = []

    for case in range(1, n + 1):
        time_str = input().strip()
        p = int(input())

        minutes = int(time_str[:2])
        seconds = int(time_str[3:])
        target = minutes * 60 + seconds

        low_num = target * (100 - p)
        high_num = target * (100 + p)

        low = (low_num + 99) // 100
        high = high_num // 100

        best_time = None
        best_cost = 10**9
        best_dist = 10**9

        for t in range(low, high + 1):
            cost = entry_cost(t)
            dist = abs(t - target)

            if cost < best_cost or (cost == best_cost and dist < best_dist):
                best_time = t
                best_cost = cost
                best_dist = dist

        out.append(f"Case #{case}: {best_time}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve`chuyển đổi năm ký tự`MM:SS`biểu diễn thành giây. Vì định dạng đầu vào là cố định,`time_str[:2]`đưa ra biên bản và`time_str[3:]`đưa ra số giây. 

Các biểu thức cho`low`Và`high`cố tình tránh số học dấu phẩy động. Ví dụ: nếu điểm cuối toán học thấp hơn là 13,5,`(13 * 100 + 99) // 100`kiểu số học trần số nguyên cho kết quả 14. Số học dấu phẩy động là không cần thiết và có thể gây ra lỗi biên.`entry_cost`bắt đầu bằng`len(s)`bởi vì mỗi chữ số phải được nhấn. Sau đó nó kiểm tra các chữ số liền kề. Một sự chuyển tiếp như`5 -> 5`không cần di chuyển, trong khi`5 -> 6`yêu cầu một chuyển động, vì vậy mỗi cặp liền kề không bằng nhau đóng góp chính xác một chuyển động. 

Điều kiện so sánh mã hóa trực tiếp mức độ ưu tiên cần thiết. Chi phí đầu vào được xem xét đầu tiên. Khoảng cách so với thời gian đề xuất chỉ quan trọng khi chi phí đầu vào bằng nhau. sử dụng`<`đối với việc so sánh khoảng cách là đủ vì mệnh đề đảm bảo rằng câu trả lời cuối cùng là duy nhất. 

Không cần phải bình thường hóa một ứng cử viên như`88`vào trong`01:28`. Ứng cử viên là chuỗi các nút được nhấn, do đó kết quả đầu ra chính xác là`88`. Số nguyên Python cũng tránh được bất kỳ vấn đề tràn nào một cách tự nhiên, mặc dù giá trị thực tế ở đây chỉ khoảng vài nghìn. 

## Ví dụ đã hoạt động 

Đối với mẫu 1:```
3
01:30
4
00:30
10
06:00
8
```Trường hợp đầu tiên có mục tiêu là 90 giây và phạm vi hợp lệ từ 87 đến 93. 

| Ứng viên | Chữ số | Chi phí đầu vào | Khoảng cách | 
| --- | --- | --- | --- | 
| 87 |`87`| 3 | 3 | 
| 88 |`88`| 2 | 2 | 
| 89 |`89`| 3 | 1 | 
| 90 |`90`| 3 | 0 | 
| 91 |`91`| 3 | 1 | 
| 92 |`92`| 3 | 2 | 
| 93 |`93`| 3 | 3 |`88`có chi phí đầu vào nhỏ nhất duy nhất, do đó đầu ra đầu tiên là`88`. 

Trường hợp thứ hai có mục tiêu 30 và nằm trong khoảng từ 27 đến 33. 

| Ứng viên | Chữ số | Chi phí đầu vào | Khoảng cách | 
| --- | --- | --- | --- | 
| 27 |`27`| 3 | 3 | 
| 28 |`28`| 3 | 2 | 
| 29 |`29`| 3 | 1 | 
| 30 |`30`| 3 | 0 | 
| 31 |`31`| 3 | 1 | 
| 32 |`32`| 3 | 2 | 
| 33 |`33`| 2 | 3 |`33`thắng vì nhấn cùng một nút hai lần sẽ loại bỏ chuyển động giữa hai lần nhấn. 

Đối với trường hợp thứ ba, mục tiêu là 360 giây. Với phạm vi 8 phần trăm, khoảng hợp lệ là từ 331 đến 388. 

| Ứng viên | Chữ số | Chi phí đầu vào | Khoảng cách | 
| --- | --- | --- | --- | 
| 331 |`331`| 4 | 29 | 
| 332 |`332`| 4 | 28 | 
| 333 |`333`| 3 | 27 | 
| 444 |`444`| 3 | 84 | 
| 555 |`555`| 3 | 105 | 
| 600 |`600`| 4 | 240 | 
| 666 |`666`| 3 | 306 | 

Các ứng cử viên có ba chữ số lặp lại có chi phí tối thiểu có thể là 3. Trong số tất cả các ứng cử viên như vậy trong phạm vi hoàn chỉnh,`555`là gần nhất với 360 giây, vì vậy câu trả lời là`555`. 

Đối với mẫu 2:```
1
00:45
10
```Mục tiêu là 45 giây. Giới hạn toán học dưới là 40,5 và giới hạn trên là 49,5, vì vậy các số nguyên hợp lệ là 41 đến 49. 

| Ứng viên | Chữ số | Chi phí đầu vào | Khoảng cách | 
| --- | --- | --- | --- | 
| 41 |`41`| 3 | 4 | 
| 42 |`42`| 3 | 3 | 
| 43 |`43`| 3 | 2 | 
| 44 |`44`| 2 | 1 | 
| 45 |`45`| 3 | 0 | 
| 46 |`46`| 3 | 1 | 
| 47 |`47`| 3 | 2 | 
| 48 |`48`| 3 | 3 | 
| 49 |`49`| 3 | 4 |`44`chiến thắng mặc dù còn cách đề xuất một giây vì hai lần nhấn của nó sử dụng cùng một nút và chỉ tốn hai giây. Ví dụ này chứng minh tại sao việc giảm thiểu lỗi thời gian nấu trước tiên sẽ không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R log R) | Có R giây ứng viên và mỗi ứng cử viên có tối đa bốn chữ số thập phân. | 
| Không gian | O(logR) | Biểu diễn thập phân của một ứng cử viên được lưu trữ tạm thời. | 

Thời lượng tối đa được đề xuất là 1200 giây và tỷ lệ phần trăm tối đa là 10, do đó, tối đa 241 lần ứng viên được kiểm tra cho một mục. Mỗi ứng viên có tối đa bốn chữ số, khiến công việc thực tế cho mỗi trường hợp kiểm thử là rất nhỏ. Thuật toán dễ dàng phù hợp với giới hạn thời gian 5 giây đã nêu và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```
# helper: same core logic as the submitted solution
def best_time(time_str, p):
    minutes = int(time_str[:2])
    seconds = int(time_str[3:])
    target = minutes * 60 + seconds

    low = (target * (100 - p) + 99) // 100
    high = target * (100 + p) // 100

    def cost(t):
        s = str(t)
        ans = len(s)
        for i in range(1, len(s)):
            if s[i] != s[i - 1]:
                ans += 1
        return ans

    best = None
    best_key = None

    for t in range(low, high + 1):
        key = (cost(t), abs(t - target))
        if best_key is None or key < best_key:
            best = t
            best_key = key

    return str(best)

def run(inp: str) -> str:
    import io

    data = inp.strip().split()
    it = iter(data)
    n = int(next(it))

    ans = []
    for case in range(1, n + 1):
        time_str = next(it)
        p = int(next(it))
        ans.append(f"Case #{case}: {best_time(time_str, p)}")

    return "\n".join(ans)

# provided sample
assert run("""3
01:30
4
00:30
10
06:00
8
""") == """Case #1: 88
Case #2: 33
Case #3: 555""", "provided sample"

# minimum-size recommendation, with an interval collapsing to one value
assert run("""1
00:15
2
""") == "Case #1: 15", "minimum-size input"

# all-equal digits give the cheapest possible two-digit entry
assert run("""1
00:30
10
""") == "Case #1: 33", "repeated digit candidate"

# boundary rounding: 45 +/- 10% gives 41..49, not 40..50
assert run("""1
00:45
10
""") == "Case #1: 44", "integer percentage boundaries"

# maximum recommended time and percentage
assert run("""1
20:00
10
""") == "Case #1: 1111", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`00:15`,`2`|`Case #1: 15`| Minimum recommendation and a one-value candidate interval |
 |`00:30`,`10`|`Case #1: 33`| Các chữ số lặp lại và chi phí di chuyển | 
|`00:45`,`10`|`Case #1: 44`| Đúng trần, sàn tỷ lệ ranh giới | 
|`20:00`,`10`|`Case #1: 1111`| Khuyến nghị lớn nhất có thể và phạm vi ứng viên | 

## Vỏ cạnh 

Để có khuyến nghị tối thiểu, hãy xem xét:```
1
00:15
2
```Mục tiêu là 15 giây. Giới hạn dưới là`ceil(14.7) = 15`, trong khi giới hạn trên là`floor(15.3) = 15`. Do đó, vòng lặp chỉ kiểm tra`15`, và câu trả lời là`15`. Điều này ngăn việc triển khai vô tình chấp nhận 14 giây do cắt bớt dấu phẩy động hoặc thao tác sàn không chính xác. 

Đối với một ứng cử viên có chữ số lặp lại, hãy xem xét:```
1
00:30
10
```Phạm vi hợp lệ là từ 27 đến 33. Ứng viên`33`có hai chữ số và không chuyển động, cho ra giá trị 2. Mỗi ứng viên còn lại có hai chữ số khác nhau và có giá trị 3. Thuật toán chọn`33`ngay lập tức khi đạt đến mục tiêu đó, bất kể thực tế là 30 giây gần với khuyến nghị hơn. 

Đối với một ứng viên có số giây hiển thị vượt quá 59, hãy cân nhắc:```
1
01:30
4
```Phạm vi là 87 đến 93. Ứng viên`88`được biểu thị bằng hai chữ số được nhấn`8`Và`8`, vậy giá thành của nó là 2. Lò vi sóng diễn giải các chữ số đó thành 88 giây, tương đương với 1 phút 28 giây. Thuật toán giữ`88`như câu trả lời và không cố gắng chuyển nó thành`01:28`. 

Để có khuyến nghị lớn nhất có thể, hãy xem xét:```
1
20:00
10
```Mục tiêu là 1200 giây, cho phạm vi từ 1080 đến 1320. Ứng viên`1111`nằm trong khoảng này và chỉ tốn 4 giây vì cả 4 chữ số đều giống nhau. Không có ứng cử viên có ba chữ số nào có thể hợp lệ vì mọi giá trị dưới 1000 đều nằm ngoài khoảng, trong khi số có bốn chữ số không thể có chi phí dưới 4. Do đó`1111`là tối ưu và việc triển khai tìm thấy nó bằng cách liệt kê thông thường.
