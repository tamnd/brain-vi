---
title: "CF 1092A - Chuỗi đồng nhất"
description: "Chúng ta được yêu cầu xây dựng một chuỗi cho mỗi truy vấn sao cho chuỗi đó có độ dài cố định n và chỉ sử dụng k chữ cái Latinh viết thường đầu tiên, từ 'a' cho đến chữ cái thứ k. Mỗi k chữ cái này phải xuất hiện ít nhất một lần trong chuỗi cuối cùng."
date: "2026-06-13T04:36:13+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1092
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 527 (Div. 3)"
rating: 800
weight: 1092
solve_time_s: 586
verified: false
draft: false
---

[CF 1092A - Chuỗi đồng nhất](https://codeforces.com/problemset/problem/1092/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 9 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một chuỗi cho mỗi truy vấn sao cho chuỗi đó có độ dài cố định`n`và chỉ sử dụng cái đầu tiên`k`chữ cái Latinh viết thường, từ`'a'`lên đến`k`- lá thư thứ Mỗi cái này`k`các chữ cái phải xuất hiện ít nhất một lần trong chuỗi cuối cùng. Trong số tất cả các cấu trúc hợp lệ, chúng ta muốn tối đa hóa tần suất tối thiểu của bất kỳ chữ cái nào được sử dụng trong chuỗi. 

Nói cách khác, chúng tôi đang phân phối`n`vị trí giữa`k`những biểu tượng riêng biệt. Mỗi biểu tượng phải xuất hiện ít nhất một lần và chúng tôi muốn phân phối cân bằng nhất có thể, trong đó nhóm nhỏ nhất càng lớn càng tốt. 

Các hạn chế là nhỏ:`n ≤ 100`,`k ≤ 26`, và lên đến`t ≤ 100`truy vấn. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc kỹ thuật tối ưu hóa. Bất kỳ giải pháp nào chạy theo thời gian tuyến tính hoặc thậm chí bậc hai cho mỗi truy vấn đều dễ dàng đủ. Cấu trúc gợi ý một giải pháp mang tính xây dựng hơn là tìm kiếm hoặc lập trình động. 

Một điểm tinh tế quan trọng là hiểu "tối đa hóa tần số tối thiểu" ngụ ý gì. Nếu chúng ta chỉ định số lượng`c1, c2, ..., ck`, chúng tôi đang tối đa hóa`min(ci)`dưới những ràng buộc mà tất cả`ci ≥ 1`và tổng hợp thành`n`. Đây là một vấn đề cân bằng thuần túy. 

Trường hợp cạnh xuất hiện khi`n`chỉ lớn hơn một chút so với`k`. Ví dụ, nếu`n = k`, thì mỗi ký tự phải xuất hiện đúng một lần, vì chúng ta đã cần tất cả các chữ cái và không còn khoảng trắng. Mọi nỗ lực để “cân bằng hơn nữa” đều không thể thực hiện được. 

Một trường hợp cạnh khác là khi`n`lớn hơn nhiều so với`k`. Một cách tiếp cận tham lam ngây thơ luôn gắn thêm chữ cái tiếp theo hoạt động theo chu kỳ, nhưng không có lý do về cân bằng tần số, người ta có thể cho rằng cần phải sắp xếp lại phức tạp hơn một cách sai lầm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng phân phối`n`vị trí giữa`k`các chữ cái theo mọi cách có thể sao cho mỗi chữ cái xuất hiện ít nhất một lần. Đối với mỗi phân phối, chúng tôi sẽ tính toán tần suất tối thiểu và theo dõi tần suất tốt nhất. Số lượng các bản phân phối như vậy tương đương với các phân vùng của`n`vào trong`k`các số nguyên dương tăng theo cấp số nhân với`n`. Ngay cả đối với`n = 100`, điều này trở nên hoàn toàn không thể thực hiện được. 

Cấu trúc của hàm mục tiêu làm cho bài toán trở nên đơn giản hơn nhiều. Vì chúng tôi chỉ quan tâm đến việc tối đa hóa số lượng tối thiểu nên giải pháp tốt nhất có thể sẽ cố gắng cân bằng tất cả các tần số nhiều nhất có thể. Nếu mọi chữ cái đều có chính xác`n / k`xảy ra, đó sẽ là lý tưởng, nhưng phép chia số nguyên ngăn cản sự bình đẳng hoàn hảo. Do đó, tần số tối thiểu tối ưu được xác định bằng bao nhiêu khối kích thước đầy đủ`k`phù hợp với`n`. 

Một khi chúng ta hiểu mục tiêu là sự phân bổ đồng đều thì việc xây dựng sẽ trở nên đơn giản. Chúng tôi liên tục gán các chữ cái theo cách vòng tròn trên chữ cái đầu tiên`k`nhân vật. Điều này đảm bảo mọi tiền tố của`k`các ký tự được cân bằng hoàn hảo và mọi ký tự còn lại sẽ được phân phối lại từ đầu, duy trì tần suất tối thiểu tốt nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong n | O(k) | Quá chậm | 
| Xây dựng tối ưu | O(n) mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng chuỗi độc lập cho từng truy vấn. 

1. Khởi tạo một chuỗi kết quả trống. 
2. Lặp lại từ`0`ĐẾN`n - 1`. 
3. Đối với từng vị trí`i`, nối thêm ký tự tương ứng với`(i mod k)`. 

Điều này đảm bảo chúng ta sẽ thực hiện chu kỳ đầu tiên`k`các chữ cái lặp đi lặp lại. 
4. Sau khi kết thúc vòng lặp, xuất chuỗi đã tạo. 

Lý do đằng sau bước 3 là việc đạp xe đảm bảo việc sử dụng thống nhất. Mỗi khối của`k`các vị trí liên tiếp chứa chính xác một lần xuất hiện của mỗi ký tự được phép. Điều này ngăn không cho bất kỳ ký tự nào bị tụt lại phía sau về tần số so với các ký tự khác. 

### Tại sao nó hoạt động 

Mỗi chu kỳ đầy đủ có độ dài`k`đóng góp chính xác một lần xuất hiện của mỗi ký tự`'a'`ĐẾN`'a' + k - 1`. Sau đó`⌊n / k⌋`chu kỳ đầy đủ, tất cả các ký tự có tần số cơ sở bằng nhau. Mọi thứ còn lại`n mod k`vị trí được gán cho một vài ký tự đầu tiên, tăng tần suất của chúng lên tối đa một. Điều này đảm bảo rằng sự khác biệt giữa hai tần số ký tự bất kỳ nhiều nhất là một, đây là mức cân bằng tốt nhất có thể. Do đó, tần số tối thiểu được tối đa hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    
    res = []
    for i in range(n):
        res.append(chr(ord('a') + (i % k)))
    
    print("".join(res))
```Việc thực hiện trực tiếp theo sau việc xây dựng theo chu kỳ. Danh sách`res`được sử dụng thay vì nối chuỗi để đạt hiệu quả, mặc dù có những ràng buộc nhất định, cả hai đều sẽ vượt qua. 

Chi tiết chính là hoạt động modulo`(i % k)`, điều này đảm bảo chúng tôi luôn nằm trong phạm vi bảng chữ cái được phép. Bản đồ`chr(ord('a') + x)`chuyển đổi một offset số nguyên thành một chữ cái viết thường. 

Không cần xử lý đặc biệt đối với các trường hợp khó khăn như`n = k`hoặc`k = 1`, vì cùng một vòng lặp tự nhiên tạo ra đầu ra hợp lệ trong cả hai trường hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n = 7, k = 3`Chúng tôi xây dựng bằng cách sử dụng các chữ cái`a, b, c`. 

| tôi | tôi %k | nhân vật | kết quả cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 0 | một | một | 
| 1 | 1 | b | ab | 
| 2 | 2 | c | abc | 
| 3 | 0 | một | abca | 
| 4 | 1 | b | abcab | 
| 5 | 2 | c | abcabc | 
| 6 | 0 | một | abcabca | 

Đầu ra là`abcabca`. Điều này đảm bảo số lượng là`a:3, b:2, c:2`, do đó tần số tối thiểu được tối đa hóa. 

### Ví dụ 2 

đầu vào:`n = 6, k = 2`Chúng tôi sử dụng chữ cái`a, b`. 

| tôi | tôi %k | nhân vật | kết quả cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 0 | một | một | 
| 1 | 1 | b | ab | 
| 2 | 0 | một | aba | 
| 3 | 1 | b | abab | 
| 4 | 0 | một | ba ba | 
| 5 | 1 | b | ababab | 

kết quả`ababab`cho tần số hoàn toàn bằng nhau,`a:3, b:3`, đó là tối ưu. 

Những dấu vết này cho thấy mô hình tuần hoàn thực thi sự cân bằng một cách tự nhiên mà không cần tính toán hoặc điều chỉnh rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | Mỗi ký tự được tạo một lần trong một vòng lặp đơn giản | 
| Không gian | O(n) | Lưu trữ chuỗi đầu ra cho mỗi truy vấn | 

Cho rằng`n ≤ 100`Và`t ≤ 100`, tổng số thao tác nhiều nhất là`10^4`, đó là tầm thường trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        res = []
        for i in range(n):
            res.append(chr(ord('a') + (i % k)))
        out.append("".join(res))
    return "\n".join(out)

def run(inp: str) -> str:
    return solve(inp)

# provided samples
assert run("3\n7 3\n4 4\n6 2\n") == "abcabca\nabcd\nababab"

# custom cases
assert run("1\n1 1\n") == "a", "single character"
assert run("1\n5 1\n") == "aaaaa", "single letter repeated"
assert run("1\n5 5\n") == "abcde", "each letter once"
assert run("1\n10 3\n") == "abcabcabca", "repeating cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | một | trường hợp nhỏ nhất | 
| 5 1 | aaa | lặp lại ký tự đơn | 
| 5 5 | abcde | ranh giới phân phối chính xác | 
| 10 3 | abcabcaca | cân bằng theo chu kỳ trên nhiều khối | 

## Vỏ cạnh 

Khi nào`n = k`, vòng lặp tạo ra mỗi ký tự đúng một lần, vì`(i % k)`bao gồm tất cả các giá trị từ`0`ĐẾN`k - 1`đúng một lần. Ví dụ,`n = 4, k = 4`sản xuất`abcd`, vốn đã đáp ứng yêu cầu là mỗi chữ cái xuất hiện ít nhất một lần và không thể cân bằng được ngoài điều đó. 

Khi`k = 1`, mọi vị trí đều ánh xạ tới`'a'`. Phép toán modulo luôn bằng 0, do đó đầu ra là một chuỗi`n` `'a'`nhân vật. Điều này là tối ưu vì chỉ có một chữ cái được phép. 

Khi`n`không chia hết cho`k`, các ký tự còn lại chỉ đơn giản là tăng tần suất của một vài chữ cái đầu tiên. Ví dụ,`n = 7, k = 3`sản xuất`a, b, c, a, b, c, a`, đưa ra số đếm khác nhau nhiều nhất là một. Điều này xác nhận rằng không thể đạt được tần số tối thiểu tốt hơn vì bất kỳ sự phân phối lại nào cũng sẽ buộc ít nhất một ký tự giảm xuống dưới mức sàn này.
