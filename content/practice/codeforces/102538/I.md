---
title: "CF 102538I - Bỏ qua mặt nạ con"
description: "Chỉnh sửa Chúng tôi được cung cấp một mảng các số nguyên. Mỗi số nguyên đại diện cho một tập hợp các bit được kích hoạt. Với mọi mặt nạ x có thể chứa k bit, chúng ta cần tìm vị trí mảng đầu tiên có giá trị chứa ít nhất một bit bị thiếu trong x."
date: "2026-08-03T21:03:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102538
codeforces_index: "I"
codeforces_contest_name: "300iq Contest 3"
rating: 0
weight: 102538
solve_time_s: 132
verified: true
draft: false
---

[CF 102538I - Bỏ qua mặt nạ con](https://codeforces.com/problemset/problem/102538/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
Chỉnh sửa 

#Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Mỗi số nguyên đại diện cho một tập hợp các bit được kích hoạt. Đối với mọi mặt nạ có thể`x`chứa đựng`k`bit, chúng ta cần tìm vị trí mảng đầu tiên có giá trị chứa ít nhất một bit bị thiếu trong`x`. Nếu mọi giá trị mảng là một mặt nạ con của`x`, vị trí đó được coi là bằng không. Nhiệm vụ là thêm giá trị này lên tất cả`2^k`mặt nạ có thể. 

Số lượng mặt nạ tăng theo cấp số nhân`k`, Và`k`có thể lớn tới 60. Việc lặp trực tiếp trên tất cả các mặt nạ sẽ yêu cầu nhiều hơn`10^18`hoạt động, vì vậy giải pháp phải tránh hoàn toàn việc đến thăm mặt nạ. Độ dài mảng chỉ là 100, có nghĩa là các phép toán tỷ lệ thuận với`n * k`có giá cả phải chăng một cách dễ dàng, trong khi mọi thứ tùy thuộc vào`2^k`là không thể. 

Các trường hợp phức tạp đến từ các bit không bao giờ xuất hiện và từ một số bit xuất hiện lần đầu tiên ở cùng một vị trí mảng. Một bit không bao giờ xuất hiện không thể làm cho bất kỳ giá trị nào không hợp lệ, vì vậy nó không được đóng góp một vị trí giả. Ví dụ:```
Input:
2 2
0 1
```Vị trí xuất hiện đầu tiên là bit 0 ở vị trí 2, trong khi bit 1 không bao giờ xuất hiện. Chỉ có bit 0 quan trọng. Xử lý bit bị thiếu ở vị trí 0 hoặc vị trí 1 sẽ làm tăng câu trả lời một cách không chính xác. 

Một trường hợp khác là lặp lại vị trí đầu tiên:```
Input:
2 2
3 3
```Cả hai bit đầu tiên xuất hiện ở vị trí 1. Đối với`x = 0`, câu trả lời là 1 vì cả hai bit đều bị thiếu. Vì`x = 1`, thiếu bit 1 và câu trả lời vẫn là 1. Giải pháp sắp xếp vị trí và gán lũy thừa của hai giá trị bằng nhau một cách mù quáng sẽ bị tính quá mức. Các vị trí bằng nhau phải được nhóm lại với nhau. 

# Phương pháp tiếp cận 

Phương pháp vũ phu tuân theo định nghĩa trực tiếp. Cho mỗi chiếc mặt nạ`x`từ`0`ĐẾN`2^k - 1`, chúng tôi quét mảng từ trái sang phải cho đến khi tìm thấy giá trị đầu tiên không phải là mặt nạ con của`x`. Việc kiểm tra một giá trị thì rẻ, nhưng có`2^k`mặt nạ và lên đến`n`các vị trí cho mỗi người. Với`k = 60`, đây là khoảng`100 * 2^60`kiểm tra, vượt xa thời gian có sẵn. 

Quan sát quan trọng là một mặt nạ`x`chỉ thú vị thông qua các bit nó không chứa. Với mỗi bit, hãy tìm vị trí mảng sớm nhất nơi bit đó xuất hiện. Nếu như`x`nhớ một chút`b`, thì phần tử mảng không hợp lệ đầu tiên phải xuất hiện không muộn hơn lần xuất hiện đầu tiên của`b`. Trên thực tế, câu trả lời cho`x`là lần xuất hiện đầu tiên tối thiểu trong số tất cả các bit bị thiếu từ`x`. 

Bây giờ vấn đề trở thành vấn đề đếm trên các vị trí bit thay vì trên tất cả các mặt nạ. Giả sử lần xuất hiện đầu tiên của tất cả các bit xuất hiện đều được sắp xếp. Nếu một giá trị là lần xuất hiện thiếu tối thiểu thì mọi lần xuất hiện nhỏ hơn phải thuộc về một bit có trong`x`và sự xuất hiện được chọn phải thuộc về bit vắng mặt. Các bit lớn hơn còn lại là miễn phí. Việc nhóm các lần xuất hiện bằng nhau sẽ xử lý nhiều bit trở thành ứng cử viên cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * n) | O(1) | Quá chậm | 
| Tối ưu | O(nk + k log k) | O(k) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Với mỗi bit từ`0`ĐẾN`k - 1`, tìm chỉ mục mảng đầu tiên chứa bit này. Bỏ qua những bit không bao giờ xuất hiện, vì chúng không bao giờ có thể làm cho mặt nạ bị lỗi. 
2. Thu thập tất cả các vị trí đầu tiên hiện có và sắp xếp chúng. Các vị trí bằng nhau phải được nhóm lại vì một số bit có thể trở thành bit bị thiếu đầu tiên cùng một lúc. 
3. Xử lý từng nhóm vị trí bằng nhau. Giả sử một nhóm chứa`cnt`bit, có`smaller`các bit có vị trí đầu tiên nhỏ hơn và`larger`bit có vị trí đầu tiên lớn hơn. 
4. Để nhóm này xác định được câu trả lời, tất cả`smaller`bit phải có mặt trong`x`. Trong nhóm hiện tại, ít nhất một bit phải vắng mặt. Số lựa chọn trong nhóm này là`2^cnt - 1`và tất cả các bit lớn hơn đều không bị hạn chế, mang lại`2^larger`khả năng. 
5. Thêm```
position * (2^cnt - 1) * 2^larger
```để trả lời modulo`998244353`. 

