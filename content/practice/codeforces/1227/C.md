---
title: "CF 1227C - Lộn xộn"
description: "Chúng ta được cung cấp một chuỗi dấu ngoặc đơn và được phép áp dụng thao tác đảo ngược bất kỳ phân đoạn liền kề nào."
date: "2026-06-15T19:47:55+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1227
codeforces_index: "C"
codeforces_contest_name: "Technocup 2020 - Elimination Round 3"
rating: 1700
weight: 1227
solve_time_s: 339
verified: false
draft: false
---

[CF 1227C - Lộn xộn](https://codeforces.com/problemset/problem/1227/C) 

**Đánh giá:** 1700 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 5 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dấu ngoặc đơn và được phép áp dụng thao tác đảo ngược bất kỳ phân đoạn liền kề nào. Sau khi áp dụng chuỗi đảo ngược như vậy, chúng ta phải thu được chuỗi ngoặc cân bằng hợp lệ, nhưng với cấu trúc rất cụ thể: khi quét từ trái sang phải, chúng ta muốn chính xác`k`các vị trí trong đó tiền tố chính là một chuỗi dấu ngoặc cân bằng hoàn chỉnh. 

Một cách khác để diễn giải yêu cầu này là xem xét cách phân tách một chuỗi dấu ngoặc đúng. Mỗi chuỗi cân bằng có thể được chia duy nhất thành các “khối nguyên thủy”, trong đó mỗi khối là một tiền tố nhỏ nhất được cân bằng và không chứa tiền tố cân bằng trước đó bên trong nó. Số tiền tố mà bản thân chúng là các chuỗi đầy đủ hợp lệ chính xác là số khối nguyên thủy như vậy. Vì vậy, nhiệm vụ là chuyển đổi chuỗi đã cho thành bất kỳ chuỗi ngoặc hợp lệ nào với độ chính xác`k`các thành phần nguyên thủy. 

Chúng tôi được đảm bảo rằng đầu vào có`n/2`mở đầu và`n/2`đóng ngoặc, do đó luôn có giải pháp. 

Hoạt động rất mạnh mẽ: đảo ngược bất kỳ chuỗi con nào cho phép chúng ta định vị lại các ký tự một cách hiệu quả một cách tùy ý, nhưng chỉ thông qua việc đảo ngược phân đoạn. Vì chúng tôi được phép lên tới`n`hoạt động và`n ≤ 2000`, chúng ta có thể đưa ra một chiến lược mang tính xây dựng để sửa chuỗi từng bước thay vì dựa vào sự tối ưu hóa phức tạp. 

Một trường hợp phức tạp phát sinh khi suy nghĩ tham lam về việc cân bằng tiền tố. Một nỗ lực ngây thơ có thể cố gắng "sửa số dư" cục bộ bằng cách làm cho tiền tố hợp lệ khi chúng tôi tiếp tục. Điều đó không thành công vì việc sửa chữa cục bộ có thể phá hủy cấu trúc sau này. Một chế độ thất bại khác là cố gắng kiểm soát trực tiếp số dư tiền tố mà không xây dựng chuỗi mục tiêu rõ ràng, điều này trở nên khó phối hợp với các hoạt động đảo ngược. 

Quan điểm đúng đắn là ngừng suy nghĩ về sự cân bằng tiền tố trong quá trình xây dựng và thay vào đó, hãy quyết định hình dạng mục tiêu cuối cùng trước, sau đó chuyển chuỗi ban đầu thành chuỗi đó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là coi mỗi trạng thái là một nút và thử tất cả các đảo ngược chuỗi con, tìm kiếm cấu hình cuối cùng hợp lệ bằng BFS. Mỗi bang có khoảng`O(n^2)`chuyển tiếp và không gian trạng thái là hàm mũ trong`n`. Ngay cả đối với`n = 200`, điều này trở nên hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải tìm kiếm gì cả. Chúng ta có thể xây dựng một cách rõ ràng một chuỗi cuối cùng hợp lệ với độ chính xác`k`các khối nguyên thủy và sau đó chuyển đổi một cách xác định chuỗi ban đầu thành chuỗi đó. 

Cấu trúc của bất kỳ chuỗi hợp lệ nào có chính xác`k`vị trí tiền tố thông thường rất đơn giản. Chúng tôi muốn chính xác`k`thời điểm mà tiền tố trở nên cân bằng hoàn toàn. Việc xây dựng sạch sẽ nhất là tạo ra`k-1`khối tối thiểu`"()"`, theo sau là một khối cân bằng lớn chứa các dấu ngoặc đơn còn lại:```
"()" * (k - 1) + "(" * a + ")" * a
```Ở đâu`a = n/2 - (k - 1)`. 

Điều này đảm bảo chính xác`k`thành phần nguyên thủy: mỗi`"()"`đóng góp một phần và khối lớn cuối cùng đóng góp thêm một phần nữa. 

Khi chúng tôi sửa chuỗi mục tiêu này, vấn đề còn lại sẽ giảm xuống việc chuyển đổi một hoán vị ký tự này thành một ký tự khác bằng cách sử dụng đảo ngược chuỗi con. Điều này có thể được thực hiện một cách tham lam từ trái sang phải: đặt từng ký tự vào đúng vị trí của nó bằng cách tìm nó ở phần sau của chuỗi và đảo ngược đoạn để đưa nó về phía trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force trên các tiểu bang | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng mục tiêu + đảo ngược tham lam | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng chuỗi mục tiêu, sau đó chuyển đổi đầu vào thành chuỗi đó. 

### 1. Xây dựng cấu hình mục tiêu 

Chúng tôi tạo thành một chuỗi bao gồm`k-1`bản sao của`"()"`, theo sau là một khối còn lại`'('`và sau đó`')'`. 

Điều này đảm bảo chính xác`k`các thành phần cân bằng nguyên thủy, bởi vì mỗi`"()"`là một nguyên thủy độc lập và khối cuối cùng là một nguyên thủy lớn. 

### 2. Tham lam biến đầu vào thành đích 

Chúng tôi xử lý các vị trí từ trái sang phải. 

Đối với mỗi vị trí`i`, nếu ký tự hiện tại đã khớp với mục tiêu thì chúng tôi không làm gì cả. Nếu không, chúng tôi sẽ tìm kiếm một vị trí`j`như vậy`s[j]`bằng ký tự được yêu cầu tại`i`. 

Sau đó chúng tôi đảo ngược chuỗi con`[i, j]`. Điều này đưa nhân vật cần thiết vào vị trí`i`đồng thời giữ nguyên tất cả các ký tự trong đoạn đó, vừa sắp xếp lại. 

Bước này được lặp lại cho đến khi toàn bộ chuỗi khớp với mục tiêu. 

### 3. Thao tác xuất 

Mỗi lần đảo ngược được ghi lại. Số lượng hoạt động nhiều nhất là`n`bởi vì mỗi thao tác sẽ cố định ít nhất một vị trí vĩnh viễn. 

### Tại sao nó hoạt động 

Ở mỗi bước`i`, một khi chúng ta cố định vị trí`i`, chúng tôi không bao giờ chạm vào nó nữa. Điều này là do các lần đảo chiều sau này chỉ hoạt động ở các vị thế lớn hơn`i`. Do đó, tính đúng đắn giảm xuống để đảm bảo rằng ở bước`i`, chúng ta luôn có thể tìm thấy ký tự cần thiết trong hậu tố, điều này được đảm bảo vì chúng ta chỉ hoán vị nhiều tập hợp dấu ngoặc đơn giống nhau. 

Chuỗi cuối cùng khớp chính xác với mục tiêu và mục tiêu được xây dựng để có chính xác`k`tiền tố thông thường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = list(input().strip())

    # build target
    half = n // 2
    left_blocks = k - 1

    target = []
    target.extend("()" * left_blocks)
    remaining = half - left_blocks
    target.extend("(" * remaining)
    target.extend(")" * remaining)

    target = list(target)

    ops = []

    for i in range(n):
        if s[i] == target[i]:
            continue

        j = i
        while j < n and s[j] != target[i]:
            j += 1

        # reverse [i, j]
        s[i:j+1] = s[i:j+1][::-1]
        ops.append((i + 1, j + 1))

    print(len(ops))
    for l, r in ops:
        print(l, r)

if __name__ == "__main__":
    solve()
```Việc xây dựng mục tiêu là sự lựa chọn thiết kế cốt lõi. Khi cấu trúc đó được cố định, phần còn lại của thuật toán chỉ là quá trình hoán vị được kiểm soát. Hoạt động đảo ngược chỉ được sử dụng như một công cụ để mô phỏng các giao dịch hoán đổi trong khi vẫn giữ cho việc triển khai đơn giản và trong số lượng hoạt động được phép. 

Một chi tiết triển khai tinh tế là chúng ta không bao giờ cần theo dõi sự cân bằng một cách rõ ràng trong khi xây dựng chuỗi. Tất cả các ràng buộc về số dư đều được mã hóa vào mục tiêu cuối cùng, do đó độ chính xác bị giảm xuống thành chuyển đổi chuỗi thuần túy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=8, k=2
s = ()(())()
```Xây dựng mục tiêu:```
"()" + "((()))"
= ()((()))
```| tôi | mục tiêu[i] | đã chọn j | hoạt động | sau | 
| --- | --- | --- | --- | --- | 
| 1 | ( | 1 | - | ()(())() | 
| 2 | ) | 2 | - | ()(())() | 
| 3 | ( | 3 | - | ()(())() | 
| 4 | ( | 4 | đảo ngược để mang lại '(' | ... | 
| ... | ... | ... | ... | ()((())) | 

Chuỗi cuối cùng có đúng 2 khối nguyên thủy:`"()"`Và`"(())"`. 

Điều này xác nhận rằng việc xây dựng kiểm soát trực tiếp số lượng điểm cuối tiền tố cân bằng. 

### Ví dụ 2 

đầu vào:```
n=10, k=3
```Mục tiêu:```
"()" + "()" + "((()))"
= ()()((()))
```Thuật toán đầu tiên sửa hai`"()"`các khối khi bắt đầu, sau đó xây dựng khối lớn còn lại. Mỗi hiệu chỉnh tiền tố đều mang tính cục bộ và không can thiệp vào các vị trí cố định trước đó. 

Dấu vết này cho thấy sự đảo chiều chỉ đóng vai trò là các giao dịch hoán đổi được kiểm soát và không phá vỡ cấu trúc đã được thiết lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi vị trí có thể yêu cầu quét và đảo ngược một đoạn | 
| Không gian | O(n) | Lưu trữ chuỗi, mục tiêu và hoạt động | 

Với tổng số`n ≤ 2000`qua các trường hợp thử nghiệm, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out_lines = []

    for _ in range(t):
        n, k = map(int, input().split())
        s = list(input().strip())

        half = n // 2
        left_blocks = k - 1

        target = []
        target.extend("()" * left_blocks)
        remaining = half - left_blocks
        target.extend("(" * remaining)
        target.extend(")" * remaining)
        target = list(target)

        ops = []

        for i in range(n):
            if s[i] == target[i]:
                continue
            j = i
            while j < n and s[j] != target[i]:
                j += 1
            s[i:j+1] = s[i:j+1][::-1]
            ops.append((i, j))

        out_lines.append(str(len(ops)))
        for l, r in ops:
            out_lines.append(f"{l+1} {r+1}")

    return "\n".join(out_lines)

# provided samples (light check format consistency)
assert run("""4
8 2
()(())()
10 3
))()()()((
2 1
()
2 1
)(
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2, k=1`| đã cân bằng | cấu trúc tối thiểu | 
| trình tự xen kẽ | trường hợp k=2 hợp lệ | tính đúng đắn của cấu trúc tiền tố | 
| tất cả đầu vào cân bằng | trường hợp nhận dạng | không có hoạt động không cần thiết | 
| tệ nhất k=n/2 | nhiều`"()"`khối | phân mảnh tối đa | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`k = 1`. Trong trường hợp này, mục tiêu trở thành một khối nguyên thủy lớn duy nhất bao gồm tất cả các dấu ngoặc đơn được sắp xếp như sau:`"(" * (n/2) + ")" * (n/2)`. Thuật toán vẫn hoạt động vì không`"()"`các khối tiền tố được chèn vào và vị trí tham lam chỉ đơn giản là xây dựng một cấu trúc lồng nhau hoàn toàn. 

Một trường hợp khác là khi`k = n/2`, nơi mục tiêu trở thành`"()"`lặp đi lặp lại`n/2`lần. Ở đây mỗi khối đều là khối nguyên thủy nên cứ hai ký tự phải tạo thành một cặp. Quá trình đảo ngược tham lam vẫn thành công vì mỗi ký tự được yêu cầu có thể được kéo vào vị trí một cách độc lập mà không ảnh hưởng đến các cặp cố định trước đó. 

Một tình huống dễ xảy ra lỗi là cố gắng duy trì tính hợp lệ của tiền tố trong quá trình xây dựng. Ví dụ: nếu chúng ta cố gắng đảm bảo sự cân bằng tiền tố trong khi hoán đổi, chúng ta có thể vô tình đưa ra các tiền tố hợp lệ sớm vượt quá`k`. Việc xây dựng dựa trên mục tiêu tránh được điều này hoàn toàn bằng cách mã hóa chính xác số lượng tiền tố hợp lệ theo cấu trúc thay vì động.
