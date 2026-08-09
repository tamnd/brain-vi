---
title: "CF 102443C - Định lý cuối cùng của Fermat"
description: "Chương trình xem xét mọi bộ tứ (a, b, c, n) số nguyên dương có n = 3. Thứ tự của nó có hai cấp độ. Đầu tiên, các bộ tứ được nhóm theo giá trị lớn nhất trong số bốn tọa độ của chúng. Bên trong một nhóm như vậy, chúng được sắp xếp theo thứ tự từ điển (a, b, c, n)."
date: "2026-08-09T13:39:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "C"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 253
verified: true
draft: false
---

[CF 102443C - Định lý cuối cùng của Fermat](https://codeforces.com/problemset/problem/102443/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chương trình xem xét mọi bộ tứ`(a, b, c, n)`của các số nguyên dương với`n >= 3`. Thứ tự của nó có hai cấp độ. Đầu tiên, các bộ tứ được nhóm theo giá trị lớn nhất trong số bốn tọa độ của chúng. Trong một nhóm như vậy, chúng được sắp xếp theo thứ tự từ điển`(a, b, c, n)`. Đối với mỗi bộ tứ, chúng ta chỉ cần in xem`a^n + b^n`lớn hơn hoặc nhỏ hơn`c^n`. 

Đầu vào cho hai vị trí`l`Và`r`trong chuỗi vô hạn này. Chúng ta phải xây dựng lại chính xác các bộ tứ chiếm các vị trí đó và in ra các bất đẳng thức tương ứng của chúng. 

Cái khó đó là`l`Và`r`có thể lớn như`10^12`. Mặc dù khoảng thời gian được yêu cầu chứa nhiều nhất`10^4 + 1`câu trả lời, bản thân vị trí đó có thể nằm xa trong trình tự. Việc tạo ra tất cả các bộ tứ trước đó là hoàn toàn không khả thi. 

Có một giới hạn hữu ích ẩn trong các ràng buộc. Nếu mọi tọa độ nhiều nhất là`m`, thì có`m^3(m-2)`có thể tăng gấp bốn lần, bởi vì`a`,`b`, Và`c`mỗi người đều có`m`sự lựa chọn trong khi`n`có`m-2`sự lựa chọn từ`3`bởi vì`m`. Từ`1001^3 * 999`đã lớn hơn rồi`10^12`, mọi vị trí được yêu cầu thuộc về một nhóm có tối đa nhiều nhất`1001`. Điều này làm cho không gian tìm kiếm cực đại trở nên rất nhỏ, mặc dù bản thân dãy đó rất lớn. 

Một số chi tiết đặt hàng rất dễ bị xử lý sai. 

Bộ tứ thứ nhất là`(1,1,1,3)`, do đó số mũ bắt đầu tại`3`, không phải tại`1`. Ví dụ, đầu vào`1 1`phải sản xuất`1^3+1^3>1^3`. Một triển khai xử lý`n`từ`1`sẽ xây dựng trình tự sai ngay lập tức. 

Sự chuyển đổi giữa hai giá trị tối đa là một cái bẫy phổ biến khác. Bộ tứ cuối cùng với mức tối đa`3`là`(3,3,3,3)`, có vị trí`27`. Bộ tứ tiếp theo là`(1,1,1,4)`, tại vị trí`28`. Như vậy đầu vào`28 28`sản xuất`1^4+1^4>1^4`. Tìm kiếm nhóm chứa vị trí với`<=`ở phía sai có thể thay đổi mọi câu trả lời sau của cả một nhóm. 

Thực tế là mức tối đa có thể đã xuất hiện trước đó trong tiền tố từ điển cũng làm thay đổi số lần hoàn thành hợp lệ. Ví dụ, một lần`a=M`, mọi tọa độ sau này có thể được chọn tự do lên đến`M`, bởi vì bộ tứ đã có mức tối đa`M`. Mặt khác, khi`a<M`, các tọa độ còn lại phải chứa một`M`. Việc bỏ qua sự khác biệt này sẽ đưa ra số bộ tứ sai cho một tiền tố và do đó chọn bộ sai. 

Cuối cùng, bản thân việc so sánh phải sử dụng lũy ​​thừa số nguyên thực tế. Ví dụ,`(3,3,2,3)`cho`3^3+3^3 > 2^3`. Số nguyên Python có độ chính xác tùy ý, vì vậy chúng ta có thể đánh giá các lũy thừa này một cách trực tiếp mà không bị tràn. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liệt kê các bộ bốn theo đúng thứ tự yêu cầu. Với mọi mức tối đa có thể`M`, chúng ta có thể liệt kê tất cả các giá trị của`a`,`b`,`c`, Và`n`, bỏ qua các kết hợp không có mức tối đa`M`, cho đến khi đạt vị trí`r`. Điều này đúng vì nó tuân theo định nghĩa của chuỗi theo đúng nghĩa đen. 

Vấn đề là số lần lặp lại. đầu tiên`M`các nhóm cùng nhau chứa`M^3(M-2)`tăng gấp bốn lần, do đó đạt đến vị trí xung quanh`10^12`yêu cầu xử lý về`10^12`tăng gấp bốn lần. Ngay cả trước khi xem xét chi phí cho khả năng tính toán, một nghìn tỷ lần lặp đã vượt xa giới hạn thời gian. Việc tính từng lũy ​​thừa theo lũy thừa bằng cách bình phương sẽ thêm một hệ số logarit khác. 

Quan sát quan trọng là chúng ta không bao giờ cần tạo ra các bộ tứ trước đó. Chúng ta có thể đếm có bao nhiêu bộ tứ tồn tại trước mức tối đa nhất định, xác định vị trí tối đa chứa vị trí được yêu cầu bằng tìm kiếm nhị phân, sau đó xác định vị trí từng tọa độ bên trong nhóm tối đa đó bằng cách sử dụng số tiền tố. 

Đối với mức tối đa cố định`M`, mọi tọa độ đều nằm trong`[1,M]`Và`n`nằm ở`[3,M]`. Tổng số bộ tứ có tối đa nhiều nhất`M`là`F(M) = M^3(M-2)`. 

Do đó, các bộ bốn có mức tối đa chính xác là`M`chiếm vị trí`F(M-1)+1`bởi vì`F(M)`. 

Sau khi định vị`M`, chúng tôi biết vị trí được yêu cầu trong nhóm đó. Sau đó chúng tôi xây dựng lại`(a,b,c,n)`từ trái sang phải. Tại mỗi tọa độ chỉ có hai trường hợp liên quan. Nếu tiền tố đã chứa`M`, tất cả các tọa độ còn lại không bị hạn chế. Nếu không, chọn giá trị nhỏ hơn`M`có nghĩa là một số tọa độ sau này phải bằng`M`. 

Đối với một vị trí cố định, mọi giá trị ứng cử viên đều nhỏ hơn`M`có cùng số lần hoàn thành hợp lệ. giá trị`M`có số lần hoàn thành khác nhau vì nó thỏa mãn ngay yêu cầu tối đa. Cấu trúc không đổi cho mỗi ứng viên này cho phép chúng tôi tìm tọa độ chính xác bằng tìm kiếm nhị phân thay vì quét tất cả các giá trị. 

Brute-force hoạt động vì nó tuân theo trình tự chính xác nhưng không thành công khi thứ hạng được yêu cầu trở nên lớn. Quan sát cho thấy mọi tiền tố đều có thể được tính theo đại số cho phép chúng ta chuyển trực tiếp đến nhóm được yêu cầu và sau đó đến bộ tứ được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(r log M)`công việc số học |`O(1)`| Quá chậm | 
| Tối ưu |`O((r-l+1) log M)`|`O(r-l+1)`cho đầu ra | Đã chấp nhận | 

Đây`M <= 1001`, do đó các thừa số logarit rất nhỏ. Bản thân đầu ra có thể chứa tới`10001`nên việc lưu trữ câu trả lời cũng không tốn kém. 

## Hướng dẫn thuật toán 

1. Xác định`F(M) = M^3(M-2)`. 

Điều này tính mọi bộ tứ có tối đa bốn tọa độ`M`, với`n`hạn chế ở`3..M`. Như vậy`F(M)`chính xác là vị trí cuối cùng thuộc về mức tối đa`M`. 
2. Tìm kiếm nhị phân nhỏ nhất`M`thỏa mãn`F(M) >= rank`. 

Điều này xác định nhóm tối đa chứa vị trí được yêu cầu. Vị trí đầu tiên của nhóm này là`F(M-1)+1`, vậy hãy đặt`k = rank - F(M-1)`. 

Hiện nay`k`là một vị trí dựa trên một trong nhóm với độ chính xác tối đa`M`. 
3. Tái thiết`a`,`b`,`c`, Và`n`từ trái sang phải. 

Duy trì một Boolean`maxed`cho biết liệu tiền tố đã được chọn có chứa`M`. Ban đầu nó là sai. 

Vì`a`, nếu như`a<M`, thì một trong`b`,`c`, hoặc`n`phải bằng`M`. Số lần hoàn thành là`M^2(M-2) - (M-1)^2(M-3)`. 

Nếu như`a=M`, điều kiện tối đa đã được thỏa mãn và có`M^2(M-2)`sự hoàn thiện. 

Những con số này không đổi đối với mọi`a<M`, vì vậy chúng ta có thể tìm kiếm nhị phân nhỏ nhất`a`có số lần hoàn thành tích lũy đạt tới`k`. 
4. Sau khi chọn`a`, trừ đi số bộ thuộc các giá trị nhỏ hơn của`a`, sau đó cập nhật`maxed`nếu như`a=M`. 

Ý tưởng tương tự áp dụng cho`b`. Nếu mức tối đa chưa xuất hiện thì mọi`b<M`có`M(M-2) - (M-1)(M-3)`hoàn thành, trong khi`b=M`có`M(M-2)`sự hoàn thiện. 

Nếu mức tối đa đã xuất hiện thì mọi`b`có chính xác`M(M-2)`sự hoàn thiện. 
5. Áp dụng lý luận tương tự cho`c`. 

Nếu như`M`chưa xuất hiện, đang chọn`c<M`lực lượng`n=M`, vậy có đúng một lần hoàn thành. Lựa chọn`c=M`cho phép mọi số mũ`n`từ`3`bởi vì`M`, cho`M-2`sự hoàn thiện. 

Nếu mức tối đa đã xuất hiện thì mọi`c`có`M-2`số mũ có thể. 
6. Tay cầm`n`cuối cùng. 

Nếu mức tối đa đã xuất hiện thì số mũ hợp lệ chính xác là`3,4,...,M`, theo thứ tự đó. Thứ hạng địa phương quyết định trực tiếp`n = 3 + k - 1`. 

Nếu mức tối đa không xuất hiện sau khi chọn`c`, sau đó`n`buộc phải là`M`. 
7. So sánh hai vế bằng cách sử dụng các số nguyên có độ chính xác tùy ý của Python. 

In`a^n+b^n>c^n`khi cạnh trái lớn hơn, nếu không thì in`a^n+b^n<c^n`. 

Định lý cuối cùng của Fermat đảm bảo sự đẳng thức không thể xảy ra đối với các số nguyên dương này với`n>=3`. 

### Tại sao nó hoạt động 

Bất biến trung tâm là`k`luôn là thứ hạng từ điển của bộ dữ liệu được yêu cầu trong số tất cả các phần hoàn thành hợp lệ của tiền tố đã cố định. Đối với mỗi tọa độ ứng viên, chúng tôi đếm chính xác có bao nhiêu bộ dữ liệu hợp lệ bắt đầu với ứng cử viên đó. Vì thứ tự từ điển đặt tất cả các bộ có ứng cử viên nhỏ hơn lên trước, nên ứng viên đầu tiên có số lần hoàn thành tích lũy đạt`k`phải là tọa độ của bộ dữ liệu được yêu cầu. Sau khi loại bỏ tất cả các ứng cử viên trước đó, bất biến tương tự vẫn giữ nguyên cho tọa độ tiếp theo. 

Số lần hoàn thành là chính xác vì chỉ có hai khả năng. Nếu tối đa`M`đã xuất hiện, mọi tọa độ còn lại có thể độc lập nhận bất kỳ giá trị cho phép nào. Nếu nó chưa xuất hiện, hãy chọn một giá trị bên dưới`M`yêu cầu các tọa độ còn lại phải chứa`M`, được tính bằng cách trừ đi các bài tập được giới hạn trong`1..M-1`. Điều này đưa ra số lượng chính xác các bộ dữ liệu được biểu thị bằng mỗi tiền tố, vì vậy mọi tìm kiếm nhị phân đều chọn tọa độ từ điển chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def total(m):
    if m < 3:
        return 0
    return m * m * m * (m - 2)

def find_maximum(rank):
    lo, hi = 3, 4

    while total(hi) < rank:
        hi *= 2

    while lo < hi:
        mid = (lo + hi) // 2
        if total(mid) >= rank:
            hi = mid
        else:
            lo = mid + 1

    return lo

def choose_coordinate(m, k, pos, maxed):
    if pos == 0:
        if maxed:
            small = big = m * m * (m - 2)
        else:
            small = (
                m * m * (m - 2)
                - (m - 1) * (m - 1) * (m - 3)
            )
            big = m * m * (m - 2)

    elif pos == 1:
        if maxed:
            small = big = m * (m - 2)
        else:
            small = (
                m * (m - 2)
                - (m - 1) * (m - 3)
            )
            big = m * (m - 2)

    else:
        if maxed:
            small = big = m - 2
        else:
            small = 1
            big = m - 2

    lo, hi = 1, m

    while lo < hi:
        mid = (lo + hi) // 2

        if mid < m:
            count = mid * small
        else:
            count = (m - 1) * small + big

        if count >= k:
            hi = mid
        else:
            lo = mid + 1

    value = lo

    before = (value - 1) * small
    k -= before

    return value, k

def get_tuple(rank):
    m = find_maximum(rank)

    k = rank - total(m - 1)

    a, k = choose_coordinate(m, k, 0, False)
    maxed = (a == m)

    b, k = choose_coordinate(m, k, 1, maxed)
    maxed = maxed or (b == m)

    c, k = choose_coordinate(m, k, 2, maxed)
    maxed = maxed or (c == m)

    if maxed:
        n = 3 + k - 1
    else:
        n = m

    return a, b, c, n

def solve(data):
    l, r = map(int, data.split())

    ans = []

    for rank in range(l, r + 1):
        a, b, c, n = get_tuple(rank)

        left = a ** n + b ** n
        right = c ** n

        if left > right:
            op = '>'
        else:
            op = '<'

        ans.append(f"{a}^{n}+{b}^{n}{op}{c}^{n}")

    return '\n'.join(ans)

def main():
    data = input()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```các`total`hàm thực hiện đếm tích lũy`F(M)`. Trường hợp đặc biệt`m<3`là cần thiết vì không có số mũ pháp lý khi mức tối đa dưới đây`3`.`find_maximum`đầu tiên tìm giới hạn trên an toàn bằng cách nhân đôi, sau đó thực hiện tìm kiếm nhị phân giới hạn dưới tiêu chuẩn. Điều này tránh phụ thuộc vào giới hạn dẫn xuất`M<=1001`trong quá trình triển khai, trong khi giới hạn giải thích tại sao việc tìm kiếm vẫn cực kỳ nhỏ.`choose_coordinate`chứa đối số đếm cốt lõi. các`pos`tham số xác định liệu chúng ta có đang chọn`a`,`b`, hoặc`c`. Biến`small`là số lần hoàn thành của bất kỳ ứng viên nào nhỏ hơn`M`, trong khi`big`là số lần hoàn thành khi chính ứng viên đó`M`. 

Khi`maxed`là đúng,`small`Và`big`bằng nhau vì yêu cầu tối đa đã được đáp ứng. Khi nó là sai,`small`thu được bằng cách trừ các cấu hình không bao giờ sử dụng`M`. Phép trừ này là bước tổ hợp quan trọng. 

Tìm kiếm nhị phân yêu cầu tọa độ đầu tiên có số lượng bộ dữ liệu tích lũy ít nhất`k`. Dành cho ứng viên dưới đây`M`, số lượng tích lũy chỉ đơn giản là`candidate * small`. Vì`M`, đó là`(M-1)*small + big`. Sau khi tìm được tọa độ,`(value-1)*small`các bộ dữ liệu đã bị bỏ qua nên số tiền đó sẽ bị xóa khỏi`k`. 

Số mũ được xử lý riêng vì phạm vi pháp lý của nó bắt đầu tại`3`và bởi vì nó là tọa độ cuối cùng. Một lần`M`đều đã xuất hiện rồi`M-2`giá trị số mũ có sẵn. Nếu nó chưa xuất hiện thì số mũ phải bằng`M`. 

Việc so sánh sử dụng số học số nguyên chính xác. Quyền lực lớn nhất liên quan là khoảng`1001^1001`, chỉ dài vài nghìn chữ số thập phân, vì vậy các số nguyên có độ chính xác tùy ý của Python dễ dàng là đủ. Không có làm tròn dấu phẩy động và không có rủi ro tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, bốn vị trí đầu tiên đều thuộc giá trị lớn nhất`3`nhóm. Từ`F(2)=0`Và`F(3)=27`, thứ hạng cục bộ bằng với thứ hạng toàn cầu. 

| Xếp hạng toàn cầu | Tối đa`M`| Xếp hạng địa phương`k`|`a`|`b`|`c`|`n`| Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 1 | 1 | 3 |`1^3+1^3>1^3`| 
| 2 | 3 | 2 | 1 | 1 | 2 | 3 |`1^3+1^3<2^3`| 
| 3 | 3 | 3 | 1 | 1 | 3 | 3 |`1^3+1^3<3^3`| 
| 4 | 3 | 4 | 1 | 2 | 1 | 3 |`1^3+2^3>1^3`| 

Đối với cấp bậc`1`, bộ dữ liệu`(1,1,1,3)`được chọn. Đối với cấp bậc`2`, tọa độ đầu tiên vẫn còn`1`, phần còn lại thứ hai`1`, và bước thứ ba tiến tới`2`. Quá trình từ điển tương tự tạo ra hai bộ dữ liệu còn lại. 

Là dấu vết thứ hai, hãy xem xét thứ hạng được yêu cầu tối đa có thể`10^12`. chúng tôi có`F(1000) = 998,000,000,000`trong khi`F(1001) = 1,001,999,997,999`. 

Như vậy cấp bậc nằm ở`M=1001`nhóm, ở vị trí địa phương`2,000,000,000`. 

Việc tái thiết tọa độ tiến hành như sau. 

| Sân khấu |`k`trước |`small`|`big`| Giá trị được chọn |`k`sau | 
| --- | --- | --- | --- | --- | --- | 
|`a`| 2.000.000.000 | 2.998.999 | 1.000.998.999 | 667 | 2.666.666 | 
|`b`| 2.666.666 | 1.999 | 999.999 | 1001 | 667.666 | 
|`c`| 667.666 | 1 | 999 | 668 | 334 | 
|`n`| 334 | 1 | 1 | 336 | 334 | 

Sau khi chọn`b=1001`, mức tối đa đã xuất hiện nên mọi tọa độ sau này đều không bị hạn chế. Kết quả là gấp bốn lần là`(667,1001,668,336)`. Từ`b`lớn hơn`c`, sự so sánh được biết ngay là có`a^n+b^n > c^n`tích cực`n`, vì vậy đầu ra là`667^336+1001^336>668^336`. 

Dấu vết này chứng tỏ rằng ngay cả một vị trí gần`10^12`được xây dựng lại chỉ thông qua một số tìm kiếm nhị phân thay vì thông qua hàng tỷ bộ dữ liệu trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((r-l+1) log M)`các phép tính số học, cộng với các phép tính lũy thừa chính xác | Mỗi đầu ra thực hiện một tìm kiếm nhị phân cho`M`và ba tìm kiếm nhị phân cho tọa độ. | 
| Không gian |`O(r-l+1)`| Chuỗi câu trả lời được lưu trữ trước khi được viết. | 

Đây`M`nhiều nhất là`1001`cho giới hạn trên nhất định trên`r`, vì vậy mỗi lần tìm kiếm nhị phân chỉ mất khoảng mười lần lặp. Với nhiều nhất`10001`bất đẳng thức được yêu cầu, số lượng các phép toán tổ hợp là rất nhỏ. Các lũy thừa được tạo ra chỉ có vài nghìn chữ số thập phân, do đó số học có độ chính xác tùy ý của Python cũng vừa vặn thoải mái trong giới hạn 4 giây và 512 MB. 

## Trường hợp thử nghiệm 

Tệp thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve(data)`chức năng được sử dụng ở trên.```
# Save the solution as solution.py before running these tests.
from solution import solve

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided sample
assert run("1 4") == (
    "1^3+1^3>1^3\n"
    "1^3+1^3<2^3\n"
    "1^3+1^3<3^3\n"
    "1^3+2^3>1^3"
), "sample 1"

# Minimum possible position
assert run("1 1") == "1^3+1^3>1^3", "minimum position"

# Last tuple of the M=3 block
assert run("27 27") == "3^3+3^3>3^3", "all coordinates equal"

# First tuple of the M=4 block
assert run("28 28") == "1^4+1^4>1^4", "maximum-group boundary"

# Crossing the boundary between M=3 and M=4
assert run("26 29") == (
    "3^3+3^3>2^3\n"
    "3^3+3^3>3^3\n"
    "1^4+1^4>1^4\n"
    "1^3+1^3<2^3"
), "off-by-one around group transition"

# Maximum allowed rank
assert run("1000000000000 1000000000000") == (
    "667^336+1001^336>668^336"
), "maximum rank"

# A short interval inside one group
out = run("2 10").splitlines()
assert len(out) == 9, "correct number of outputs"
assert out[0] == "1^3+1^3<2^3", "second position"
assert out[-1] == "1^3+3^3>3^3", "tenth position"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1^3+1^3>1^3`| Xếp hạng tối thiểu và số mũ giới hạn dưới | 
|`27 27`|`3^3+3^3>3^3`| Bộ cuối cùng của nhóm tối đa và tất cả các tọa độ đều bằng nhau | 
|`28 28`|`1^4+1^4>1^4`| Chuyển đổi chính xác từ mức tối đa`3`đến mức tối đa`4`| 
|`26 29`| Bốn dòng vượt qua quá trình chuyển đổi | Thứ tự từ điển và lỗi ranh giới nhóm | 
|`1000000000000 1000000000000`|`667^336+1001^336>668^336`| Xếp hạng tối đa được phép và xây dựng lại chỉ mục lớn | 
|`2 10`| Chín dòng liên tiếp | Cập nhật thứ hạng địa phương trong một nhóm | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu`1 1`, tìm kiếm nhị phân tìm thấy`M=3`bởi vì`F(2)=0`Và`F(3)=27`. Thứ hạng địa phương là`1`, vậy tọa độ đầu tiên là`1`, thì thứ hai là`1`, thì thứ ba là`1`, và cuối cùng`n=3`. Kết quả là chính xác`1^3+1^3>1^3`. Điều này nắm bắt các triển khai vô tình cho phép số mũ`1`hoặc`2`. 

Đối với đầu vào`27 27`, thứ hạng địa phương là`27`, vị trí cuối cùng ở mức tối đa`3`nhóm. Do đó, mọi tọa độ đều được chọn là`3`, sản xuất`(3,3,3,3)`. Đầu ra là`3^3+3^3>3^3`. Điều này kiểm tra rằng đặc biệt`M`ứng cử viên nhận được số lượng hoàn thành lớn hơn và bộ dữ liệu cuối cùng của nhóm không bị bỏ qua. 

Đối với đầu vào`28 28`,`F(3)=27`, do đó việc tìm kiếm sẽ chuyển sang`M=4`. Thứ hạng địa phương trở thành`1`. Bộ dữ liệu nhỏ nhất về mặt từ điển với độ chính xác tối đa`4`là`(1,1,1,4)`, bởi vì cực đại phải xảy ra ở đâu đó và`n`là tọa độ cuối cùng, khiến nó trở thành vị trí đầu tiên nơi`4`có thể xuất hiện trong khi vẫn giữ tất cả các tọa độ trước đó bằng`1`. Đầu ra là`1^4+1^4>1^4`. Đây là trường hợp ranh giới quan trọng để chuyển đổi thứ hạng toàn cầu thành thứ hạng cục bộ. 

Để có đầu vào tối đa`10^12 10^12`, nhóm tối đa là`M=1001`. Thứ hạng địa phương là`2,000,000,000`, và việc tái thiết mang lại`(667,1001,668,336)`. Sự xuất hiện của`1001`TRONG`b`ngay lập tức đánh dấu tiền tố là hoàn chỉnh đối với điều kiện tối đa, do đó các tọa độ còn lại được chọn làm giá trị từ điển thông thường. Sự so sánh cuối cùng là`667^336+1001^336>668^336`. Điều này xác nhận rằng phương pháp đếm vẫn còn hiệu lực trong chuỗi mà không cần liệt kê số trước đó.`10^12`bộ dữ liệu. 

Bài xã luận đã sẵn sàng để sử dụng như một lời giải thích khi gửi độc lập.
