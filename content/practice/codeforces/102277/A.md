---
title: "CF 102277A - Cửa sổ trên tường"
description: "Bài toán yêu cầu cửa sổ hình chữ nhật lớn nhất có thể cắt thành một bức tường hình chữ nhật. Bức tường có chiều rộng w và chiều cao h, và cửa sổ phải cách mọi mép tường ít nhất d đơn vị. Khoảng cách yêu cầu áp dụng độc lập cho cả bốn phía."
date: "2026-08-17T03:26:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "A"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 419
verified: true
draft: false
---

[CF 102277A - Cửa sổ trên tường](https://codeforces.com/problemset/problem/102277/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 59 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu cửa sổ hình chữ nhật lớn nhất có thể cắt thành một bức tường hình chữ nhật. Bức tường có chiều rộng`w`và chiều cao`h`, và cửa sổ ít nhất phải ở lại`d`các đơn vị cách xa mọi cạnh của bức tường. Khoảng cách yêu cầu áp dụng độc lập cho cả bốn phía. Chúng ta cần xuất diện tích cửa sổ tối đa có thể, hoặc`0`khi khoảng trống cần thiết không còn chỗ cho cửa sổ. Cuộc thi ban đầu chỉ định`w, h < 1000`Và`d < 100`, với giới hạn thời gian một giây và bộ nhớ 256 MB. 

Hình học ngay lập tức giảm chiều rộng có sẵn bằng cách`d`ở bên trái và`d`ở bên phải, vì vậy chiều rộng cửa sổ lớn nhất có thể là`w - 2d`. Lý do tương tự cho chiều cao tối đa của`h - 2d`. Nếu một trong hai giá trị không dương thì không có cửa sổ nào có vùng dương có thể vừa. Ngược lại, hình chữ nhật lớn nhất sử dụng cả hai kích thước có sẵn. 

Các giới hạn nhỏ thậm chí có nghĩa là`O(w h)`việc liệt kê sẽ chỉ kiểm tra khoảng một triệu cặp trong trường hợp xấu nhất, vì`w`Và`h`ở dưới`1000`. Điều đó không thể so sánh được với`10^5`-ràng buộc tỷ lệ trong đó các thuật toán bậc hai được tự động loại trừ. Tuy nhiên, bài toán này chứa đủ cấu trúc hình học nên việc liệt kê là không cần thiết: câu trả lời có thể được tính bằng một số phép tính số học không đổi. 

Có hai trường hợp ranh giới thường gây ra việc triển khai không chính xác. Đầu tiên, khoảng trống có thể tiêu thụ toàn bộ chiều. Ví dụ, với đầu vào`30 20 12`, chiều rộng có sẵn là`30 - 24 = 6`, trong khi chiều cao có sẵn là`20 - 24 = -4`, vì vậy đầu ra đúng là`0`. Việc thực hiện bất cẩn làm nhân hai giá trị mà không kiểm tra chúng sẽ tạo ra`-24`, không thể đại diện cho một khu vực cửa sổ. 

Thứ hai, khoảng cách có thể lớn hơn một nửa của cả hai chiều. Vì`40 25 50`, kích thước có sẵn là`-60`Và`-75`, vậy câu trả lời lại là`0`. Chỉ kiểm tra xem`w - 2d`là dương là không đủ vì chiều cao cũng phải đủ lớn. Các mẫu chính thức bao gồm cả hai trường hợp không thể xảy ra này. 

Ranh giới khác là trường hợp phù hợp chính xác. Nếu như`w = 2d`hoặc`h = 2d`, kích thước cửa sổ tương ứng bằng không. Một hình chữ nhật có diện tích bằng 0 không phải là một cửa sổ có thể sử dụng được, vì vậy câu trả lời vẫn phải là`0`. Ví dụ,`10 20 5`chiều rộng lá`0`, cho đầu ra`0`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp có thể liệt kê mọi chiều rộng số nguyên từ`1`bởi vì`w - 1`và mọi chiều cao nguyên từ`1`bởi vì`h - 1`. Đối với mỗi cặp, chúng tôi sẽ kiểm tra xem hình chữ nhật có vừa khít hay không trong khi vẫn chừa khoảng trống cần thiết ở mọi cạnh và giữ diện tích hợp lệ lớn nhất. Phương pháp này đúng vì mọi cặp kích thước ứng viên đều được xem xét, do đó cuối cùng phải tìm ra cặp kích thước hợp lệ tốt nhất. 

Với`w, h < 1000`, phép liệt kê đó thực hiện tối đa khoảng`(w - 1)(h - 1)`, dưới một triệu cặp thứ nguyên. Vì vậy, không giống như nhiều vấn đề của Codeforce, sức mạnh vũ phu không thực sự là thảm họa dưới những ràng buộc này. Tuy nhiên, nó đang giải quyết một vấn đề tổng quát hơn mức cần thiết, và nó`O(w h)`công việc là điều có thể tránh được. 

Quan sát quan trọng là việc mở rộng một cửa sổ hợp lệ không bao giờ có thể làm cho diện tích của nó nhỏ hơn. Khi đường viền yêu cầu đã được đặt trước, cửa sổ có thể chiếm mọi đơn vị chiều rộng còn lại và mọi đơn vị chiều cao còn lại. Không có sự tương tác giữa các ràng buộc ngang và dọc, do đó chiều rộng tối ưu và chiều cao tối ưu có thể được xác định một cách độc lập. 

Bức tường mang lại`w`đơn vị theo chiều ngang. Đường viền bên trái tiêu thụ`d`và đường viền bên phải sử dụng một đường viền khác`d`, rời đi`w - 2d`. Theo chiều dọc, đường viền trên và dưới tương tự nhau`h - 2d`. Nếu cả hai đại lượng đều dương thì tích của chúng có diện tích lớn nhất có thể. Nếu một trong hai giá trị không dương thì không tồn tại cửa sổ vùng dương. 

Brute-force hoạt động vì cuối cùng nó kiểm tra các kích thước tối đa, nhưng không khai thác được thực tế là mọi kích thước hợp lệ nhỏ hơn đều bị thống trị bởi kích thước lớn hơn. Việc quan sát thấy rằng cả hai chiều có thể được đẩy lên giá trị cực đại của chúng một cách đơn giản sẽ làm giảm toàn bộ bài toán về số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(w h) | O(1) | Được chấp nhận theo giới hạn nhất định, nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chiều rộng của bức tường`w`, chiều cao tường`h`và khoảng cách biên giới yêu cầu`d`. 
2. Tính chiều rộng cửa sổ tối đa có thể là`w - 2d`. Phép trừ là bằng`2d`vì cửa sổ phải rời đi`d`đơn vị ở cả bên trái và bên phải. 
3. Tính chiều cao cửa sổ tối đa có thể là`h - 2d`. Lập luận tương tự áp dụng cho mặt trên và mặt dưới. 
4. Kiểm tra xem thứ nguyên khả dụng có nhỏ hơn hoặc bằng 0 hay không. Nếu vậy, xuất`0`, vì chiều rộng hoặc chiều cao không dương không thể tạo thành cửa sổ hình chữ nhật có diện tích dương. 
5. Nếu không, nhân chiều rộng và chiều cao có sẵn và xuất sản phẩm. Việc chọn giá trị khả dụng tối đa theo một trong hai hướng không thể làm tổn hại đến khu vực, vì vậy hai chiều này kết hợp với nhau sẽ mang lại cửa sổ lớn nhất có thể. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ cửa sổ hợp lệ nào. Bởi vì ít nhất nó phải để lại`d`đơn vị giữa chu vi của nó và mỗi cạnh tường, chiều rộng của nó không bao giờ vượt quá`w - 2d`và chiều cao của nó không bao giờ vượt quá`h - 2d`. Khi cả hai đại lượng đều dương, một cửa sổ có chính xác các kích thước đó sẽ khớp bằng cách đặt khoảng cách cần thiết ở mỗi bên, do đó không có cửa sổ hợp lệ nào có thể có chiều rộng hoặc chiều cao lớn hơn. Vì diện tích là tích của chiều rộng và chiều cao nên`(w - 2d)(h - 2d)`là diện tích lớn nhất có thể. Khi một trong hai chiều không dương, không có hình chữ nhật có diện tích dương nào có thể đáp ứng yêu cầu về đường viền, do đó trả về số 0 là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

w, h, d = map(int, input().split())

window_w = w - 2 * d
window_h = h - 2 * d

if window_w <= 0 or window_h <= 0:
    print(0)
else:
    print(window_w * window_h)
```Dòng đầu tiên đọc ba số nguyên từ dòng đầu vào duy nhất. Không có trường hợp kiểm thử nào để lặp lại nên chương trình thực hiện tính toán chính xác một lần. 

Hai bài tập tiếp theo chuyển hình học trực tiếp thành số học.`2 * d`được sử dụng thay vì`d`vì đường viền được yêu cầu xuất hiện ở cả hai phía đối diện của mỗi chiều. 

Điều kiện sử dụng`<= 0`, không`< 0`. Khi kích thước còn lại chính xác bằng 0, hình chữ nhật thu được có diện tích bằng 0 và không phải là cửa sổ vùng dương hợp lệ. 

Các số nguyên Python không bị tràn và các giới hạn đã cho làm cho diện tích kết quả dù sao cũng rất nhỏ. Giải pháp cũng chỉ sử dụng một lượng bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`40 25 5`. Khoảng cách cần thiết sẽ loại bỏ`5`đơn vị từ mỗi bên. 

| Bước |`w`|`h`|`d`|`window_w`|`window_h`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 40 | 25 | 5 | | | | 
| Tính kích thước | 40 | 25 | 5 | 30 | 15 | | 
| Kiểm tra tính khả thi | 40 | 25 | 5 | 30 | 15 | hợp lệ | 
| Khu vực tính toán | 40 | 25 | 5 | 30 | 15 | 450 | 

Cả hai chiều còn lại đều dương nên toàn bộ`30 × 15`nội thất có thể được sử dụng. Đầu ra là`450`, phù hợp với mẫu 

Đối với Mẫu 2, hãy xem xét`30 20 12`. 

| Bước |`w`|`h`|`d`|`window_w`|`window_h`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 30 | 20 | 12 | | | | 
| Tính kích thước | 30 | 20 | 12 | 6 | -4 | | 
| Kiểm tra tính khả thi | 30 | 20 | 12 | 6 | -4 | không hợp lệ | 
| Đầu ra | 30 | 20 | 12 | 6 | -4 | 0 | 

Chiều ngang vẫn còn chỗ, nhưng chiều dọc thì không. Một thứ nguyên không hợp lệ là đủ để làm cho toàn bộ cửa sổ không thể thực hiện được, do đó thuật toán trả về`0`. 

Những dấu vết này chứng minh tại sao phải kiểm tra tính khả thi trước khi nhân các kích thước. Bản thân phép nhân không có ý nghĩa hình học khi một trong các chiều không dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện hai phép trừ, một phép so sánh và nhiều nhất một phép nhân | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ | 

Giới hạn chính thức dưới đây`1000`cho kích thước tường và bên dưới`100`đối với khoảng trống, do đó giải pháp thời gian không đổi thấp hơn nhiều so với yêu cầu về giới hạn một giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    w, h, d = map(int, input().split())

    window_w = w - 2 * d
    window_h = h - 2 * d

    if window_w <= 0 or window_h <= 0:
        print(0)
    else:
        print(window_w * window_h)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
assert run("40 25 5\n") == "450\n", "sample 1"
assert run("30 20 12\n") == "0\n", "sample 2"
assert run("40 25 50\n") == "0\n", "sample 3"
assert run("999 888 7\n") == "860890\n", "sample 4"

# Minimum-size dimensions
assert run("1 1 1\n") == "0\n", "no room for a window"

# Exact-fit boundary
assert run("10 20 5\n") == "0\n", "zero remaining width"

# All dimensions comfortably fit
assert run("100 100 10\n") == "6400\n", "all-equal wall dimensions"

# Maximum-size input values
assert run("999 999 99\n") == "641601\n", "maximum bounds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`0`| Kích thước tối thiểu và không thể cài đặt | 
|`10 20 5`|`0`| Ranh giới có chiều rộng bằng 0 chính xác | 
|`100 100 10`|`6400`| Kích thước đối xứng và trường hợp hợp lệ thông thường | 
|`999 999 99`|`641601`| Giới hạn đầu vào tối đa và số học | 

## Vỏ cạnh 

cho`30 20 12`, thuật toán tính toán`window_w = 6`Và`window_h = -4`. Từ`window_h <= 0`, nó ngay lập tức xuất ra`0`. Điều này giúp sản phẩm tiêu cực không bị nhầm lẫn với một khu vực. 

Vì`40 25 50`, kích thước có sẵn là`-60`Và`-75`. Việc kiểm tra tính khả thi tương tự trả về`0`. Điều này nắm bắt các triển khai giả định đầu vào luôn đủ chỗ chỉ vì`w`Và`h`bản thân họ là tích cực. 

Đối với trường hợp phù hợp chính xác`10 20 5`, chiều rộng có sẵn là`10 - 2(5) = 0`. Thuật toán coi số 0 là không thể và kết quả đầu ra`0`, thay vì chấp nhận một hình chữ nhật suy biến. 

Đối với một trường hợp hợp lệ bình thường như`40 25 5`, kích thước có sẵn là`30`Và`15`. Cả hai đều dương nên thuật toán nhân chúng và thu được`450`. Phép tính sử dụng mọi phần tường được phép thuộc về cửa sổ, điều này chứng tỏ không có ứng cử viên nào nhỏ hơn có thể là tối ưu.
