---
title: "CF 102386D - \u0410\u0440\u0442\u0435\u043c \u0432 \u0430\u0440\u043c\u0438\u0438"
description: "Có chính xác ba xe tăng được đánh số 1, 2 và 3 và Artem xuất phát ở xe tăng k. Mỗi lệnh đặt tên cho hai xe tăng khác nhau. Kíp lái của hai chiếc xe tăng đó trao đổi xe tăng của họ, vì vậy Artem chỉ di chuyển khi xe tăng hiện tại của anh ta là một trong hai chiếc được đề cập trong bộ chỉ huy."
date: "2026-08-15T07:38:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "D"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 261
verified: false
draft: false
---

[CF 102386D - \u0410\u0440\u0442\u0435\u043c \u0432 \u0430\u0440\u043c\u0438\u0438](https://codeforces.com/problemset/problem/102386/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Có chính xác ba xe tăng được đánh số 1, 2 và 3 và Artem xuất phát trong xe tăng`k`. Mỗi lệnh đặt tên cho hai xe tăng khác nhau. Kíp lái của hai chiếc xe tăng đó trao đổi xe tăng của họ, vì vậy Artem chỉ di chuyển khi xe tăng hiện tại của anh ta là một trong hai chiếc được đề cập trong bộ chỉ huy. 

Đầu vào chứa`n`các lệnh tuân theo thứ tự nhất định của chúng. Dòng đầu tiên đưa ra số lượng lệnh và xe tăng ban đầu của Artem. Mỗi dòng tiếp theo chứa hai số xe tăng liên quan đến một lần hoán đổi. Đầu ra được yêu cầu là số lượng xe tăng chứa Artem sau mỗi lệnh được xử lý. 

Ràng buộc`n <= 10^5`có nghĩa là giải pháp sẽ xử lý mỗi lệnh một số lần không đổi. MỘT`O(n)`Thuật toán chỉ thực hiện vài trăm nghìn thao tác cơ bản, dễ dàng phù hợp với giới hạn thời gian lập trình cạnh tranh thông thường. MỘT`O(n^2)`mô phỏng có thể yêu cầu khoảng`10^10`hoạt động ở kích thước đầu vào tối đa, vượt xa những gì thực tế. 

Trường hợp cạnh đầu tiên là lệnh không liên quan đến xe tăng hiện tại của Artem. Ví dụ,```
1 1
2 3
```sản xuất`1`. Artem vẫn ở trong xe tăng 1 vì việc hoán đổi diễn ra hoàn toàn giữa xe tăng 2 và xe tăng 3. Việc thực hiện bất cẩn làm thay đổi vị trí của Artem sau mỗi lệnh mà không kiểm tra xem xe tăng của anh ta có tham gia hay không, có thể khiến anh ta di chuyển không chính xác. 

Trường hợp thứ hai là khi Artem ở trong một trong những chiếc xe tăng được đổi chỗ và lệnh chuyển anh ta sang chiếc xe tăng khác. Ví dụ,```
1 2
1 2
```sản xuất`1`. Artem bắt đầu ở xe tăng 2, do đó việc trao đổi xe tăng 1 và 2 sẽ chuyển anh ta sang xe tăng 1. Hai số trong lệnh phải được coi là hai điểm cuối của việc hoán đổi chứ không phải là một hoạt động được ra lệnh trong đó một số thay thế số kia. 

Trường hợp hữu ích thứ ba là hoán đổi lặp đi lặp lại. Ví dụ,```
2 3
2 3
2 3
```sản xuất`3`. Lần hoán đổi đầu tiên chuyển Artem từ 3 lên 2 và lần thứ hai chuyển anh ta trở lại 3. Việc triển khai vô tình bỏ qua các lệnh lặp lại hoặc cố gắng đơn giản hóa các lệnh mà không giữ nguyên thứ tự của chúng có thể mắc lỗi này. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp nhưng tốn kém không cần thiết là tính toán lại vị trí của Artem ngay từ đầu sau mỗi lệnh. Lệnh sau khi xử lý`i`, nó sẽ mô phỏng tất cả`i`các lệnh để xác định Artem ở đâu, mặc dù câu trả lời nối tiếp lệnh`i-1`đã được biết đến. Tổng số lệnh mô phỏng sẽ là`1 + 2 + 3 + ... + n = n(n + 1) / 2`. 

Vì`n = 100000`, đó là`5,000,050,000`mô phỏng lệnh. Bản thân mô phỏng là chính xác vì nó thực hiện chính xác các giao dịch hoán đổi giống như quy trình ban đầu, nhưng nó lặp lại gần như toàn bộ công việc nhiều lần. 

Quan sát quan trọng là chúng ta không cần bố trí đầy đủ các đội. Chúng tôi chỉ quan tâm đến xe tăng hiện tại của Artem. Đối với một lệnh`(a, b)`, chỉ có ba khả năng. Nếu Artem không ở trong đó`a`cũng không`b`, vị trí của anh ta không thay đổi. Nếu anh ấy ở trong`a`, anh ấy chuyển đến`b`. Nếu anh ấy ở trong`b`, anh ấy chuyển đến`a`. 

Tính toán lại brute-force hoạt động vì mọi lệnh đều mang tính xác định nhưng không thành công khi nó liên tục tái tạo lại thông tin đã được tính toán. Quan sát rằng vị trí tiếp theo chỉ phụ thuộc vào vị trí hiện tại của Artem và lệnh hiện tại cho phép chúng tôi giảm toàn bộ quá trình xuống một lần cập nhật liên tục cho mỗi lệnh. 

Thuật toán kết quả không cần có mảng nội dung trong xe tăng và không cần có sự đại diện của các tổ lái. Một số nguyên duy nhất chứa xe tăng hiện tại của Artem là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và xe tăng ban đầu của Artem`k`. Cửa hàng`k`làm vị trí hiện tại vì trước khi bất kỳ lệnh nào được thực thi, đó chính xác là vị trí của Artem. 
2. Xử lý`n`các lệnh theo thứ tự đầu vào của chúng. Đối với mỗi lệnh, hãy đọc hai xe tăng được hoán đổi`a`Và`b`. 
3. Nếu vị trí hiện tại bằng`a`, đổi nó thành`b`. Artem đang ngồi ở một trong hai chiếc xe tăng đang được đổi nên anh ấy chuyển sang chiếc xe tăng còn lại. 
4. Ngược lại, nếu vị trí hiện tại bằng`b`, đổi nó thành`a`. Quy tắc hoán đổi tương tự được áp dụng theo hướng ngược lại. 
5. Nếu vị trí hiện tại không`a`cũng không`b`, giữ nguyên. Lệnh ảnh hưởng đến hai xe tăng khác và không thể di chuyển Artem. 
6. Sau khi tất cả các lệnh đã được xử lý, hãy in vị trí hiện tại. Mọi bản cập nhật đều được áp dụng theo thứ tự ban đầu, vì vậy giá trị này là xe tăng cuối cùng của Artem. 

### Tại sao nó hoạt động 

Điều bất biến là ngay trước khi xử lý mọi lệnh,`k`lưu trữ xe tăng hiện tại của Artem. Đối với một lệnh`(a, b)`, nếu như`k`là`a`, tổ lái trong xe tăng`a`di chuyển đến xe tăng`b`, vì vậy Artem chuyển đến`b`. Trường hợp đối xứng xảy ra khi`k`là`b`, và khi nào`k`không phải là xe tăng, Artem không di chuyển. Do đó, bất biến vẫn đúng sau mỗi lệnh. Sau lệnh cuối cùng,`k`do đó phải là bể chứa cuối cùng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    for _ in range(n):
        a, b = map(int, input().split())

        if k == a:
            k = b
        elif k == b:
            k = a

    print(k)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên đọc cả số lượng lệnh và xe tăng ban đầu của Artem. Biến`k`được tái sử dụng một cách có chủ ý để đại diện cho bể hiện tại, do đó không cần có cấu trúc trạng thái riêng biệt. 

Bên trong vòng lặp, mỗi lệnh được xử lý chính xác một lần. các`if`nhánh xử lý trường hợp Artem ở trong xe tăng đầu tiên của lệnh, trong khi`elif`xử lý xe tăng thứ hai. Nếu cả hai điều kiện đều không đúng thì không có gì được gán cho`k`, nên Artem vẫn ở nguyên vị trí của mình. 

Thứ tự của các cập nhật này rất quan trọng vì lệnh sau phải sử dụng vị trí của Artem sau tất cả các lệnh trước đó. Không có chuyển đổi lập chỉ mục nào ở đây vì các thùng đã được đánh số từ 1 đến 3. Số nguyên Python cũng không có vấn đề tràn đối với các giá trị này, mặc dù trên thực tế`k`không bao giờ trở thành bất cứ thứ gì ngoài phạm vi từ 1 đến 3. 

Đầu vào chỉ chứa một ca kiểm thử, do đó không có vòng lặp ca kiểm thử bên ngoài. sử dụng`sys.stdin.readline`cung cấp khả năng xử lý đầu vào hiệu quả cho tối đa`10^5`lệnh. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, Artem bắt đầu ở xe tăng 1. Lệnh đầu tiên trao đổi xe tăng 1 và 2, do đó Artem chuyển sang 2. Lệnh thứ hai đổi xe tăng 2 và 3, do đó anh ấy chuyển sang 3. 

| Lệnh |`a`|`b`|`k`trước |`k`sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | | 1 | 
| 1 | 1 | 2 | 1 | 2 | 
| 2 | 2 | 3 | 2 | 3 | 

Giá trị cuối cùng là`3`, phù hợp với đầu ra mẫu. Dấu vết này thể hiện quy tắc chuyển tiếp chính hai lần liên tiếp. 

Đối với ví dụ thứ hai, hãy xem xét:```
5 3
1 2
2 3
1 3
1 2
2 3
```Trạng thái thay đổi như sau. 

| Lệnh |`a`|`b`|`k`trước |`k`sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | | 3 | 
| 1 | 1 | 2 | 3 | 3 | 
| 2 | 2 | 3 | 3 | 2 | 
| 3 | 1 | 3 | 2 | 2 | 
| 4 | 1 | 2 | 2 | 1 | 
| 5 | 2 | 3 | 1 | 1 | 

Câu trả lời cuối cùng là`1`. Lệnh đầu tiên và thứ ba không liên quan đến xe tăng hiện tại của Artem, vì vậy vị trí của anh ta không thay đổi trong các bước đó. Điều này xác nhận rằng mọi lệnh phải được kiểm tra theo vị trí hiện tại thay vì di chuyển Artem một cách mù quáng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trong số`n`các lệnh được đọc và xử lý một lần với công việc liên tục. | 
| Không gian | O(1) | Chỉ một`n`,`k`,`a`, Và`b`được lưu trữ, bất kể số lượng lệnh. | 

Với`n <= 10^5`, thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi lệnh, do đó tổng số thao tác tăng tuyến tính với kích thước đầu vào. Nó cũng không lưu trữ danh sách lệnh, làm cho việc sử dụng bộ nhớ của nó không phụ thuộc vào`n`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n, k = map(int, input().split())

    for _ in range(n):
        a, b = map(int, input().split())

        if k == a:
            k = b
        elif k == b:
            k = a

    print(k)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("""2 1
1 2
2 3
""") == "3\n", "sample 1"

# Minimum-size input
assert run("""1 1
2 3
""") == "1\n", "Artem is not involved in the only swap"

# Artem starts at the largest tank number
assert run("""1 3
1 3
""") == "1\n", "boundary tank 3 moves to tank 1"

# Repeated swaps return Artem to the original tank
assert run("""2 3
2 3
2 3
""") == "3\n", "two identical swaps cancel each other"

# Multiple commands, including commands that do not involve Artem
assert run("""5 3
1 2
2 3
1 3
1 2
2 3
""") == "1\n", "mixed swaps"

# Maximum-size input, repeated identical swaps
n = 100000
large_input = f"{n} 1\n" + "1 2\n" * n
expected = "1\n" if n % 2 == 0 else "2\n"
assert run(large_input) == expected, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 2 3`|`1`| Đầu vào tối thiểu và trao đổi không liên quan đến Artem | 
|`1 3 / 1 3`|`1`| Số bể ranh giới và chuyển động trực tiếp | 
|`2 3 / 2 3 / 2 3`|`3`| Hoán đổi lặp đi lặp lại và thứ tự hoạt động | 
|`5 3 / 1 2 / 2 3 / 1 3 / 1 2 / 2 3`|`1`| Các lệnh liên quan và không liên quan đến Artem | 
|`100000 1 / 1 2`lặp đi lặp lại |`1`| Kích thước đầu vào tối đa và chuyển đổi trạng thái lặp lại | 

## Vỏ cạnh 

Khi Artem không tham gia vào việc hoán đổi, vị trí của anh ta phải không thay đổi. Đối với đầu vào```
1 1
2 3
```vị trí ban đầu là`1`. Lệnh chỉ trao đổi xe tăng`2`Và`3`, vậy cũng không`k == a`cũng không`k == b`là đúng. Thuật toán rời đi`k`bằng`1`và in`1`. Điều này ngăn ngừa sai lầm phổ biến khi cho rằng mọi lệnh đều di chuyển Artem. 

Khi Artem khởi động ở bể 2 và bể 1 và 2 được trao đổi, bản cập nhật phải sử dụng điểm cuối khác của lệnh. Vì```
1 2
1 2
```thuật toán bắt đầu bằng`k = 2`. Điều kiện đầu tiên không thành công vì`2 != 1`, trong khi lần thứ hai thành công vì`2 == 2`, Vì thế`k`trở thành`1`. Đầu ra là`1`. 

Hoán đổi lặp đi lặp lại phải được xử lý độc lập. Vì```
2 3
2 3
2 3
```lệnh đầu tiên thay đổi`k`từ`3`ĐẾN`2`. Lệnh thứ hai xem giá trị cập nhật`2`và thay đổi nó trở lại`3`. Đầu ra cuối cùng là`3`. Điều này chứng tỏ tại sao các lệnh không thể được sắp xếp lại hoặc xử lý như một ánh xạ cố định duy nhất. 

Trường hợp kích thước tối đa không yêu cầu bất kỳ logic đặc biệt nào. Với`100000`các lệnh như```
100000 1
1 2
1 2
1 2
...
```thuật toán thực hiện một lần so sánh theo thời gian không đổi cho mỗi lệnh. Vì có số lần hoán đổi chẵn, Artem bắt đầu ở bể 1 và quay trở lại bể 1, do đó đầu ra là`1`. Bản thân trạng thái không bao giờ tăng theo đầu vào, đó là lý do tại sao giải pháp vẫn còn`O(1)`trong không gian phụ trợ.
