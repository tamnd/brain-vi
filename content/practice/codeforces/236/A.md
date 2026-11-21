---
title: "CF 236A - Bé Trai hay Bé Gái"
description: "Chúng tôi được cấp một tên người dùng chỉ bao gồm các chữ cái tiếng Anh viết thường. Nhiệm vụ là đếm xem có bao nhiêu ký tự khác nhau xuất hiện trong tên người dùng đó. Nguyên tắc quyết định rất đơn giản. Nếu số lượng ký tự riêng biệt là số chẵn, chúng tôi sẽ in \"CHAT WITH HER!\"."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 236
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 146 (Div. 2)"
rating: 800
weight: 236
solve_time_s: 207
verified: true
draft: false
---

[CF 236A - Trai hay Gái](https://codeforces.com/problemset/problem/236/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một tên người dùng chỉ bao gồm các chữ cái tiếng Anh viết thường. Nhiệm vụ là đếm xem có bao nhiêu ký tự khác nhau xuất hiện trong tên người dùng đó. 

Nguyên tắc quyết định rất đơn giản. Nếu số ký tự riêng biệt là số chẵn thì chúng ta in`"CHAT WITH HER!"`. Nếu số lẻ thì in ra`"IGNORE HIM!"`. 

Kích thước đầu vào rất nhỏ. Độ dài tên người dùng tối đa là 100 ký tự, do đó, ngay cả những cách tiếp cận tương đối kém hiệu quả cũng sẽ chạy thoải mái trong thời gian giới hạn. Một giải pháp bậc hai sẽ thực hiện tối đa khoảng 10.000 phép so sánh ký tự, điều này không đáng kể đối với phần cứng hiện đại. Điều này có nghĩa là vấn đề không phải ở áp lực tối ưu hóa mà là ở việc triển khai logic một cách chính xác và rõ ràng. 

Nguyên nhân chính của sai lầm là do hiểu sai ý nghĩa của “các ký tự riêng biệt”. Chúng tôi chỉ đếm mỗi chữ cái duy nhất một lần, bất kể nó xuất hiện bao nhiêu lần. 

Hãy xem xét đầu vào:```
aaaa
```Số ký tự riêng biệt đúng là`1`, không`4`. Từ`1`là số lẻ, kết quả đúng là:```
IGNORE HIM!
```Việc triển khai bất cẩn chỉ kiểm tra tổng chiều dài chuỗi sẽ in không chính xác`"CHAT WITH HER!"`. 

Một lỗi dễ mắc phải khác là quên các ký tự lặp lại xuất hiện cách xa nhau trong chuỗi. 

Ví dụ:```
abac
```Các chữ cái riêng biệt là`a`,`b`, Và`c`, vậy số đếm là`3`. Đầu ra đúng là:```
IGNORE HIM!
```Nếu chúng ta chỉ so sánh các ký tự lân cận, chúng ta sẽ đếm sai`a`hai lần. 

Ngoài ra còn có một trường hợp ranh giới với tên người dùng một ký tự:```
z
```Có chính xác một ký tự riêng biệt, vì vậy câu trả lời là:```
IGNORE HIM!
```Điều này kiểm tra xem việc triển khai có xử lý chính xác đầu vào hợp lệ nhỏ nhất hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là kiểm tra từng ký tự và kiểm tra thủ công xem chúng ta đã từng thấy nó trước đây hay chưa. Đối với mỗi vị trí, chúng tôi quét tất cả các vị trí trước đó để xác định xem ký tự đó có phải là ký tự mới hay không. 

Đối với một chuỗi có độ dài`n`, điều này thực hiện đại khái:```
1 + 2 + 3 + ... + n = O(n²)
```so sánh trong trường hợp xấu nhất 

Với`n ≤ 100`, điều này vẫn hoàn toàn có thể chấp nhận được. Thậm chí 10.000 thao tác cũng không đáng kể. 

Ý tưởng brute-force hoạt động vì bảng chữ cái nhỏ và kích thước đầu vào nhỏ. Tuy nhiên, vẫn có một quan sát rõ ràng hơn. Chúng tôi thực sự không quan tâm đến thứ tự các ký tự hoặc tần số của chúng. Chúng tôi chỉ quan tâm đến tập hợp các chữ cái duy nhất. 

Python đã cung cấp sẵn`set`cấu trúc tự động chỉ lưu trữ các phần tử riêng biệt. Việc chuyển đổi chuỗi thành một tập hợp sẽ ngay lập tức loại bỏ các chuỗi trùng lặp. 

Ví dụ:```
set("wjmzbmr")
```trở thành:```
{'w', 'j', 'm', 'z', 'b', 'r'}
```Kích thước của bộ này chính xác là số lượng ký tự riêng biệt. Khi chúng ta biết số đó, chúng ta chỉ cần kiểm tra xem nó là chẵn hay lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi tên người dùng từ đầu vào. 
2. Chuyển chuỗi thành một tập hợp. 

Một bộ tự động loại bỏ các ký tự trùng lặp, chỉ để lại các chữ cái riêng biệt. 
3. Tính kích thước của tập hợp. 

Điều này cung cấp số lượng ký tự duy nhất trong tên người dùng. 
4. Kiểm tra xem số này là chẵn hay lẻ. 

Nếu số chẵn thì in`"CHAT WITH HER!"`. 

Ngược lại, in`"IGNORE HIM!"`. 

### Tại sao nó hoạt động 

Thuật toán dựa vào thuộc tính là một tập hợp chứa mỗi giá trị nhiều nhất một lần. Sau khi chuyển đổi tên người dùng thành một bộ, mỗi lần xuất hiện lặp lại của một ký tự sẽ tự động biến mất. Do đó, kích thước của tập hợp chính xác bằng số ký tự riêng biệt trong tên người dùng. 

Quy tắc của bài toán chỉ phụ thuộc vào tính chẵn lẻ của số đếm này. Kiểm tra`count % 2`xác định chính xác số ký tự riêng biệt là chẵn hay lẻ nên thuật toán luôn cho kết quả đầu ra theo yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

distinct_count = len(set(s))

if distinct_count % 2 == 0:
    print("CHAT WITH HER!")
else:
    print("IGNORE HIM!")
```Dòng đầu tiên đọc tên người dùng và xóa dòng mới ở cuối bằng cách sử dụng`strip()`. 

biểu hiện`set(s)`tạo một bộ sưu tập chỉ chứa các ký tự duy nhất từ ​​chuỗi. Đang gọi`len()`trên bộ này cho biết số lượng các chữ cái riêng biệt. 

Điều kiện cuối cùng kiểm tra tính chẵn lẻ của số đó. Nếu số dư sau khi chia cho`2`bằng 0, số đếm là số chẵn và chúng tôi in`"CHAT WITH HER!"`. Nếu không, chúng tôi in`"IGNORE HIM!"`. 

Một chi tiết triển khai tinh tế là việc sử dụng`strip()`. Không có nó, ký tự dòng mới`\n`từ đầu vào có thể vô tình trở thành một phần của tập hợp, làm tăng số lượng ký tự riêng biệt lên một và tạo ra câu trả lời sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
wjmzbmr
```| Bước | Nhân vật hiện tại | Nhân vật khác biệt | 
| --- | --- | --- | 
| 1 | w | {w} | 
| 2 | j | {w, j} | 
| 3 | m | {w, j, m} | 
| 4 | z | {w, j, m, z} | 
| 5 | b | {w, j, m, z, b} | 
| 6 | m | {w, j, m, z, b} | 
| 7 | r | {w, j, m, z, b, r} | 

Bộ cuối cùng chứa 6 ký tự riêng biệt. Vì 6 là số chẵn nên kết quả là:```
CHAT WITH HER!
```Ví dụ này chứng minh rằng các ký tự lặp đi lặp lại như`m`chỉ được tính một lần. 

### Ví dụ 2 

đầu vào:```
xiaodao
```| Bước | Nhân vật hiện tại | Nhân vật khác biệt | 
| --- | --- | --- | 
| 1 | x | {x} | 
| 2 | tôi | {x, tôi} | 
| 3 | một | {x, tôi, a} | 
| 4 | o | {x, tôi, a, o} | 
| 5 | d | {x, i, a, o, d} | 
| 6 | một | {x, i, a, o, d} | 
| 7 | o | {x, i, a, o, d} | 

Bộ cuối cùng chứa 5 ký tự riêng biệt. Vì 5 là số lẻ nên kết quả là:```
IGNORE HIM!
```Dấu vết này xác nhận rằng nhiều chữ cái lặp lại không ảnh hưởng đến số lượng riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được chèn vào bộ một lần | 
| Không gian | O(n) | Bộ này có thể lưu trữ tất cả các ký tự riêng biệt | 

Độ dài tên người dùng tối đa chỉ là 100, vì vậy giải pháp này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. Ngay cả cách tiếp cận bậc hai chậm hơn cũng có thể vượt qua, nhưng giải pháp dựa trên tập hợp vừa sạch hơn vừa có khả năng mở rộng hơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    s = input().strip()

    if len(set(s)) % 2 == 0:
        print("CHAT WITH HER!")
    else:
        print("IGNORE HIM!")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("wjmzbmr\n") == "CHAT WITH HER!", "sample 1"

# custom cases
assert run("a\n") == "IGNORE HIM!", "single character"
assert run("aaaa\n") == "IGNORE HIM!", "all characters equal"
assert run("ab\n") == "CHAT WITH HER!", "small even distinct count"
assert run("abcdefghijklmnopqrstuvwxyz\n") == "CHAT WITH HER!", "maximum distinct letters"
assert run("abac\n") == "IGNORE HIM!", "non-adjacent duplicate characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`IGNORE HIM!`| Đầu vào kích thước tối thiểu | 
|`aaaa`|`IGNORE HIM!`| Ký tự lặp lại được tính một lần | 
|`ab`|`CHAT WITH HER!`| Số lượng nhỏ thậm chí khác biệt | 
|`abcdefghijklmnopqrstuvwxyz`|`CHAT WITH HER!`| Các chữ cái khác biệt tối đa có thể | 
|`abac`|`IGNORE HIM!`| Các chữ cái trùng lặp xuất hiện cách xa nhau | 

## Vỏ cạnh 

Một lỗi phổ biến là nhầm lẫn độ dài chuỗi với số lượng ký tự riêng biệt. 

Coi như:```
aaaa
```Thuật toán chuyển đổi chuỗi thành:```
{'a'}
```Số đếm riêng biệt là`1`, là số lẻ nên kết quả đầu ra sẽ trở thành:```
IGNORE HIM!
```Điều này chính xác bỏ qua sự xuất hiện lặp đi lặp lại của cùng một ký tự. 

Một trường hợp phức tạp khác là khi các chữ cái trùng lặp được tách ra. 

đầu vào:```
abac
```Bộ này phát triển như sau:```
{'a'}
{'a', 'b'}
{'a', 'b'}
{'a', 'b', 'c'}
```Số đếm cuối cùng là`3`, do đó đầu ra là:```
IGNORE HIM!
```Điều này xác nhận rằng thuật toán theo dõi tính duy nhất trên toàn cầu, không chỉ giữa các ký tự lân cận. 

Đầu vào hợp lệ nhỏ nhất cũng hoạt động chính xác. 

đầu vào:```
z
```Tập hợp trở thành:```
{'z'}
```Số lượng là`1`, điều này là số lẻ nên thuật toán in ra:```
IGNORE HIM!
```Điều này xác minh việc xử lý chính xác đầu vào có kích thước ranh giới.
