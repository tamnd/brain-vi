---
title: "CF 102397J - AbuTahun và ký ức chớp nhoáng"
description: "Chúng tôi có n tệp giải pháp và mỗi tệp chiếm chính xác x GB. Một bộ nhớ flash có thể chứa tối đa một GB và một tệp phải nằm hoàn toàn trong một bộ nhớ. Mục tiêu là tìm số lượng bộ nhớ flash nhỏ nhất cần thiết để lưu trữ tất cả n tệp."
date: "2026-08-10T18:17:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "J"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 279
verified: true
draft: false
---

[CF 102397J - AbuTahun và Ký ức chớp nhoáng](https://codeforces.com/problemset/problem/102397/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`các tệp giải pháp và mọi tệp đều chiếm chính xác`x`GB. Một bộ nhớ flash có thể chứa tối đa`a`GB và tệp phải nằm hoàn toàn trong một bộ nhớ. Mục đích là tìm số lượng bộ nhớ flash nhỏ nhất cần thiết để lưu trữ tất cả`n`tập tin. 

Số lượng quan trọng không phải là tổng dung lượng lưu trữ của một bộ nhớ mà là bao nhiêu tệp hoàn chỉnh nằm trong đó. Vì mọi tập tin đều có cùng kích thước nên một bộ nhớ có thể lưu trữ chính xác`floor(a / x)`tập tin. Một khi đã biết con số này, vấn đề trở thành là cần bao nhiêu nhóm có kích thước như vậy để chứa tất cả`n`tập tin. 

Ràng buộc`n <= 10^5`có nghĩa là ngay cả một thuật toán tuyến tính cũng có thể dễ dàng đủ nhanh trong giới hạn 1,5 giây. Tuy nhiên, cấu trúc của bài toán đơn giản hơn nhiều so với bài toán đóng gói tổng quát nên ta có thể giải nó trong thời gian không đổi. Giới hạn trên`x`Và`a`cũng đủ nhỏ cho số học số nguyên thông thường và số nguyên Python không có vấn đề tràn ở đây. 

Có một số trường hợp ranh giới có thể cho thấy việc triển khai không chính xác. Coi như`1 5 5`. Một tập tin sẽ lấp đầy chính xác một bộ nhớ, vì vậy câu trả lời là`1`. Việc triển khai bất cẩn sử dụng sự bất bình đẳng nghiêm ngặt thay vì cho phép sự bình đẳng có thể từ chối tệp một cách không chính xác. 

Một trường hợp quan trọng khác là`5 2 5`. Mỗi kỷ niệm lưu giữ`floor(5 / 2) = 2`các tập tin, do đó cần có ba bộ nhớ. Đầu ra đúng là`3`. Chỉ cần chia tổng kích thước tệp cho dung lượng bộ nhớ sẽ cho`ceil(10 / 5) = 2`, điều này sai vì không thể kết hợp 1 GB chưa sử dụng trong mỗi bộ nhớ để lưu trữ một tệp. 

Cuối cùng, khi`x = a`, Ví dụ`4 7 7`, mỗi bộ nhớ có thể chứa chính xác một tệp, vì vậy câu trả lời là`4`. Bất kỳ công thức nào vô tình giả định nhiều hơn một tệp đều có thể phù hợp do làm tròn số nguyên sẽ không đạt được ranh giới này. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu hoàn toàn tổng quát có thể xử lý vấn đề như một vấn đề đóng gói và thử các phép gán tệp có thể có vào bộ nhớ. Đối với một số cố định`k`của những kỷ niệm, mỗi một trong những`n`ban đầu các tập tin có thể được gán cho bất kỳ tập tin nào trong số đó`k`kỷ niệm, trao tặng`k^n`các nhiệm vụ có thể. Cố gắng mọi cách có thể`k`từ`1`bởi vì`n`sản xuất tổng cộng`1^n + 2^n + ... + n^n`bài tập. Trong trường hợp xấu nhất`n = 10^5`, cái này lớn về mặt thiên văn, gần như bị chi phối bởi`10^(500000)`, vì vậy cách tiếp cận như vậy là hoàn toàn không khả thi. 

Cách tiếp cận bạo lực đó là chính xác vì nó xem xét mọi cách có thể để phân phối tệp và có thể xác định liệu gói đóng gói hợp lệ có tồn tại hay không. Vấn đề là nó bỏ qua thuộc tính cấu trúc mạnh nhất: mọi tệp đều có cùng kích thước. 

Bởi vì tất cả các tệp đều bình đẳng nên không có lý do gì để quyết định từng tệp riêng lẻ sẽ đi đâu. Chúng ta chỉ cần biết số lượng tệp hoàn chỉnh tối đa phù hợp với một bộ nhớ. Nếu con số đó là`k = floor(a / x)`, thì mỗi bộ nhớ có thể chứa tối đa`k`các tập tin và chúng tôi luôn có thể đạt được chính xác`k`các tập tin trong bộ nhớ bất cứ khi nào vẫn còn đủ tập tin. Như vậy`n`các tập tin yêu cầu mức trần`n / k`ký ức. 

Việc chia trần có thể được tính bằng số học số nguyên như`(n + k - 1) // k`. Từ`x <= a`, chúng tôi luôn có`k >= 1`, vậy phép chia có hiệu lực. 

Brute-force hoạt động vì nó tìm kiếm cách đóng gói hợp lệ một cách rõ ràng, nhưng không thành công khi số lượng nhiệm vụ có thể thực hiện trở nên quá lớn. Việc quan sát thấy tất cả các tệp có kích thước giống hệt nhau cho phép chúng tôi loại bỏ hoàn toàn danh tính của các tệp và giảm vấn đề xuống một phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^n) hoặc tệ hơn khi xem xét tất cả số lượng bộ nhớ | O(n) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`, `x`, Và`a`. Đây`n`là số lượng tập tin,`x`là kích thước của mỗi tập tin, và`a`là dung lượng của một bộ nhớ flash. 
2. Tính toán`files_per_memory = a // x`. Đây là số nguyên lớn nhất của các tệp hoàn chỉnh có thể chứa vừa trong một bộ nhớ. Bất kỳ phần dung lượng còn lại nào đều không thể sử dụng được vì không thể chia nhỏ các tệp. 
3. Tính số lượng bộ nhớ bằng phép chia trần:`answer = (n + files_per_memory - 1) // files_per_memory`. Vòng này`n / files_per_memory`trở lên, vì bộ nhớ cuối cùng có thể chứa ít tệp hơn những bộ nhớ khác. 
4. In`answer`. 

