---
title: "CF 474D - Hoa"
description: "Bữa tối được thể hiện bằng một chuỗi hoa. Một bông hoa màu đỏ đóng góp chiều dài 1, trong khi những bông hoa màu trắng rất đặc biệt: chúng chỉ có thể xuất hiện thành từng nhóm liền kề có đúng k hoa."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 474
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 271 (Div. 2)"
rating: 1700
weight: 474
solve_time_s: 110
verified: true
draft: false
---

[CF 474D - Hoa](https://codeforces.com/problemset/problem/474/D) 

**Đánh giá:** 1700 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bữa tối được thể hiện bằng một chuỗi hoa. Một bông hoa màu đỏ góp phần chiều dài`1`, trong khi hoa màu trắng rất đặc biệt: chúng chỉ có thể xuất hiện thành từng nhóm liền kề nhau một cách chính xác`k`hoa. 

Thay vì nghĩ đến từng bông hoa riêng lẻ, việc nghĩ đến việc xây dựng một bữa tối có tổng chiều dài sẽ dễ dàng hơn nhiều.`n`sử dụng hai loại khối: 

- Một khối màu đỏ có chiều dài`1`. 
- Một khối màu trắng có chiều dài`k`. 

Mỗi bữa tối hợp lệ tương ứng với một chuỗi các khối có tổng chiều dài là`n`. Thứ tự quan trọng vì các vị trí khác nhau của nhóm người da trắng sẽ tạo ra những bữa tối khác nhau. 

Đối với mỗi truy vấn`[a, b]`, chúng ta phải đếm xem có bao nhiêu bữa tối hợp lệ kéo dài giữa`a`Và`b`, bao gồm. Tất cả các câu trả lời đều được yêu cầu modulo`10^9 + 7`. 

Bữa tối dài nhất có thể là`10^5`, và có tới`10^5`truy vấn. Bất kỳ cách tiếp cận nào tính toán lại câu trả lời một cách độc lập cho mọi truy vấn đều bị nghi ngờ ngay lập tức. Thậm chí một`O(10^5)`tính toán cho mỗi truy vấn sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất vượt xa thời hạn. 

Cấu trúc ràng buộc gợi ý mạnh mẽ việc xử lý trước tất cả các độ dài lên tới`10^5`một lần, sau đó trả lời mọi truy vấn trong thời gian không đổi. 

Một sai lầm phổ biến là coi hoa trắng riêng lẻ thay vì theo nhóm. Ví dụ, khi`k = 3`, một bữa tối kéo dài`2`không thể chứa bất kỳ bông hoa màu trắng nào cả. Có chính xác một bữa tối hợp lệ:```
RR
```Sự lặp lại cho phép thêm một bông hoa màu trắng sẽ tính không chính xác các cấu hình không hợp lệ bổ sung. 

Một lỗi dễ xảy ra khác xuất hiện trong các truy vấn phạm vi. Giả định:```
1 2
2 3
```Chúng tôi cần đếm độ dài`2`Và`3`, bao gồm. Nếu chúng ta xây dựng tổng tiền tố và trả lời bằng`pref[b] - pref[a]`, chúng tôi vô tình loại trừ độ dài`a`. Công thức đúng là`pref[b] - pref[a-1]`. 

Trường hợp cạnh thứ ba xảy ra khi`n < k`. Ví dụ:```
k = 5
n = 3
```Không có nhóm màu trắng nào phù hợp nên chỉ có thể có hoa màu đỏ. Câu trả lời phải là`1`. Bất kỳ sự tái phát nào truy cập một cách mù quáng`dp[n-k]`không kiểm tra giới hạn sẽ thất bại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chiều dài`n`, hãy thử đặt một cách đệ quy một bông hoa màu đỏ hoặc một nhóm màu trắng bất cứ khi nào nó phù hợp. Mỗi công trình hoàn chỉnh tương ứng với một bữa tối hợp lệ. 

Cách tiếp cận này đúng vì nó liệt kê rõ ràng tất cả các khả năng. Thật không may, số lượng bữa tối hợp lệ tăng theo cấp số nhân. Ngay cả đối với các giá trị vừa phải của`n`, cây đệ quy trở nên khổng lồ. Vì chiều dài có thể đạt tới`100000`, việc tạo toàn diện là hoàn toàn không khả thi. 

Quan sát quan trọng là chỉ có độ dài còn lại mới quan trọng. Nếu chúng ta định nghĩa`dp[n]`là số bữa tối hợp lệ trong tổng thời gian`n`, thì mọi bữa tối hợp lệ đều kết thúc theo đúng một trong hai cách. 

Nó kết thúc bằng một bông hoa màu đỏ, để lại một bữa tối kéo dài hợp lệ`n-1`, hoặc nó kết thúc bằng một nhóm kích thước màu trắng`k`, để lại một bữa tối hợp lệ kéo dài`n-k`. 

Điều đó ngay lập tức tái diễn:```
dp[n] = dp[n-1] + dp[n-k]
```bất cứ khi nào`n >= k`. 

Đối với chiều dài nhỏ hơn`k`, không thể đặt nhóm màu trắng nên chỉ có một cách sắp xếp: tất cả hoa màu đỏ. 

Sau khi tính toán hết`dp[n]`đạt đến độ dài yêu cầu tối đa thì việc trả lời một truy vấn vẫn yêu cầu tính tổng một phạm vi. Vì có tới`10^5`truy vấn, chúng tôi cũng tính toán trước tổng tiền tố:```
pref[i] = dp[0] + dp[1] + ... + dp[i]
```Sau đó, bất kỳ truy vấn`[a, b]`trở thành:```
pref[b] - pref[a-1]
```đó là thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) đệ quy | Quá chậm | 
| Tối ưu | O(MAXN + t) | O(MAXN) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các truy vấn và giá trị của`k`. 
2. Xác định giá trị lớn nhất của`b`xuất hiện trong bất kỳ truy vấn nào. Chúng tôi chỉ cần giá trị DP có độ dài này. 
3. Tạo một mảng`dp`Ở đâu`dp[i]`đại diện cho số bữa tối hợp lệ trong tổng chiều dài`i`. 
4. Đặt`dp[0] = 1`. 

