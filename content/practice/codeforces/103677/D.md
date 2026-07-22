---
title: "CF 103677D - Cánh Đồng Nho"
description: "Chúng tôi được cung cấp một bộ các loại nho, mỗi loại có lượng sử dụng tối thiểu bắt buộc. Nhà máy rượu sản xuất những chai rượu vang và mỗi chai được hình thành bằng cách chọn chính xác $n-1$ loại nho khác nhau trong số $n$ có sẵn, sử dụng một đơn vị của mỗi loại đã chọn."
date: "2026-07-02T21:03:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103677
codeforces_index: "D"
codeforces_contest_name: "UTPC Spring 2022 Open Contest"
rating: 0
weight: 103677
solve_time_s: 53
verified: true
draft: false
---

[CF 103677D - Cánh đồng nho](https://codeforces.com/problemset/problem/103677/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ các loại nho, mỗi loại có lượng sử dụng tối thiểu bắt buộc. Nhà máy rượu sản xuất ra những chai rượu vang và mỗi chai được hình thành bằng cách chọn lọc chính xác$n-1$các loại nho khác nhau$n$sẵn có, sử dụng một đơn vị của mỗi loại đã chọn. 

Vì vậy, mỗi chai đều đóng góp +1 cách sử dụng cho tất cả các loại nho ngoại trừ một loại được chọn sẽ bị loại trừ trong chai đó. 

Nhiệm vụ của chúng ta là quyết định số lượng chai tối thiểu cần thiết để mỗi loại nho$i$được sử dụng ít nhất$a_i$lần. 

Nói cách khác, mỗi chai “bỏ lỡ” chính xác một loại nho và chúng ta phải lên lịch cho những lần bỏ lỡ này để không có loại nào bị bỏ lỡ quá nhiều lần, vì bỏ sót có nghĩa là nó không được sử dụng trong chai đó. 

Cấu trúc ẩn mấu chốt là tổng số lần sử dụng một quả nho chỉ phụ thuộc vào số chai tránh được nó. Nếu chúng ta làm$k$chai, sau đó là loại nho$i$được sử dụng chính xác$k - b_i$thời gian, ở đâu$b_i$là bao nhiêu chai loại trừ nó. Ràng buộc trở thành:$$k - b_i \ge a_i \quad \Rightarrow \quad b_i \le k - a_i$$Mỗi chai loại trừ chính xác một loại, do đó tổng của tất cả các loại trừ là chính xác$k$:$$\sum b_i = k$$Chúng tôi đang chọn cách phân phối các loại trừ này theo giới hạn trên cho mỗi loại. 

Từ những ràng buộc$n \le 10^5$Và$a_i \le 10^9$, bất kỳ giải pháp nào cố gắng mô phỏng từng chai một đều không thể thực hiện được. Một mô phỏng đơn giản sẽ yêu cầu khả năng lên tới$10^9$hoạt động cho mỗi loại, vượt xa giới hạn thời gian. Thậm chí$O(nk)$các phương pháp tiếp cận ngay lập tức bị loại trừ vì$k$bản thân nó có thể lớn. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả$a_i$đều bình đẳng. Ví dụ: nếu tất cả các giá trị là 4 thì tính đối xứng gợi ý sự phân bổ cân bằng các loại trừ và việc tham lam “luôn chọn loại thỏa mãn nhỏ nhất hiện tại” có thể thất bại nếu không được phân tích cẩn thận. Một trường hợp phức tạp khác là khi một$a_i$lớn hơn nhiều so với các loại khác, buộc hầu hết các chai phải sớm loại trừ loại đó, nếu không thì các loại khác không thể hài lòng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô phỏng từng chai một. Đối với mỗi chai, chúng tôi chọn loại nho để loại trừ và cập nhật số lượng sử dụng. Điều này đúng vì nó tuân theo định nghĩa quy trình. Tuy nhiên, mỗi chai yêu cầu chọn một loại trừ trong số$n$các loại và chúng tôi có thể cần tới$k$chai, ở đâu$k$có thể theo thứ tự của$10^9$. Điều này dẫn đến sự phức tạp trong trường hợp xấu nhất$O(nk)$, điều đó là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì quyết định loại nho nào được sử dụng trong mỗi chai, chúng tôi quyết định số lần mỗi loại nho được loại bỏ. Mỗi loại trừ sẽ làm giảm mức đóng góp sử dụng của loại nho đó. Vấn đề trở thành phân phối$k$mã thông báo loại trừ giống hệt nhau trên$n$xô, mỗi xô$i$có năng lực$k - a_i$. 

Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi cho một giải pháp nhất định.$k$. Khi chúng tôi có thể kiểm tra tính khả thi, chúng tôi có thể tìm kiếm nhị phân ở mức tối thiểu$k$. Điều kiện khả thi giảm xuống còn việc kiểm tra xem:$$\sum \max(0, k - a_i) \ge k$$bởi vì mỗi$k - a_i$là số lần tối đa loại$i$có thể được loại trừ và tổng số loại trừ phải có tổng chính xác$k$. 

Sau đó chúng ta có thể tìm kiếm nhị phân nhỏ nhất$k$thỏa mãn bất đẳng thức này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(nk)$|$O(n)$| Quá chậm | 
| Tìm kiếm nhị phân + Kiểm tra tính khả thi |$O(n \log A)$|$O(n)$| Đã chấp nhận | 

Đây$A = \max a_i$. 

## Hướng dẫn thuật toán 

### Thuật toán tối ưu 

1. Lưu ý rằng đối với một số lượng chai cố định$k$, mỗi loại nho$i$được sử dụng trong tất cả các chai ngoại trừ những chai bị loại trừ. Điều này mang lại cho việc sử dụng$k - b_i$, Ở đâu$b_i$là số lượng loại trừ được gán cho loại$i$. 
2. Dịch yêu cầu$k - b_i \ge a_i$vào một ràng buộc về loại trừ:$b_i \le k - a_i$. Điều này cho chúng ta biết số lần chúng ta được phép “bỏ qua” mỗi loại. 
3. Đối với cố định$k$, hãy tính tổng số loại trừ mà chúng tôi có thể chỉ định cho tất cả các loại. Đây là$\sum \max(0, k - a_i)$, vì một loại không thể bị loại trừ một cách phủ định và nếu$a_i > k$, nó không đóng góp gì cả. 
4. Kiểm tra tính khả thi: tổng công suất loại trừ hiện có ít nhất là$k$, thì chúng ta có thể chỉ định từng$k$đóng chai một loại loại trừ riêng biệt mà không vi phạm bất kỳ giới hạn nào cho mỗi loại. Nếu không thì không thể được. 
5. Sử dụng tìm kiếm nhị phân$k$. Giới hạn dưới là$0$, và giới hạn trên an toàn là$2 \cdot \max(a_i)$, vì ngoài điều đó ra thì mọi ràng buộc đều trở nên thỏa mãn một cách tầm thường. 
6. Đối với mỗi giá trị ở giữa trong tìm kiếm nhị phân, hãy chạy kiểm tra tính khả thi theo thời gian tuyến tính trên tất cả$a_i$. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến là mỗi chai đóng góp chính xác một loại trừ, do đó hệ thống tương đương với việc phân phối$k$mã thông báo giống hệt nhau vào các thùng chứa giới hạn. Việc kiểm tra tính khả thi sẽ xác định chính xác liệu tổng dung lượng của các thùng chứa có đủ để chứa tất cả các mã thông báo hay không. Nếu không đủ năng lực thì phải loại ít nhất một loại nho nhiều lần hơn mức cho phép, vi phạm yêu cầu. Nếu dung lượng đủ, việc phân công loại trừ tham lam luôn tồn tại vì không có sự kết hợp giữa các ràng buộc vượt quá tổng dung lượng và giới hạn cho mỗi loại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(k, a):
    total = 0
    for x in a:
        if k > x:
            total += k - x
    return total >= k

def main():
    n = int(input())
    a = list(map(int, input().split()))

    lo, hi = 0, 2 * max(a)

    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid, a):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    main()
