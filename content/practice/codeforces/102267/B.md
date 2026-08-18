---
title: "CF 102267B - Số nguyên tố"
description: "Chúng ta cần chia số nguyên tố (n) đã cho thành hai số nguyên tố (a) và (b) sao cho [ a+b=n. ] Bất kỳ cặp hợp lệ nào cũng được chấp nhận, do đó không cần phải tìm một phân tách cụ thể nếu tồn tại một vài cặp. Nếu không tồn tại cặp như vậy, chúng ta in ra (-1)."
date: "2026-08-17T19:13:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "B"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 280
verified: false
draft: false
---

[CF 102267B - Số nguyên tố](https://codeforces.com/problemset/problem/102267/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chia số nguyên tố (n) đã cho thành hai số nguyên tố (a) và (b) sao cho 

[ 
a+b=n. 
] 

Bất kỳ cặp hợp lệ nào cũng được chấp nhận, do đó không cần phải tìm một phân rã cụ thể nếu tồn tại một vài cặp. Nếu không tồn tại cặp như vậy, chúng ta in ra (-1). 

Giới hạn (n\le 10^7) đủ lớn để việc kiểm tra mọi cặp có thể và liên tục kiểm tra các số về tính nguyên tố là quá tốn kém trong thời gian 1 giây. Tuy nhiên, (10^7) đủ nhỏ nên việc kiểm tra tính nguyên tố bằng cách chia thử sẽ rất rẻ, vì căn bậc hai của nó chỉ bằng khoảng (3162). Thách thức chính không phải là kích thước của (n), mà là nhận ra rằng thực tế rằng bản thân (n) là số nguyên tố đã hạn chế đáng kể những cặp nào có thể xảy ra. 

Có ba tình huống ranh giới dễ gây ra sai lầm. Đối với (n=2), số nguyên tố nhỏ nhất, cách duy nhất để viết nó dưới dạng tổng các số nguyên tố dương sẽ yêu cầu số nguyên tố nhỏ hơn (2), do đó kết quả đúng là`-1`. Với (n=3), cách sử dụng duy nhất của số nguyên tố nhỏ nhất là (2+1), nhưng (1) không phải là số nguyên tố, nên đáp án cũng là`-1`. Việc triển khai bất cẩn coi (1) là số nguyên tố sẽ chấp nhận trường hợp này một cách không chính xác. 

Tính chẵn lẻ của các con số tạo ra một cái bẫy phổ biến khác. Ví dụ, với (n=11), cả hai số nguyên tố đều không thể lẻ vì lẻ cộng lẻ là chẵn. Cặp duy nhất có thể là (2+9), nhưng (9) là hợp số nên kết quả đúng là`-1`. Việc thử các cặp tùy ý mà không sử dụng tính chẵn lẻ có thể lãng phí phần lớn thời gian sẵn có. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu trực tiếp có thể thử mọi (a) có thể từ (2) đến (n-2), đặt (b=n-a) và kiểm tra cả hai số về tính nguyên tố. Phương pháp này đúng vì mọi phân tách có thể có một trong các số nguyên tố của nó trong phạm vi đó, vì vậy cuối cùng mọi cặp ứng cử viên đều được xem xét. Tuy nhiên, nếu tính nguyên tố được kiểm tra bằng phép chia thử thì mỗi phép thử có thể yêu cầu phép chia (O(\sqrt n)). Trong trường hợp xấu nhất, điều này mang lại hiệu quả (O(n\sqrt n)). Tại (n=10^7), đó là theo thứ tự (10^7\cdot3162\approx3.16\times10^{10}) phân chia thử nghiệm, vượt xa những gì phù hợp trong một giây. 

Cấu trúc của đầu vào cho chúng ta khả năng quan sát mạnh mẽ hơn nhiều. Mọi số nguyên tố khác (2) đều là số lẻ. Vì (n) là số nguyên tố nên nó là (2) hoặc là số lẻ. Đối với số nguyên tố lẻ (n), tổng của hai số nguyên tố lẻ sẽ là số chẵn, vì vậy một cặp hợp lệ phải chứa (2). Chỉ có một cặp có thể: 

[ 
2+(n-2)=n. 
] 

Do đó, toàn bộ vấn đề được rút gọn thành việc kiểm tra xem (n-2) có phải là số nguyên tố hay không. Chúng ta không cần phải tìm kiếm các ứng viên, sàng lọc hoặc kiểm tra nhiều cặp. 

Phương pháp brute-force hoạt động vì nó khám phá rõ ràng tất cả các phân tách, nhưng không thành công vì nó thực hiện nhiều kiểm tra tính nguyên tố hơn mức cần thiết. Việc quan sát tính chẵn lẻ làm giảm không gian tìm kiếm từ các cặp ứng cử viên (O(n)) xuống còn đúng một cặp ứng cử viên. Một bài kiểm tra tính nguyên tố phân chia thử nghiệm duy nhất dễ dàng đủ nhanh cho giới hạn nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\sqrt n)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(\sqrt n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên tố (n). Nếu (n=2), in ngay`-1`, vì hai số nguyên tố có ít nhất là (2) nên tổng của chúng ít nhất là (4). 
2. Đối với số nguyên tố lẻ (n), sử dụng tính chẵn lẻ để kết luận rằng một trong hai số nguyên tố cần tìm phải là (2). Giá trị còn lại buộc phải là (n-2), vì vậy không có lý do gì để thử bất kỳ ứng cử viên nào khác. 
3. Kiểm tra xem (n-2) có phải là số nguyên tố hay không bằng phép chia thử. Việc kiểm tra các ước số lên tới (\sqrt{n-2}) là đủ, bởi vì nếu một số tổng hợp có thừa số lớn hơn căn bậc hai của nó thì thừa số tương ứng của nó phải nhỏ hơn căn bậc hai. 
4. Nếu (n-2) là số nguyên tố, in`2 n-2`. Nếu không thì in`-1`, bởi vì mọi cặp hợp lệ cho số nguyên tố lẻ (n) sẽ phải chính xác là cặp này. 

### Tại sao nó hoạt động 

Với mọi số nguyên tố lẻ (n), giả sử (n=a+b) trong đó cả (a) và (b) đều là số nguyên tố. Nếu cả hai đều là số lẻ thì tổng của chúng sẽ là số chẵn, mâu thuẫn với thực tế là (n) là số lẻ. Do đó một trong số chúng phải là số nguyên tố chẵn duy nhất, (2). Do đó, cái còn lại phải bằng (n-2). Thuật toán kiểm tra chính xác ứng viên bắt buộc này và chấp nhận nó một cách chính xác khi (n-2) là số nguyên tố. Với (n=2), không có cặp nào có thể tồn tại vì tổng tối thiểu của hai số nguyên tố là (4). Do đó mọi đầu vào có thể được xử lý chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(x):
    if x < 2:
        return False

    if x % 2 == 0:
        return x == 2

    d = 3
    while d * d <= x:
        if x % d == 0:
            return False
        d += 2

    return True

def solve():
    n = int(input())

    if n == 2:
        print(-1)
        return

    other = n - 2

    if is_prime(other):
        print(2, other)
    else:
        print(-1)

if __name__ == "__main__":
    solve()
```các`is_prime`trước tiên, hàm từ chối các giá trị bên dưới (2), xử lý trường hợp (n=3) vì sau đó`other`bằng (1). Nó cũng xử lý các số chẵn riêng biệt. Sau đó, chỉ cần xem xét các ước số lẻ, do đó vòng lặp bắt đầu ở (3) và tăng thêm (2). 

điều kiện`d * d <= x`là ranh giới chuẩn để chia thử. Các ước số kiểm tra ngoài (\sqrt{x}) không thể phát hiện ra thừa số mới nếu không có thừa số bổ sung nhỏ hơn đã được kiểm tra. Sử dụng phép nhân thay vì tính căn bậc hai dấu phẩy động cũng tránh được những lo ngại về độ chính xác. 

Hàm chính xử lý (n=2) trước khi hình thành ứng cử viên. Đối với mọi đầu vào hợp lệ khác,`other = n - 2`là số nguyên tố thứ hai duy nhất có thể có. Chương trình không bao giờ thực hiện việc liệt kê cặp không cần thiết. 

Số nguyên Python không tràn ở đây và phép nhân lớn nhất trong kiểm tra tính nguyên tố chỉ ở khoảng (3162^2), do đó không có vấn đề gì về số. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, (n=5). Vì (5) là số lẻ nên cặp duy nhất có thể chứa (2), còn lại (5-2=3). 

| (n) | Ứng viên (n-2) | Kiểm tra số chia | Kết quả | 
| --- | --- | --- | --- | 
| 5 | 3 | Không cần thiết ngoài việc kiểm tra đồng đều |`2 3`| 

Ứng viên (3) là số nguyên tố nên cặp bắt buộc là hợp lệ. Dấu vết cho thấy tại sao không có cặp nào khác cần được xem xét. 

Đối với mẫu thứ hai, (n=11). Một lần nữa, tính chẵn lẻ buộc một số nguyên tố phải là (2), để lại (9). 

| (n) | Ứng viên (n-2) | Kiểm tra số chia | Kết quả | 
| --- | --- | --- | --- | 
| 11 | 9 | (3\mid9) |`-1`| 

Số chia (3) được tìm thấy trước khi vòng lặp cần tiếp tục. Vì (9) là hợp số nên cặp duy nhất có thể thất bại, chứng tỏ rằng không có câu trả lời nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\sqrt n)) | Chỉ có một phép kiểm tra tính nguyên tố được thực hiện, kiểm tra các ước số lẻ lên đến (\sqrt{n-2}). | 
| Không gian | (O(1)) | Thuật toán chỉ lưu trữ một số nguyên không đổi. | 

Với (n\le10^7), bài kiểm tra tính nguyên tố kiểm tra tối đa khoảng (3162) giá trị ước số có thể có và trên thực tế chỉ khoảng một nửa trong số đó được kiểm tra vì ngay cả các ước số cũng bị bỏ qua. Điều này thoải mái trong giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def is_prime(x):
    if x < 2:
        return False

    if x % 2 == 0:
        return x == 2

    d = 3
    while d * d <= x:
        if x % d == 0:
            return False
        d += 2

    return True

def solve():
    n = int(input())
    if n == 2:
        print(-1)
        return

    other = n - 2
    if is_prime(other):
        print(2, other)
    else:
        print(-1)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

    return output.getvalue().strip()

assert run("5\n") == "2 3", "sample 1"
assert run("11\n") == "-1", "sample 2"

assert run("2\n") == "-1", "minimum prime"
assert run("3\n") == "-1", "n - 2 equals 1"
assert run("7\n") == "2 5", "smallest successful odd case"
assert run("9999991\n") == "-1", "maximum valid prime input"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`-1`| Đầu vào tối thiểu và ranh giới nguyên tố bằng nhau không thể | 
|`3`|`-1`| Ngăn chặn việc coi (1) là số nguyên tố | 
|`7`|`2 5`| Phân tách thành công nhỏ và ranh giới chính xác (n-2) | 
|`9999991`|`-1`| Gần kích thước đầu vào tối đa và ứng cử viên tổng hợp lớn | 

Số nguyên tố lớn nhất bên dưới (10^7) là (9,999,991), khiến nó trở thành đầu vào thử nghiệm có kích thước tối đa hợp lệ. Ứng cử viên bắt buộc của nó (9,999,989) là hợp số, có thừa số (223) và (44,843), vì vậy câu trả lời dự kiến ​​là`-1`. 

## Vỏ cạnh 

cho`n = 2`, thuật toán sẽ tiến hành kiểm tra ranh giới rõ ràng và in`-1`. Lý do mạnh mẽ hơn việc chỉ đơn giản nói rằng ứng viên thất bại: mỗi số có ít nhất (2) số nguyên tố, nên tổng của chúng không thể là (2). Một giải pháp tính toán mù quáng`n - 2`sẽ thu được (0), đây không phải là số nguyên tố, nhưng việc xử lý trường hợp này một cách rõ ràng sẽ làm cho lý do trở nên rõ ràng. 

Vì`n = 3`, giá trị thứ hai bắt buộc là (3-2=1). Hàm nguyên thủy trả về ngay lập tức`False`vì các giá trị bên dưới (2) không phải là số nguyên tố nên chương trình sẽ in ra`-1`. Điều này mắc phải một lỗi phổ biến khi kiểm tra tính nguyên tố tùy chỉnh bắt đầu vòng chia số của nó ở (2) và vô tình coi (1) là số nguyên tố vì nó không tìm thấy ước số nào. 

Vì`n = 5`, cặp bắt buộc là (2+3). Kiểm tra tính nguyên tố chấp nhận (3) nên chương trình in ra`2 3`. Đây là đầu vào hợp lệ nhỏ nhất tồn tại sự phân tách và xác nhận rằng ranh giới ứng cử viên được xử lý chính xác. 

Vì`n = 11`, cặp bắt buộc là (2+9). Kiểm tra tính nguyên tố kiểm tra (3), thấy rằng (9) chia hết cho (3) và trả về`False`. Chương trình in`-1`. Việc thử một cặp khác cũng không giúp ích được gì, vì hai số nguyên tố lẻ sẽ có tổng bằng một số chẵn, vì vậy (2+9) là cấu trúc duy nhất có thể. 

Để phân tách số nguyên tố bằng nhau, giả sử (a=b=p). Khi đó (n=2p), là số chẵn. Vì đầu vào (n) là số nguyên tố nên khả năng chẵn duy nhất là (n=2), nhưng (2) không thể bằng (2p) với bất kỳ số nguyên tố (p) nào. Do đó không có đầu vào hợp lệ nào có thể có hai số nguyên tố đầu ra bằng nhau. Tổng số nguyên tố bằng nhau tối thiểu theo lý thuyết sẽ là (2+2=4), nhưng (4) không phải là đầu vào được phép vì nó không phải là số nguyên tố. 

Đối với đầu vào hợp lệ lớn nhất (n=9,999,991), thuật toán vẫn chỉ thực hiện một thử nghiệm nguyên tố, lần này là trên (9,999,989). Ứng cử viên đó chia hết cho (223), vì vậy việc kiểm tra sẽ dừng ngay khi tìm thấy thừa số đó và chương trình xuất ra`-1`. Kích thước của (n) không làm thay đổi chiến lược cơ bản, vì số lượng cặp ứng cử viên vẫn chính xác là một.