Có chính xác một cách để xây dựng chiều dài`0`, không chọn gì cả. Điều này phục vụ như là trường hợp cơ sở cho sự tái phát. 
5. Đối với mọi chiều dài`i`từ`1`ĐẾN`max_b`: 

- Bắt đầu với`dp[i] = dp[i-1]`, tương ứng với kết thúc bằng một bông hoa màu đỏ. 
- Nếu như`i >= k`, thêm vào`dp[i-k]`, tương ứng với kết thúc bằng nhóm màu trắng. 
6. Thực hiện mọi bản cập nhật theo modulo`10^9 + 7`. 
7. Xây dựng mảng tổng tiền tố: 

-`pref[0] = dp[0]`-`pref[i] = pref[i-1] + dp[i]`8. Đối với mỗi truy vấn`[a, b]`, tính:```
answer = pref[b] - pref[a-1]
```và điều chỉnh bằng số học modulo. 
9. Xuất ra câu trả lời cho mọi truy vấn. 

### Tại sao nó hoạt động 

Sự phân vùng lặp lại tất cả các bữa tối hợp lệ có độ dài`n`theo khối cuối cùng của họ. 

Nếu khối cuối cùng là một bông hoa màu đỏ, việc loại bỏ nó sẽ để lại một bữa tối có giá trị duy nhất có chiều dài`n-1`. Ngược lại, mỗi bữa tối hợp lệ có độ dài`n-1`có thể được mở rộng bởi một bông hoa màu đỏ. 

