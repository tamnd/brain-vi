---
title: "CF 110B - Sợi Dây May Mắn"
description: "Chúng ta cần xây dựng một chuỗi chữ thường có độ dài n thỏa mãn một điều kiện đặc biệt trên các chữ cái lặp lại. Đối với mỗi ký tự, chúng tôi xem xét tất cả các vị trí mà nó xuất hiện."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "strings"]
categories: ["algorithms"]
codeforces_contest: 110
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 84 (Div. 2 Only)"
rating: 1100
weight: 110
solve_time_s: 178
verified: true
draft: false
---

[CF 110B - Chuỗi may mắn](https://codeforces.com/problemset/problem/110/B) 

**Đánh giá:** 1100 
**Tas:** thuật toán xây dựng, chuỗi 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một chuỗi chữ thường có độ dài`n`thỏa mãn một điều kiện đặc biệt trên các chữ cái lặp đi lặp lại. 

Đối với mỗi ký tự, chúng tôi xem xét tất cả các vị trí mà nó xuất hiện. Nếu một ký tự xuất hiện nhiều lần thì sự chênh lệch giữa các vị trí liên tiếp luôn phải là con số may mắn. Trong bài toán này, số may mắn là số nguyên mà biểu diễn thập phân chỉ chứa các chữ số`4`Và`7`. 

Nhiệm vụ không chỉ là xây dựng bất kỳ chuỗi hợp lệ nào. Trong số tất cả các chuỗi có độ dài hợp lệ`n`, chúng ta phải xuất ra giá trị nhỏ nhất về mặt từ điển. 

Các ràng buộc đủ nhỏ để việc xây dựng câu trả lời một cách trực tiếp là hướng đi dự định. Với`n ≤ 10^5`, chúng ta có thể dễ dàng có được thời gian tuyến tính hoặc thậm chí`O(n log n)`, nhưng bất cứ điều gì theo cấp số nhân hoặc dựa trên tìm kiếm vũ phu trên chuỗi đều không thể thực hiện được. Ngay cả việc thử tất cả các chuỗi trên một bảng chữ cái nhỏ cũng sẽ phát nổ ngay lập tức, vì có`26^n`có thể có các chuỗi chữ thường. 

Khó khăn chính là sự tối giản về mặt từ điển. Một sự lựa chọn tham lam ngây thơ có thể vô tình tạo ra một trạng thái tương lai không hợp lệ. Chúng ta cần một mẫu vừa luôn hợp lệ vừa có thể chứng minh được là ở mức tối thiểu. 

Một trường hợp dễ bị bỏ lỡ xuất hiện khi`n < 4`. Vì số may mắn nhỏ nhất là`4`, không chữ cái nào có thể lặp lại trước khoảng cách`4`. Ví dụ: 

đầu vào:```
3
```Đầu ra đúng:```
abc
```Một giải pháp bất cẩn lặp lại các chữ cái quá sớm có thể tạo ra`"aba"`, nhưng hai`'a'`các vị trí khác nhau bởi`2`, thật không may mắn. 

Một trường hợp tinh tế khác là khi độ dài chuỗi chính xác`5`. 

đầu vào:```
5
```Đầu ra đúng:```
abcda
```Việc lặp đi lặp lại`'a'`vị trí là`1`Và`5`, sự khác biệt`4`, thật là may mắn. Nếu chúng ta cố gắng`"abcab"`thay vào đó,`'a'`các vị trí khác nhau bởi`3`, không hợp lệ. 

Cái bẫy thứ ba là cho rằng chúng ta cần nhiều khoảng cách may mắn. Trong thực tế, khoảng cách`4`một mình là đủ để xây dựng toàn bộ câu trả lời. Một số giải pháp làm phức tạp quá mức việc xây dựng bằng cách trộn`4`Và`7`, nhưng điều đó là không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Tạo các chuỗi theo thứ tự từ điển và kiểm tra xem mỗi chuỗi có thỏa mãn điều kiện hay không. Để xác minh một chuỗi, chúng tôi lưu trữ tất cả các vị trí cho từng ký tự và đảm bảo mọi sự khác biệt liền kề đều là may mắn. 

Bản thân việc xác minh là rẻ, đại khái là`O(n)`. Vấn đề là số lượng chuỗi ứng cử viên. Ngay cả trên một bảng chữ cái nhỏ như`{a,b,c,d}`, đã có rồi`4^100000`khả năng, điều đó hoàn toàn không thể thực hiện được. 

Quan sát tiếp theo sẽ thay đổi hoàn toàn vấn đề. Chúng ta thực sự không cần những khoảng cách may mắn tùy ý. Con số may mắn nhỏ nhất là`4`và nếu mỗi lần xuất hiện lặp lại của một chữ cái đều chính xác`4`cách nhau một khoảng thì điều kiện sẽ tự động được thỏa mãn. 

Điều đó gợi ý một khối có độ dài lặp lại`4`. 

Hãy xem xét mô hình:```
abcdabcdabcd...
```Mọi`'a'`xuất hiện ở các vị trí khác nhau bởi`4`. Điều tương tự cũng đúng đối với`'b'`,`'c'`, Và`'d'`. Từ`4`thật may mắn, chuỗi đó hợp lệ. 

Bây giờ chúng ta phải chỉ ra tại sao mô hình này lại tối giản về mặt từ điển. 

Để giảm thiểu về mặt từ điển, ở mọi vị trí, chúng tôi muốn ký tự nhỏ nhất có thể không vi phạm điều kiện. Ký tự đầu tiên rõ ràng phải là`'a'`. Cái thứ hai không thể`'a'`bởi vì khoảng cách`1`không may mắn nên nó trở thành`'b'`. Cái thứ ba không thể`'a'`hoặc`'b'`, vì vậy nó trở thành`'c'`. Thứ tư trở thành`'d'`. 

Ở vị trí thứ năm, chúng ta có thể sử dụng lại`'a'`bởi vì trước đó`'a'`chính xác là`4`các vị trí trước đó. Từ`'a'`là lựa chọn hợp lệ nhỏ nhất, chúng tôi chấp nhận nó. Lý do tương tự lặp đi lặp lại mãi mãi, tạo ra chuỗi tuần hoàn`"abcd"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mẫu cơ sở lặp lại`"abcd"`. 
2. Xây dựng câu trả lời theo từng ký tự. 
3. Đối với chức vụ`i`, nối thêm`pattern[i % 4]`. 

Điều này lặp lại trình tự:```
a b c d a b c d ...
```4. In chuỗi kết quả. 

Lý do điều này có tác dụng là vì mỗi lần xuất hiện lặp đi lặp lại của cùng một ký tự đều chính xác`4`các vị trí cách xa nhau. Từ`4`là một con số may mắn, tất cả các khác biệt liền kề trong mỗi danh sách ký tự đều hợp lệ. 

### Tại sao nó hoạt động 

Việc xây dựng duy trì hai thuộc tính cùng một lúc. 

Đầu tiên, tính hợp lệ. Mỗi ký tự chỉ lặp lại sau khi chính xác`4`các vị trí. Các chữ cái bằng nhau liên tiếp cho`'a'`,`'b'`,`'c'`, Và`'d'`luôn khác nhau bởi`4`, thật là may mắn. 

Thứ hai, sự tối giản về mặt từ điển. Ở mọi vị trí, chúng ta chọn ký tự nhỏ nhất không vi phạm quy tắc. Bốn vị trí đầu tiên đều phải khác biệt vì khoảng cách`1`,`2`, Và`3`không may mắn. Điều đó buộc`"abcd"`. Sau đó, việc sử dụng lại ký tự từ bốn vị trí trước đó là hợp lệ và việc chọn bất kỳ ký tự nào nhỏ hơn là không thể. Bằng quy nạp, tuần hoàn`"abcd"`mẫu là chuỗi hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pattern = "abcd"

    ans = []
    for i in range(n):
        ans.append(pattern[i % 4])

    print("".join(ans))

solve()
```Việc thực hiện rất đơn giản vì bản thân việc xây dựng đã là toàn bộ giải pháp. 

Biến`pattern`lưu trữ khối lặp lại. Đối với mọi vị trí`i`, chúng tôi lấy`pattern[i % 4]`, chu kỳ thông qua các chỉ số`0, 1, 2, 3`. 

Sử dụng danh sách và`"".join()`là cách hiệu quả tiêu chuẩn để xây dựng các chuỗi lớn trong Python. Việc nối chuỗi lặp đi lặp lại bên trong một vòng lặp có thể trở thành bậc hai. 

Việc lập chỉ mục rất dễ bị sai nếu sử dụng lý luận dựa trên một tinh thần. Hoạt động modulo hoạt động tự nhiên với các chỉ số dựa trên số 0: 

| tôi | tôi % 4 | nhân vật | 
| --- | --- | --- | 
| 0 | 0 | một | 
| 1 | 1 | b | 
| 2 | 2 | c | 
| 3 | 3 | d | 
| 4 | 0 | một | 

Điều đó tạo ra chính xác trình tự định kỳ dự định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```Dấu vết xây dựng: 

| Vị trí (dựa trên 0) | tôi % 4 | Nhân vật | Chuỗi hiện tại | 
| --- | --- | --- | --- | 
| 0 | 0 | một | một | 
| 1 | 1 | b | ab | 
| 2 | 2 | c | abc | 
| 3 | 3 | d | abcd | 
| 4 | 0 | một | abcda | 

Đầu ra:```
abcda
```Việc lặp đi lặp lại`'a'`vị trí là`1`Và`5`, sự khác biệt`4`, thật là may mắn. 

### Ví dụ 2 

đầu vào:```
10
```Dấu vết xây dựng: 

| Vị trí (dựa trên 0) | tôi % 4 | Nhân vật | Chuỗi hiện tại | 
| --- | --- | --- | --- | 
| 0 | 0 | một | một | 
| 1 | 1 | b | ab | 
| 2 | 2 | c | abc | 
| 3 | 3 | d | abcd | 
| 4 | 0 | một | abcda | 
| 5 | 1 | b | abcdab | 
| 6 | 2 | c | abcdabc | 
| 7 | 3 | d | abcdabcd | 
| 8 | 0 | một | abcdabcda | 
| 9 | 1 | b | abcdabcdab | 

Đầu ra:```
abcdabcdab
```Dấu vết này thể hiện sự bất biến một cách rõ ràng. Mỗi chữ cái lặp lại sẽ xuất hiện chính xác bốn vị trí sau lần xuất hiện trước đó của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tạo chính xác một ký tự cho mỗi vị trí | 
| Không gian | O(n) | Chuỗi câu trả lời lưu trữ`n`nhân vật | 

Với`n ≤ 10^5`, độ phức tạp tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    pattern = "abcd"

    ans = []
    for i in range(n):
        ans.append(pattern[i % 4])

    print("".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout
    return out.getvalue().strip()

# provided sample
assert run("5\n") == "abcda", "sample 1"

# minimum size
assert run("1\n") == "a", "single character"

# small boundary
assert run("4\n") == "abcd", "first full block"

# repeat begins
assert run("8\n") == "abcdabcd", "two full blocks"

# off-by-one check
assert run("9\n") == "abcdabcda", "correct modulo cycling"

# large input
res = run("100000\n")
assert len(res) == 100000, "maximum size length"
assert res[:8] == "abcdabcd", "pattern consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`a`| Kích thước tối thiểu có thể | 
|`4`|`abcd`| Phân đoạn không lặp lại hoàn chỉnh đầu tiên | 
|`8`|`abcdabcd`| Sự lặp lại định kỳ hợp lệ | 
|`9`|`abcdabcda`| Tính chính xác của lập chỉ mục Modulo | 
|`100000`| Chuỗi tuần hoàn lớn | Hiệu suất ở mức hạn chế tối đa | 

## Vỏ cạnh 

Khi nào`n = 1`, thuật toán đầu ra:```
a
```Không có ký tự lặp lại nên điều kiện là đúng. Việc xây dựng xử lý việc này một cách tự nhiên vì nó chỉ lấy ký tự đầu tiên của mẫu. 

Khi`n = 3`, thuật toán đầu ra:```
abc
```Dấu vết: 

| Vị trí | Nhân vật | 
| --- | --- | 
| 0 | một | 
| 1 | b | 
| 2 | c | 

Không có ký tự nào lặp lại, điều này tránh được khoảng cách không hợp lệ như`1`,`2`, hoặc`3`. Một giải pháp tham lam bất cẩn mà tái sử dụng`'a'`quá sớm sẽ thất bại ở đây. 

Khi`n = 5`, đầu ra trở thành:```
abcda
```hai`'a'`vị trí là`1`Và`5`, sự khác biệt`4`. Đây là thời điểm đầu tiên mà việc lặp lại trở nên hợp pháp và thuật toán ngay lập tức tận dụng lợi thế của nó để duy trì mức tối thiểu về mặt từ điển. 

Đối với đầu vào rất lớn như`100000`, mô hình tiếp tục định kỳ mà không có bất kỳ logic bổ sung nào. Mỗi lần xuất hiện lặp đi lặp lại vẫn khác nhau một cách chính xác`4`, do đó độ chính xác không phụ thuộc vào độ dài chuỗi.
