---
title: "CF 102267I - Đội quân tối thượng"
description: "Đầu vào mô tả một cây lính có gốc, được mã hóa không có cạnh rõ ràng. Mỗi người lính xuất hiện dưới dạng một ID số nguyên và ngay sau ID đó không có hoặc nhiều mô tả trong ngoặc đơn về cấp dưới trực tiếp của nó."
date: "2026-08-17T19:27:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "I"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 206
verified: false
draft: false
---

[CF 102267I - Đội quân tối thượng](https://codeforces.com/problemset/problem/102267/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một cây lính có gốc, được mã hóa không có cạnh rõ ràng. Mỗi người lính xuất hiện dưới dạng một ID số nguyên và ngay sau ID đó không có hoặc nhiều mô tả trong ngoặc đơn về cấp dưới trực tiếp của nó. Một cặp dấu ngoặc đơn chứa đúng một cây con hoàn chỉnh. Ví dụ,`2(3(4))(1)`mô tả một cái cây có gốc là người lính 2, có con 3 và 1, còn cây 3 có con 4. 

Nhiệm vụ là tìm lại cha mẹ của mỗi người lính. Đối với mỗi ID từ 1 đến`n`, chúng tôi xuất ID của người giám sát trực tiếp của nó. Người lính duy nhất ở gốc có cha mẹ là 0. 

Ràng buộc đầu tiên cung cấp tối đa 140.000 binh sĩ, trong khi chuỗi được mã hóa có thể chứa tới một triệu ký tự. Điều này có nghĩa là một thuật toán tỷ lệ thuận với số lượng binh lính hoặc ký tự sẽ dễ dàng phù hợp, nhưng việc quét liên tục một phần lớn chuỗi cho mỗi binh sĩ là quá tốn kém. Với giới hạn thời gian một giây, chúng ta nên hướng tới thời gian tuyến tính, hoặc tệ nhất là thứ gì đó gần với nó. Thuật toán bậc hai trên chuỗi triệu ký tự có thể yêu cầu khoảng`10^12`hoạt động của nhân vật, vượt xa giới hạn. 

Trường hợp cạnh đầu tiên là một người lính. đầu vào```
1
1
```không có dấu ngoặc đơn nào cả và kết quả đúng là```
0
```Một trình phân tích cú pháp giả định mỗi người lính đều có ít nhất một đứa con sẽ thất bại ở đây. 

Một trường hợp khác là một chuỗi, trong đó mỗi người lính có đúng một con:```
4
1(2(3(4)))
```Đầu ra đúng là```
0 1 2 3
```Việc triển khai bất cẩn coi người lính được phân tích gần đây nhất là anh chị em sau khi gặp phải`)`có thể mất đi mối quan hệ tổ tiên. Sau khi người lính 4 kết thúc, trình phân tích cú pháp phải quay lại người lính 3, rồi đến người lính 2, rồi đến người lính 1. 

Trường hợp thứ ba là vài anh chị em:```
4
1(2)(3)(4)
```Câu trả lời là```
0 1 1 1
```Dấu ngoặc đơn đóng của người lính 2 phải khôi phục người lính 1 làm người giám sát hiện tại trước khi người lính 3 được phân tích cú pháp. Việc triển khai quên loại bỏ một người lính đã hoàn thành khỏi đường dẫn hoạt động của nó có thể khiến 2 trở thành cha của 3 một cách không chính xác. 

Cuối cùng, ID có thể có nhiều chữ số, do đó việc phân tích từng ký tự dưới dạng ID riêng biệt là không chính xác. Ví dụ,```
3
10(2)(30)
```sẽ mô tả các ID 10, 2 và 30 nếu những ID đó được người được chọn cho phép`n`; trình phân tích cú pháp phải sử dụng chuỗi chữ số đầy đủ trước khi xử lý người lính. Theo ràng buộc thực tế, ID chính xác là từ 1 đến`n`, vấn đề tương tự xuất hiện bất cứ khi nào`n >= 10`. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xử lý từng người lính một cách độc lập. Sau khi tìm thấy vị trí xuất hiện ID, chúng tôi có thể quét ngược qua chuỗi và xác định dấu ngoặc đơn của người lính nào chứa vị trí đó. Dấu ngoặc đơn cung cấp đủ thông tin để khôi phục cây con kèm theo, vì vậy cách tiếp cận này là chính xác. 

Vấn đề là các ký tự giống nhau được quét đi quét lại. Hãy xem xét một cây được lồng sâu như`1(2(3(4(...)))`. Đối với một người lính ở gần cuối, tìm kiếm ngược có thể duyệt gần như toàn bộ tiền tố của chuỗi. Nếu có`n`những người lính và sợi dây có chiều dài`L`, công việc trong trường hợp xấu nhất là`O(nL)`. Với`n = 140000`Và`L`gần với`10^6`, điều này có thể đạt tới khoảng`1.4 × 10^11`kiểm tra nhân vật. Mặc dù mức tối đa chính xác của cả hai đại lượng không thể xảy ra độc lập ở mọi hình dạng, nhưng giới hạn này đã quá lớn. 

Cấu trúc của mã hóa cho chúng ta khả năng quan sát tốt hơn nhiều. Tại bất kỳ thời điểm nào trong khi đọc chuỗi từ trái sang phải, có một đường dẫn duy nhất từ ​​gốc đến người lính hiện đang được mô tả. Con đường đó chính xác bao gồm những người lính mở đầu`(`đã gặp phải nhưng có tương ứng`)`vẫn chưa gặp phải. 

Đường dẫn hoạt động này có thể được duy trì bằng một ngăn xếp. Khi một ID lính mới được đọc, phần trên cùng của ngăn xếp là người giám sát của nó. Sau đó chúng ta đẩy lính mới vì tất cả các con của nó, nếu có, đều thuộc về nó. Khi`)`gặp phải, toàn bộ mô tả về người lính hiện tại đã kết thúc nên chúng tôi bật nó lên. Mỗi ký tự được xử lý một lần và mỗi người lính được đẩy và bật một lần. 

Phương pháp vũ phu hoạt động vì dấu ngoặc đơn ngầm cho chúng ta biết người lính nào chứa người lính khác, nhưng nó liên tục xây dựng lại mối quan hệ đó từ đầu. Việc quan sát thấy các tổ tiên đang hoạt động chính xác là một ngăn xếp sẽ giảm vấn đề xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nL)`trong trường hợp xấu nhất |`O(n)`| Quá chậm | 
| Phân tích ngăn xếp |`O(L)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và chuỗi được mã hóa. Phân bổ một mảng câu trả lời có độ dài`n + 1`, Ở đâu`parent[x]`cuối cùng sẽ chứa người giám sát của người lính`x`. 
2. Khởi tạo một ngăn xếp trống. Ngăn xếp biểu thị đường dẫn hiện tại từ gốc đến người lính có mô tả đang được xử lý. Đứng đầu của nó luôn là người lính hiện đang hoạt động. 
3. Quét chuỗi từ trái sang phải. Khi tìm thấy một chữ số, tiếp tục sử dụng các chữ số cho đến khi đọc hết ID số nguyên. Điều này là cần thiết vì một ID như`123`là một người lính, không phải ba người lính riêng biệt. 
4. Sau khi ID hoàn chỉnh`x`đã được phân tích cú pháp, hãy kiểm tra ngăn xếp. Nếu không trống thì trên cùng của nó là người giám sát trực tiếp của`x`, vậy hãy đặt`parent[x]`tới giá trị đó. Nếu ngăn xếp trống,`x`là gốc và cha mẹ của nó vẫn là 0. 
5. Đẩy`x`lên ngăn xếp. Từ thời điểm này cho đến dấu ngoặc đơn đóng tương ứng,`x`là người giám sát tích cực cho mọi thành phần con trực tiếp gặp phải trong cây con của nó. 
6. Khi quét gặp`(`, chỉ cần vượt qua nó. Dấu ngoặc đơn mở đầu không giới thiệu lính mới nên không có thao tác ngăn xếp nào để thực hiện. 
7. Khi quét gặp`)`, bật người lính hàng đầu. Cây con hoàn chỉnh của nó vừa kết thúc nên nó không còn có thể là người giám sát tích cực nữa. Đầu ngăn xếp mới là cha mẹ của nó. 
8. Sau khi quét toàn bộ chuỗi, xuất ra`parent[1]`bởi vì`parent[n]`. Mỗi người lính đều đã gặp đúng một lần nên mọi mối quan hệ cha mẹ đều đã được xác định. 