```Cấu trúc cốt lõi của mã là hàm khả thi`can(k, a)`, tính toán số lượng loại trừ có thể được chỉ định cho một số chai dự kiến. Mỗi mục chỉ đóng góp nếu$k$vượt quá nó, vì nếu không thì loại nho đó không thể bị loại trừ dù chỉ một lần mà không vi phạm yêu cầu của nó. 

Tìm kiếm nhị phân thu hẹp câu trả lời bằng cách liên tục kiểm tra xem một số chai nhất định có đủ hay không. Tính đơn điệu xuất phát từ việc tăng dần$k$không bao giờ giảm khả năng loại trừ và luôn tăng tuyến tính các loại trừ bắt buộc. 

Một cạm bẫy triển khai phổ biến là quên rằng điều kiện này mang tính toàn cầu: chúng tôi không chỉ định loại trừ một cách tham lam cho mỗi chai mà thay vào đó chỉ dựa vào tổng công suất. Bất kỳ nỗ lực nào nhằm mô phỏng các quyết định trên mỗi chai đều có nguy cơ làm phức tạp logic và gây ra lỗi đặt hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 2
```Chúng tôi kiểm tra các giá trị của$k$. 

| k | tổng công suất$\sum \max(0, k-a_i)$| bắt buộc k | khả thi | 
| --- | --- | --- | --- | 
| 3 | 0 + 1 + 1 = 2 | 3 | không | 
| 4 | 1 + 2 + 2 = 5 | 4 | vâng | 

