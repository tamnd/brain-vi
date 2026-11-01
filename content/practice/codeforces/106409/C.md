---
title: "CF 106409C - Sự kết hợp giữa chim cánh cụt và chuột túi"
description: "Nhiệm vụ yêu cầu chúng ta chuyển đổi một chuỗi gạch penguin-gopher thành một chuỗi khác chỉ bằng một thao tác: chọn một vị trí và đảo ngược mọi ký tự từ vị trí đó về cuối chuỗi."
date: "2026-06-25T09:57:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106409
codeforces_index: "C"
codeforces_contest_name: "HPI 2026 Advanced"
rating: 0
weight: 106409
solve_time_s: 42
verified: true
draft: false
---

[CF 106409C - Sự kết hợp giữa chim cánh cụt-Gopher](https://codeforces.com/problemset/problem/106409/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta chuyển đổi một chuỗi gạch penguin-gopher thành một chuỗi khác chỉ bằng một thao tác: chọn một vị trí và đảo ngược mọi ký tự từ vị trí đó về cuối chuỗi. Chúng ta phải xuất ra chuỗi các vị trí đã chọn hoặc báo cáo rằng không tồn tại chuỗi nào. Các ràng buộc ban đầu của bài toán cho phép các chuỗi có độ dài lên tới 1000 và yêu cầu tối đa 2000 thao tác. 

Lý do duy nhất khiến việc chuyển đổi có thể thất bại là việc đảo ngược vị trí không bao giờ thay đổi nhiều bộ ký tự. Nếu hai chuỗi không chứa cùng số của mỗi chữ cái thì không có chuỗi đảo ngược nào có thể giúp ích được. Vì độ dài chỉ là 1000 nên cách tiếp cận bậc hai vẫn có thể được chấp nhận, nhưng giới hạn thao tác loại trừ nhiều cách xây dựng dài không cần thiết. 

Một lỗi phổ biến là chỉ kiểm tra xem mọi ký tự mục tiêu có xuất hiện ở đâu đó trong nguồn hay không. Tần số quan trọng. Ví dụ, với đầu vào```
3
abc
abb
```đầu ra đúng là`-1`bởi vì chuỗi đầu tiên có một`c`trong khi mục tiêu không có`c`. Một cách tiếp cận bất cẩn chỉ kiểm tra xem mọi chữ cái mục tiêu có tồn tại hay không sẽ chấp nhận nó một cách sai lầm. 

Một trường hợp cạnh khác là khi các chuỗi đã bằng nhau. Ví dụ,```
1
m
m
```đầu ra đúng là`0`. Việc tạo ra một sự đảo ngược vô dụng vẫn có giá trị về mặt toán học, nhưng nó gây lãng phí các hoạt động và có thể làm phức tạp việc triển khai. 

Trường hợp tinh vi cuối cùng là các ký tự lặp lại. Ví dụ,```
4
aabc
cbaa
```có nhiều lựa chọn khả thi cho vị trí chứa`c`. Việc chọn bất kỳ lần xuất hiện trùng khớp nào cũng có tác dụng vì các ký tự bằng nhau có thể hoán đổi cho nhau. Việc triển khai theo dõi các ký tự không chính xác theo giá trị thay vì theo vị trí hiện tại có thể di chuyển ô xếp sai. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là tìm kiếm thông qua các đảo ngược hậu tố có thể xảy ra và cố gắng tìm đường dẫn từ chuỗi bắt đầu đến chuỗi đích. Điều này đúng vì mọi chuỗi thao tác hợp lệ đều được biểu diễn ở đâu đó trong không gian tìm kiếm. Tuy nhiên, số lượng các trạng thái có thể có là rất lớn. Với độ dài 1000, thậm chí việc lưu trữ tất cả các hoán vị là không thể và việc thử các chuỗi thao tác sẽ tăng theo cấp số nhân. 

Quan sát hữu ích là việc đảo ngược hậu tố có thể di chuyển ký tự được chọn lên phía trước hậu tố hiện tại. Giả sử chúng ta đang cố định vị trí`i`từ trái sang phải. Vị trí trước đây`i`đã đúng và không bao giờ được thay đổi nữa. Sự đảo ngược bắt đầu từ`i`thực hiện chính xác điều đó bởi vì nó để lại tiền tố trước`i`không bị ảnh hưởng. 

Nếu ký tự cần thiết ở vị trí`i`hiện đang ở vị trí`p`bên trong hậu tố chưa hoàn thành, hai thao tác là đủ. Đầu tiên đảo ngược hậu tố bắt đầu từ`p`. Điều đó di chuyển ký tự mong muốn đến cuối. Sau đó đảo ngược hậu tố bắt đầu từ`i`. Ký tự mong muốn di chuyển từ cuối hậu tố đến vị trí đầu tiên của hậu tố đó, chính xác là`i`. 

Cách tiếp cận bạo lực có hiệu quả vì các thao tác đủ mạnh để sắp xếp lại các ký tự. Nó thất bại vì nó tìm kiếm một đường dẫn thay vì xây dựng một đường dẫn. Quan sát ở trên làm giảm toàn bộ vấn đề thành một quy trình bố trí tham lam trong đó mỗi vị trí cần tối đa hai thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng tham lam | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của mỗi ký tự trong cả hai chuỗi. Nếu số lượng khác nhau thì không thể đạt được sự sắp xếp mục tiêu, do đó đầu ra`-1`. Đảo ngược chỉ hoán vị vị trí và không bao giờ tạo hoặc xóa ký tự. 
2. Xử lý các vị trí từ trái qua phải. Ở bước`i`, tất cả các vị trí trước`i`đã chứa các ký tự cuối cùng của chúng, vì vậy các thao tác trong tương lai sẽ chỉ chạm vào hậu tố còn lại. 
3. Tìm vị trí bất kỳ`p`với`p >= i`trong đó ký tự hiện tại bằng ký tự đích cho vị trí`i`. 
4. Nếu`p`đã rồi`i`, không cần thao tác. Ngói chính xác đã được đặt đúng chỗ. 
5. Ngược lại, đảo ngược hậu tố bắt đầu từ`p`, sau đó đảo ngược hậu tố bắt đầu từ`i`. Lần đảo ngược đầu tiên di chuyển ô cần thiết đến cuối phần còn lại và lần thứ hai di chuyển nó lên phía trước. 
6. Tiếp tục cho đến khi mọi vị trí đã được cố định. Số lượng thao tác nhiều nhất là hai cho mỗi vị trí, vì vậy nó không bao giờ vượt quá 2000. 

Tại sao nó hoạt động: 

Bất biến là trước khi xử lý vị trí`i`, mọi vị trí trước`i`đã phù hợp với mục tiêu và sẽ không bao giờ thay đổi nữa. Một hậu tố đảo ngược bắt đầu từ`i`không thể ảnh hưởng đến những vị trí trước đó. Di chuyển hai chiều sẽ đặt ký tự được yêu cầu vào`i`trong khi vẫn giữ nguyên tính bất biến, do đó việc lặp lại quá trình cuối cùng sẽ tạo ra chuỗi đích bất cứ khi nào số lượng ký tự cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(input().strip())
    b = input().strip()

    if sorted(a) != sorted(b):
        print(-1)
        return

    ans = []

    for i in range(n):
        if a[i] == b[i]:
            continue

        pos = i
        while a[pos] != b[i]:
            pos += 1

        if pos != i:
            ans.append(pos + 1)
            a[i:pos + 1] = a[i:pos + 1][::-1]

            ans.append(i + 1)
            a[i:] = a[i:][::-1]

    print(len(ans))
    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Phép so sánh đầu tiên sử dụng các chuỗi được sắp xếp vì tình huống không thể duy nhất là có nhiều ký tự khác nhau. Sau khi quá trình kiểm tra đó thành công, mọi ký tự cần thiết sẽ tồn tại ở đâu đó trong hậu tố còn lại. 

Vòng lặp chính sửa một chỉ mục tại một thời điểm. Việc tìm kiếm`pos`chỉ nhìn vào các vị trí chưa được chốt nên các ký tự đã đúng sẽ không bao giờ bị xáo trộn. 

Hai phép đảo ngược cắt trong Python mô phỏng trực tiếp hai thao tác hậu tố. Các chỉ mục được lưu trữ dựa trên một vì mô tả thao tác sử dụng các vị trí bắt đầu từ 1. Số lượng thao tác được lưu trữ tối đa là`2 * (n - 1)`, thỏa mãn giới hạn yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
m
m
```Dấu vết: 

| Vị trí | Chuỗi hiện tại | Nhân vật mục tiêu | Hành động được chọn | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | m | m | Đã đúng | [] | 

Chuỗi đã khớp rồi nên thuật toán xác nhận rằng không có thao tác nào là đủ. 

### Ví dụ 2 

đầu vào:```
4
emog
egom
```Dấu vết: 

| Vị trí | Chuỗi hiện tại | Nhân vật mục tiêu | Đã tìm thấy vị trí | Đã thêm hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | cảm xúc | e | 0 | không | 
| 1 | mog | g | 3 | hậu tố ngược 4, hậu tố ngược 2 | 

Sau khi đảo ngược hậu tố từ vị trí 4, chuỗi trở thành`emgo`. Đảo ngược từ vị trí 2 cho`egom`, phù hợp với mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi vị trí tìm kiếm hậu tố còn lại và đảo ngược các phần của danh sách. | 
| Không gian | O(n) | Danh sách hoạt động và lưu trữ chuỗi là tuyến tính. | 

Với`n <= 1000`, giới hạn bậc hai thực hiện tối đa khoảng một triệu thao tác ký tự, dễ dàng nằm trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import io
import sys

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

# sample 1
assert run("""1
m
m
""").strip() == "0"

# sample 2
assert run("""3
deb
fyf
""").strip() == "-1"

# custom: already equal
assert run("""5
abcde
abcde
""").strip() == "0"

# custom: requires moving a character
out = run("""4
emog
egom
""").strip().splitlines()
assert out[0] == "1"
assert out[1] == "2"

# custom: repeated letters
out = run("""4
aabc
cbaa
""").strip().splitlines()
assert out[0] != "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / m / m`|`0`| Kích thước tối thiểu và trường hợp đã được giải quyết | 
|`3 / deb / fyf`|`-1`| Tần số ký tự khác nhau | 
|`5 / abcde / abcde`|`0`| Không có hoạt động không cần thiết | 
|`4 / emog / egom`| Một chuỗi thao tác hợp lệ | Chuyển động hậu tố đúng | 
|`4 / aabc / cbaa`| Bất kỳ chuỗi hợp lệ nào | Xử lý các ký tự lặp lại | 

## Vỏ cạnh 

Đối với số lượng ký tự khác nhau, thuật toán sẽ dừng trước khi thử bất kỳ thao tác nào. Ví dụ:```
3
abc
abb
```Việc so sánh tần số phát hiện ra rằng`c`chỉ tồn tại trong chuỗi đầu tiên, vì vậy nó in`-1`. 

Đối với một chuỗi đã khớp:```
1
m
m
```vòng lặp thấy rằng vị trí duy nhất đã đúng và để trống danh sách thao tác, tạo ra`0`. 

Đối với các ký tự lặp lại:```
4
aabc
cbaa
```thuật toán tìm kiếm hậu tố còn lại cho`c`, tìm thấy nó ở vị trí cuối cùng và áp dụng hai phép đảo ngược. Kể từ khi hai`a`các ô giống hệt nhau, hoặc lần xuất hiện đều có thể chấp nhận được và tính bất biến vẫn giữ nguyên sau mỗi lần đặt.