### Tại sao nó hoạt động 

Bất biến chính là ngay trước khi đọc ID người lính, ngăn xếp chứa chính xác tổ tiên của người lính đó, theo thứ tự từ gốc đến cha. Việc mở mô tả của một người lính khiến người lính đó bị đẩy lên, nên con của nó nhìn thấy nó ở đầu ngăn xếp. Khi đạt đến dấu ngoặc đơn đóng của nó, toàn bộ cây con của nó kết thúc và việc bật nó lên sẽ khôi phục phần tử gốc của chính nó làm phần tử trên cùng. Do đó, bất cứ khi nào một ID được phân tích cú pháp, đỉnh ngăn xếp chính xác là người giám sát trực tiếp của nó. Gốc là ID duy nhất được phân tích cú pháp với ngăn xếp trống, vì vậy nó nhận chính xác giá trị gốc 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data):
    lines = data.splitlines()
    n = int(lines[0])
    s = lines[1].strip()

    parent = [0] * (n + 1)
    stack = []

    i = 0
    length = len(s)

    while i < length:
        c = s[i]

        if '0' <= c <= '9':
            x = 0

            while i < length:
                c = s[i]
                if c < '0' or c > '9':
                    break
                x = x * 10 + (ord(c) - 48)
                i += 1

            if stack:
                parent[x] = stack[-1]

            stack.append(x)
        elif c == ')':
            stack.pop()
            i += 1
        else:
            # '('
            i += 1

    return ' '.join(map(str, parent[1:]))