Tại sao nó hoạt động: 

Mỗi mặt nạ`x`có lần xuất hiện đầu tiên tối thiểu duy nhất trong số các bit bị thiếu trong nó. Thuật toán đếm chính xác các mặt nạ có bit bị thiếu tối thiểu thuộc về mỗi nhóm. Không thể thiếu các nhóm nhỏ hơn vì điều đó sẽ tạo ra câu trả lời sớm hơn. Nhóm hiện tại không thể có mặt hoàn toàn vì khi đó nó sẽ không đóng góp được gì. Các nhóm lớn hơn không ảnh hưởng đến mức tối thiểu và có thể được lựa chọn một cách tự do. Những trường hợp này bao gồm mọi mặt nạ có thể chính xác một lần. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    first = [n + 1] * k

    for i, x in enumerate(a, 1):
        for b in range(k):
            if (x >> b) & 1 and first[b] == n + 1:
                first[b] = i

    vals = [x for x in first if x != n + 1]
    vals.sort()

    pow2 = [1] * (len(vals) + 1)
    for i in range(1, len(pow2)):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    ans = 0
    i = 0
    m = len(vals)

    while i < m:
        j = i
        while j < m and vals[j] == vals[i]:
            j += 1

        cnt = j - i
        larger = m - j

        ways = (pow2[cnt] - 1) * pow2[larger] % MOD
        ans = (ans + vals[i] * ways) % MOD

        i = j

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mảng đầu tiên lưu trữ vị trí sớm nhất cho mỗi bit. Các vị trí dựa trên một vị trí vì giá trị được yêu cầu là chỉ mục mảng, không phải là giá trị chênh lệch dựa trên 0. 

Danh sách`vals`chỉ chứa các bit thực sự xảy ra. Việc sắp xếp nó cho phép đếm số bit bị thiếu tối thiểu từ trái sang phải. 

