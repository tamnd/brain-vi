---
title: "CF 102621K - Chia sẻ con dấu"
description: "Chúng ta có một tờ giấy hình chữ nhật có kích thước a × b và n con dấu hình chữ nhật. Mỗi con dấu tạo ra một hình chữ nhật khi được đặt và nó có thể xoay 90 độ. Chúng ta cần chọn hai con dấu khác nhau và đặt cả hai con dấu đó lên tờ giấy mà không bị chồng lên nhau."
date: "2026-08-01T08:50:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102621
codeforces_index: "K"
codeforces_contest_name: "mBIT Advanced June 2020"
rating: 0
weight: 102621
solve_time_s: 64
verified: true
draft: false
---

[CF 102621K - Chia sẻ con dấu](https://codeforces.com/problemset/problem/102621/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có một tờ giấy hình chữ nhật có kích thước`a × b`Và`n`con dấu hình chữ nhật. Mỗi con dấu tạo ra một hình chữ nhật khi được đặt và nó có thể xoay 90 độ. Chúng ta cần chọn hai con dấu khác nhau và đặt cả hai con dấu đó lên tờ giấy mà không bị chồng lên nhau. Hai hình chữ nhật phải thẳng hàng với các cạnh giấy và mục tiêu là tối đa hóa tổng diện tích được che phủ. 

Đầu vào mô tả kích thước giấy, theo sau là kích thước của mỗi con dấu có sẵn. Đầu ra là tổng diện tích lớn nhất có thể có của hai con dấu mà cả hai con dấu đều có thể được đặt. Nếu không có cặp seal nào có thể khớp với nhau thì câu trả lời là`0`. 

Các hạn chế là nhỏ:`n`,`a`, Và`b`nhiều nhất là`100`. Điều này thay đổi hoàn toàn chiến lược. MỘT`O(n²)`việc liệt kê thực hiện tối đa khoảng mười nghìn kiểm tra cặp, rất nhỏ đối với bất kỳ giới hạn thời gian thông thường nào. Các cách tiếp cận phức tạp hơn sử dụng lập trình động hoặc cấu trúc hình học sẽ làm tăng thêm độ phức tạp không cần thiết. 

Các bẫy chính là do xoay và thực tế là hai hình chữ nhật không cần phải được đặt theo cách sắp xếp ở góc. Chúng có thể được đặt cạnh nhau theo chiều ngang hoặc chiều dọc. 

Ví dụ:```
Input:
2 2 2
1 2
2 1

Output:
4
```Một giải pháp bất cẩn mà bỏ qua việc xoay sẽ loại bỏ con dấu thứ hai, mặc dù việc xoay nó sẽ khiến hai con dấu lấp đầy tờ giấy một cách chính xác. 

Một trường hợp khác là khi một con dấu vừa khít nhưng không con dấu thứ hai nào có thể chia sẻ không gian với nó.```
Input:
3 10 10
6 6
7 7
20 5

Output:
0
```Không thể sử dụng con dấu thứ ba và hai con dấu đầu tiên không thể khớp với nhau vì kích thước tổng hợp của chúng vượt quá tờ giấy trong mọi cách sắp xếp. 

Một lỗi phổ biến cuối cùng là chỉ kiểm tra một hướng của tờ giấy. Một hình chữ nhật có kích thước`x × y`có thể được đặt như`y × x`, vì vậy mỗi cặp ứng cử viên phải xem xét cả hai phép quay. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là thử từng cặp con dấu. Đối với mỗi cặp, chúng tôi quyết định cách xoay mỗi con dấu và kiểm tra xem hai hình chữ nhật có thể được đặt cùng nhau hay không. Nếu hình chữ nhật đầu tiên có kích thước`w1 × h1`và cái thứ hai có kích thước`w2 × h2`, họ có thể chia sẻ bài viết theo hai bố cục cơ bản. Chúng có thể được xếp chồng lên nhau theo chiều dọc, đòi hỏi`w1`Và`w2`để vừa với chiều rộng của giấy và`h1 + h2`để vừa với chiều cao của giấy. Chúng cũng có thể được đặt cạnh nhau, đòi hỏi`w1 + w2`để vừa với chiều rộng và cả hai chiều cao để vừa với chiều cao. 

Ý tưởng vũ phu này đã đủ cho các ràng buộc. Có nhiều nhất`100 × 100`cặp và mỗi cặp chỉ kiểm tra một số lần quay và bố cục không đổi. Tổng công việc chỉ có vài chục nghìn thao tác. 

Quan sát hữu ích là không có lựa chọn vị trí ẩn nào. Vì cả hai con dấu đều là hình chữ nhật thẳng hàng với giấy nên mọi cách sắp xếp hợp lệ đều có thể được chuyển đổi thành một hình chữ nhật trong đó một hình chữ nhật nằm ngay phía trên, bên dưới, bên trái hoặc bên phải của hình chữ nhật kia. Vị trí bên trong tờ giấy không quan trọng, chỉ là chiều rộng và chiều cao kết hợp có thỏa mãn giới hạn hay không. 

Lực lượng vũ phu hoạt động vì số lượng con dấu ít. Sẽ thất bại nếu có hàng trăm nghìn con dấu vì việc kiểm tra tất cả các cặp sẽ trở thành phương trình bậc hai. Ở đây, ý tưởng tương tự là giải pháp dự định vì những ràng buộc cho phép điều đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các kích thước dấu niêm phong và lưu trữ chúng. 
2. Hãy xem xét từng cặp con dấu khác nhau. Chỉ có các cặp mới quan trọng vì câu trả lời cuối cùng phải chứa đúng hai con dấu. 
3. Đối với mỗi cặp, hãy thử cả hai hướng của con dấu thứ nhất và cả hai hướng của con dấu thứ hai. Xoay thay đổi kích thước nhưng không thay đổi diện tích. 
4. Đối với mỗi sự kết hợp hướng, hãy kiểm tra hai bố cục có thể có. Bố cục đầu tiên đặt các con dấu theo chiều dọc, do đó chiều rộng của chúng phải vừa và chiều cao của chúng phải cộng với chiều cao của giấy. Bố cục thứ hai đặt chúng theo chiều ngang, do đó chiều cao của chúng phải vừa và chiều rộng của chúng phải cộng lại trong chiều rộng của giấy. 
5. Nếu cách sắp xếp hợp lệ, hãy cập nhật câu trả lời bằng tổng của hai diện tích hình chữ nhật. 

Lý do điều này bao gồm mọi vị trí có thể là vì hai hình chữ nhật thẳng hàng theo trục không chồng lên nhau luôn có thể được tách biệt dọc theo trục ngang hoặc trục dọc. Sự phân tách đó tương ứng chính xác với hai bố cục được thuật toán kiểm tra. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi lựa chọn có thể có của hai con dấu và mọi khả năng xoay của những con dấu đó. Đối với bất kỳ vị trí hợp lệ nào, hai hình chữ nhật phải có một hình chữ nhật hoàn toàn ở bên trái hoặc bên phải của hình chữ nhật kia hoặc một hình chữ nhật hoàn toàn ở trên hoặc bên dưới hình chữ nhật kia. Việc kiểm tra kích thước tương ứng được thực hiện trực tiếp, vì vậy mọi cặp hợp lệ đều được xem xét. Vì mọi cách sắp xếp được coi là hợp lệ đều đóng góp diện tích của nó đến mức tối đa và các cách sắp xếp không hợp lệ đều bị từ chối, nên câu trả lời cuối cùng chính xác là diện tích được bao phủ tốt nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())
    seals = [tuple(map(int, input().split())) for _ in range(n)]

    ans = 0

    def check(w1, h1, w2, h2):
        if w1 <= a and w2 <= a and h1 + h2 <= b:
            return True
        if h1 <= b and h2 <= b and w1 + w2 <= a:
            return True
        return False

    for i in range(n):
        for j in range(i + 1, n):
            x1, y1 = seals[i]
            x2, y2 = seals[j]

            for w1, h1 in ((x1, y1), (y1, x1)):
                for w2, h2 in ((x2, y2), (y2, x2)):
                    if check(w1, h1, w2, h2):
                        ans = max(ans, x1 * y1 + x2 * y2)

    print(ans)