def main():
    data = sys.stdin.read()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```Trình phân tích cú pháp sử dụng một chỉ mục`i`thay vì tách chuỗi. Điều này tránh tạo ra một tập hợp lớn các chuỗi con tạm thời và cho phép chúng tôi xử lý dữ liệu đầu vào một cách trực tiếp. 

Khi`s[i]`là một chữ số, vòng lặp bên trong sẽ xây dựng ID hoàn chỉnh bằng số. Sau khi nó kết thúc,`i`đã trỏ vào ký tự không có chữ số đầu tiên, do đó không có phần tăng thêm ở cuối nhánh chữ số. Chi tiết này ngăn chặn việc bỏ qua dấu ngoặc đơn sau. 

Ngăn xếp lưu trữ ID số nguyên thay vì vị trí chuỗi. Điều này làm cho việc tra cứu cha mẹ ngay lập tức, bởi vì`stack[-1]`chính xác là người giám sát mà chúng tôi cần. 

Đối với việc đọc ID khi ngăn xếp trống, không cần gán vì mảng câu trả lời được khởi tạo bằng số 0. Có chính xác một người lính như vậy vì đầu vào đại diện cho một cây có gốc. 

Khi`)`được xử lý, ngăn xếp không được để trống đối với đầu vào hợp lệ. Thao tác pop sẽ loại bỏ người lính vừa kết thúc mô tả. Một sự mở đầu`(`không cần thao tác ngăn xếp vì người lính đang được mở sẽ được đẩy khi ID của nó được đọc. 

Số nguyên Python không có vấn đề tràn ở đây. ID lớn nhất chỉ là 140.000 và bản thân chuỗi chứa tối đa một triệu ký tự. 

Việc thực hiện sử dụng`sys.stdin.read()`bên trong`solve`thay vì liên tục gọi`input()`. Điều này thuận tiện cho việc nhập hàng triệu ký tự và cũng làm cho bộ giải dễ dàng kiểm tra. yêu cầu`input = sys.stdin.readline`được định nghĩa là I/O nhanh lập trình cạnh tranh tiêu chuẩn, mặc dù bộ giải chính đọc toàn bộ đầu vào cùng một lúc. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, cây được mã hóa là`2(3(4))(1)`. Dấu vết sau đây ghi lại các hành động phân tích cú pháp có ý nghĩa. Dấu ngoặc đơn được hiển thị rõ ràng vì chúng chịu trách nhiệm về những thay đổi của ngăn xếp. 

| Vị trí/mã thông báo | Hành động | Xếp chồng sau hành động | Bài tập của phụ huynh | 
| --- | --- | --- | --- | 
|`2`| Đọc người lính 2, không có cha mẹ tích cực |`[2]`|`parent[2] = 0`| 
|`(`| Nhập mô tả con |`[2]`| không thay đổi | 
|`3`| Đọc người lính 3 |`[2, 3]`|`parent[3] = 2`| 
|`(`| Nhập mô tả con |`[2, 3]`| không thay đổi | 
|`4`| Đọc người lính 4 |`[2, 3, 4]`|`parent[4] = 3`| 
|`)`| Lính Phần Lan 4 |`[2, 3]`| không thay đổi | 
|`)`| Lính Phần Lan 3 |`[2]`| không thay đổi | 
|`(`| Nhập con tiếp theo |`[2]`| không thay đổi | 
|`1`| Đọc người lính 1 |`[2, 1]`|`parent[1] = 2`| 
|`)`| Người lính Phần Lan 1 |`[2]`| không thay đổi | 

Mảng cha cuối cùng là`2 0 2 3`. Dấu vết cho thấy tại sao các phần tử anh em hoạt động chính xác: sau dấu ngoặc đơn đóng của lính 3, lính 2 sẽ hoạt động trở lại trước khi lính 1 được phân tích cú pháp. 

Đối với Mẫu 2, chuỗi là`4(2)(5(3(6)(1))(7))`. 

| Mã thông báo | Hành động | Xếp chồng sau hành động | Bài tập của phụ huynh | 
| --- | --- | --- | --- | 
|`4`| Đọc người lính 4 |`[4]`|`parent[4] = 0`| 
|`(`| Nhập mô tả con |`[4]`| không thay đổi | 
|`2`| Đọc người lính 2 |`[4, 2]`|`parent[2] = 4`| 
|`)`| Lính Phần Lan 2 |`[4]`| không thay đổi | 
|`(`| Nhập con tiếp theo |`[4]`| không thay đổi | 
|`5`| Đọc người lính 5 |`[4, 5]`|`parent[5] = 4`| 
|`(`| Nhập mô tả con |`[4, 5]`| không thay đổi | 
|`3`| Đọc người lính 3 |`[4, 5, 3]`|`parent[3] = 5`| 
|`(`| Nhập mô tả con |`[4, 5, 3]`| không thay đổi | 
|`6`| Đọc người lính 6 |`[4, 5, 3, 6]`|`parent[6] = 3`| 
|`)`| Lính Phần Lan 6 |`[4, 5, 3]`| không thay đổi | 
|`(`| Nhập con tiếp theo |`[4, 5, 3]`| không thay đổi | 
|`1`| Đọc người lính 1 |`[4, 5, 3, 1]`|`parent[1] = 3`| 
|`)`| Người lính Phần Lan 1 |`[4, 5, 3]`| không thay đổi | 
|`)`| Lính Phần Lan 3 |`[4, 5]`| không thay đổi | 
|`(`| Nhập con tiếp theo |`[4, 5]`| không thay đổi | 
|`7`| Đọc người lính 7 |`[4, 5, 7]`|`parent[7] = 5`| 
|`)`| Lính Phần Lan 7 |`[4, 5]`| không thay đổi | 
|`)`| Lính Phần Lan 5 |`[4]`| không thay đổi | 
|`)`| Lính Phần Lan 4 |`[]`| không thay đổi | 

Kết quả đầu ra là`3 4 5 0 4 3 5`. Ví dụ này thực hiện cả việc lồng và nhiều anh chị em. Đặc biệt, lính 7 trở thành con của 5 chứ không phải 3 vì dấu ngoặc đơn đóng sau lính 3 sẽ loại bỏ 3 khỏi đường dẫn hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(L)`| Mỗi ký tự được kiểm tra một lần và mỗi ID được đẩy chính xác một lần. | 
| Không gian |`O(n)`| Mảng câu trả lời và ngăn xếp mỗi mảng chứa tối đa`n`số nguyên. | 

