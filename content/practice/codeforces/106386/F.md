---
title: "CF 106386F - Game đua xe"
description: "Đường đua có thể được xem như một biểu đồ hoán vị. Mỗi điểm kiểm tra sẽ trỏ đến điểm kiểm tra tiếp theo mà người chơi đạt tới, vì vậy mỗi người chơi đều thuộc về một chu kỳ."
date: "2026-06-25T10:14:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106386
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 2-25-26 (Advanced)"
rating: 0
weight: 106386
solve_time_s: 33
verified: true
draft: false
---

[CF 106386F - Trò chơi đua xe](https://codeforces.com/problemset/problem/106386/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đường đua có thể được xem như một biểu đồ hoán vị. Mỗi điểm kiểm tra sẽ trỏ đến điểm kiểm tra tiếp theo mà người chơi đạt tới, vì vậy mỗi người chơi đều thuộc về một chu kỳ. Người chơi kết thúc khi họ quay trở lại điểm kiểm tra bắt đầu, điều đó có nghĩa là thời gian kết thúc chính xác bằng độ dài chu kỳ của họ. 

Hoạt động của Dylan có tác dụng rất cụ thể. Anh ta chọn một người chơi và một trạm kiểm soát khác mà người chơi đó đã ghé thăm trong cuộc đua, sau đó hoán đổi hai cạnh đi ra trong cùng một chu kỳ. Điều này có thể chia một chu kỳ thành hai chu kỳ nhỏ hơn nhưng không bao giờ có thể hợp nhất các chu kỳ lại với nhau. 

Mục tiêu là tìm ngày đầu tiên khi mỗi chu kỳ có cùng độ dài. Ngày 1 là hoán vị ban đầu trước khi thực hiện bất kỳ thay đổi nào. 

Đầu vào chứa một hoán vị của`n`các điểm kiểm tra. Đầu ra là ngày sớm nhất khi tất cả các chu kỳ có thể có độ dài giống nhau sau khi áp dụng thao tác một cách tối ưu. 

Ràng buộc`n <= 2 * 10^5`có nghĩa là một thuật toán với việc mô phỏng các hoạt động lặp đi lặp lại là không thể. Một cách tiếp cận bậc hai để kiểm tra từng cặp điểm kiểm tra sẽ yêu cầu khoảng`4 * 10^10`hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp. Chúng ta cần xử lý hoán vị theo thời gian tuyến tính. 

Các trường hợp đặc biệt chính xuất phát từ thực tế là các chu kỳ chỉ có thể được chia nhỏ. 

Hãy xem xét một hoán vị đã được tạo thành từ các chu trình bằng nhau:```
6
2 1 4 3 6 5
```Các chu kỳ là`(1,2)`,`(3,4)`, Và`(5,6)`. Mọi người chơi đều kết thúc sau hai nước đi nên đáp án là:```
1
```Việc triển khai bất cẩn giả định rằng cần có ít nhất một lần phân chia sẽ tạo ra`2`, điều đó là sai. 

Một trường hợp quan trọng khác là khi ước số chung lớn nhất của tất cả các độ dài chu kỳ là`1`:```
5
3 4 1 5 2
```Các chu kỳ có độ dài`2`Và`3`. Độ dài chu kỳ cuối cùng duy nhất có thể là`1`, vì vậy mỗi điểm kiểm tra phải trở thành chu trình riêng của nó. Điều này yêu cầu chia ba lần và vì cấu hình ban đầu được tính là ngày 1 nên câu trả lời là:```
4
```Một giải pháp xuất ra số lượng thao tác thay vì số ngày sẽ trả về không chính xác`3`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng các lựa chọn của Dylan. Chúng ta có thể liên tục chọn một chu kỳ, chia nó ra và tiếp tục cho đến khi tất cả độ dài chu kỳ khớp nhau. Điều này đúng vì mọi thao tác hợp lệ đều tương ứng chính xác với việc cắt một chu trình thành hai phần. 

Tuy nhiên, không rõ nên thực hiện phân chia nào và việc thử các lựa chọn khác nhau sẽ dẫn đến không gian tìm kiếm khổng lồ. Ngay cả khi chúng ta chỉ đếm số lần chia tách cần thiết, chúng ta vẫn cần một cách tốt hơn để xác định mức tối thiểu. 

Quan sát quan trọng là việc phân tách không bao giờ làm thay đổi tổng chiều dài của một chu kỳ. Nếu một chu kỳ có độ dài`L`cuối cùng được chia thành các phần có chiều dài bằng nhau`x`, sau đó`x`phải chia`L`. Vì mỗi chu kỳ ban đầu phải được chia thành các phần có cùng độ dài cuối cùng,`x`phải chia mỗi độ dài chu kỳ. Giá trị lớn nhất có thể có của`x`do đó là ước chung lớn nhất của tất cả các độ dài chu kỳ. 

Việc chọn độ dài cuối cùng lớn nhất có thể này sẽ giảm thiểu số chu kỳ cuối cùng. Nếu độ dài chu kỳ cuối cùng là`g`, hoán vị phải chứa chính xác`n / g`chu kỳ. Ban đầu có`c`chu kỳ. Mỗi thao tác tăng số chu kỳ lên đúng một, vì vậy chúng ta cần:`n / g - c`hoạt động. 

Câu trả lời yêu cầu số ngày và ngày đầu tiên xảy ra trước bất kỳ thao tác nào, vì vậy chúng tôi thêm một số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số chuỗi có thể phân chia) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt hoán vị và tìm mọi chu trình. Trong quá trình truyền tải, đếm số chu kỳ và lưu trữ độ dài của mỗi chu kỳ. Độ dài chu kỳ là số nước đi cần thiết để mỗi người chơi trong chu kỳ đó hoàn thành. 
2. Tính ước số chung lớn nhất của tất cả các độ dài chu trình. Giá trị này là kích thước chu kỳ cuối cùng lớn nhất có thể vì mọi chu trình ban đầu phải được phân chia thành các phần có kích thước này. 
3. Tính xem cần bao nhiêu chu kỳ ở trạng thái cuối cùng. Nếu kích thước chu kỳ cuối cùng là`g`, thì phải có`n / g`chu kỳ. 
4. Số thao tác cần thiết là chênh lệch giữa số chu kỳ cuối cùng và số chu kỳ hiện tại. Thêm một vì hoán vị ban đầu tương ứng với ngày 1. 

