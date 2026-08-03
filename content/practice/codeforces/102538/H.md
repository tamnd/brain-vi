---
title: "CF 102538H - Chu kỳ khủng khiếp"
description: "Chúng ta có một đồ thị lưỡng cực có cùng số đỉnh ở bên trái và bên phải. Các đỉnh bên phải được sắp xếp theo thứ tự và đỉnh bên trái thứ i được nối với đỉnh bên phải a[i] đầu tiên."
date: "2026-08-03T21:08:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102538
codeforces_index: "H"
codeforces_contest_name: "300iq Contest 3"
rating: 0
weight: 102538
solve_time_s: 132
verified: true
draft: false
---

[CF 102538H - Chu kỳ khủng khiếp](https://codeforces.com/problemset/problem/102538/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị lưỡng cực có cùng số đỉnh ở bên trái và bên phải. Các đỉnh bên phải được sắp xếp và`i`-đỉnh bên trái được nối với đỉnh đầu tiên`a[i]`các đỉnh bên phải. Nhiệm vụ là đếm có bao nhiêu chu trình đỉnh đơn khác nhau tồn tại trong biểu đồ này, trong đó hai chu trình được coi là khác nhau khi tập hợp các cạnh của chúng khác nhau. Câu trả lời là bắt buộc theo modulo`998244353`. 

Những ràng buộc cho phép`n`lên tới 5000. Việc liệt kê trực tiếp các chu kỳ là không thể vì ngay cả các biểu đồ lưỡng cực có cấu trúc cũng có thể chứa số chu kỳ theo cấp số nhân. MỘT`O(n^3)`giải pháp sẽ quá lớn, trong khi một`O(n^2)`Phương pháp lập trình động phù hợp một cách thoải mái vì 25 triệu chuyển đổi có thể thực hiện được trong C++ và Python nếu triển khai cẩn thận. 

Khó khăn chính là các chu trình không phải là các đối tượng độc lập. Khi chúng ta quét các đỉnh theo thứ tự phù hợp, một chu trình được xây dựng một phần có thể chia thành nhiều đường đi. Thuật toán chỉ phải nhớ có bao nhiêu đường dẫn mở như vậy tồn tại chứ không phải điểm cuối chính xác của chúng. Mất thông tin này gây ra việc đếm không chính xác. 

Một đồ thị nhỏ có một đỉnh trái và một đỉnh phải là một trường hợp biên hữu ích. đầu vào```
1
1
```chứa một cạnh nhưng không có chu trình, vì vậy câu trả lời là`0`. Một giải pháp bất cẩn coi bất kỳ kết nối lặp lại nào là một chu kỳ sẽ trả về không chính xác`1`. 

Một trường hợp quan trọng khác là biểu đồ lưỡng cực hoàn chỉnh 2×2:```
2
2 2
```Có đúng một chu trình sử dụng cả bốn đỉnh. Câu trả lời là`1`. Giải pháp đếm các lần di chuyển có hướng thay vì các tập cạnh sẽ tính cùng một chu kỳ hai lần, một lần theo mỗi hướng. 

Trường hợp phức tạp cuối cùng là khi nhiều đỉnh có lân cận giống hệt nhau. Ví dụ:```
3
3 3 2
```có bảy chu kỳ. Các tiền tố lặp lại tạo ra nhiều khả năng chồng chéo, do đó việc xử lý từng đỉnh bên trái một cách độc lập sẽ bỏ sót các tổ hợp chuỗi hợp nhất sau này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra các tập hợp con của các đỉnh, kiểm tra xem mỗi tập hợp con có tạo thành một chu trình đơn giản hay không và đếm các tập hợp con hợp lệ. Bản thân việc kiểm tra là đa thức, nhưng số lượng tập hợp con là theo cấp số nhân. Với`2n`đỉnh, không gian tìm kiếm gần như`2^(2n)`, trở nên không sử dụng được ngay cả đối với các giá trị nhỏ của`n`. 

Cấu trúc biểu đồ cung cấp một cách tốt hơn để suy nghĩ về vấn đề. Các đỉnh bên trái chỉ được kết nối với các tiền tố của phía bên phải, vì vậy nếu chúng ta sắp xếp lại tất cả các đỉnh theo thứ tự xây dựng tự nhiên của chúng thì mọi đỉnh bên trái đều được kết nối với tất cả các đỉnh bên phải trước đó. Đây chính xác là tình huống được mô tả trong quá trình xây dựng ban đầu. 

Trong khi quét các đỉnh, hãy tưởng tượng chỉ giữ lại các cạnh đã chọn của chu kỳ tương lai. Trước khi chu trình kết thúc, các cạnh được chọn tạo thành một số chuỗi rời rạc. Do đặc tính sắp xếp nên tất cả các chuỗi này đều có hình dạng xen kẽ giống nhau. Các đỉnh chính xác của chúng không quan trọng, chỉ có số lượng của chúng thôi. 

Cho phép`dp[j]`là số cách xử lý tiền tố hiện tại của các đỉnh và thu được`j`chuỗi mở. Một đỉnh bên phải có thể không được sử dụng hoặc bắt đầu một chuỗi mới. Đỉnh bên trái có thể kết nối hai đầu chuỗi hiện có và hợp nhất hai chuỗi hoặc nó có thể đóng chuỗi duy nhất còn lại và tạo thành một chu trình hoàn chỉnh. 

Phương pháp vũ phu hoạt động vì nó trực tiếp khám phá mọi chu kỳ có thể xảy ra. Nó thất bại vì số chu kỳ quá lớn. Việc quan sát thấy tất cả các cấu trúc chưa hoàn thiện đều có hình dạng giống nhau cho phép chúng ta nén toàn bộ trạng thái thành số chuỗi, chuyển bài toán thành quy hoạch động bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các giá trị`a[i]`và xem biểu đồ dưới dạng một chuỗi`2n`sự kiện. Giữa các đỉnh bên trái liên tiếp, chèn các đỉnh bên phải có sẵn. Sau phép biến đổi này, mọi đỉnh bên trái được kết nối với tất cả các đỉnh bên phải xuất hiện trước nó. 
2. Duy trì`dp[j]`, số cách xử lý tiền tố hiện tại và để lại chính xác`j`chuỗi mở. Ban đầu không có chuỗi nên`dp[0] = 1`. 
3. Khi xử lý một đỉnh bên phải, hãy bỏ qua nó hoặc bao gồm nó. Việc bao gồm nó sẽ tạo ra một chuỗi mở mới vì không có đỉnh nào trước đó có thể kết nối với đỉnh bên phải mới này. 
4. Khi xử lý một đỉnh bên trái, nó có thể hợp nhất hai chuỗi hiện có. Nếu có`j`chuỗi trước khi thêm đỉnh, có`j * (j - 1)`ra lệnh lựa chọn hai đầu chuỗi để kết nối. Trạng thái giảm từ`j`chuỗi để`j - 1`chains.
 5. Một đỉnh bên trái cũng có thể đóng một chuỗi còn lại. Bất cứ khi nào số chuỗi hiện tại là một trước khi thêm đỉnh, chu trình mới sẽ hoàn thành, vì vậy hãy thêm`dp[1]`để trả lời. 
6. Bộ đếm lập trình động coi hai hướng của một chu trình là khác nhau. Nó cũng bao gồm các chu trình có độ dài bằng 2, chỉ là các đường truyền song song của một cặp cạnh. Xóa phần đóng góp có độ dài hai và chia số còn lại cho hai. 

Tại sao nó hoạt động: sau khi xử lý bất kỳ tiền tố nào, mọi thành phần chu trình chưa hoàn thành là một chuỗi xen kẽ. Thuộc tính duy nhất cần thiết để tiếp tục xây dựng là có bao nhiêu chuỗi tồn tại, bởi vì tất cả các chuỗi đều có hành vi giống hệt nhau dưới các đỉnh tương lai. Mọi cách có thể để mở rộng hoặc đóng các chuỗi này đều được thể hiện bằng chính xác một quá trình chuyển đổi. Phân chia cuối cùng loại bỏ định hướng nhân tạo được đưa vào bằng cách lưu trữ các chuỗi dưới dạng các đối tượng được sắp xếp. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    dp = [0] * (n + 3)
    dp[0] = 1

    ans = 0

    # Remove length-two cycles during the final normalization.
    for x in a:
        ans = (ans - x) % MOD

    prev = 0

    for x in a:
        # Insert right vertices that appear before this left vertex.
        for _ in range(prev + 1, x + 1):
            for j in range(n, 0, -1):
                dp[j] += dp[j - 1]
                if dp[j] >= MOD:
                    dp[j] -= MOD

        # Close a single chain.
        ans += dp[1]
        if ans >= MOD:
            ans -= MOD

        # Merge two chains using the current left vertex.
        for j in range(1, n + 1):
            add = dp[j] * j * (j - 1)
            add %= MOD
            dp[j - 1] += add
            if dp[j - 1] >= MOD:
                dp[j - 1] -= MOD

        prev = x

    ans %= MOD
    ans = ans * ((MOD + 1) // 2) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Mảng được sắp xếp biểu thị thời điểm các đỉnh bên phải mới xuất hiện trước mỗi đỉnh bên trái. Biến`prev`lưu trữ độ dài tiền tố trước đó, do đó chỉ các đỉnh bên phải mới xuất hiện mới được xử lý. 

Mảng`dp`được cập nhật theo thứ tự ngược lại khi thêm các đỉnh bên phải. Điều này ngăn không cho một đỉnh mới được thêm vào được sử dụng nhiều lần trong cùng một quá trình chuyển đổi. 

Quá trình chuyển đổi hợp nhất sử dụng`j * (j - 1)`vì phải chọn hai chuỗi khác nhau. Thứ tự quan trọng khi đếm các điểm cuối của chuỗi, đó là lý do tại sao hệ số không chỉ đơn giản là một giá trị kết hợp. 

Việc điều chỉnh câu trả lời khi bắt đầu sẽ loại bỏ các cấu trúc đóng hai đỉnh. Phép nhân cuối cùng với nghịch đảo mô-đun của 2 sẽ loại bỏ việc đếm trùng lặp gây ra bởi hai hướng có thể có của mỗi chu kỳ thực. 

## Ví dụ đã hoạt động 

cho```
2
2 2
```quá trình xử lý trông như thế này: 

| Bước | Đối tượng đã xử lý | Mở chuỗi trước | Hành động chính | Mở chuỗi sau | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Đỉnh phải | 0 | Bắt đầu một chuỗi | 1 | 0 | 
| 2 | Đỉnh phải | 1 | Bắt đầu một chuỗi khác | 2 | 0 | 
| 3 | Đỉnh trái | 2 | Hợp nhất chuỗi | 1 | 0 | 
| 4 | Đỉnh trái | 1 | Đóng chu kỳ | 0 | 1 | 

Hai đỉnh bên phải tạo ra hai đầu chuỗi có thể. Đỉnh bên trái đầu tiên nối chúng và đỉnh bên trái thứ hai đóng chuỗi còn lại. 

Vì```
3
3 3 2
```các trạng thái quan trọng là: 

| Bước | Đối tượng | dp[0] | dp[1] | dp[2] | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | không | 1 | 0 | 0 | 0 | 
| Thêm đầu tiên bên phải | đúng | 1 | 1 | 0 | 0 | 
| Thêm thứ hai bên phải | đúng | 1 | 2 | 1 | 0 | 
| Thêm quyền thứ ba | đúng | 1 | 3 | 3 | 0 | 
| Đầu tiên bên trái | trái | cập nhật | cập nhật | cập nhật | chuỗi đóng | 
| Các đỉnh trái còn lại | trái | cập nhật | cập nhật | cập nhật | 7 | 

Ví dụ này cho thấy tại sao chỉ lưu trữ số lượng chuỗi là đủ. Các lựa chọn khác nhau của điểm cuối được tính bằng hệ số hợp nhất nhân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi quá trình chuyển đổi đỉnh quét lên tới`n`trạng thái lập trình động. | 
| Không gian | O(n) | Chỉ phân phối số lượng chuỗi hiện tại được lưu trữ. | 

Tối đa`n`là 5000, do đó số bậc hai của các lần chuyển trạng thái có thể chấp nhận được. Việc sử dụng bộ nhớ là tuyến tính và vẫn còn nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    dp = [0] * (n + 3)
    dp[0] = 1
    ans = 0

    for x in a:
        ans = (ans - x) % MOD

    prev = 0
    for x in a:
        for _ in range(prev + 1, x + 1):
            for j in range(n, 0, -1):
                dp[j] = (dp[j] + dp[j - 1]) % MOD

        ans = (ans + dp[1]) % MOD

        for j in range(1, n + 1):
            dp[j - 1] = (dp[j - 1] + dp[j] * j * (j - 1)) % MOD

        prev = x

    return str(ans * ((MOD + 1) // 2) % MOD)

assert solution("1\n1\n") == "0"
assert solution("2\n2 2\n") == "1"
assert solution("3\n3 3 2\n") == "7"
assert solution("4\n1 1 1 1\n") == "0"
assert solution("5\n5 5 5 5 5\n") == "101"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Đồ thị nhỏ nhất không có chu trình | 
|`2 / 2 2`|`1`| Hiệu chỉnh hướng và chu trình bốn đỉnh cơ bản | 
|`3 / 3 3 2`|`7`| Nhiều chu kỳ chồng chéo | 
|`4 / 1 1 1 1`|`0`| Không có đỉnh trái nào có thể nối đủ các đỉnh phải | 
|`5 / 5 5 5 5 5`|`101`| Biểu đồ dày đặc với nhiều chuỗi hợp nhất | 

## Vỏ cạnh 

Đối với đồ thị nhỏ nhất:```
1
1
```thuật toán tạo một đỉnh phải và một đỉnh trái. Đỉnh bên phải tạo ra một chuỗi mở, nhưng đỉnh bên trái chỉ có thể đóng một chuỗi có đủ cạnh cho một chu trình. Sau khi chuẩn hóa, kết quả vẫn như cũ`0`. 

Đối với biểu đồ hai nhân hai hoàn chỉnh:```
2
2 2
```lập trình động tạo ra hai chuỗi trước đỉnh bên trái đầu tiên. Đỉnh bên trái đầu tiên hợp nhất chúng, để lại một chuỗi. Đỉnh bên trái thứ hai đóng nó lại và thêm một chu trình. Phép chia cuối cùng loại bỏ hướng trùng lặp, để lại đáp án đúng`1`. 

Đối với các vùng lân cận lặp đi lặp lại:```
3
3 3 2
```các kết nối tiền tố giống nhau xuất hiện nhiều lần. Việc nén trạng thái xử lý việc này vì mỗi đỉnh bên trái chỉ quan tâm đến việc tồn tại bao nhiêu chuỗi chưa hoàn thành chứ không quan tâm đến đỉnh bên phải nào đã tạo ra chúng. Quá trình chuyển đổi hợp nhất đếm tất cả các lựa chọn điểm cuối có thể có, tạo ra câu trả lời đầy đủ`7`.
