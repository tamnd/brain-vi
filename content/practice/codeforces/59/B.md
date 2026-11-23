---
title: "CF 59B - Bói"
description: "Marina có thể hái bất kỳ loại hoa nào trên cánh đồng. Mỗi bông hoa có một số lượng cánh hoa nhất định và cô ấy sẽ ngắt từng cánh hoa từ tất cả những bông hoa đã chọn. Các cụm từ xen kẽ giữa \"Yêu\" và \"Không yêu\", bắt đầu từ \"Yêu\" trên cánh hoa đầu tiên."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 59
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 55 (Div. 2)"
rating: 1200
weight: 59
solve_time_s: 117
verified: false
draft: false
---

[CF 59B - Bói toán](https://codeforces.com/problemset/problem/59/B) 

**Đánh giá:** 1200 
**Tags:** thực hiện, lý thuyết số 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Marina có thể hái bất kỳ loại hoa nào trên cánh đồng. Mỗi bông hoa có một số lượng cánh hoa nhất định và cô ấy sẽ ngắt từng cánh hoa từ tất cả những bông hoa đã chọn. Các cụm từ xen kẽ giữa`"Loves"`Và`"Doesn't love"`, bắt đầu từ`"Loves"`trên cánh hoa đầu tiên. 

Cụm từ cuối cùng chỉ phụ thuộc vào tổng số cánh hoa trong bó hoa. Nếu tổng số là số lẻ thì cụm từ được nói cuối cùng là`"Loves"`. Nếu tổng số là số chẵn thì cụm từ cuối cùng là`"Doesn't love"`. 

Nhiệm vụ trở nên đơn giản hơn nhiều sau khi quan sát này. Chúng tôi hoàn toàn không mô phỏng quá trình bói toán. Chúng ta chỉ cần chọn một tập con có tổng lẻ, đồng thời làm cho tổng đó càng lớn càng tốt. 

Số lượng hoa nhiều nhất là 100, mỗi bông hoa có nhiều nhất 100 cánh hoa. Ngay cả một giải pháp vũ phu trên tất cả các tập hợp con cũng sẽ liên quan đến việc kiểm tra$2^{100}$khả năng, điều đó hoàn toàn không thể xảy ra. Một triệu thao tác thường là ổn trong lập trình cạnh tranh, nhưng$2^{100}$lớn hơn về mặt thiên văn. 

Giới hạn giá trị nhỏ trên số lượng cánh hoa gợi ý rằng giải pháp có thể phụ thuộc vào tính chẵn lẻ hơn là sự tối ưu hóa phức tạp. Khi chúng ta rút gọn bài toán thành "tìm tổng lẻ lớn nhất", cấu trúc sẽ trở nên rất trực tiếp. 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. 

Giả sử mỗi bông hoa có số cánh chẵn:```
3
2 4 6
```Tổng số là số chẵn và việc loại bỏ bất kỳ số chẵn nào vẫn để lại số tiền chẵn. Không có bó hoa lẻ nào tồn tại nên câu trả lời đúng là:```
0
```Cách tiếp cận ngây thơ luôn in tổng số tiền khi có thể sẽ thất bại ở đây. 

Một trường hợp phức tạp khác là khi tổng số tiền là số chẵn nhưng lại có hoa lẻ:```
4
1 2 4 8
```Tổng số là 15, vốn đã là số lẻ nên câu trả lời là 15. Một số cách triển khai xóa nhầm một bông hoa lẻ bất cứ khi nào họ nhìn thấy một bông hoa lẻ, ngay cả khi tổng số đã đúng. 

Một chế độ lỗi khác xuất hiện khi tổng số tiền chẵn và có nhiều bông hoa lẻ:```
5
1 3 5 2 2
```Tổng số tiền là 13, đã là số lẻ nên chúng tôi giữ lại mọi thứ. Nhưng nếu tổng số là số chẵn thì chiến lược đúng đắn sẽ là loại bỏ bông hoa lẻ nhỏ nhất chứ không phải bất kỳ bông hoa lẻ nào. Loại bỏ một bông hoa lẻ lớn hơn sẽ làm giảm câu trả lời một cách không cần thiết. 

Ví dụ:```
3
3 5 8
```Tổng số là 16. Loại bỏ 5 được 11, loại bỏ 3 được 13. Câu trả lời tối ưu là 13. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi tập hợp con hoa có thể có, tính tổng số cánh hoa trong tập hợp con đó và giữ số lẻ lớn nhất. 

Điều này hiệu quả vì mỗi bó hoa tương ứng với chính xác một tập hợp con. Nếu chúng tôi kiểm tra tất cả các tập hợp con, chúng tôi đảm bảo sẽ tìm được tập hợp lệ tốt nhất. 

Vấn đề là số lượng tập hợp con. Với$n = 100$, tổng số tập con là:$$2^{100}$$Điều đó vượt xa những gì bất kỳ chương trình nào có thể xử lý trong thời gian giới hạn. 

Quan sát quan trọng là chỉ có tính chẵn lẻ mới quan trọng. 

Tổng số lẻ tạo ra`"Loves"`, và tổng số chẵn tạo ra`"Doesn't love"`. Vì vậy, chúng tôi muốn số tiền lẻ lớn nhất có thể. 

Bắt đầu với tổng số hoa. Chỉ có hai trường hợp. 

Nếu tổng số đã là số lẻ thì rõ ràng chúng ta nên giữ lại từng bông hoa vì việc loại bỏ bất kỳ số dương nào chỉ làm giảm tổng. 

Nếu tổng là số chẵn thì chúng ta phải thay đổi tính chẵn lẻ của nó. Việc loại bỏ một bông hoa chẵn sẽ giữ cho số tiền chẵn, vì vậy điều đó không giúp ích gì. Loại bỏ một bông hoa lẻ sẽ biến tính chẵn lẻ thành số lẻ. 

Trong số tất cả các bông hoa lẻ, chúng ta nên loại bỏ bông hoa nhỏ nhất vì chúng ta muốn tổng số còn lại càng lớn càng tốt. 

Nếu không có bông hoa lẻ nào thì mọi tổng tập con có thể có đều là số chẵn và câu trả lời là 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng hoa và số cánh hoa. 
2. Tính tổng số cánh hoa. 

Giữ tất cả số hoa sẽ mang lại số tiền lớn nhất có thể, vì vậy đây là điểm khởi đầu tốt nhất. 
3. Kiểm tra tổng số tiền có phải là số lẻ hay không. 

Nếu là số lẻ, hãy in tổng ngay lập tức vì không có tập hợp con nào nhỏ hơn có thể cải thiện câu trả lời. 
4. Nếu tổng số là số chẵn, hãy tìm bông hoa nhỏ nhất có số cánh hoa là số lẻ. 

Việc loại bỏ một số lẻ sẽ biến tổng chẵn thành tổng lẻ. 
5. Nếu có một bông hoa như vậy, hãy in:$$\text{total sum} - \text{smallest odd flower}$$Điều này tạo ra số tiền lẻ tối đa có thể vì chúng tôi loại bỏ số tiền tối thiểu cần thiết để cố định số chẵn lẻ. 

1. Nếu không có hoa lẻ thì in ra số 0. 

Trong trường hợp này mọi bông hoa đều chẵn nên mọi tổng tập hợp con cũng chẵn. 

### Tại sao nó hoạt động 

Thuật toán dựa trên các thuộc tính chẵn lẻ. 

Tổng các số chỉ thay đổi tính chẵn lẻ khi một số lẻ được thêm vào hoặc bớt đi. Bắt đầu từ số tiền tối đa có thể, chỉ có hai khả năng. 

Nếu tổng số đã là số lẻ thì bất kỳ việc loại bỏ nào cũng sẽ làm giảm câu trả lời, vì vậy việc giữ lại mọi thứ là tối ưu. 

Nếu tổng là số chẵn thì chúng ta phải loại bỏ giá trị lẻ để thành số lẻ. Việc loại bỏ giá trị lẻ nhỏ nhất sẽ giảm thiểu tổn thất, đảm bảo tổng số lẻ thu được càng lớn càng tốt. 

Không có hoạt động nào khác có thể tạo ra câu trả lời tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)

