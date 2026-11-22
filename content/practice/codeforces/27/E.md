---
title: "CF 27E - Số Có Số Ước Cho Cho"
description: "Chúng ta được cho một số nguyên n và chúng ta cần xây dựng số nguyên dương nhỏ nhất có số ước chính xác là n."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 27
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 27 (Codeforces format, Div. 2)"
rating: 2000
weight: 27
solve_time_s: 94
verified: true
draft: false
---
[CF 27E - Số có ước số cho trước](https://codeforces.com/problemset/problem/27/E) 

**Xếp hạng:** 2000 
**Tags:** vũ lực, dp, lý thuyết số 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất`n`và chúng ta cần xây dựng số nguyên dương nhỏ nhất có số ước chính xác là`n`. 

Thực tế toán học cốt lõi đằng sau vấn đề này là công thức chia. Nếu một số có hệ số nguyên tố$$x = p_1^{a_1} p_2^{a_2} \cdots p_k^{a_k}$$thì số ước của`x`bằng$$(a_1 + 1)(a_2 + 1)\cdots(a_k + 1)$$Nhiệm vụ không phải là đếm các ước số của một số cho trước. Thay vào đó, chúng ta phải đảo ngược quá trình. Ta cần chọn số mũ sao cho tích của chúng bằng`n`, sau đó xây dựng số nhỏ nhất có thể từ chúng. 

Ràng buộc`n ≤ 1000`đủ nhỏ để chúng ta có thể khám phá nhiều phân rã ước số một cách đệ quy. Thử thách thực sự đến từ chính câu trả lời. Số kết quả có thể lớn bằng`10^18`, loại trừ việc ép buộc các số nguyên và đếm từng ước số một. Thậm chí kiểm tra mọi số lên đến`10^18`rõ ràng là không thể. 

Không gian tìm kiếm của các tổ hợp số mũ cũng có tính lừa đảo. Một giải pháp đệ quy bất cẩn có thể tạo ra số lượng lớn các trạng thái hoặc tràn trong khi nhân các số nguyên tố liên tục. Quan sát quan trọng là các số tối thiểu có số ước cố định luôn sử dụng các số nguyên tố nhỏ có số mũ không tăng. Cấu trúc đó thu hẹp tìm kiếm một cách đáng kể. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Vì`n = 1`, câu trả lời đúng là`1`. 

đầu vào:```
1
```Đầu ra:```
1
```Một cấu trúc đệ quy chỉ dựa trên việc nhân các số nguyên tố có thể vô tình bỏ qua trường hợp này vì hệ số nguyên tố trống tương ứng với`1`. 

Một trường hợp tinh vi khác là khi nhiều cách sắp xếp số mũ tạo ra cùng một số chia. Vì`n = 12`, chúng ta có thể xây dựng:$$12 = (3+1)(2+1)$$mang lại$$2^3 \cdot 3^2 = 72$$Nhưng chúng ta cũng có thể sử dụng$$12 = (5+1)(1+1)$$mang lại$$2^5 \cdot 3 = 96$$Cả hai số đều có 12 ước, nhưng 72 nhỏ hơn. Sự lựa chọn tham lam chỉ dựa trên số mũ lớn hơn sẽ thất bại ở đây. 

Một lỗi phổ biến khác là gán số mũ lớn hơn cho số nguyên tố lớn hơn. Giả sử chúng ta muốn số chia`6`. 

Sử dụng số mũ`(2,1)`đưa ra một cách chính xác:$$2^2 \cdot 3 = 12$$Hoán đổi chúng mang lại:$$2 \cdot 3^2 = 18$$Số thứ hai lớn hơn. Các cấu trúc tối thiểu luôn đặt số mũ lớn hơn trên các số nguyên tố nhỏ hơn. 

Tràn cũng nguy hiểm. Mặc dù câu trả lời cuối cùng phù hợp`10^18`, phép nhân đệ quy trung gian có thể vượt quá giới hạn đó nếu không được kiểm tra cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là lặp đi lặp lại từ`1`, tính số ước cho mỗi số nguyên và dừng ở số đầu tiên có số chia bằng`n`. 

Việc tính toán các ước số cho một số bằng phép chia thử mất khoảng`O(sqrt(x))`. Bản thân câu trả lời có thể gần`10^18`, vì vậy ngay cả việc kiểm tra các số lên đến một phần rất nhỏ của phạm vi đó cũng là vô vọng. Cách tiếp cận này trở nên không khả thi gần như ngay lập tức. 

Một chiến lược bạo lực mang tính toán học hơn sẽ tốt hơn. Vì số chia đến từ tích số mũ nên chúng ta có thể thử tất cả các phân tích nhân tử của`n`thành những thuật ngữ như`(a_i + 1)`, xây dựng số tương ứng và giữ mức tối thiểu. 

Điều này đã sử dụng đúng cấu trúc, nhưng nếu chúng ta tạo ra các tổ hợp số mũ một cách bất cẩn, chúng ta sẽ tạo ra nhiều bản sao và nhiều trạng thái vô dụng. Quan sát quan trọng là số hợp lệ nhỏ nhất luôn có các tính chất sau: 

Đầu tiên, các số nguyên tố được sử dụng theo thứ tự tăng dần:`2, 3, 5, 7, ...`Thứ hai, số mũ không tăng:$$a_1 \ge a_2 \ge a_3 \ge \cdots$$Tại sao điều này phải là sự thật? Giả sử chúng ta đã có`a_i < a_j`vì`i < j`. Sau đó hoán đổi số mũ sẽ đặt số mũ lớn hơn vào số nguyên tố nhỏ hơn, tạo ra số nhỏ hơn trong khi vẫn giữ nguyên số chia. 

Điều này biến vấn đề thành một DFS bị chặn đối với các lựa chọn số mũ. 

Ở mỗi bước, chúng tôi chọn một số mũ cho số nguyên tố hiện tại. Nếu chúng ta chọn số mũ`e`, thì số nhân của số chia sẽ trở thành`(e + 1)`. Chúng ta tiếp tục đệ quy chỉ khi`(e + 1)`chia mục tiêu chia còn lại. 

Vì số mũ phải không tăng nên mỗi cấp độ đệ quy chỉ xét số mũ bằng số mũ trước đó. Hạn chế này làm giảm đáng kể không gian tìm kiếm. 

Số lượng các trạng thái rất dễ quản lý vì`n ≤ 1000`, và số mũ giảm nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên số nguyên | Hàm mũ trong kích thước câu trả lời | O(1) | Quá chậm | 
| DFS về phân rã số mũ | Tìm kiếm thực tế rất nhỏ, khoảng O(số trạng thái số mũ hợp lệ) | O(độ sâu) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuẩn bị một danh sách nhỏ các số nguyên tố bắt đầu từ`2`. 

Câu trả lời tối thiểu luôn sử dụng các số nguyên tố nhỏ nhất trước tiên, vì vậy chỉ cần một số số nguyên tố đầu tiên. 
2. Bắt đầu DFS với các tham số sau:`idx`= chỉ số nguyên tố hiện tại,`last_exp`= số mũ tối đa được phép,`remaining`= vẫn cần số chia,`current`= số được xây dựng hiện tại. 

các`last_exp`hạn chế thực thi số mũ không tăng. 
3. Nếu`remaining == 1`, chúng ta đã xây dựng thành công một số có số ước số khớp với`n`. 

So sánh`current`dựa trên câu trả lời tối thiểu toàn cầu và cập nhật nếu nhỏ hơn. 
4. Thử mọi số mũ`e`từ`1`ĐẾN`last_exp`. 

Chọn số mũ`e`đóng góp một yếu tố`(e + 1)`đến số chia. 
5. Bỏ qua bất kỳ số mũ nào`(e + 1)`không chia`remaining`. 

Sự lựa chọn như vậy không bao giờ có thể dẫn đến số ước số được yêu cầu. 
6. Nhân số nguyên tố hiện tại nhiều lần để xây dựng`p^e`. 

Dừng ngay lập tức nếu phép nhân vượt quá câu trả lời tốt nhất đã được tìm thấy. 
7. Sử dụng lại:`idx + 1`,`e`là số mũ tối đa mới,`remaining // (e + 1)`,`current * p^e`. 

Đi qua`e`chuyển tiếp bảo toàn quy tắc số mũ không tăng. 
8. Tiếp tục cho đến khi khám phá hết tất cả các phép gán số mũ hợp lệ. 

### Tại sao nó hoạt động 

Mỗi số nguyên dương có một hệ số nguyên tố duy nhất và số ước của nó chỉ phụ thuộc vào số mũ. DFS liệt kê tất cả các tổ hợp số mũ có thể có tích số chia bằng`n`. 

Giới hạn số mũ không tăng không loại bỏ bất kỳ giải pháp tối ưu nào. Nếu một giải pháp vi phạm trật tự này, việc hoán đổi số mũ lớn hơn thành các số nguyên tố nhỏ hơn luôn tạo ra một số nhỏ hơn có cùng số chia. Hoán đổi lặp đi lặp lại cuối cùng tạo ra một sự sắp xếp không tăng. 

Vì DFS khám phá tất cả các cách sắp xếp chính tắc như vậy và theo dõi giá trị được xây dựng tối thiểu, nên câu trả lời cuối cùng được đảm bảo là số nguyên nhỏ nhất với chính xác`n`số chia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41]