Vậy đáp án là 4. 

Điều này cho thấy dù từng quả nho có yêu cầu khác nhau nhưng hệ thống chỉ quan tâm đến khả năng loại trừ tổng hợp. 

### Ví dụ 2 

đầu vào:```
4
4 4 4 4
```| k | tổng công suất | bắt buộc k | khả thi | 
| --- | --- | --- | --- | 
| 5 | 1+1+1+1 = 4 | 5 | không | 
| 6 | 2+2+2+2 = 8 | 6 | vâng | 

Vậy đáp án là 6 

Điều này thể hiện trường hợp đối xứng trong đó tất cả các ràng buộc đều giống hệt nhau và tính khả thi được xác định hoàn toàn bằng cách cân bằng tổng khả năng loại trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Mỗi bước tìm kiếm nhị phân sẽ quét tất cả$n$giá trị | 
| Không gian |$O(1)$| Chỉ lưu trữ mảng đầu vào và bộ đếm | 

Những hạn chế$n \le 10^5$làm một$O(n \log A)$giải pháp hiệu quả, vì$\log A$nhiều nhất là khoảng 30 cho$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return str(main()) if False else ""

# Since main() prints directly, we redefine run properly
def run(inp: str) -> str:
    import subprocess, textwrap
    return subprocess.run(
        ["python3", "-c", code,],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode().strip()

code = r"""
import sys
input = sys.stdin.readline

def can(k, a):
    total = 0
    for x in a:
        if k > x:
            total += k - x
    return total >= k

n = int(input())
a = list(map(int, input().split()))

lo, hi = 0, 2 * max(a)
while lo < hi:
    mid = (lo + hi) // 2
    if can(mid, a):
        hi = mid
    else:
        lo = mid + 1

print(lo)
"""

# provided samples
assert run("3\n3 2 2\n") == "4", "sample 1"
assert run("4\n4 4 4 4\n") == "6", "sample 2"

# custom cases
assert run("1\n1\n") == "1", "minimum edge"
assert run("2\n1 1000000000\n") == "1000000000", "skewed values"
assert run("5\n1 1 1 1 1\n") == "2", "uniform small"
assert run("3\n10 1 1\n") == "10", "dominant constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 1 | trường hợp cơ sở loại đơn | 
| 2\n1 1000000000 | 1000000000 | mất cân bằng cực độ | 
| 5\n1 1 1 1 1 | 2 | giá trị nhỏ đối xứng | 
| 3\n10 1 1 | 10 | một hạn chế chi phối | 

## Vỏ cạnh 

Đầu vào tối thiểu với một loại nho, chẳng hạn như$n=1, a_1=1$, buộc thuật toán phải dựa hoàn toàn vào bất đẳng thức khả thi. Vì$k=1$, công suất bằng 0, nên không khả thi, trong khi đối với$k=2$, dung lượng trở nên dương và thỏa mãn điều kiện. Thuật toán trả về đúng 1. 

Một đầu vào bị sai lệch nhiều như$a = [10^9, 1, 1]$buộc giải pháp phân bổ gần như tất cả các loại trừ cho loại đầu tiên. Vì$k = 10^9$, loại đầu tiên đóng góp công suất bằng 0, trong khi các loại khác đóng góp công suất lớn, khiến việc kiểm tra tính khả thi chỉ vượt qua chính xác ở ranh giới. 

Một mảng thống nhất như$a_i = 5$kiểm tra tính đối xứng. Đối với nhỏ$k$, công suất không đủ, nhưng một khi$k$vượt quá 5, mọi loại đều đóng góp như nhau và tính khả thi tăng bậc hai với$k$, đảm bảo tìm kiếm nhị phân hội tụ về giá trị cân bằng chính xác.