if total % 2 == 1:
    print(total)
else:
    smallest_odd = float('inf')

    for x in a:
        if x % 2 == 1:
            smallest_odd = min(smallest_odd, x)

    if smallest_odd == float('inf'):
        print(0)
    else:
        print(total - smallest_odd)
```Chương trình bắt đầu bằng cách đọc số lượng cánh hoa và tính tổng số cánh hoa. 

Nhánh đầu tiên xử lý trường hợp dễ nhất. Nếu tổng số đã là số lẻ, chúng tôi sẽ in trực tiếp vì việc giữ tất cả hoa luôn tối đa hóa kích thước bó hoa. 

Nhánh thứ hai tìm kiếm bông hoa lẻ nhỏ nhất. Biến`smallest_odd`bắt đầu ở vô cùng để chúng ta có thể giảm thiểu một cách an toàn so với các giá trị thực trong vòng lặp. 

Việc kiểm tra tính chẵn lẻ sử dụng`x % 2 == 1`. Bất kỳ bông hoa lẻ nào cũng có thể lật ngược số chẵn lẻ của tổng khi bỏ đi. 

Điều kiện:```
smallest_odd == float('inf')
```có nghĩa là không tìm thấy bông hoa kỳ lạ nào. Trong trường hợp đó mọi bông hoa đều chẵn và không tồn tại tập con lẻ nào. 

Không có lo ngại về tràn vì tổng số tối đa có thể chỉ là:$$100 \times 100 = 10000$$Tuy nhiên, số nguyên Python tự động xử lý các giá trị lớn hơn nhiều. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1
```| Bước | Giá trị | 
| --- | --- | 
| Tổng số tiền | 1 | 
| Tổng số chẵn lẻ | lẻ | 
| Đầu ra | 1 | 