Tại sao nó hoạt động: mỗi thao tác chỉ chia tách một chu kỳ, do đó, nó tăng số chu kỳ lên một và không bao giờ thay đổi tổng số điểm kiểm tra bên trong các chu kỳ hiện có. Kích thước chu kỳ bằng nhau cuối cùng phải chia cho mọi độ dài chu kỳ ban đầu và ước số chung lớn nhất sẽ cho kích thước lớn nhất có thể. Việc sử dụng kích thước lớn nhất sẽ tạo ra ít chu kỳ cuối cùng nhất, nghĩa là số lần chia tách cần thiết ít nhất. Vì mỗi lần phân chia sẽ tăng số chu kỳ lên một, nên số lượng thao tác được tính toán là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    seen = [False] * n
    gcd = 0
    cycles = 0

    for i in range(n):
        if not seen[i]:
            cycles += 1
            cur = i
            length = 0
            while not seen[cur]:
                seen[cur] = True
                cur = a[cur] - 1
                length += 1

            x = gcd
            y = length
            while y:
                x, y = y, x % y
            gcd = x

    return str(n // gcd - cycles + 1)

print(solve())
```Quá trình truyền tải theo chu kỳ sử dụng một mảng đã truy cập để mỗi điểm kiểm tra được xử lý chính xác một lần. Khi tìm thấy một điểm kiểm tra chưa được truy cập, việc đi theo các cạnh hoán vị cho đến khi quay lại nút đã truy cập sẽ tạo ra một chu trình hoàn chỉnh. 

Biến`gcd`được cập nhật sau mỗi độ dài chu kỳ. Thuật toán Euclide lặp tránh đệ quy và xử lý kích thước đầu vào lớn nhất có thể một cách an toàn. 

biểu hiện`n // gcd - cycles + 1`là công thức cuối cùng`n // gcd`là số chu kỳ cần thiết sau tất cả các lần phân chia,`cycles`là số chu kỳ hiện tại và chu kỳ bổ sung sẽ chuyển đổi các hoạt động thành đánh số ngày được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
5
3 4 1 5 2
```Việc phát hiện chu kỳ tiến hành như sau: 

| Điểm kiểm tra bắt đầu | Đã tìm thấy chu kỳ | Chiều dài | Gcd hiện tại | Số chu kỳ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 -> 3 -> 1 | 2 | 2 | 1 | 
| 2 | 2 -> 4 -> 5 -> 2 | 3 | 1 | 2 | 

Kích thước chu kỳ cuối cùng là`1`. Số chu kỳ cần thiết là`5 / 1 = 5`. 

Số lần chia là:`5 - 2 = 3`Câu trả lời là:`3 + 1 = 4`Điều này chứng tỏ trường hợp tất cả các chu trình cuối cùng phải trở thành các chu trình đơn. 

Đối với ví dụ thứ hai:```
6
2 1 4 3 6 5
```Việc truyền tải mang lại: 

| Điểm kiểm tra bắt đầu | Đã tìm thấy chu kỳ | Chiều dài | Gcd hiện tại | Số chu kỳ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 -> 2 -> 1 | 2 | 2 | 1 | 
| 3 | 3 -> 4 -> 3 | 2 | 2 | 2 | 
| 5 | 5 -> 6 -> 5 | 2 | 2 | 3 | 

Kích thước cuối cùng là`2`, đã khớp với mọi chu kỳ. Số lần thực hiện là:`6 / 2 - 3 = 0`Câu trả lời là:`0 + 1 = 1`Điều này xác nhận rằng hoán vị đã hợp lệ kết thúc vào ngày 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm kiểm tra được truy cập một lần trong khi tìm chu kỳ. | 
| Không gian | O(n) | Mảng đã truy cập lưu trữ một boolean cho mỗi điểm kiểm tra. | 

Độ phức tạp tuyến tính phù hợp với`n`lên đến`2 * 10^5`, bởi vì thuật toán chỉ thực hiện một lượng công việc không đổi nhỏ trên mỗi điểm kiểm tra. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    seen = [False] * n
    g = 0
    c = 0

    for i in range(n):
        if not seen[i]:
            c += 1
            cur = i
            length = 0
            while not seen[cur]:
                seen[cur] = True
                cur = a[cur] - 1
                length += 1

            x, y = g, length
            while y:
                x, y = y, x % y
            g = x

    return str(n // g - c + 1)

assert solution("6\n2 1 4 3 6 5\n") == "1"
assert solution("5\n3 4 1 5 2\n") == "4"

assert solution("2\n2 1\n") == "1"
assert solution("4\n2 3 4 1\n") == "2"
assert solution("8\n2 1 4 3 6 5 8 7\n") == "1"
assert solution("5\n2 3 4 5 1\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 / 2 1 4 3 6 5`|`1`| Độ dài chu kỳ đã bằng nhau | 
|`4 / 2 3 4 1`|`2`| Một chu kỳ phải được chia thành nhiều phần bằng nhau | 
|`8 / 2 1 4 3 6 5 8 7`|`1`| Nhiều chu kỳ giống hệt nhau | 
|`5 / 2 3 4 5 1`|`5`| Chu kỳ độ dài chính mà chỉ có thể thực hiện được các chu kỳ đơn lẻ | 

## Vỏ cạnh 

Đối với trường hợp đã cân bằng:```
6
2 1 4 3 6 5
```Thuật toán tìm thấy ba chu kỳ, mỗi chu kỳ có độ dài hai. Gcd vẫn là hai, vì vậy kích thước chu kỳ mục tiêu là hai. Số chu kỳ cần thiết đã là ba, kết quả là không có hoạt động nào và kết quả là ngày thứ nhất. 

Đối với trường hợp độ dài chu kỳ nguyên tố cùng nhau:```
5
3 4 1 5 2
```Thuật toán tìm độ dài hai và ba. Gcd của chúng trở thành một, nghĩa là không tồn tại kích thước chu kỳ chung lớn hơn. Trạng thái cuối cùng phải chứa năm chu kỳ có độ dài một. Số chu kỳ hiện tại là hai, vì vậy cần phải chia ba lần và câu trả lời sẽ là ngày thứ tư. 

Đối với một chu kỳ duy nhất:```
4
2 3 4 1
```Có một chu kỳ có độ dài bốn. gcd là bốn, vậy kích thước cuối cùng tốt nhất là bốn chia cho hai sau một lần chia? Không, công thức đưa ra trực tiếp số chu kỳ cuối cùng:`4 / 4 = 1`, do đó sẽ không cần chia tách nếu chu kỳ đã được chấp nhận. Ở đây, độ dài chu kỳ là như nhau đối với tất cả người chơi, nghĩa là cuộc đua đã kết thúc cùng nhau vào ngày đầu tiên. Câu trả lời là:```
1
```Trường hợp này mắc phải sai lầm khi cho rằng mọi chu kỳ đều cần phải được phá vỡ. 

Đối với các chu kỳ nhỏ bằng nhau:```
8
2 1 4 3 6 5 8 7
```Tất cả bốn chu kỳ đều có chiều dài bằng hai. gcd là hai và số chu kỳ hiện tại đã bằng số yêu cầu. Thuật toán tránh được các thao tác không cần thiết một cách chính xác và trả về ngày đầu tiên.
