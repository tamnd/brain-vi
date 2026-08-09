---
title: "CF 102443A - Hoa hấp dẫn"
description: "Đối với mỗi loại hoa, bó hoa có thể chứa một số lượng hoa thuộc loại đó và bất cứ khi nào một loại được sử dụng, số lượng được chọn của nó phải là số lẻ. Chúng tôi muốn tổng số hoa lớn nhất có thể."
date: "2026-08-09T01:41:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "A"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 273
verified: true
draft: false
---

[CF 102443A - Những bông hoa hấp dẫn](https://codeforces.com/problemset/problem/102443/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi loại hoa, bó hoa có thể chứa một số lượng hoa thuộc loại đó và bất cứ khi nào một loại được sử dụng, số lượng được chọn của nó phải là số lẻ. Chúng tôi muốn tổng số hoa lớn nhất có thể. Một loại hoàn toàn không nhất thiết phải xuất hiện trong bó hoa, vì vậy sự đóng góp của nó có thể bằng không. 

Đối với một loại với`a_i`hoa có sẵn, số lẻ đóng góp tốt nhất có thể chỉ đơn giản là số lẻ lớn nhất không vượt quá`a_i`. Nếu như`a_i`đã lẻ rồi, chúng ta có thể sử dụng tất cả chúng. Nếu như`a_i`chẵn thì phải bỏ lại một bông hoa, như vậy số tiền đóng góp sẽ trở thành`a_i - 1`. 

Sau khi thực hiện điều chỉnh này một cách độc lập cho từng loại, mọi đóng góp khác 0 đều là số lẻ. Tính chẵn lẻ của tổng bây giờ chỉ phụ thuộc vào số lượng loại chúng ta sử dụng. Số lẻ các giá trị lẻ có tổng lẻ, trong khi số chẵn các giá trị lẻ có tổng chẵn. Theo đó, nếu`n`thật kỳ quặc, chúng ta có thể giữ mọi loại. Nếu như`n`là số chẵn, chúng ta phải bỏ đi hoàn toàn một loại. Để mất ít nhất có thể, chúng tôi bỏ qua loại có đóng góp được điều chỉnh nhỏ nhất. 

Ràng buộc`n <= 100000`có nghĩa là giải pháp sẽ xử lý mảng theo thời gian gần như tuyến tính. MỘT`O(n^2)`phương pháp có thể thực hiện xung quanh`10^10`hoạt động ở giới hạn trên, vượt xa giới hạn một giây. Thậm chí một`O(n log n)`phương pháp này không cần thiết ở đây vì câu trả lời chỉ cần một tổng và một giá trị tối thiểu. Các giá trị riêng lẻ thỏa mãn`a_i <= 1000`, vì vậy câu trả lời dễ dàng khớp với số nguyên 32 bit tiêu chuẩn, mặc dù số nguyên Python cũng xử lý nó một cách tự nhiên. 

Có một số trường hợp đặc biệt có thể đánh lừa việc triển khai chỉ tập trung vào việc làm cho mỗi số được chọn trở thành số lẻ. Ví dụ, với một loại,```
1
2
```bó hoa hợp lệ lớn nhất chứa`1`hoa, không`2`, vì số đếm duy nhất có thể sử dụng phải là số lẻ. 

Khi số lượng loại là chẵn, chúng ta phải loại bỏ toàn bộ một loại. Ví dụ,```
2
2 3
```trở thành số lượng được điều chỉnh`1`Và`3`. Giữ cả hai mang lại`4`, là số chẵn nên đáp án đúng là`3`. Một giải pháp bất cẩn chỉ tính tổng số lẻ lớn nhất cho mỗi loại sẽ tạo ra`4`. 

Loại chúng tôi loại bỏ phải được chọn sau khi điều chỉnh số lượng có sẵn của nó thành giá trị lẻ. Ví dụ,```
4
2 7 4 9
```trở thành`1, 7, 3, 9`. Vì có bốn loại nên phải bỏ đi một loại và loại bỏ phần đóng góp`1`đưa ra câu trả lời tối đa`19`. Chọn một loại dựa trên bản gốc của nó`a_i`không xem xét điều chỉnh lẻ có thể đưa ra lựa chọn sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể xem xét mọi tập hợp con của các loại hoa. Đối với mỗi tập hợp con, chúng tôi sẽ lấy số lẻ lớn nhất có thể từ mọi loại đã chọn, tính tổng các đóng góp đó, kiểm tra xem số loại đã chọn có phải là số lẻ hay không và giữ tổng số hợp lệ lớn nhất. Điều này đúng vì sau khi cố định tập hợp các loại đã chọn, việc lấy ít hoa hơn từ bất kỳ loại đã chọn nào chỉ có thể làm cho bó hoa nhỏ hơn. 

Vấn đề là số lượng tập hợp con. Với`n`có những loại`2^n`tập hợp con có thể. Tại`n = 100000`, đây là về`10^30103`khả năng xảy ra, do đó ngay cả việc kiểm tra một tập hợp con trong thời gian không đổi cũng sẽ vô vọng. Lực lượng vũ phu hoạt động vì nó khám phá rõ ràng mọi bộ sưu tập loại có thể có, nhưng nó không thành công khi số lượng loại trở nên lớn. 

Quan sát quan trọng loại bỏ gần như tất cả những lựa chọn đó. Đối với mỗi loại, không bao giờ có lý do để lấy bất cứ thứ gì ngoại trừ số lẻ lớn nhất hiện có của nó. Sau lần điều chỉnh đó, mỗi loại được sử dụng đều đóng góp một số lẻ. Vì vậy, quyết định duy nhất còn lại là sử dụng bao nhiêu loại. Nếu như`n`là số lẻ, sử dụng tất cả các loại đã cho tổng số lẻ. Nếu như`n`là số chẵn, sử dụng tất cả các loại sẽ cho tổng số chẵn, vì vậy phải bỏ đi chính xác một loại. Vì tất cả các đóng góp được điều chỉnh đều là dương nên việc bỏ qua nhiều loại sẽ chỉ làm cho câu trả lời nhỏ hơn. Loại được bỏ qua tốt nhất chỉ đơn giản là loại có đóng góp được điều chỉnh nhỏ nhất. 

Điều này làm giảm toàn bộ vấn đề xuống còn một lần duyệt qua mảng, duy trì tổng số số lẻ đã điều chỉnh và mức tối thiểu của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n * n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(1)`thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`số lượng hoa có sẵn và khởi tạo tổng số tiền cũng như mức đóng góp được điều chỉnh tối thiểu. 
2. Đối với mọi`a_i`, chuyển đổi nó thành số lẻ lớn nhất không vượt quá nó. Nếu như`a_i`là số chẵn, trừ đi một. Thêm giá trị đã điều chỉnh này vào tổng và cập nhật giá trị tối thiểu. 

Điều này là tối ưu cho một loại cá nhân vì việc giảm khoản đóng góp lẻ bằng bất kỳ số tiền dương nào sẽ làm cho nó không hợp lệ hoặc làm cho bó hoa nhỏ hơn. 
3. Nếu`n`là số lẻ, tổng số giữ nguyên. Mọi khoản đóng góp được điều chỉnh đều là số lẻ và có số lẻ trong số đó, nên tổng của chúng là số lẻ. 
4. Nếu`n`là số chẵn, trừ đi phần đóng góp đã điều chỉnh nhỏ nhất từ ​​tổng số. Điều này có hiệu quả loại bỏ toàn bộ loại hoa, để lại`n - 1`các loại đã qua sử dụng. Từ`n - 1`là số lẻ thì tổng kết quả là số lẻ. 

Loại bỏ chính xác một loại là tối ưu vì tất cả các đóng góp đã điều chỉnh đều tích cực và trong số tất cả các loại đơn lẻ, loại bỏ loại nhỏ nhất sẽ mất ít hoa nhất. 
5. In tổng kết quả. 

### Tại sao nó hoạt động 

Sau khi điều chỉnh, mọi loại mà chúng tôi có thể đưa vào đều đóng góp một số lẻ dương và đó là mức đóng góp hợp lệ tối đa cho loại đó. Do đó, bất kỳ bó hoa tối ưu nào đều sử dụng mức đóng góp lẻ tối đa cho mọi loại mà nó bao gồm. 

Tính chẵn lẻ của tổng các số lẻ được xác định bởi số số hạng. Khi`n`là số lẻ, tất cả các khoản đóng góp đã điều chỉnh có thể được sử dụng và tổng số sẽ tự động là số lẻ. Khi`n`là số chẵn, việc sử dụng tất cả các đóng góp sẽ tạo ra một tổng chẵn, do đó tổng số lẻ yêu cầu một số lẻ các loại được sử dụng. Số lớn nhất như vậy là`n - 1`, nghĩa là chính xác một loại nên được loại bỏ. Vì mọi đóng góp được điều chỉnh đều dương nên việc loại bỏ nhiều loại hơn không thể cải thiện câu trả lời và việc loại bỏ đóng góp nhỏ nhất sẽ mang lại số tiền lớn nhất còn lại. Do đó, thuật toán luôn xây dựng một bó hoa tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    total = 0
    mn = 10**18

    for x in a:
        if x % 2 == 0:
            x -= 1

        total += x
        mn = min(mn, x)

    if n % 2 == 0:
        total -= mn

    print(total)

if __name__ == "__main__":
    solve()
```Vòng lặp thực hiện hai bước thuật toán đầu tiên cùng nhau. Thậm chí`x`, trừ đi một sẽ thay đổi nó thành giá trị lẻ lớn nhất nhiều nhất`x`. Đối với một điều kỳ lạ`x`, không có gì thay đổi. Giá trị điều chỉnh sau đó được thêm vào`total`, trong khi`mn`ghi lại loại rẻ nhất để loại bỏ nếu số lượng loại là số chẵn. 

Việc kiểm tra tính chẵn lẻ sử dụng`n`, không`total`. Khi mọi khoản đóng góp được điều chỉnh là số lẻ, tính chẵn lẻ của tổng của chúng hoàn toàn được xác định bởi số lượng đóng góp. Số lẻ trong số chúng cho ra tổng lẻ, trong khi số chẵn cho tổng chẵn. 

Khi`n`là chẵn,`mn`được đảm bảo tồn tại vì`n >= 1`và mọi giá trị được điều chỉnh ít nhất là`1`. Trừ`mn`loại bỏ toàn bộ sự đóng góp của một loại. Không có vấn đề riêng biệt nào trong trường hợp chẵn vì số loại được sử dụng còn lại chính xác là`n - 1`. 

Kiểu số nguyên của Python không bị tràn và câu trả lời lớn nhất có thể nằm ở bên dưới`100000 * 999`, do đó, ngay cả số nguyên có dấu 32 bit cũng đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
3
3 5 8
```Sự đóng góp điều chỉnh của từng loại được lấy một cách độc lập. 

| Loại | Có sẵn | Số lẻ đã điều chỉnh | Tổng số sau loại | Tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 3 | 
| 2 | 5 | 5 | 8 | 3 | 
| 3 | 8 | 7 | 15 | 3 | 

Có ba loại, đó là số lẻ nên không cần loại bỏ loại nào. Câu trả lời là`15`. 

Điều này thể hiện trường hợp cơ bản trong đó mọi loại có thể đóng góp số lẻ lớn nhất của nó và số thuật ngữ thu được đã có tính chẵn lẻ được yêu cầu. 

### Mẫu 2 

Đầu vào là:```
3
1 1 1
```Cả ba giá trị đều đã là số lẻ. 

| Loại | Có sẵn | Số lẻ đã điều chỉnh | Tổng số sau loại | Tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 1 | 
| 3 | 1 | 1 | 3 | 1 | 

Có ba loại nên tổng số còn lại`3`. Không điều chỉnh tính chẵn lẻ của`n`là cần thiết. 

Ví dụ này xác nhận rằng thuật toán không loại bỏ các bông hoa một cách không cần thiết khi tất cả các giá trị đã ở mức số lẻ nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi loại hoa được xử lý chính xác một lần. | 
| Không gian |`O(n)`đối với mảng đầu vào,`O(1)`thêm | Thuật toán lưu trữ các giá trị đầu vào và chỉ duy trì tổng và giá trị tối thiểu trong khi xử lý chúng. | 

Với`n <= 100000`, quá trình quét tuyến tính chỉ thực hiện khoảng một trăm nghìn lần lặp, thoải mái trong giới hạn một giây. Việc sử dụng bộ nhớ cũng nhỏ so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    total = 0
    mn = 10**18

    for x in a:
        if x % 2 == 0:
            x -= 1
        total += x
        mn = min(mn, x)

    if n % 2 == 0:
        total -= mn

    print(total)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("3\n3 5 8\n") == "15", "sample 1"
assert run("3\n1 1 1\n") == "3", "sample 2"

# Minimum-size input
assert run("1\n1\n") == "1", "single type with one flower"

# Even number of types, smallest adjusted value must be removed
assert run("2\n2 3\n") == "3", "remove adjusted contribution 1"

# All equal values with an even number of types
assert run("4\n7 7 7 7\n") == "21", "remove one 7"

# Boundary case with an even value of 1000
assert run("2\n1 1000\n") == "999", "1000 becomes 999 and type 1 is removed"

# Maximum-size case
assert run("100000\n" + "1000 " * 100000 + "\n") == "99899001", "maximum n and ai"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| tối thiểu`n`và đóng góp nhỏ nhất có thể | 
|`2 / 2 3`|`3`| Số loại chẵn và loại bỏ giá trị điều chỉnh tối thiểu | 
|`4 / 7 7 7 7`|`21`| Tất cả các giá trị bằng nhau và thậm chí`n`| 
|`2 / 1 1000`|`999`| Chuyển đổi giá trị biên chẵn`1000`ĐẾN`999`| 
|`100000 / all 1000`|`99899001`| Kích thước đầu vào tối đa và xử lý tuyến tính | 

## Vỏ cạnh 

Với một loại duy nhất, không có sự điều chỉnh chẵn lẻ nào liên quan đến các loại bị bỏ qua vì`n`đã là kỳ quặc rồi. Vì```
1
2
```giá trị`2`được giảm xuống`1`, đưa ra câu trả lời`1`. Lấy cả hai bông hoa sẽ vi phạm yêu cầu số lượng được chọn cho loại đó là số lẻ. 

Khi số lượng loại là chẵn, việc giữ nguyên mọi đóng góp đã điều chỉnh luôn tạo ra tổng số chẵn. Coi như```
2
2 3
```Các giá trị được điều chỉnh là`1`Và`3`, tổng của nó là`4`. Thuật toán tìm mức đóng góp tối thiểu`1`và trừ đi, để lại`3`. Bó hoa kết quả sử dụng một loại, có số lượng hoa lẻ và tối đa. 

Khoản đóng góp được điều chỉnh tối thiểu có thể đến từ loại có số lượng ban đầu là số chẵn. Vì```
4
2 7 4 9
```các giá trị được điều chỉnh là`1, 7, 3, 9`. Nhỏ nhất là`1`, do đó thuật toán sẽ loại bỏ loại đó và trả về`19`. Chỉ nhìn vào các giá trị ban đầu có thể gợi ý không chính xác rằng`2`không phải là một ứng cử viên có ý nghĩa vì nó chẵn, nhưng sau khi thực thi yêu cầu đếm số lẻ, đóng góp thực sự của nó là`1`. 

Cuối cùng, số lượng lớn nhất có sẵn là chính nó ngay cả trong trường hợp ranh giới`a_i = 1000`. Vì```
2
1 1000
```các khoản đóng góp được điều chỉnh là`1`Và`999`. Vì có hai loại nên một loại phải được bỏ qua và loại bỏ phần đóng góp`1`cho`999`. Điều này kiểm tra cả chuyển đổi giá trị chẵn và logic loại bỏ tối thiểu ở mức tối đa được phép`a_i`.
