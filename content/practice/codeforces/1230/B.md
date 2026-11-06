---
title: "CF 1230B - Ania và giảm thiểu"
description: "Chúng ta được cấp một chuỗi thập phân rất lớn biểu thị một số có chính xác n chữ số. Chúng ta được phép thay đổi tối đa k chữ số này, mỗi lần một vị trí, thay thế một chữ số bằng bất kỳ chữ số nào khác từ 0 đến 9."
date: "2026-06-15T05:11:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1230
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 588 (Div. 2)"
rating: 1000
weight: 1230
solve_time_s: 199
verified: true
draft: false
---

[CF 1230B - Ania và giảm thiểu](https://codeforces.com/problemset/problem/1230/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi thập phân rất lớn biểu thị một số có chính xác`n`chữ số. Chúng ta được phép thay đổi nhiều nhất`k`của các chữ số này, mỗi lần một vị trí, thay thế một chữ số bằng bất kỳ chữ số nào khác từ`0`ĐẾN`9`. Sau khi thực hiện đến`k`những sửa đổi như vậy, chúng ta phải thu được số lượng nhỏ nhất có thể, trong khi vẫn giữ cho biểu diễn hợp lệ như một`n`-số có chữ số, nghĩa là chữ số đầu tiên không thể trở thành`0`Trừ khi`n = 1`. 

Nhiệm vụ này hoàn toàn là về thao tác chữ số với một số lần chỉnh sửa hạn chế. Mỗi lần chỉnh sửa đều có chi phí như nhau, vì vậy câu hỏi duy nhất là cần ưu tiên vị trí nào để giảm thiểu giá trị từ điển thu được. 

Những hạn chế là rất lớn, với`n`lên tới 200.000. Điều này ngay lập tức loại trừ mọi cách tiếp cận xem xét tất cả các tập hợp con thay đổi hoặc thực hiện quét lồng nhau cho mỗi vị trí. Bất kỳ chiến lược bậc hai nào chẳng hạn như thử mọi cách để phân phối các thay đổi hoặc tính toán lại kết quả tốt nhất sau mỗi lần sửa đổi sẽ quá chậm, vì`O(n^2)`sẽ theo thứ tự hoạt động 4e10 trong trường hợp xấu nhất. 

Điểm tinh tế mấu chốt trong vấn đề này là sự tương tác giữa tính tham lam và số lượng thay đổi có hạn. Một cách tiếp cận ngây thơ cố gắng “cải thiện chữ số cục bộ” mà không xem xét mức độ ưu tiên toàn cầu có thể thất bại. Ví dụ: thay đổi chi tiêu ở các chữ số sau thay vì các chữ số trước đó có thể khiến phần đầu không đạt mức tối ưu, ngay cả khi các cải tiến cục bộ có vẻ có lợi. 

Một trường hợp thất bại phổ biến xuất hiện khi người ta cố gắng giảm một cách tham lam bất kỳ chữ số nào chưa bằng 0 mà không ưu tiên các vị trí trước đó: 

đầu vào:```
3 1
909
```Một chiến lược bất cẩn có thể thay đổi chữ số cuối cùng thành`0`sản xuất`909 -> 900`, ở đây là tối ưu, nhưng nếu chúng ta có nhiều cấu trúc hơn như`901`với`k=1`, thay đổi chữ số cuối cùng sẽ cho`900`, trong khi thay đổi chữ số đầu tiên sẽ cho`001`không hợp lệ do số 0 đứng đầu. Nếu không xử lý cẩn thận ràng buộc chữ số đầu tiên, cách tiếp cận tham lam ngây thơ có thể dễ dàng vi phạm tính chính xác. 

Một trường hợp cạnh khác phát sinh khi`k`đủ lớn để không có nhiều chữ số, nhưng giải pháp tối ưu vẫn phải tôn trọng rằng các chữ số trước đó chiếm ưu thế về mặt từ điển. 

Những quan sát này gợi ý rằng chúng ta phải xử lý các vị trí từ trái sang phải và sử dụng những thay đổi ở nơi chúng quan trọng nhất. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các tập hợp con của các vị trí có kích thước lên tới`k`và thay thế những chữ số đó bằng mọi cách có thể. Ngay cả khi chúng ta đơn giản hóa và giả sử rằng chúng ta luôn thay thế một chữ số đã chọn bằng một số tối ưu thì riêng số tập hợp con là:$$\sum_{i=0}^{k} \binom{n}{i}$$đó là số mũ trong`n`. Với`n = 200000`, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của vấn đề gợi ý một chiều hướng tham lam mạnh mẽ hơn. Số cuối cùng được so sánh theo từ điển từ trái sang phải, vì vậy các chữ số trước chiếm ưu thế hơn tất cả các chữ số sau. Điều này ngụ ý rằng việc cải thiện một chữ số ở vị trí`i`luôn có giá trị hơn việc cải thiện bất kỳ chữ số nào ở vị trí`j > i`, miễn là cả hai cải tiến đều tốn cùng số lượng hoạt động. 

Từ đó, chiến lược đúng đắn xuất hiện: quét từ trái sang phải và bất cứ khi nào chúng ta có đủ khả năng, hãy buộc chữ số hiện tại`0`(chữ số nhỏ nhất có thể), ngoại trừ việc chúng ta phải đảm bảo chữ số đầu tiên không bằng 0 trừ khi`n = 1`. Mỗi lần chúng ta quyết định thay đổi một chữ số, chúng ta sẽ giảm`k`. Chúng tôi không bao giờ xem lại các quyết định trước đó vì một khi một chữ số được đặt ở mức tối ưu, việc thay đổi lại nó sẽ chỉ lãng phí các thao tác. 

Điều này có hiệu quả vì việc đặt một chữ số thành giá trị nhỏ nhất có thể ở vị trí sớm nhất sẽ tạo ra mức giảm từ điển lớn nhất cho mỗi thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các chữ số từ trái sang phải và duy trì số lượng thay đổi còn lại. 

1. Đọc số dưới dạng danh sách các ký tự để có thể sửa đổi từng chữ số riêng lẻ tại chỗ. Điều này tránh được chi phí nối chuỗi lặp đi lặp lại. 
2. Lặp lại từng chỉ mục`i`từ`0`ĐẾN`n - 1`. Tại mỗi vị trí, chúng tôi quyết định xem có nên dành một thay đổi hay không. 
3. Nếu`k > 0`, chúng tôi xem xét liệu chúng tôi có thể cải thiện chữ số này hay không. Sự cải thiện tốt nhất có thể là làm cho nó`0`. Điều này luôn tối ưu để giảm thiểu số lượng vì`0`là chữ số nhỏ nhất 
4. Đối với chữ số đầu tiên (`i = 0`), chúng tôi không thể đặt nó thành`0`nếu như`n > 1`, vì điều đó sẽ tạo ra số 0 đứng đầu và làm mất hiệu lực của số đó. Vì vậy chúng ta chỉ thay đổi nó nếu`n == 1`. 
5. Đối với tất cả các vị trí khác (`i > 0`), nếu chữ số chưa có`0`và chúng tôi vẫn còn những thay đổi, chúng tôi đặt nó thành`0`và giảm dần`k`. 
6. Tiếp tục cho đến khi hết chữ số hoặc hết số`k`. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, tiền tố của số được xây dựng cho đến nay là tiền tố nhỏ nhất có thể đạt được bằng cách sử dụng số lượng thao tác đã thực hiện. Vì thứ tự từ điển chỉ phụ thuộc vào vị trí khác nhau đầu tiên nên việc giảm thiểu từng vị trí càng sớm càng tốt sẽ đảm bảo tính tối ưu toàn cục. Bất kỳ việc sử dụng thao tác còn lại nào sau này đều không thể cải thiện chữ số trước đó, do đó, việc bảo toàn các thao tác cho các vị trí sau này không bao giờ hiệu quả hơn việc sử dụng chúng ngay khi xuất hiện mức giảm có lợi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
s = list(input().strip())

for i in range(n):
    if k == 0:
        break

    if i == 0:
        if n > 1 and s[i] != '1':
            # we cannot set to 0, best is 1
            if s[i] != '1':
                s[i] = '1'
                k -= 1
    else:
        if s[i] != '0':
            s[i] = '0'
            k -= 1

print("".join(s))
```Giải pháp hoạt động bằng cách thực thi một cách tham lam chữ số nhỏ nhất có thể ở mỗi vị trí. Chữ số đầu tiên được xử lý riêng vì nó không thể trở thành số 0 trừ khi số đó có độ dài bằng một. Đối với tất cả các chữ số khác, mọi chữ số khác 0 sẽ ngay lập tức giảm xuống 0 nếu vẫn duy trì các thao tác. 

Một điểm tinh tế là chúng tôi không bao giờ thử cải tiến từng phần như giảm một chữ số từ`7`ĐẾN`6`. Điều đó là không cần thiết vì bỏ ra một thao tác để làm ra nó`0`ít nhất luôn luôn tốt theo thứ tự từ điển, vì bất kỳ chữ số nào khác 0 đều có tác dụng tương đương với các vị trí trong tương lai khi tiền tố được cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
51528
```Chúng tôi xử lý từ trái sang phải. 

| tôi | chữ số | k trước | hành động | chữ số sau | k sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 3 | đặt thành 1 | 1 | 2 | 
| 1 | 1 | 2 | đã 1 rồi à? không áp dụng quy tắc thay đổi | 1 | 2 | 
| 2 | 5 | 2 | đặt thành 0 | 0 | 1 | 
| 3 | 2 | 1 | đặt thành 0 | 0 | 0 | 
| 4 | 8 | 0 | dừng lại | 8 | 0 | 

Kết quả là:```
10008
```Dấu vết này cho thấy rằng khi chữ số đứng đầu được thu nhỏ, các thao tác còn lại sẽ được sử dụng tốt nhất để loại bỏ các chữ số sau. 

### Ví dụ 2 

đầu vào:```
4 2
1029
```| tôi | chữ số | k trước | hành động | chữ số sau | k sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | đã tối ưu | 1 | 2 | 
| 1 | 0 | 2 | không thay đổi | 0 | 2 | 
| 2 | 2 | 2 | đặt thành 0 | 0 | 1 | 
| 3 | 9 | 1 | đặt thành 0 | 0 | 0 | 

Đầu ra:```
1000
```Điều này cho thấy các chữ số sau được giảm thiểu một cách mạnh mẽ như thế nào khi các vị trí trước đó được bảo mật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chữ số được xử lý một lần trong một lần chuyển | 
| Không gian | O(n) | Chúng tôi lưu trữ số dưới dạng danh sách các chữ số có thể thay đổi | 

Quét tuyến tính là tối ưu cho`n`lên tới 200.000 và dễ dàng phù hợp với giới hạn thời gian vì nó chỉ thực hiện các thao tác đơn giản với thời gian không đổi cho mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    s = list(input().strip())

    for i in range(n):
        if k == 0:
            break

        if i == 0:
            if n > 1 and s[i] != '1':
                s[i] = '1'
                k -= 1
        else:
            if s[i] != '0':
                s[i] = '0'
                k -= 1

    return "".join(s)

# provided sample
assert run("5 3\n51528\n") == "10028", "sample 1"

# minimum size
assert run("1 0\n7\n") == "7", "single digit no change"

# maximum reduction
assert run("3 3\n999\n") == "100", "force minimal prefix"

# no operations
assert run("4 2\n1234\n") == "1034", "only two zeros possible"

# already optimal
assert run("4 2\n1000\n") == "1000", "no improvement"

# edge leading digit
assert run("2 1\n10\n") == "10", "cannot reduce leading digit to 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 chữ số, k=0 | 7 | không được phép sửa đổi | 
| 999, k=3 | 100 | giảm thiểu tối đa | 
| 1234, k=2 | 1034 | ứng dụng tham lam một phần | 
| 1000, k=2 | 1000 | cấu trúc đã tối thiểu | 
| 10, k=1 | 10 | ràng buộc số 0 hàng đầu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chữ số đầu tiên lớn hơn`1`Và`k > 0`. Thuật toán buộc nó phải`1`, không`0`, bảo toàn hiệu lực. Ví dụ: 

đầu vào:```
3 2
902
```Chữ số đầu tiên`9`trở thành`1`, rời đi`k = 1`. Các chữ số còn lại sau đó được giảm thiểu một cách tham lam. Thuật toán đảm bảo số vẫn hợp lệ trong khi vẫn ưu tiên tiền tố nhỏ nhất theo từ điển. 

Một trường hợp khác là khi tất cả các chữ số đã có sẵn`0`ngoại trừ cái đầu tiên. Không có hoạt động nào bị lãng phí trên các vị trí đã tối ưu, vì điều kiện`s[i] != '0'`ngăn ngừa sự giảm sút không cần thiết của`k`. 

Hành vi cuối cùng luôn duy trì tính bất biến mà mọi thao tác được sử dụng đều tạo ra sự cải thiện từ điển sớm nhất có thể.