Tổng số đã là số lẻ nên lấy bông hoa duy nhất là tối ưu. 

### Ví dụ 2 

đầu vào:```
3
3 5 8
```| Bước | Giá trị | 
| --- | --- | 
| Tổng số tiền | 16 | 
| Tổng số chẵn lẻ | Thậm chí | 
| Hoa lẻ | 3, 5 | 
| Số lẻ nhỏ nhất | 3 | 
| Câu trả lời cuối cùng | 16 - 3 = 13 | 

Dấu vết cho thấy tại sao việc loại bỏ những bông hoa nhỏ nhất lại quan trọng. Loại bỏ 5 sẽ chỉ tạo ra 11, điều này còn tệ hơn. 

### Ví dụ 3 

đầu vào:```
4
2 4 6 8
```| Bước | Giá trị | 
| --- | --- | 
| Tổng số tiền | 20 | 
| Tổng số chẵn lẻ | Thậm chí | 
| Hoa kỳ lạ được tìm thấy | Không có | 
| Đầu ra | 0 | 

Mọi bông hoa đều chẵn nên mọi tập hợp con đều chẵn. Không có bó hoa hợp lệ tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lượt tính tổng và một lượt tìm bông hoa lẻ nhỏ nhất | 
| Không gian |$O(1)$| Chỉ có một vài biến được sử dụng | 

Với tối đa 100 bông hoa, giải pháp sẽ diễn ra ngay lập tức. Thuật toán tuyến tính và sử dụng bộ nhớ bổ sung không đổi, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)

    if total % 2 == 1:
        print(total)
    else:
        smallest_odd = float('inf')

        for x in a:
            if x % 2 == 1:
                smallest_odd = min(smallest_odd, x)

        if smallest_odd == float('inf'):
            print(0)
        else:
            print(total - smallest_odd)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("1\n1\n") == "1", "sample 1"

# all even values
assert run("4\n2 4 6 8\n") == "0", "all sums are even"

# total already odd
assert run("4\n1 2 4 8\n") == "15", "keep all flowers"

# remove smallest odd
assert run("3\n3 5 8\n") == "13", "must remove minimum odd"

# minimum size with even flower
assert run("1\n2\n") == "0", "single even flower"

# maximum size style case
assert run("5\n100 100 100 100 99\n") == "499", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 2 4 6 8`|`0`| Không tồn tại tập hợp con lẻ nào | 
|`4 / 1 2 4 8`|`15`| Giữ lại toàn bộ số hoa khi tổng số đã là số lẻ | 
|`3 / 3 5 8`|`13`| Phải loại bỏ bông hoa lẻ nhỏ nhất | 
|`1 / 2`|`0`| Vỏ đơn cạnh hoa chẵn | 
|`5 / 100 100 100 100 99`|`499`| Hoạt động chính xác với giá trị lớn hơn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
3
2 4 6
```Thuật toán tính tổng là 12. Vì 12 là số chẵn nên nó tìm kiếm bông hoa lẻ nhỏ nhất. Không tồn tại vì mọi giá trị đều chẵn. 

Biến`smallest_odd`không bao giờ thay đổi từ vô cùng nên thuật toán in ra 0. 

Điều này đúng vì mọi tập con của số chẵn cũng có tổng chẵn. 

Bây giờ hãy xem xét:```
3
3 5 8
```Tổng số là 16, là số chẵn. Thuật toán quét mảng và tìm ra hoa lẻ 3 và 5. Tối thiểu là 3. 

Loại bỏ 3 tạo ra:```
16 - 3 = 13
```đó là số lẻ và tối đa. 

Thay vào đó, nếu bỏ đi 5 thì kết quả sẽ là 11, vì vậy việc chọn bông hoa lẻ nhỏ nhất là điều cần thiết. 

Cuối cùng, kiểm tra:```
4
1 2 4 8
```Tổng số tiền là 15, đã là số lẻ. 

Thuật toán in ngay 15 mà không xóa bất cứ thứ gì. 

Điều này tránh được một lỗi phổ biến khi việc triển khai loại bỏ một bông hoa lẻ một cách không cần thiết mặc dù tính chẵn lẻ đã chính xác.
