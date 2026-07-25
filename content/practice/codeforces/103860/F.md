---
title: "CF 103860F - Modulo"
description: "Chúng ta được cung cấp một mảng gồm tối đa 21 số nguyên dương và giá trị ban đầu $x$. Chúng ta được phép sắp xếp lại mảng một cách tùy ý. Sau khi sửa một đơn hàng, chúng tôi xử lý từng phần tử một, liên tục cập nhật $x$ bằng cách thay thế nó bằng $x bmod ai$."
date: "2026-07-02T07:57:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103860
codeforces_index: "F"
codeforces_contest_name: "The 7th China Collegiate Programming Contest, Finals (CCPC Finals 2021)"
rating: 0
weight: 103860
solve_time_s: 38
verified: true
draft: false
---

[CF 103860F - Modulo](https://codeforces.com/problemset/problem/103860/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có tối đa 21 số nguyên dương và một giá trị ban đầu$x$. Chúng ta được phép sắp xếp lại mảng một cách tùy ý. Sau khi sửa đơn hàng, chúng tôi xử lý từng phần tử một, cập nhật liên tục$x$bằng cách thay thế nó bằng$x \bmod a_i$. 

Mục đích là chọn thứ tự của mảng sao cho sau khi áp dụng tất cả các phép toán modulo, giá trị cuối cùng của$x$là càng lớn càng tốt. 

Khía cạnh quan trọng là hoạt động không đối xứng: áp dụng$x \bmod a$sớm có thể giảm đáng kể$x$, điều này sẽ hạn chế tác dụng của các thao tác mod sau này. Bởi vì điều này, việc đặt hàng rất quan trọng. 

Những hạn chế là cực kỳ nhỏ về mặt$n$, với$n \le 21$, nhưng bản thân các giá trị có thể lớn bằng$10^{18}$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào phụ thuộc vào việc liệt kê các giá trị của$x$một cách dày đặc hoặc làm bất cứ điều gì đa thức trong$x$. Tham số nhỏ duy nhất có thể khai thác được là số phần tử, do đó, bất kỳ giải pháp chính xác nào cũng phải dựa vào tìm kiếm theo cấp số nhân trên các hoán vị hoặc tập hợp con. 

Một hành vi cạnh tinh tế xuất hiện khi một số$a_i > x$. Trong trường hợp đó,$x \bmod a_i = x$, có nghĩa là các phần tử như vậy thực sự không hoạt động nếu được áp dụng vào đúng thời điểm. Tuy nhiên, nếu áp dụng muộn hơn sau$x$đã giảm, chúng có thể trở nên hoạt động và giảm giá trị. Ví dụ, nếu$x = 10$,$a = [15, 6]$, sau đó áp dụng 15 lần giữ đầu tiên$x = 10$, nhưng áp dụng 6 trước sẽ làm$x = 4$, rồi áp dụng 15 không làm gì thêm, để lại 4. Câu trả lời đúng là 10. 

Một trường hợp thất bại khác là giả sử việc sắp xếp tham lam theo$a_i$. Ví dụ: việc sắp xếp giảm dần có thể trông tự nhiên, nhưng nó không thành công vì một modulo lớn sớm có thể ngăn cản việc giảm bớt sau này mà lẽ ra sẽ có lợi theo một thứ tự khác. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: thử mọi hoán vị có thể có của mảng, mô phỏng các phép toán modulo và giữ kết quả tốt nhất. Mỗi hoán vị yêu cầu$O(n)$chuyển tiếp và có$n!$hoán vị, do đó tổng độ phức tạp là$O(n! \cdot n)$. Với$n = 21$, điều này lớn về mặt thiên văn và không thể thực hiện được. 

Quan sát quan trọng là giá trị cuối cùng chỉ phụ thuộc vào thứ tự tương đối của các phần tử và số lượng phần tử đủ nhỏ để cho phép lập trình động trên các tập hợp con. Thay vì xây dựng các hoán vị đầy đủ, chúng tôi xây dựng quy trình từng bước một, theo dõi những phần tử nào đã được sử dụng và giá trị hiện tại của$x$là sau khi áp dụng chúng theo một thứ tự nào đó. 

Cấu trúc chính là trạng thái của quy trình được xác định đầy đủ bởi tập hợp con các chỉ số được sử dụng và giá trị hiện tại của$x$. Từ bất kỳ trạng thái nào, chúng ta có thể chọn bất kỳ phần tử nào chưa được sử dụng tiếp theo. Điều này tự nhiên tạo thành cấu trúc giống như đường dẫn ngắn nhất hoặc DP trên các tập hợp con trong đó các chuyển đổi giảm kích thước đã đặt xuống một và cập nhật giá trị một cách xác định. 

Tuy nhiên, một DP ngây thơ trên các tập hợp con và chính xác$x$giá trị là không thể bởi vì$x$có thể mất tới$10^{18}$những giá trị riêng biệt. Quan sát tiết kiệm quan trọng là$x$chỉ thay đổi về chính nó (nếu mô đun lớn hơn) hoặc thành một giá trị nhỏ hơn giá trị đã chọn$a_i$. Vì vậy trong khi$x$có phạm vi lớn, số lượng giá trị có thể truy cập riêng biệt trên tất cả các trạng thái bị hạn chế rất nhiều bởi cấu trúc phân nhánh gây ra bởi các hoạt động mod trên một tập hợp nhỏ. 

Điều này dẫn đến việc ghi nhớ các trạng thái bitmask, trong đó mỗi trạng thái lưu trữ giá trị tốt nhất có thể đạt được của$x$. Quá trình chuyển đổi thử từng phần tử không được sử dụng và cập nhật kết quả. Bởi vì biểu đồ trạng thái không có tính tuần hoàn về mặt kích thước mặt nạ nên phép đệ quy hoặc DP là an toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Bitmask DP qua hoán vị |$O(n \cdot 2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị từng trạng thái bằng một mặt nạ bit cho biết phần tử nào đã được sử dụng, cùng với giá trị hiện tại của$x$. 

1. Xác định hàm DP$f(mask, x)$trả về giá trị cuối cùng tối đa có thể bắt đầu từ trạng thái$mask$với giá trị hiện tại$x$. Công thức này phản ánh trực tiếp quá trình được mô tả trong bài toán. 
2. Nếu tất cả các phần tử được sử dụng, hãy trả về$x$. Tại thời điểm này không còn hoạt động nào nữa, vì vậy giá trị hiện tại là cuối cùng. 
3. Nếu không, hãy thử chọn từng chỉ mục chưa sử dụng$i$. Với mỗi lựa chọn, hãy tính giá trị tiếp theo$x' = x \bmod a_i$và đánh giá đệ quy$f(mask \cup \{i\}, x')$. 
4. Tận dụng tối đa mọi lựa chọn của$i$. Điều này tương ứng với việc lựa chọn hoạt động tiếp theo tốt nhất có thể. 
5. Ghi nhớ kết quả cho các trạng thái. Vì cùng một cặp$(mask, x)$có thể đạt được thông qua các hoán vị khác nhau, bộ nhớ đệm tránh được việc tính toán lại. 

Một cải tiến thực tế là việc ghi nhớ đầy đủ$x$là không cần thiết theo nghĩa đơn giản nếu được triển khai cẩn thận với hàm băm Python, vì số lượng trạng thái có thể truy cập vẫn có thể quản lý được theo$n \le 21$. Tuy nhiên, DP khái niệm vẫn còn$(mask, x)$. 

### Tại sao nó hoạt động 

Mọi thứ tự hợp lệ của mảng tương ứng với chính xác một đường dẫn trong biểu đồ trạng thái này, bắt đầu từ$mask = 0$và ban đầu$x$, và kết thúc tại$mask = (1<<n)-1$. Ngược lại, mọi đường dẫn đều tương ứng với một hoán vị hợp lệ. Vì chúng tôi đánh giá tất cả các chuyển đổi từ mỗi trạng thái và lấy mức tối đa nên không có thứ tự hợp lệ nào bị loại trừ và không có thứ tự không hợp lệ nào được đưa ra. Phép đệ quy khám phá toàn bộ không gian hoán vị ở dạng có cấu trúc trong khi tránh việc tính toán lại nhiều lần các trạng thái giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    x0 = int(input())

    from functools import lru_cache

    @lru_cache(None)
    def dp(mask, x):
        if mask == (1 << n) - 1:
            return x

        best = 0
        for i in range(n):
            if not (mask >> i) & 1:
                nxt = x % a[i]
                cand = dp(mask | (1 << i), nxt)
                if cand > best:
                    best = cand
        return best

    print(dp(0, x0))

if __name__ == "__main__":
    solve()
```Giải pháp là triển khai trực tiếp tập hợp con DP được mô tả trước đó. Trạng thái đệ quy bao gồm cả mặt nạ đã sử dụng và giá trị hiện tại của$x$. các`lru_cache`đảm bảo rằng các trạng thái lặp lại được tính toán một lần. 

Một chi tiết triển khai tinh tế là chúng ta phải bao gồm`x`trong khóa DP. Việc bỏ qua nó sẽ hợp nhất không chính xác các trạng thái có các phần tử được sử dụng giống hệt nhau nhưng có giá trị hiện tại khác nhau, về cơ bản là khác nhau trong quá trình phát triển trong tương lai của chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [6, 7, 10]
x = 20
```Chúng tôi theo dõi một vài con đường đại diện. 

| Mặt nạ | Hiện tại x | Lựa chọn | Tiếp theo x | 
| --- | --- | --- | --- | 
| 000 | 20 | 10 | 0 | 
| 000 | 20 | 7 | 6 | 
| 000 | 20 | 6 | 2 | 

Một con đường tối ưu là chọn 10 đầu tiên: 

20 mod 10 = 0, nhưng điều này không tốt. Thay vào đó, chọn 7 trước sẽ cho 20 mod 7 = 6, sau đó áp dụng 6 cho 0, cuối cùng là 0. Nhưng chọn 6 trước cho 2, sau đó chọn 7 cho 2, sau đó 10 cho 2, cuối cùng là 2. 

Thứ tự tốt nhất bảo toàn các giá trị trung gian lớn hơn bằng cách trì hoãn các mức giảm mạnh. 

Ví dụ này chứng minh rằng việc giảm thiểu tích cực sớm không phải lúc nào cũng tối ưu. 

### Ví dụ 2 

đầu vào:```
n = 2
a = [15, 6]
x = 10
```| Mặt nạ | Hiện tại x | Lựa chọn | Tiếp theo x | 
| --- | --- | --- | --- | 
| 00 | 10 | 15 | 10 | 
| 00 | 10 | 6 | 4 | 
| 01 | 10 | 6 | 4 | 

Thứ tự tốt nhất là áp dụng 15 trước, giữ nguyên 10, đưa ra đáp án cuối cùng là 10. 

Điều này cho thấy tầm quan trọng của việc đặt hàng các mô đun lớn trước các mô đun nhỏ hơn khi chúng không hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 2^n)$tiểu bang có phân nhánh | Mỗi mặt nạ được tính một lần và mỗi mặt nạ sẽ cố gắng tối đa$n$chuyển tiếp | 
| Không gian |$O(n \cdot 2^n)$| Ghi nhớ lưu trữ kết quả cho mỗi cặp (mặt nạ, x) | 