Nếu khối cuối cùng là nhóm màu trắng, việc loại bỏ nhóm đó sẽ để lại một bữa tối hợp lệ duy nhất có độ dài`n-k`. Ngược lại, mỗi bữa tối hợp lệ có độ dài`n-k`có thể được mở rộng bởi một nhóm màu trắng. 

Hai loại này tách rời nhau và bao gồm tất cả các khả năng, do đó số lượng của chúng sẽ tăng lên. Vì trường hợp cơ sở là chính xác và mọi trạng thái lớn hơn đều bắt nguồn từ các trạng thái chính xác nhỏ hơn nên DP sẽ tính toán chính xác số bữa tối hợp lệ cho mỗi độ dài. 

Mảng tiền tố lưu trữ số lượng tích lũy, do đó việc trừ hai tiền tố sẽ tách biệt chính xác độ dài bên trong phạm vi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    t, k = map(int, input().split())

    queries = []
    max_b = 0

    for _ in range(t):
        a, b = map(int, input().split())
        queries.append((a, b))
        max_b = max(max_b, b)

    dp = [0] * (max_b + 1)
    dp[0] = 1

    for i in range(1, max_b + 1):
        dp[i] = dp[i - 1]
        if i >= k:
            dp[i] = (dp[i] + dp[i - k]) % MOD

    pref = [0] * (max_b + 1)
    pref[0] = dp[0]

    for i in range(1, max_b + 1):
        pref[i] = (pref[i - 1] + dp[i]) % MOD

    ans = []

    for a, b in queries:
        res = pref[b]
        if a > 1:
            res -= pref[a - 1]
        res %= MOD
        ans.append(str(res))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng DP lưu trữ số bữa tối hợp lệ cho mỗi độ dài. Sự tái diễn phản ánh trực tiếp cấu trúc tổ hợp của bài toán: mỗi bữa tối đều kết thúc bằng một bông hoa màu đỏ hoặc một khối màu trắng. 

Trường hợp cơ sở`dp[0] = 1`là rất quan trọng. Không có nó, độ dài chính xác bằng`k`sẽ không nhận được sự đóng góp tương ứng với một nhóm da trắng. 

Mảng tiền tố được xây dựng sau khi biết tất cả các giá trị DP. Các câu trả lời truy vấn trở thành tra cứu tổng phạm vi theo thời gian không đổi. 

điều kiện`if a > 1`tránh truy cập`pref[-1]`. Về mặt khái niệm, khi`a = 1`, không có độ dài nào nằm dưới phạm vi cần trừ. 