Đây`L`là độ dài của chuỗi được mã hóa, với`L <= 10^6`, Và`n <= 1.4 × 10^5`. Quét tuyến tính một triệu ký tự phù hợp với giới hạn một giây, trong khi ngăn xếp có thể chứa tối đa tất cả binh lính trong một chuỗi, nằm trong giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    lines = data.splitlines()
    n = int(lines[0])
    s = lines[1].strip()

    parent = [0] * (n + 1)
    stack = []

    i = 0
    length = len(s)

    while i < length:
        c = s[i]

        if '0' <= c <= '9':
            x = 0

            while i < length:
                c = s[i]
                if c < '0' or c > '9':
                    break
                x = x * 10 + ord(c) - 48
                i += 1

            if stack:
                parent[x] = stack[-1]

            stack.append(x)

        elif c == ')':
            stack.pop()
            i += 1

        else:
            i += 1

    return ' '.join(map(str, parent[1:]))

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided sample 1
assert run("""4
2(3(4))(1)
""") == "2 0 2 3", "sample 1"

# Provided sample 2
assert run("""7
4(2)(5(3(6)(1))(7))
""") == "3 4 5 0 4 3 5", "sample 2"

# Minimum-size tree: one soldier, no parentheses.
assert run("""1
1
""") == "0", "single soldier"

# A deep chain. Every closing parenthesis must restore the previous ancestor.
assert run("""6
1(2(3(4(5(6)))))
""") == "0 1 2 3 4 5", "deep chain"

# Many siblings. Every child must return to the same parent after ')'.
assert run("""6
1(2)(3)(4)(5)(6)
""") == "0 1 1 1 1 1", "many siblings"

# Multiple-digit IDs and nested subtrees.
# 100 is the root, 12 and 99 are its children, and 50 is a child of 99.
assert run("""100
100(12)(99(50))
""") == "99 100 0 99", "multi-digit IDs"