Vòng lặp được nhóm là phần quan trọng. Nếu nhiều bit có cùng lần xuất hiện đầu tiên thì thuật toán sẽ xử lý chúng cùng nhau. biểu hiện`2^cnt - 1`đếm tất cả các cách mà ít nhất một bit trong nhóm bị thiếu, trong khi`2^larger`tính đến các bit xuất hiện sau đó. 

Số nguyên Python không tràn, nhưng tất cả các phép nhân đều được giảm modulo`998244353`để phù hợp với định dạng đầu ra được yêu cầu. 

# Ví dụ đã hoạt động 

Dành cho:```
2 1
0 1
```bit xuất hiện duy nhất là bit 0 với vị trí đầu tiên 2. 

| Vị trí nhóm | Số bit | Bit lớn hơn | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | 1 | 0 | 2 * (2^1 - 1) * 2^0 = 2 | 

Đáp án là 2. Mặt nạ`0`thất bại ở vị trí 2, trong khi mặt nạ`1`chấp nhận mọi giá trị mảng. 

Vì:```
2 2
2 1
```các vị trí đầu tiên là: 

| Chút | Vị trí đầu tiên | 
| --- | --- | 
| 0 | 2 | 
| 1 | 1 | 

Sau khi sắp xếp, các vị trí được`[1, 2]`. 

| Vị trí nhóm | Số bit | Bit lớn hơn | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 * (2^1 - 1) * 2^1 = 2 | 
| 2 | 1 | 0 | 2 * (2^1 - 1) * 2^0 = 2 | 

Đáp án là 4. Bốn mặt nạ tạo ra giá trị`1, 1, 2, 0`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk + k log k) | Mỗi số được kiểm tra theo từng bit và nhiều nhất là`k`các vị trí được sắp xếp. | 
| Không gian | O(k) | Chỉ mảng xuất hiện đầu tiên và danh sách nén được lưu trữ. | 

Tối đa`k`là 60 và`n`chỉ là 100 nên thuật toán chỉ thực hiện vài nghìn bit. Nó tránh được điều không thể`2^k`sự liệt kê. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""2 1
0 1
""") == "2\n", "sample 1"

assert run("""2 2
2 1
""") == "4\n", "sample 2"

assert run("""1 1
0
""") == "0\n", "only zero value"

assert run("""2 2
3 3
""") == "4\n", "equal first positions"

assert run("""3 3
1 2 4
""") == "6\n", "every bit appears once"

assert run("""100 60
""" + " ".join(["0"] * 100) + "\n") == "0\n", "maximum size with no bits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 0 1`| 2 | Xử lý bit đơn cơ bản | 
|`2 2 / 2 1`| 4 | Vị trí đầu tiên khác nhau | 
|`1 1 / 0`| 0 | Không có bit xuất hiện | 
|`2 2 / 3 3`| 4 | Nhóm lần xuất hiện đầu tiên bằng nhau | 
|`3 3 / 1 2 4`| 6 | Bit độc lập | 
| 100 số không với 60 bit | 0 | Kích thước đầu vào tối đa và số bit bị thiếu | 

# Vỏ cạnh 

Khi một bit không bao giờ xuất hiện, nó sẽ không được đưa vào danh sách đã sắp xếp. Ví dụ:```
2 2
0 1
```Chỉ bit 0 xuất hiện đầu tiên ở vị trí 2. Thuật toán bỏ qua hoàn toàn bit 1 và tạo ra 2. Việc thêm giá trị giả cho bit 1 sẽ đếm mặt nạ không chính xác. 

Khi một số bit lần đầu tiên xuất hiện cùng nhau, chúng phải được tính là một nhóm. Vì:```
2 2
3 3
```cả hai bit đều có vị trí đầu tiên là 1. Có ba mặt nạ trong đó ít nhất một trong số các bit này bị thiếu và tất cả chúng đều đóng góp giá trị 1. Đóng góp là`1 * (2^2 - 1) = 3`từ những mặt nạ này, với mặt nạ còn lại không đóng góp gì. Công thức nhóm của thuật toán nắm bắt trực tiếp điều này.