n = int(input())

ans = LIMIT

def dfs(idx, last_exp, remaining, current):
    global ans

    if remaining == 1:
        ans = min(ans, current)
        return

    if idx >= len(primes):
        return

    prime = primes[idx]
    value = 1

    for exp in range(1, last_exp + 1):
        if value > ans // prime:
            break

        value *= prime

        if remaining % (exp + 1) != 0:
            continue

        nxt = current * value

        if nxt >= ans:
            continue

        dfs(idx + 1, exp, remaining // (exp + 1), nxt)

dfs(0, 64, n, 1)

print(ans)
```DFS xây dựng câu trả lời theo số nguyên tố. tham số`remaining`lưu trữ bao nhiêu yếu tố chia vẫn cần thiết. Nếu chúng ta chọn số mũ`e`, sau đó`(e + 1)`phải chia`remaining`, bởi vì số mũ đó đóng góp chính xác`(e + 1)`vào công thức tính số chia. 

Đệ quy bắt đầu với giới hạn số mũ rất lớn như`64`. Giá trị chính xác không quan trọng miễn là nó vượt quá tất cả số mũ thực tế trong phạm vi`10^18`. 

Biến`value`tăng dần lưu trữ lũy thừa của số nguyên tố hiện tại. Thay vì tính toán lại`prime ** exp`lặp đi lặp lại, chúng tôi nhân một lần cho mỗi lần lặp vòng lặp. Điều này vừa nhanh hơn vừa an toàn hơn. 

Bộ phận bảo vệ chống tràn:```
if value > ans // prime:
```ngăn chặn phép nhân vượt quá câu trả lời tốt nhất hiện tại. Việc cắt tỉa này rất quan trọng. Không có nó, số trung gian có thể vượt quá`10^18`và tạo các nhánh đệ quy không cần thiết. 

Một chi tiết tinh tế khác là:```
dfs(idx + 1, exp, ...)
```Đi qua`exp`thay vì`last_exp`thực thi số mũ không tăng. Hạn chế duy nhất đó loại bỏ số lượng lớn các trạng thái dư thừa và đảm bảo chúng tôi chỉ tạo ra các dạng tối thiểu chuẩn mực. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```Chúng tôi muốn một số có chính xác 4 ước. 

| Bước | Thủ tướng | Số mũ | Sản phẩm chia | Số hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | 1 | 1 | 
| Chọn 2² | 2 | 2 | 3 | 4 | 
| Không thể hoàn thành | - | - | - | - | 
| Quay lại | - | - | - | - | 
| Chọn 2¹ | 2 | 1 | 2 | 2 | 
| Chọn 3¹ | 3 | 1 | 4 | 6 | 

Thuật toán đầu tiên thử số mũ`2`trên nguyên tố`2`, cho số nhân chia`3`. Vì mục tiêu chia còn lại trở nên không thể thực hiện được nên nó sẽ quay lại. 

Sau đó nó thử số mũ`(1,1)`:$$(1+1)(1+1)=4$$cấu trúc nào:$$2^1 \cdot 3^1 = 6$$Đây là số hợp lệ nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
12
```| Bước | Thủ tướng | Số mũ | Sản phẩm chia | Số hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | 1 | 1 | 
| Chọn 2³ | 2 | 3 | 4 | 8 | 
| Chọn 3² | 3 | 2 | 12 | 72 | 
| Đã tìm thấy ứng viên | - | - | 12 | 72 | 
| Chọn 2⁵ | 2 | 5 | 6 | 32 | 
| Chọn 3¹ | 3 | 1 | 12 | 96 | 

Dấu vết cho thấy tại sao thứ tự lũy thừa lại quan trọng. Cả hai bộ số mũ đều tạo ra 12 ước số:$$(3+1)(2+1)=12$$Và$$(5+1)(1+1)=12$$Nhưng số được xây dựng nhỏ hơn là 72, vì vậy DFS giữ nó là câu trả lời tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(số trạng thái số mũ hợp lệ) | DFS chỉ khám phá các kết hợp số mũ tương thích với số chia | 
| Không gian | O(độ sâu đệ quy) | Độ sâu được giới hạn bởi số lượng số nguyên tố được sử dụng | 

Không gian tìm kiếm thực tế rất nhỏ vì`n ≤ 1000`, số mũ giảm đơn điệu và việc cắt tỉa sẽ loại bỏ các nhánh lớn một cách nhanh chóng. Độ sâu đệ quy không bao giờ vượt quá một hằng số nhỏ, do đó cả thời gian và bộ nhớ đều dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    LIMIT = 10**18
    primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41]

    n = int(input())
    ans = LIMIT

    def dfs(idx, last_exp, remaining, current):
        nonlocal ans

        if remaining == 1:
            ans = min(ans, current)
            return

        if idx >= len(primes):
            return

        prime = primes[idx]
        value = 1

        for exp in range(1, last_exp + 1):
            if value > ans // prime:
                break

            value *= prime

            if remaining % (exp + 1) != 0:
                continue

            nxt = current * value

            if nxt >= ans:
                continue

            dfs(idx + 1, exp, remaining // (exp + 1), nxt)

    dfs(0, 64, n, 1)

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("4\n") == "6", "sample 1"

# custom cases
assert run("1\n") == "1", "minimum case"
assert run("2\n") == "2", "prime divisor count"
assert run("6\n") == "12", "mixed exponents"
assert run("12\n") == "60", "multiple exponent decompositions"
assert run("24\n") == "360", "larger divisor structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Trường hợp thừa số nguyên tố trống | 
|`2`|`2`| Số mũ nguyên tố đơn | 
|`6`|`12`| Sự kết hợp của số mũ`(2,1)`| 
|`12`|`60`| Cạnh tranh phân rã số mũ | 
|`24`|`360`| Phân nhánh đệ quy sâu hơn | 

## Vỏ cạnh 

Đối với đầu vào:```
1
```DFS bắt đầu bằng`remaining = 1`. Trường hợp cơ sở kích hoạt ngay lập tức và thuật toán trả về`1`. 

Điều này tương ứng với hệ số trống:$$1 = 1$$số ước của nó cũng là`1`. 

Đối với đầu vào:```
6
```số chia có thể được hình thành như sau:$$6 = (2+1)(1+1)$$Cấu trúc DFS:$$2^2 \cdot 3 = 12$$Nếu số mũ được gán theo thứ tự ngược lại, chúng ta sẽ nhận được:$$2 \cdot 3^2 = 18$$Giới hạn số mũ không tăng sẽ ngăn cản việc xây dựng lớn hơn này. 

Đối với đầu vào:```
12
```tồn tại nhiều phân rã số mũ:$$12 = 12$$mang lại:$$2^{11} = 2048$$trong khi:$$12 = (3+1)(2+1)$$mang lại:$$2^3 \cdot 3^2 = 72$$Và:$$12 = (4+1)(1+1)$$mang lại:$$2^4 \cdot 3 = 48$$Trên thực tế, câu trả lời hợp lệ nhỏ nhất là:$$2^2 \cdot 3 \cdot 5 = 60$$bởi vì:$$(2+1)(1+1)(1+1)=12$$DFS khám phá mọi phân tách số mũ hợp lệ và giữ mức tối thiểu. 

Việc cắt bớt tràn trở nên quan trọng đối với đầu vào lớn hơn. Giả sử một nhánh đã tạo ra một số lớn hơn câu trả lời tốt nhất hiện tại. Điều kiện:```
if nxt >= ans:
    continue
```ngừng khám phá nhánh đó ngay lập tức. Điều này tránh được cả công việc vô ích và sự tràn ngẫu nhiên trong quá trình nhân sâu hơn.