# Maximum-number-of-soldiers style stress test with repeated sibling structure.
# The IDs are unique by the problem definition, so this uses the uniform
# structural case where every non-root soldier has the same parent.
n = 140000
s = "1" + "".join("(" + str(x) + ")" for x in range(2, n + 1))
expected = "0 " + " ".join(["1"] * (n - 1))
assert run(f"{n}\n{s}\n") == expected, "large sibling stress test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Kích thước tối thiểu và gốc không có dấu ngoặc đơn | 
|`6 / 1(2(3(4(5(6)))))`|`0 1 2 3 4 5`| Lồng sâu và khôi phục ngăn xếp lặp đi lặp lại | 
|`6 / 1(2)(3)(4)(5)(6)`|`0 1 1 1 1 1`| Nhiều anh chị em và xử lý đúng các cây con liên tiếp | 
|`100 / 100(12)(99(50))`|`99 100 0 99`| Phân tích cú pháp ID nhiều chữ số và anh chị em lồng nhau | 
|`140000 / 1(2)(3)...(140000)`|`0 1 1 ... 1`| Kích thước đầu vào lớn và cấu trúc anh chị em lặp lại đồng nhất | 

Vấn đề đảm bảo rằng mọi ID từ 1 đến`n`xuất hiện chính xác một lần, do đó, việc kiểm tra theo nghĩa đen trong đó mọi ID có cùng giá trị sẽ vi phạm định dạng đầu vào. Kiểm tra anh chị em lớn bao gồm mô hình ứng suất "tất cả các giá trị bằng nhau" dự định về mặt cấu trúc: mọi chiến binh không phải gốc đều nhận được chính xác cùng một cha mẹ. 

## Vỏ cạnh 

Trường hợp một người lính được xử lý trước bất kỳ hoạt động ngăn xếp nào. Vì```
1
1
```trình phân tích cú pháp đọc ID 1 với một ngăn xếp trống, để lại`parent[1]`bằng 0 và đẩy 1. Quá trình quét sau đó kết thúc, tạo ra`0`. Không cần có dấu ngoặc đơn đóng vì mô tả của phần gốc đơn giản là không chứa phần phụ nào. 

Trường hợp chuỗi sâu```
4
1(2(3(4)))
```sản xuất`parent[1] = 0`,`parent[2] = 1`,`parent[3] = 2`, Và`parent[4] = 3`. Sau khi đọc 4, ngăn xếp là`[1, 2, 3, 4]`. Mỗi`)`loại bỏ chính xác một người lính, do đó tổ tiên đang hoạt động di chuyển từ 4 đến 3, sau đó từ 3 đến 2 và cuối cùng từ 2 đến 1. Đây chính xác là thông tin lồng ghép được mã hóa bởi dấu ngoặc đơn. 

Vụ án anh chị em```
4
1(2)(3)(4)
```bắt đầu với ngăn xếp`[1]`. Đọc 2 gán cha mẹ 1 và tạo ra`[1, 2]`. Dấu ngoặc đơn đóng của nó bật lên 2, khôi phục`[1]`. Trình tự tương tự xảy ra với 3 và 4. Kết quả là`0 1 1 1`. Một trình phân tích cú pháp chỉ đẩy và không bao giờ bật sẽ biến anh chị em thành một chuỗi một cách không chính xác. 

Trường hợp có nhiều chữ số```
3
3(1)(2)
```đọc 3 dưới dạng một ID hoàn chỉnh thay vì diễn giải các chữ số của nó một cách độc lập. Nó gán cha 0 cho 3, sau đó gán cha 3 cho cả 1 và 2. Đầu ra là`3 3 0`. Vòng lặp đọc chữ số bên trong giúp tính năng này hoạt động với mọi kích thước ID được phép`n`. 

Đối với trường hợp ứng suất cấu trúc có kích thước tối đa, đầu vào bắt đầu bằng lính 1, theo sau là 139.999 cây con anh em:```
140000
1(2)(3)(4)...(140000)
```Mỗi đứa trẻ được đọc trong khi 1 ở trên cùng của ngăn xếp, vì vậy mọi bài tập đều được`parent[x] = 1`. Mỗi dấu ngoặc đơn đóng ngay lập tức loại bỏ đứa trẻ đó, để lại 1 dấu ngoặc hoạt động cho đứa trẻ tiếp theo. Thuật toán xử lý toàn bộ chuỗi khoảng một triệu ký tự một lần, thay vì tìm kiếm nhiều lần trong chuỗi đó, đó chính xác là lý do tại sao cách tiếp cận tuyến tính vẫn thực tế ở kích thước đầu vào lớn nhất.