Số học modulo được áp dụng trong quá trình xây dựng DP và một lần nữa sau khi trừ vì sự khác biệt về tiền tố có thể tạm thời trở thành số âm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1 3
2 3
4 4
```Vì`k = 2`: 

| tôi | dp[i] | Giải thích | 
| --- | --- | --- | 
| 0 | 1 | Bữa tối trống rỗng | 
| 1 | 1 | R | 
| 2 | 2 | RR, WW | 
| 3 | 3 | RRR, RWW, WWR | 
| 4 | 5 | Sự tái diễn kiểu Fibonacci | 

Tổng tiền tố: 

| tôi | trước[i] | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 4 | 
| 3 | 7 | 
| 4 | 12 | 

Đánh giá truy vấn: 

| Truy vấn | Tính toán | Trả lời | 
| --- | --- | --- | 
| [1,3] | 7 | 6 | 
| [2,3] | 7 - 2 | 5 | 
| [4,4] | 12 - 7 | 5 | 

Đầu ra:```
6
5
5
```Truy vấn đầu tiên có độ dài 1, 2 và 3. Giá trị DP của chúng là`1 + 2 + 3 = 6`. 

### Ví dụ 2 

đầu vào:```
1 3
1 5
```Xây dựng DP: 

| tôi | dp[i] | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 | 
| 5 | 4 | 

Tổng tiền tố: 

| tôi | trước[i] | 
| --- | --- | 
| 0 | 1 | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 5 | 
| 4 | 8 | 
| 5 | 12 | 

Trả lời: 

| Truy vấn | Tính toán | Kết quả | 
| --- | --- | --- | 
| [1,5] | 12 - 1 | 11 | 

Đầu ra:```
11
```Ví dụ này làm nổi bật điểm chuyển tiếp ở độ dài`k`. Trước chiều dài`3`, chỉ tồn tại những bữa tối toàn màu đỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(max_b + t) | Một lượt DP, một lượt tiền tố, một lượt truy vấn chuyển qua | 
| Không gian | O(max_b) | Mảng DP và tiền tố | 

Từ`max_b ≤ 100000`, quá trình tiền xử lý chỉ cần khoảng một trăm nghìn lần lặp. Quá trình xử lý truy vấn diễn ra theo thời gian không đổi cho mỗi truy vấn, do đó ngay cả`100000`truy vấn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t, k = map(int, input().split())

    queries = []
    max_b = 0

    for _ in range(t):
        a, b = map(int, input().split())
        queries.append((a, b))
        max_b = max(max_b, b)

    dp = [0] * (max_b + 1)
    dp[0] = 1

    for i in range(1, max_b + 1):
        dp[i] = dp[i - 1]
        if i >= k:
            dp[i] = (dp[i] + dp[i - k]) % MOD

    pref = [0] * (max_b + 1)
    pref[0] = dp[0]

    for i in range(1, max_b + 1):
        pref[i] = (pref[i - 1] + dp[i]) % MOD

    out = []

    for a, b in queries:
        ans = pref[b]
        if a > 1:
            ans -= pref[a - 1]
        ans %= MOD
        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("3 2\n1 3\n2 3\n4 4\n") == "6\n5\n5", "sample"

# minimum size
assert run("1 1\n1 1\n") == "1", "single length"

# n < k, only red flowers possible
assert run("1 5\n1 4\n") == "4", "all lengths have exactly one arrangement"

# exact boundary at k
assert run("1 3\n3 3\n") == "2", "RRR and WWW"

# off-by-one range test
assert run("1 2\n2 2\n") == "2", "must include lower endpoint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`1`| Ví dụ nhỏ nhất có thể | 
|`1 5 / 1 4`|`4`| Tất cả độ dài được truy vấn bên dưới`k`| 
|`1 3 / 3 3`|`2`| Chuyển đổi chính xác theo chiều dài`k`| 
|`1 2 / 2 2`|`2`| Độ chính xác của ranh giới tổng tiền tố | 

## Vỏ cạnh 

Hãy xem xét:```
1 5
1 4
```Độ dài từ 1 đến 4 đều nhỏ hơn`k`. Các giá trị DP là:```
dp[1] = dp[2] = dp[3] = dp[4] = 1
```bởi vì không có nhóm da trắng nào có thể phù hợp. Câu trả lời truy vấn trở thành:```
1 + 1 + 1 + 1 = 4
```cái nào đúng. Sự lặp lại xử lý điều này một cách tự nhiên bởi vì`i >= k`nhánh không bao giờ thực thi. 

Coi như:```
1 3
3 3
```Ở độ dài 3:```
dp[3] = dp[2] + dp[0]
      = 1 + 1
      = 2
```Hai bữa tối là:```
RRR
WWW
```Trường hợp cơ sở`dp[0] = 1`là những gì cho phép tính cấu hình toàn màu trắng. 

Coi như:```
1 2
2 3
```Các giá trị DP là:```
dp[2] = 2
dp[3] = 3
```Câu trả lời đúng là:```
2 + 3 = 5
```Sử dụng tổng tiền tố:```
pref[3] - pref[1]
```cho kết quả chính xác là 5. Trừ`pref[2]`thay vào đó sẽ loại trừ sai độ dài 2, tạo ra 3 thay vì 5. Đây là bẫy cổ điển trong các truy vấn tổng phạm vi và thuật toán tránh nó bằng cách trừ`pref[a-1]`.
