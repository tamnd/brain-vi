---
title: "CF 102419C - Hai thao tác"
description: "Chúng ta bắt đầu với hai giá trị x = 0 và a = 1 và muốn làm cho x bằng mục tiêu n đã cho bằng cách sử dụng càng ít thao tác càng tốt. Thao tác đầu tiên thêm a hiện tại vào x."
date: "2026-08-16T08:57:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "C"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 295
verified: false
draft: false
---

[CF 102419C - Hai thao tác](https://codeforces.com/problemset/problem/102419/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 55 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai giá trị,`x = 0`Và`a = 1`, và muốn làm`x`tương đương với mục tiêu đã cho`n`sử dụng càng ít thao tác càng tốt. Thao tác đầu tiên thêm dòng điện`a`ĐẾN`x`. Hoạt động thứ hai đầu tiên thay thế`a`bởi hiện tại`x`, sau đó thêm cái mới này`a`ĐẾN`x`, do đó tác dụng của nó là làm biến đổi`(a, x)`vào trong`(x, 2x)`. Nhiệm vụ là xuất ra số lượng thao tác tối thiểu cần thiết cho mọi trường hợp thử nghiệm. Vấn đề ban đầu có tới 50 mục tiêu, nhiều nhất là mỗi mục tiêu`10^9`. 

Sự ràng buộc`n <= 10^9`loại trừ bất cứ điều gì tỷ lệ thuận với`n`mỗi trường hợp thử nghiệm. Ngay cả một mô phỏng đơn giản yêu cầu một thao tác trên mỗi đơn vị cũng sẽ cần tới một tỷ thao tác cho một mục tiêu, vượt xa giới hạn một giây. Chúng ta cần khai thác cấu trúc số học của các phép toán thay vì xây dựng chuỗi một cách rõ ràng. Số lượng ca kiểm thử chỉ có 50, do đó`O(sqrt(n))`hệ số hóa cho mỗi trường hợp là đủ nhanh. 

Có hai trường hợp đáng được quan tâm. Vì`n = 1`, câu trả lời là`1`, vì phép cộng đầu tiên thay đổi`x`từ`0`ĐẾN`1`. Một công thức dựa trên thừa số nguyên tố phải xử lý việc này một cách riêng biệt vì`1`không có thừa số nguyên tố. Vì`n = 2`, câu trả lời là`2`: lần đầu tiên có được`x = 1`, sau đó sử dụng thao tác thứ hai để nhân đôi nó. Việc triển khai bất cẩn chỉ trả về phần đóng góp của hệ số nguyên tố sẽ trả về`1`, vì vậy hằng số bổ sung trong công thức cuối cùng là cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể xử lý mọi trạng thái`(a, x)`như một nút và thử cả hai thao tác từ nó. Vì mọi hoạt động hữu ích đều tăng`x`, tìm kiếm theo chiều rộng cuối cùng sẽ tìm được đường đi ngắn nhất tới`n`. Vấn đề là kích thước của không gian tìm kiếm. Ngay cả khi chúng ta chỉ xem xét các chuỗi thao tác có độ dài tối đa`n`, có thể có`2^n`chuỗi hoạt động khác nhau. Vì`n = 10^9`, điều đó hoàn toàn không thể thực hiện được. Một chiến lược bạo lực thậm chí còn đơn giản hơn khi sử dụng nhiều lần thao tác đầu tiên có thể yêu cầu chính xác`n`hoạt động, vốn cũng đã quá lớn. 

Quan sát hữu ích xuất phát từ thực tế là`a`luôn luôn chia rẽ`x`. Ban đầu`a = 1`chia rẽ`x = 0`, và thêm`a`bảo toàn khả năng chia hết. Hoạt động thứ hai thay đổi`(a, x)`ĐẾN`(x, 2x)`, do đó tính chia hết được bảo toàn lần nữa. 

Giả sử trạng thái hiện tại là`x = ka`. Nếu chúng ta thực hiện thao tác đầu tiên một lần, tỷ lệ sẽ thay đổi từ`k`ĐẾN`k + 1`. Nếu cuối cùng chúng ta sử dụng thao tác thứ hai, giá trị hiện tại của`x`được sao chép vào`a`rồi nhân đôi. Điều này có nghĩa là toàn bộ nhóm thao tác có thể được hiểu là nhân giá trị tích lũy với một hệ số nguyên. 

Quan điểm nhân tố hóa đó là chìa khóa. Viết mục tiêu dưới dạng tích của các thừa số nguyên tố 

[ 
n = p_1p_2\cdots p_k. 
] 

Với mọi thừa số nguyên tố trung gian`p`, chúng ta có thể giới thiệu nó với`p - 1`hoạt động. Thừa số nguyên tố cuối cùng chỉ cần`p - 2`bổ sung sau giai đoạn nhân đôi cuối cùng. Bắt đầu từ`1`tốn hai thao tác để bước vào giai đoạn nhân đôi đầu tiên. Tổng kết quả là 

[ 
2 + \sum_{i=1}^{k-1}(p_i-1) + (p_k-2) 
= 1 + \sum_{i=1}^{k}(p_i-1). 
] 

Biểu thức tương tự là tối ưu, không chỉ đơn thuần là có thể đạt được. Với mọi số nguyên`m >= 2`, xác định 

[ 
S(m)=\tổng (p-1), 
] 

trong đó tổng chạy trên tất cả các thừa số nguyên tố của`m`, với bội số. chúng tôi có`S(m) <= m - 1`. Mỗi hệ số nhân trung gian`m`chi phí ít nhất`m - 1`, trong khi yếu tố đầu tiên có giá cao hơn yếu tố đó một và yếu tố cuối cùng có giá thấp hơn một. Kết hợp những bất bình đẳng này trên tất cả các yếu tố sẽ tạo ra giới hạn dưới của`S(n) + 1`. Việc xây dựng ở trên đạt chính xác giới hạn đó. 

Vì vậy, toàn bộ vấn đề giảm xuống thừa số nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) trong trường hợp xấu nhất | O(2^n) | Quá chậm | 
| Mô phỏng tuyến tính | O(n) | O(1) | Quá chậm | 
| Thừa số nguyên tố | O(sqrt(n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`n = 1`, trở lại`1`. Mục tiêu đạt được chỉ bằng một lần bổ sung ban đầu`a = 1`. 
2. Đặt`answer = 1`. Đây là số hạng không đổi trong công thức`1 + S(n)`. 
3. Yếu tố`n`bằng cách thử các ước số từ`2`hướng lên trong khi`p * p <= n`. Bất cứ khi nào`p`chia rẽ`n`, thêm vào`p - 1`để trả lời và chia`n`qua`p`. Chúng ta lặp lại phép chia vì cùng một số nguyên tố có thể xuất hiện nhiều lần. 
4. Sau vòng lặp, nếu giá trị còn lại của`n`lớn hơn`1`, nó là số nguyên tố. Thêm vào`n - 1`để trả lời. 
5. In câu trả lời có được. 

### Tại sao nó hoạt động 

Xem xét bất kỳ trình tự hợp lệ nào sau thao tác ban đầu vô ích trên`x = 0`đã được gỡ bỏ. Bởi vì`a`luôn luôn chia rẽ`x`, viết trạng thái hiện tại là`x = ka`. Các hoạt động đầu tiên liên tiếp tăng lên`k`bởi một. Khi thao tác thứ hai được sử dụng, giá trị hiện tại của`x`trở thành cái mới`a`, Và`x`trở thành gấp đôi giá trị cũ của nó. Do đó, mỗi phần của chuỗi giữa hai thao tác thứ hai đều đóng góp một hệ số nhân cho giá trị cuối cùng. 

Với mọi số nguyên`m >= 2`, đóng góp thừa số nguyên tố của nó thỏa mãn`S(m) <= m - 1`. Yếu tố trung gian`m`chi phí ít nhất`m - 1`, vậy ít nhất nó tốn kém`S(m)`. Yếu tố ban đầu tốn ít nhất`S(m) + 2`, và yếu tố cuối cùng có giá ít nhất`S(m) - 1`. Việc cộng các giới hạn này trên tất cả các thừa số sẽ mang lại ít nhất`S(n) + 1`hoạt động. 

Ngược lại, lấy hệ số nguyên tố của`n`. Bắt đầu với`x = 1`. Sử dụng thao tác thứ hai để vào trạng thái nhân đôi. Đối với mọi thừa số nguyên tố ngoại trừ thừa số cuối cùng, hãy sử dụng`p - 2`phép cộng tiếp theo là phép toán thứ hai, đưa ra một hệ số`p`với chi phí là`p - 1`. Đối với thừa số nguyên tố cuối cùng, hãy sử dụng`p - 2`bổ sung mà không tăng gấp đôi. Tổng số chính xác là`1 + S(n)`. Do đó công thức vừa có thể đạt được vừa tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n):
    if n == 1:
        return 1

    answer = 1
    p = 2

    while p * p <= n:
        while n % p == 0:
            answer += p - 1
            n //= p
        p += 1

    if n > 1:
        answer += n - 1

    return answer

def main():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        out.append(str(solve_case(n)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`solve_case`hàm xử lý giá trị đặc biệt`1`Đầu tiên. Đối với mọi mục tiêu khác,`answer`bắt đầu lúc`1`, biểu thị số hạng không đổi trong công thức đã được chứng minh. 

Vòng lặp nhân tử sẽ thử mọi ước số có thể có cho đến căn bậc hai của giá trị còn lại hiện tại. Khi tìm thấy số chia, vòng lặp bên trong sẽ loại bỏ mọi lần xuất hiện của số nguyên tố đó và thêm`p - 1`một lần cho mỗi lần xuất hiện. Ví dụ,`12 = 2 * 2 * 3`đóng góp`1 + 1 + 2`. 

Sau vòng lặp, giá trị còn lại lớn hơn`1`không thể có thừa số nhiều nhất là căn bậc hai của nó, nên bản thân nó phải là số nguyên tố. Thêm`n - 1`xử lý thừa số nguyên tố cuối cùng đó. 

sử dụng`p * p <= n`thay vì`p <= sqrt(n)`tránh số học dấu phẩy động và cũng trở nên chặt chẽ hơn khi loại bỏ các thừa số. Số nguyên Python không có vấn đề tràn ở đây và ước số thử lớn nhất chỉ bằng khoảng`31623`. 

## Ví dụ đã hoạt động 

Đối với mục tiêu mẫu`n = 3`, trình tự tối ưu chỉ sử dụng thao tác đầu tiên. Bắt đầu từ`(a, x) = (1, 0)`, mỗi thao tác cộng thêm`1`. 

| Bước | Hoạt động |`a`|`x`| 
| --- | --- | --- | --- | 
| 0 | Bắt đầu | 1 | 0 | 
| 1 | Thêm vào`a`| 1 | 1 | 
| 2 | Thêm vào`a`| 1 | 2 | 
| 3 | Thêm vào`a`| 1 | 3 | 

Việc phân tích thành thừa số nguyên tố chỉ đơn giản là`3`, do đó công thức cho`1 + (3 - 1) = 3`. Dấu vết đến được mục tiêu trong đúng ba thao tác. 

Đối với mục tiêu mẫu`n = 4`, hệ số hóa của nó là`2 * 2`, do đó công thức cho 

[ 
1+(2-1)+(2-1)=3. 
] 

Trình tự sử dụng thao tác thứ hai để khai thác hành vi nhân đôi. 

| Bước | Hoạt động |`a`|`x`| 
| --- | --- | --- | --- | 
| 0 | Bắt đầu | 1 | 0 | 
| 1 | Thêm vào`a`| 1 | 1 | 
| 2 | Thêm vào`a`| 1 | 2 | 
| 3 | Sao chép`x`ĐẾN`a`, sau đó thêm | 2 | 4 | 

Ba thao tác khớp với đầu ra mẫu. Trạng thái sau bước thứ hai có`a = 1`Và`x = 2`, do đó thao tác thứ hai tăng gấp đôi`x`ĐẾN`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(n)) cho mỗi trường hợp thử nghiệm | Phép chia thử kiểm tra các thừa số có thể có đến căn bậc hai | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến số nguyên không đổi | 

Với tối đa 50 trường hợp thử nghiệm và`n <= 10^9`, vòng lặp phân tích nhân tử thực hiện tối đa khoảng`31623`phép chia kiểm tra trường hợp khó. Ngay cả trong tất cả các trường hợp thử nghiệm, con số này chỉ là khoảng 1,6 triệu lượt kiểm tra, khá thoải mái trong giới hạn thời gian một giây. Việc sử dụng bộ nhớ không đổi ngoại trừ bộ đệm đầu ra nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(n):
    if n == 1:
        return 1

    answer = 1
    p = 2

    while p * p <= n:
        while n % p == 0:
            answer += p - 1
            n //= p
        p += 1

    if n > 1:
        answer += n - 1

    return answer

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    t = data[0]

    result = []
    for i in range(1, t + 1):
        result.append(str(solve_case(data[i])))

    return "\n".join(result)

assert run("""4
1
2
3
4
""") == """1
2
3
3""", "sample 1"

assert run("""1
1
""") == "1", "minimum target"

assert run("""4
2
3
4
5
""") == """2
3
3
5""", "small boundary values"

assert run("""1
64
""") == "7", "repeated prime factor"

assert run("""1
1000000000
""") == "46", "maximum target"

assert run("""1
15
""") == "7", "multiple distinct prime factors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Trường hợp tối thiểu đặc biệt | 
|`2, 3, 4, 5`|`2, 3, 3, 5`| Hành vi ranh giới xung quanh một vài giá trị đầu tiên | 
|`64`|`7`| Yếu tố lặp lại`2`, từ`64 = 2^6`| 
|`1000000000`|`46`| Mục tiêu tối đa cho phép, với`10^9 = 2^9 * 5^9`| 
|`15`|`7`| Một số thừa số nguyên tố riêng biệt,`15 = 3 * 5`| 

## Vỏ cạnh 

cho`n = 1`, đầu vào là`1`và thuật toán ngay lập tức trả về`1`. Không thử phân tích thành thừa số nguyên tố vì không có thừa số nguyên tố. Trình tự trực tiếp chỉ đơn giản là`(1,0) -> (1,1)`, vì vậy trường hợp đặc biệt là chính xác chứ không phải là sự thuận tiện khi triển khai. 

Vì`n = 2`, hệ số hóa là`2`. Công thức cho`1 + (2 - 1) = 2`. Trình tự là`(1,0) -> (1,1) -> (1,2)`. Một giải pháp quên đi hằng số`1`sẽ tuyên bố không chính xác rằng một thao tác là đủ. 

Vì`n = 4`, hệ số lặp lại là`2 * 2`. Thuật toán bổ sung`1`hai lần so với câu trả lời ban đầu, tạo ra`3`. Các thao tác tương ứng là`x = 1`, sau đó`x = 2`, thì thao tác thứ hai thay đổi`(a,x) = (1,2)`vào trong`(2,4)`. Điều này phát hiện những sai lầm liên quan đến các thừa số nguyên tố lặp đi lặp lại. 

Vì`n = 15`, các thừa số nguyên tố là`3`Và`5`, cho`1 + 2 + 4 = 7`. Một trình tự tối ưu là`x = 1`, sau đó`x = 2`, sau đó`x = 3`, thì thao tác thứ hai cho`x = 6`, tiếp theo là ba phép cộng sử dụng`a = 3`, sản xuất`9`,`12`, và cuối cùng`15`. Bảy thao tác phù hợp với công thức. 

Vì`n = 10^9`, hệ số hóa là`2^9 * 5^9`. Sự đóng góp là`9(2-1) + 9(5-1) = 9 + 36 = 45`, vậy câu trả lời là`46`. Việc triển khai không lặp lại tối đa`10^9`; nó loại bỏ các thừa số nhỏ và kết thúc ngay lập tức, đó chính xác là lý do tại sao phương pháp phân tích thừa số phù hợp với các ràng buộc.