### Tại sao nó hoạt động 

hãy để`k = floor(a / x)`. Một bộ nhớ không thể chứa nhiều hơn`k`tập tin, bởi vì`k + 1`tập tin sẽ yêu cầu`(k + 1) * x > a`GB. Đồng thời, bất kỳ nhóm nào nhiều nhất`k`tập tin phù hợp bởi vì`k * x <= a`. Do đó, các tập tin luôn có thể được chia thành các nhóm có nhiều nhất`k`các tệp và số lượng tối thiểu của các nhóm như vậy chính xác là`ceil(n / k)`. Thuật toán tính toán chính xác hai đại lượng đó, do đó nó tạo ra số lượng bộ nhớ tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, x, a = map(int, input().split())

files_per_memory = a // x
answer = (n + files_per_memory - 1) // files_per_memory

print(answer)
```Dòng đầu tiên đọc cả ba giá trị đầu vào. Chỉ có một trường hợp thử nghiệm, do đó không cần lặp lại các trường hợp thử nghiệm. 

biểu thức`a // x`sử dụng phân chia tầng, đó là điều cần thiết. Ví dụ: nếu bộ nhớ có dung lượng`7`GB và mỗi tệp sử dụng`2`GB,`7 // 2`là`3`, nghĩa là chỉ có ba tệp hoàn chỉnh phù hợp. 1 GB còn lại không thể được sử dụng cho tệp thứ tư. 

Biểu thức cuối cùng thực hiện phép chia trần mà không sử dụng số học dấu phẩy động. sử dụng`(n + files_per_memory - 1) // files_per_memory`tránh các vấn đề về độ chính xác và xử lý phân chia chính xác một cách chính xác. Ví dụ,`10`tập tin với`3`tập tin trên mỗi bộ nhớ mang lại`(10 + 3 - 1) // 3 = 4`, trong khi`9`tập tin mang lại`(9 + 3 - 1) // 3 = 3`. 

Không có vấn đề tràn số nguyên trong Python và giá trị trung gian lớn nhất ở đây rất nhỏ so với dung lượng số nguyên của Python. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là`10 2 7`. 