if __name__ == "__main__":
    solve()
```Các vòng lặp lồng nhau liệt kê từng cặp không có thứ tự chính xác một lần. sử dụng`i + 1`tránh so sánh một con dấu với chính nó và tránh lặp lại cùng một cặp theo thứ tự ngược lại. 

Các vòng quay tạo ra hai hướng có thể có cho mỗi vòng đệm. Việc tính diện tích sử dụng các kích thước ban đầu vì việc xoay hình chữ nhật không làm thay đổi diện tích của nó. 

các`check`hàm chứa điều kiện hình học hoàn chỉnh. Điều kiện đầu tiên thể hiện sự xếp chồng theo chiều dọc, trong khi điều kiện thứ hai thể hiện sự sắp xếp theo chiều ngang. Việc giữ riêng các bước kiểm tra này sẽ tránh được những sai sót khi hoán đổi chiều rộng và chiều cao. 

Số nguyên Python không tràn cho các ràng buộc này vì diện tích tối đa có thể chỉ là`100 × 100 + 100 × 100`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2 2
1 2
2 1
```| Bước | Con dấu đầu tiên | Con dấu thứ hai | Định hướng | Có hiệu lực? | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1×2 | 2×1 | bản gốc | vâng | 4 | 

Con dấu thứ hai phải được xoay để vị trí hoạt động. Thuật toán nhận thấy hai con dấu có thể xếp chồng lên nhau để lấp đầy toàn bộ tờ giấy. 