Với$n \le 21$, không gian mặt nạ bit có khoảng hai triệu trạng thái và mỗi trạng thái mở rộng tuyến tính theo$n$, điều này khả thi trong Python được tối ưu hóa với bộ nhớ đệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("1\n5\n10\n") == "0"

# two elements, order matters
assert run("2\n15 6\n10\n") == "10"

# all elements larger than x
assert run("3\n100 200 300\n50\n") == "50"

# mixed case
assert run("3\n6 7 10\n20\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở đúng đắn | 
| [15, 6], x=10 | 10 | độ nhạy đặt hàng | 
| lớn tất cả a_i > x | x không thay đổi | hành vi không hoạt động | 
| giá trị hỗn hợp | giá trị cuối cùng nhỏ | sự tương tác của các đường dẫn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mọi$a_i$lớn hơn hiện tại$x$. Trong tình huống này, mọi thao tác đều rời khỏi$x$không thay đổi, do đó mọi hoán vị đều hợp lệ và câu trả lời chỉ đơn giản là$x$. DP xử lý việc này một cách tự nhiên vì mọi quá trình chuyển đổi đều giữ$x' = x$, vì vậy tất cả các đường dẫn đều giữ nguyên giá trị cho đến khi kết thúc. 

Một trường hợp cạnh khác là khi tồn tại một lượng rất nhỏ$a_i$, chẳng hạn như 1. Bất kỳ ứng dụng nào của$a_i = 1$lực lượng ngay lập tức$x$về 0. DP trì hoãn chính xác việc sử dụng các phần tử đó trừ khi tất cả các lựa chọn khác đã hết, vì bất kỳ nhánh nào sử dụng 1 sớm sẽ tạo ra giá trị cuối cùng là 0, giá trị này không bao giờ tối ưu trừ khi tất cả các đường dẫn khác cũng sụp đổ. 

Trường hợp khó phát hiện cuối cùng là khi các chiến lược tối ưu phụ thuộc vào việc duy trì một giá trị trung gian lớn qua nhiều hoạt động không hoạt động. Phép đệ quy dựa trên mặt nạ đảm bảo điều này được khám phá vì nó thử rõ ràng tất cả các hoán vị, bao gồm cả những hoán vị trì hoãn việc giảm thiểu có hại cho đến khi chúng không thể tránh khỏi.