|`n`|`x`|`a`|`a // x`| Trả lời | 
| --- | --- | --- | --- | --- | 
| 10 | 2 | 7 | 3 | 4 | 

Mỗi bộ nhớ chứa tối đa ba tập tin vì`3 * 2 = 6`GB phù hợp trong khi`4 * 2 = 8`GB thì không. Mười tệp tạo thành ba nhóm hoàn chỉnh gồm ba tệp và một tệp còn lại, vì vậy cần có bốn bộ nhớ. Điều này chứng tỏ tại sao dung lượng chưa sử dụng của bộ nhớ không thể kết hợp với bộ nhớ khác. 

Đối với mẫu thứ hai, đầu vào là`3 5 15`. 

|`n`|`x`|`a`|`a // x`| Trả lời | 
| --- | --- | --- | --- | --- | 
| 3 | 5 | 15 | 3 | 1 | 

Một bộ nhớ có thể chứa chính xác ba tập tin vì`3 * 5 = 15`GB. Do đó, cả ba tệp đều nằm gọn trong một bộ nhớ. Điều này khẳng định rằng sự bình đẳng về công suất được cho phép và việc phân chia trần sẽ trả về chính xác`1`khi số lượng tệp chia hết cho dung lượng trên mỗi bộ nhớ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng phép tính số học không đổi được thực hiện. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Với`n`lớn như`10^5`, thời gian không đổi nằm trong giới hạn 1,5 giây. Thuật toán cũng sử dụng bộ nhớ không đổi, thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n, x, a = map(int, input().split())

    files_per_memory = a // x
    answer = (n + files_per_memory - 1) // files_per_memory

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    output = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output

# Provided samples
assert run("10 2 7\n") == "4", "sample 1"
assert run("3 5 15\n") == "1", "sample 2"

# Minimum-size input
assert run("1 1 1\n") == "1", "minimum values"

# Every file exactly fills one memory
assert run("100000 100000 100000\n") == "100000", "x equals a"

# Exact divisibility
assert run("12 3 10\n") == "4", "exact number of groups"

# Remainder after full groups
assert run("10 2 5\n") == "4", "off-by-one remainder case"

# Maximum n with one file per memory
assert run("100000 99999 99999\n") == "100000", "maximum n boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Đầu vào kích thước tối thiểu | 
|`100000 100000 100000`|`100000`| Tối đa`n`Và`x = a`| 
|`12 3 10`|`4`| Chia chính xác thành nhóm bốn | 
|`10 2 5`|`4`| Công suất còn lại và chia trần | 
|`100000 99999 99999`|`100000`| Ranh giới lớn với một tệp trên mỗi bộ nhớ | 

## Vỏ cạnh 

Khi một tệp khớp chính xác với dung lượng bộ nhớ, thuật toán sẽ cho phép nó một cách chính xác. Đối với đầu vào`1 5 5`,`a // x`là`5 // 5 = 1`, Vì thế`(1 + 1 - 1) // 1 = 1`. Đầu ra là`1`. Điều này nắm bắt các triển khai vô tình sử dụng`<`thay vì`<=`khi lý luận về năng lực. 

Khi bộ nhớ có không gian chưa sử dụng quá nhỏ đối với một tệp khác, không gian đó không thể hỗ trợ bất kỳ bộ nhớ nào khác. Đối với đầu vào`5 2 5`, mỗi bộ nhớ giữ`5 // 2 = 2`tập tin. Việc tính toán trở nên`(5 + 2 - 1) // 2 = 3`, vì vậy đầu ra là`3`. Hai bộ nhớ cung cấp chỗ cho bốn tệp và tệp thứ năm yêu cầu bộ nhớ thứ ba. 

Khi`x = a`, chỉ có một tệp phù hợp với mỗi bộ nhớ. Đối với đầu vào`4 7 7`,`a // x = 1`, vậy câu trả lời là`(4 + 1 - 1) // 1 = 4`. Mỗi tệp đều cần bộ nhớ riêng, đó chính xác là những gì thuật toán báo cáo. 

Khi tất cả các tệp vừa với một bộ nhớ, phép chia trần sẽ tự nhiên trả về một bộ nhớ. Đối với đầu vào`3 5 15`, dung lượng mỗi bộ nhớ là`15 // 5 = 3`, cho`(3 + 3 - 1) // 3 = 1`. Không có trường hợp đặc biệt nào được yêu cầu cho tình huống này, đây là một đặc tính hữu ích của công thức.
