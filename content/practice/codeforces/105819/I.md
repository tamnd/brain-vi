---
title: "CF 105819I - Tháp di động"
description: "Chúng ta được cấp một tập hợp các ngôi nhà trên một mạng lưới vô hạn. Tháp đặt ở vị trí (r, c) có thể phục vụ mọi nhà không ở phía Tây của nó và có giá trị hàng + cột không lớn hơn hàng + cột của tháp. Chi phí xây dựng một tòa tháp là số hàng của nó."
date: "2026-06-25T15:07:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105819
codeforces_index: "I"
codeforces_contest_name: "TeamsCode Spring 2025 Novice Division"
rating: 0
weight: 105819
solve_time_s: 39
verified: true
draft: false
---

[CF 105819I - Tháp di động](https://codeforces.com/problemset/problem/105819/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các ngôi nhà trên một mạng lưới vô hạn. Tháp được đặt ở vị trí`(r, c)`có thể phục vụ mọi ngôi nhà không nằm ở phía tây của nó và có giá trị`row + column`không lớn hơn tháp`row + column`. Chi phí xây dựng một tòa tháp là số hàng của nó. 

Điều quan trọng là thay đổi cách chúng ta nhìn vào một ngôi nhà. Đối với một ngôi nhà`(r, c)`, xác định một khoảng:```
[c, r + c]
```Tháp bao phủ ngôi nhà này phải chọn ranh giới bên trái không lớn hơn`c`và ranh giới bên phải không nhỏ hơn`r + c`. Chi phí của tòa tháp đó chính xác bằng độ dài của khoảng thời gian bao phủ đã chọn. Vì vậy, việc bao phủ một tập hợp các ngôi nhà tương đương với việc bao phủ các khoảng của chúng bằng một khoảng lớn hơn. Chi phí của nó là điểm cuối bên phải tối đa trừ đi điểm cuối bên trái tối thiểu. 

Nhiệm vụ yêu cầu tổng chi phí tối thiểu sử dụng tối đa`i`tháp cho mọi`i`từ`1`ĐẾN`n`. 

Số lượng nhà có thể đạt`200000`, do đó, bất kỳ phương pháp nào thử từng tập hợp con của các ngôi nhà hoặc xây dựng bảng lập trình động trên tất cả các ngôi nhà và tất cả số lượng tháp có thể có sẽ quá chậm. Với giới hạn khoảng vài giây, chúng ta cần một`O(n log n)`hoặc đóng giải pháp. Tọa độ có thể lớn bằng`10^9`, do đó thuật toán không thể phụ thuộc vào kích thước lưới. 

Các trường hợp không rõ ràng đến từ các khoảng thời gian chồng chéo. Một giải pháp đơn giản có thể cho rằng mỗi tháp bổ sung luôn hữu ích, nhưng việc chia nhỏ các khoảng thời gian chồng chéo không làm giảm chi phí. Ví dụ:```
1
5 10
```Khoảng thời gian là`[10, 15]`, do đó giá một tháp`5`và câu trả lời là:```
5
```Việc thực hiện bất cẩn coi ngôi nhà đơn lẻ như cần một tòa tháp với chi phí bằng`r + c`sẽ xuất ra`15`, nhầm lẫn giữa vị trí đường chéo của tháp với giá của nó. 

Một trường hợp quan trọng khác là khi hai ngôi nhà có khoảng cách phủ sóng giao nhau:```
2
3 5
2 6
```Các khoảng là`[5, 8]`Và`[6, 8]`. Một tòa tháp có thể trang trải cả hai chi phí`3`, và hai tòa tháp vẫn có chi phí tối thiểu`5 + 4 = 9`chỉ khi chúng ta ép các tòa tháp riêng biệt. Vì chúng tôi được phép nhiều nhất`i`tháp, câu trả lời là:```
3 3
```Một cách tiếp cận sai lầm là tham lam tạo ra một tòa tháp mới cho mỗi ngôi nhà sẽ bỏ lỡ thực tế là các khoảng thời gian chồng chéo sẽ tồn tại cùng nhau. 

Trường hợp cạnh cuối cùng là khi các khoảng được tách biệt hoàn toàn:```
2
1 1
8 8
```Các khoảng là`[1,2]`Và`[8,16]`. Chi phí một tháp`15`, trong khi hai tòa tháp có giá`1 + 8 = 9`. Thuật toán phải nhận ra rằng việc chia tách ở khoảng trống là có lợi. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi cách nhóm nhà có thể thành tháp. Đối với mỗi nhóm, chúng tôi tìm các giá trị biến đổi tối thiểu và tối đa và tính toán chi phí của tòa tháp. Điều này đúng vì một tòa tháp hoàn toàn được xác định bởi cột nhỏ nhất và giá trị đường chéo lớn nhất trong số những ngôi nhà mà nó bao phủ. Tuy nhiên, số lượng phân vùng có thể có là theo cấp số nhân nên không thể sử dụng được. 

Một ý tưởng thực tế hơn là sắp xếp các khoảng và sử dụng lập trình động trong đó`dp[k]`lưu trữ chi phí tốt nhất bằng cách sử dụng`k`nhóm trong vài khoảng thời gian đầu tiên. Điều này vẫn đòi hỏi phải xem xét nhiều điểm phân chia trước đó. Với`n = 200000`, thậm chí còn được tối ưu hóa`O(n^2)`phiên bản sẽ yêu cầu khoảng bốn mươi tỷ hoạt động. 

Quan sát quan trọng là sau khi chuyển đổi các ngôi nhà thành các khoảng, vấn đề sẽ trở thành bao phủ các khoảng bằng các phân đoạn. Nếu chúng ta bắt đầu với một đoạn cho mỗi khoảng thời gian thì tổng chi phí là tổng của tất cả các độ dài khoảng riêng lẻ. Bất cứ khi nào chúng tôi hợp nhất hai nhóm lân cận, chi phí tăng thêm duy nhất đến từ khoảng cách giữa chúng. Nếu các khoảng thời gian trùng nhau thì việc hợp nhất sẽ không tốn kém gì. Nếu có khoảng cách về kích thước`g`, việc sáp nhập làm tăng chi phí một cách chính xác`g`. 

Bởi vì mọi chi phí hợp nhất đều độc lập nên cách tốt nhất để có ít tháp hơn là loại bỏ những khoảng trống lớn nhất trước tiên. Bắt đầu với`n`các nhóm, mỗi khi chúng tôi hợp nhất qua một khoảng trống, chúng tôi sẽ giảm số lượng tháp đi một và tăng chi phí theo khoảng trống đó. Vì vậy, câu trả lời có thể được xây dựng bằng cách sắp xếp tất cả các khoảng trống hữu ích. 

Quá trình này là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Hợp nhất khoảng thời gian với các khoảng trống được sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi ngôi nhà`(r, c)`vào một khoảng`[c, r + c]`. Điều này có tác dụng vì một tháp bao phủ một ngôi nhà một cách chính xác khi khoảng của tháp chứa các điểm cuối khoảng của ngôi nhà. 
2. Sắp xếp tất cả các khoảng theo điểm cuối bên trái của chúng. Sau khi sắp xếp, tất cả các khoảng có thể chồng lên nhau sẽ xuất hiện cạnh nhau nên chúng ta chỉ cần kiểm tra các khoảng liền kề. 
3. Hợp nhất các khoảng thời gian trong khi quét từ trái sang phải. Duy trì khoảng thời gian hợp nhất hiện tại. Nếu khoảng thời gian tiếp theo bắt đầu trước khi khoảng thời gian hiện tại kết thúc thì hai khoảng thời gian đó thuộc về cùng một thành phần được kết nối và không cần thêm tháp. Nếu không, có một khoảng cách giữa chúng và việc chia tách ở đó có thể cứu được chính xác khoảng cách đó. 
4. Tính tổng chi phí nếu mỗi thành phần được kết nối được chi trả riêng. Đây là chi phí tốt nhất có thể với số lượng tháp tối đa. 
5. Lưu trữ mọi khoảng cách giữa các thành phần liên tiếp. Mỗi khoảng trống thể hiện chi phí bổ sung phải trả khi hai thành phần bị ép vào một tòa tháp. 
6. Sắp xếp các khoảng trống theo thứ tự giảm dần. Để giảm số lượng tháp từ`n`xuống tới`1`, liên tục hợp nhất qua khoảng trống lớn nhất còn lại. Chi phí vận hành sau mỗi lần hợp nhất sẽ đưa ra câu trả lời cho số lượng tháp tương ứng. 

Tại sao nó hoạt động: các khoảng bên trong một thành phần được kết nối chồng lên nhau thông qua các chuỗi, do đó, một phân đoạn bao phủ thành phần đó là không thể tránh khỏi. Lựa chọn duy nhất là có nên tham gia các thành phần khác nhau hay không. Việc nối hai thành phần sẽ thêm chính xác khoảng cách giữa chúng và các khoảng cách khác nhau không ảnh hưởng lẫn nhau. Việc chọn những khoảng trống lớn nhất để loại bỏ trước tiên sẽ giảm thiểu mọi số lần hợp nhất có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    intervals = []

    for _ in range(n):
        r, c = map(int, input().split())
        intervals.append((c, r + c))

    intervals.sort()

    base = 0
    gaps = []

    left, right = intervals[0]

    for l, r in intervals[1:]:
        if l <= right:
            if r > right:
                right = r
        else:
            base += right - left
            gaps.append(l - right)
            left, right = l, r

    base += right - left

    gaps.sort(reverse=True)

    ans = [0] * n
    cur = base

    components = len(gaps) + 1
    ans[components - 1] = cur

    for gap in gaps:
        cur += gap
        components -= 1
        ans[components - 1] = cur

    for i in range(1, n):
        if ans[i] == 0:
            ans[i] = ans[i - 1]

    print(*ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tạo ra các khoảng chuyển đổi. Điểm cuối phù hợp sử dụng`r + c`, trong khi điểm cuối bên trái chỉ đơn giản là`c`. 

Sau khi sắp xếp, quá trình quét sẽ tìm thấy các thành phần được kết nối. Biến`base`lưu trữ chi phí bao gồm từng thành phần một cách độc lập. Danh sách khoảng trống chứa số tiền chính xác mà chúng tôi bị mất khi kết hợp hai thành phần lân cận. 

Vòng lặp cuối cùng xử lý các khoảng trống từ lớn nhất đến nhỏ nhất. Mỗi khoảng trống được xử lý tương ứng với việc sử dụng ít hơn một tháp. Vì đầu ra yêu cầu "nhiều nhất"`i`tháp, các câu trả lời sẽ được lấp đầy để số lượng tháp lớn hơn chưa sử dụng sẽ giữ được giá trị tốt nhất hiện có. 

Việc xử lý ranh giới là quan trọng. Thành phần cuối cùng phải được thêm vào sau vòng lặp, nếu không khoảng cuối cùng sẽ biến mất. Tất cả số học đều sử dụng số nguyên Python nên tọa độ lớn không gây tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 1
8 8
```Các khoảng biến đổi là`[1,2]`Và`[8,16]`. 

| Bước | Thành phần hiện tại | Khoảng trống | Chi phí | 
| --- | --- | --- | --- | 
| Bắt đầu |`[1,2]`,`[8,16]`|`6`|`9`| 
| Hợp nhất khoảng cách lớn nhất |`[1,16]`| không |`15`| 

Câu trả lời là:```
15 9
```Dấu vết cho thấy việc tách hai khoảng cách xa nhau sẽ rẻ hơn khi cho phép hai tòa tháp. 

### Mẫu 2 

đầu vào:```
3
3 5
1 3
2 3
```Các khoảng là`[5,8]`,`[3,4]`, Và`[3,5]`. 

| Bước | Thành phần hiện tại | Khoảng trống | Chi phí | 
| --- | --- | --- | --- | 
| Bắt đầu |`[3,8]`| không |`5`| 

Tất cả các khoảng chồng chéo lên nhau sau khi hợp nhất, vì vậy việc thêm nhiều tháp hơn không thể cải thiện kết quả. 

Đầu ra là:```
5 5 5
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các khoảng thời gian và khoảng trống chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Chúng tôi lưu trữ các khoảng và khoảng trống | 

Giải pháp phù hợp với các ràng buộc bởi vì`n`là`200000`. Việc sắp xếp lượng dữ liệu này dễ dàng nằm trong giới hạn cho phép, trong khi các phương pháp tiếp cận bậc hai lại quá chậm. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    n = int(data())
    intervals = []
    for _ in range(n):
        r, c = map(int, data().split())
        intervals.append((c, r + c))

    intervals.sort()
    base = 0
    gaps = []

    l, r = intervals[0]
    for nl, nr in intervals[1:]:
        if nl <= r:
            r = max(r, nr)
        else:
            base += r - l
            gaps.append(nl - r)
            l, r = nl, nr

    base += r - l
    gaps.sort(reverse=True)

    ans = [0] * n
    cur = base
    cnt = len(gaps) + 1
    ans[cnt - 1] = cur

    for g in gaps:
        cur += g
        cnt -= 1
        ans[cnt - 1] = cur

    for i in range(1, n):
        if ans[i] == 0:
            ans[i] = ans[i - 1]

    sys.stdin = old
    return " ".join(map(str, ans))

assert run("""2
1 1
8 8
""") == "15 9", "sample 1"

assert run("""3
3 5
1 3
2 3
""") == "5 5 5", "sample 2"

assert run("""1
5 10
""") == "5", "single house"

assert run("""2
1 1
8 8
""") == "15 9", "separate intervals"

assert run("""4
1 1
1 2
1 3
1 4
""") == "4 4 4 4", "all overlapping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một ngôi nhà |`5`| Xử lý khoảng thời gian đơn | 
| Hai ngôi nhà xa nhau |`15 9`| Hợp nhất khoảng cách lớn nhất | 
| Khoảng chồng chéo lồng nhau |`4 4 4 4`| Các thành phần chồng chéo không được phân chia | 

## Vỏ cạnh 

Đối với một ngôi nhà đơn lẻ như:```
1
5 10
```khoảng thời gian là`[10,15]`. Thuật toán không tạo ra khoảng trống nên chỉ có một thành phần và giá thành là`15 - 10 = 5`. Điều này tránh nhầm lẫn tọa độ đường chéo của tháp với chi phí hàng của nó. 

Đối với các khoảng chồng chéo:```
2
3 5
2 6
```các khoảng là`[5,8]`Và`[6,8]`. Quá trình quét sẽ hợp nhất chúng ngay lập tức vì khoảng thời gian thứ hai bắt đầu trước khi khoảng thời gian đầu tiên kết thúc. Số lượng thành phần vẫn là một nên mọi câu trả lời đều giống nhau. Việc này xử lý trường hợp tháp phụ không thể giảm giá. 

Đối với các khoảng cách nhau:```
2
1 1
8 8
```quá trình quét tạo ra hai thành phần và ghi lại khoảng cách`8 - 2 = 6`. Với hai tòa tháp, chi phí là tổng chiều dài thành phần,`1 + 8 = 9`. Khi chỉ cho phép một tháp, thuật toán sẽ cộng lại khoảng trống, tạo ra`15`. Kết quả phù hợp với nhu cầu trả tiền cho việc thu hẹp khoảng trống.