### Mẫu 2 

đầu vào:```
4 10 9
2 3
1 1
5 10
9 11
```| Bước | Cặp | Định hướng | Vị trí | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 2×3 và 1×1 | đã kiểm tra | hợp lệ | khu 7 | 
| 2 | 2×3 và 5×10 | đã kiểm tra | hợp lệ | khu 56 | 
| 3 | 2×3 và 9×11 | đã kiểm tra | không hợp lệ | bỏ qua | 
| 4 | 1×1 và 5×10 | đã kiểm tra | hợp lệ | khu 51 | 

Cặp hợp lệ lớn nhất là con dấu thứ nhất và thứ ba, cho diện tích`6 + 50 = 56`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cặp vòng đệm đều được thử nghiệm với số lần quay và bố trí không đổi. | 
| Không gian | O(n) | Danh sách kích thước con dấu được lưu trữ. | 

Với`n ≤ 100`, số so sánh bậc hai rất nhỏ nên nghiệm dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

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

assert run("""2 2 2
1 2
2 1
""") == "4\n", "sample 1"

assert run("""4 10 9
2 3
1 1
5 10
9 11
""") == "56\n", "sample 2"

assert run("""3 10 10
6 6
7 7
20 5
""") == "0\n", "sample 3"

assert run("""1 5 5
3 3
""") == "0\n", "only one seal"

assert run("""2 5 5
5 2
2 5
""") == "20\n", "rotation boundary"

assert run("""3 10 10
4 4
4 4
4 4
""") == "32\n", "equal rectangles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chỉ một con dấu | 0 | Một con dấu không thể tạo thành một cặp. | 
| Hai con dấu xoay | 20 | Xử lý xoay và khớp ranh giới chính xác. | 
| Hình chữ nhật bằng nhau | 32 | Kích thước lặp đi lặp lại và liệt kê cặp. | 

## Vỏ cạnh 

Khi cần xoay, thuật toán sẽ thử cả hai hướng một cách rõ ràng. Đối với đầu vào:```
2 2 2
1 2
2 1
```con dấu thứ hai trở thành`1 × 2`sau khi xoay, cho phép cặp này chiếm toàn bộ tờ giấy. Một giải pháp chỉ kiểm tra hướng đã cho sẽ xuất ra không chính xác`0`. 

Khi không có cặp nào phù hợp, mọi sự kết hợp định hướng đều thất bại. Vì:```
3 10 10
6 6
7 7
20 5
```Không thể bố trí hai con dấu đầu tiên vì chiều rộng hoặc chiều cao tổng hợp của chúng quá lớn và con dấu thứ ba không thể sử dụng được. Câu trả lời vẫn còn`0`. 

Khi các kích thước chạm chính xác vào đường viền, thì sự bất bình đẳng phải cho phép sự bình đẳng. Vì:```
2 5 5
5 2
2 5
```các con dấu có thể được đặt cạnh nhau hoặc xếp chồng lên nhau mà không có chỗ trống. Thuật toán sử dụng`<=`, vì vậy nó trả về một cách chính xác`20`thay vì từ chối sự sắp xếp.
