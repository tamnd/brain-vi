---
title: "CF 102599A - \u0414\u043e\u043b\u0433\u0430\u044f \u0438\u0433\u0440\u0430"
description: "Chúng ta có N khối lập phương được đánh số. Một khối được coi là đúng nếu vị trí hiện tại của nó trong hàng khớp với số của nó. Trong mỗi lần di chuyển, tất cả các khối hiện không chính xác sẽ được sắp xếp lại một cách ngẫu nhiên, trong khi các khối đã đúng vẫn không bị ảnh hưởng."
date: "2026-07-31T05:42:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102599
codeforces_index: "A"
codeforces_contest_name: "The fifth Lipetsk collegiate programming contest. Finals. 8-11 form"
rating: 0
weight: 102599
solve_time_s: 266
verified: true
draft: false
---

[CF 102599A - \u0414\u043e\u043b\u0433\u0430\u044f \u0438\u0433\u0440\u0430](https://codeforces.com/problemset/problem/102599/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`N`hình khối được đánh số. Một khối được coi là đúng nếu vị trí hiện tại của nó trong hàng khớp với số của nó. Trong mỗi lần di chuyển, tất cả các khối hiện không chính xác sẽ được sắp xếp lại một cách ngẫu nhiên, trong khi các khối đã đúng vẫn không bị ảnh hưởng. Trò chơi bắt đầu với tất cả các khối đều có khả năng di chuyển được và chúng ta cần số lần di chuyển dự kiến ​​cho đến khi mỗi khối đạt đến vị trí riêng của nó. 

Câu trả lời là một phân số, nhưng hóa ra nó có một giá trị rất đơn giản. Vì mô đun là số nguyên tố và kết quả phải được in theo mô đun`998244353`, việc triển khai chỉ cần xuất ra giá trị mô-đun tương ứng. 

Ràng buộc`N ≤ 10^6`loại trừ các mô phỏng, lập trình động trên tất cả các số khối cố định có thể có hoặc bất kỳ phương pháp nào cần xử lý mọi trạng thái có thể có. Giải pháp phải chạy theo thời gian tuyến tính hoặc tốt hơn, chỉ sử dụng một lượng nhỏ bộ nhớ. Cấu trúc ẩn của quá trình ngẫu nhiên là cách duy nhất để đáp ứng giới hạn. 

Một sai lầm phổ biến là nghĩ rằng quy trình cần phải được mô phỏng vì mỗi lần xáo trộn sẽ làm thay đổi cách sắp xếp. Vì`N = 1`, khối lập phương đã được đảm bảo là đúng sau lần di chuyển đầu tiên, vì vậy câu trả lời là`1`. Một giải pháp bắt đầu bằng việc lặp lại các số dương nhưng quên trường hợp cơ sở này có thể tạo ra kết quả không chính xác`0`. 

Một trường hợp nhỏ khác là`N = 2`. Sau lần xáo trộn đầu tiên, các khối đều đúng hoặc cả hai đều sai. Số lần di chuyển dự kiến ​​là`2`. Một cách tiếp cận bất cẩn chỉ dựa trên xác suất hoàn thành ngay lập tức có thể bỏ lỡ thực tế là cùng một trạng thái có thể xuất hiện lặp đi lặp lại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng mô tả trạng thái theo số khối hiện không chính xác. Cho phép`E(n)`là số bước di chuyển dự kiến ​​cần thiết khi`n`hình khối vẫn cần phải được sửa chữa. Sau một lần di chuyển, một số hình khối sẽ trở thành chính xác và quá trình tiếp tục với trạng thái nhỏ hơn. Sự lặp lại này là hợp lệ, nhưng việc tính toán xác suất chuyển đổi yêu cầu đếm các hoán vị với một số vị trí cố định nhất định, được gọi là số rencontres. Vì`N = 10^6`, việc lưu trữ và xử lý tất cả những chuyển đổi đó là không thể. 

Quan sát quan trọng là chúng ta không thực sự cần toàn bộ phân phối. Chúng ta chỉ cần số khối dự kiến ​​vẫn không chính xác sau một lần xáo trộn. 

Hãy xem xét một trong những`n`hình khối hiện tại không chính xác. Sau một hoán vị ngẫu nhiên, khối này có đúng một vị trí mà nó trở thành đúng. Cơ hội hạ cánh của nó là ở đó`1/n`, vậy khả năng nó vẫn sai là`(n-1)/n`. Theo tính tuyến tính của kỳ vọng, số khối sai mong đợi sau khi di chuyển là:`n * (n - 1) / n = n - 1`. 

Điều này mang lại một sự tái phát rất ngắn. Nếu số lần di chuyển dự kiến ​​từ`n`hình khối không đúng là`E(n)`, thì sau nước đi đầu tiên, chúng ta thực hiện một nước đi và còn lại trạng thái mong đợi tương đương với`n - 1`hình khối:`E(n) = 1 + E(n - 1)`. 

Với`E(0) = 0`, điều này ngay lập tức mang lại`E(n) = n`. 

Điều quan trọng là chúng ta không bao giờ cần biết chính xác số khối còn lại sau khi xáo trộn. Kỳ vọng về số tiền còn lại là đủ vì bản thân sự tái diễn là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) hoặc tệ hơn | O(N) hoặc tệ hơn | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`N`, số lượng hình khối trong trò chơi. Số lần di chuyển dự kiến ​​bằng số khối nên không cần mô phỏng hoặc tính toán xác suất. 
2. Đầu ra`N`modulo`998244353`. Giá trị không bao giờ vượt quá mô-đun trong bài toán này, nhưng việc sử dụng đầu ra mô-đun sẽ giúp quá trình triển khai luôn nhất quán với định dạng được yêu cầu. 

Tại sao nó hoạt động: 

Bất biến đằng sau lời giải là khi có`k`các khối không đúng, một nước đi sẽ làm giảm đúng một số khối sai dự kiến. Lý do là mỗi khối lập phương độc lập đều có xác suất`1/k`được cố định trong quá trình xáo trộn, mang lại mức giảm dự kiến`k * 1/k = 1`. Kể từ khi trò chơi bắt đầu với`N`các hình khối và kết thúc không chính xác sau khi đạt đến số 0, thì số lần giảm dự kiến ​​​​cần chính xác là`N`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    print(n % MOD)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện công thức dẫn xuất`E(N) = N`. Không cần mảng, bảng xác suất hoặc nghịch đảo mô-đun. 

Giá trị đầu vào duy nhất được đọc một lần. Hoạt động modulo được đưa vào vì câu lệnh yêu cầu câu trả lời modulo`998244353`, mặc dù câu trả lời tối đa chỉ là`10^6`, nhỏ hơn nhiều so với mô đun. 

## Ví dụ đã hoạt động 

cho`N = 1`, thuật toán bắt đầu bằng: 

| Biến | Giá trị | 
| --- | --- | 
|`n`| 1 | 
| Những động thái dự kiến ​​| 1 | 

Khối đơn phải trở nên chính xác sau lần xáo trộn đầu tiên, vì vậy câu trả lời là`1`. 

Vì`N = 2`, thuật toán cho: 

| Biến | Giá trị | 
| --- | --- | 
|`n`| 2 | 
| Những động thái dự kiến ​​| 2 | 

Quá trình này có thể lặp lại vì cả hai khối có thể hoán đổi và vẫn không chính xác, nhưng số bước di chuyển dự kiến ​​vẫn chính xác là số khối. 

Những ví dụ này thể hiện cả hành vi biên và kết quả truy hồi. Công thức tự động xử lý các lần xáo trộn không thành công lặp đi lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số nguyên được đọc và in. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung được sử dụng. | 

Giải pháp dễ dàng phù hợp với`N ≤ 10^6`hạn chế vì nó không phụ thuộc vào`N`về thời gian chạy hoặc mức sử dụng bộ nhớ của nó. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    n = int(input())
    return str(n % MOD)

# provided samples
assert solution("1\n") == "1", "sample 1"
assert solution("2\n") == "2", "sample 2"

# custom cases
assert solution("3\n") == "3", "small recurrence check"
assert solution("1000000\n") == "1000000", "maximum boundary"
assert solution("998244353\n") == "0", "modulo boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Trường hợp kích thước tối thiểu và hành vi di chuyển đầu tiên | 
|`3`|`3`| Công thức chung về một trường hợp nhỏ không tầm thường | 
|`1000000`|`1000000`| Kích thước đầu vào tối đa được phép | 
|`998244353`|`0`| Xử lý đầu ra mô-đun đúng | 

## Vỏ cạnh 

cho`N = 1`, đầu ra của thuật toán`1`. Không có quy trình nào còn lại sau lần di chuyển đầu tiên, phù hợp với thực tế là khối duy nhất có chính xác một vị trí có thể. 

Vì`N = 2`, đầu ra của thuật toán`2`. Việc xáo trộn có thể kết thúc ngay lập tức hoặc có thể quay trở lại cách sắp xếp không chính xác, nhưng số bước đi dự kiến ​​từ lần lặp lại vẫn là hai. 

Đối với rất lớn`N`, chẳng hạn như đầu vào tối đa`1000000`, thuật toán không phân bổ bộ nhớ tỷ lệ thuận với`N`và chỉ trả về giá trị. Điều này tránh được các vấn đề về hiệu suất của các phương pháp cố gắng mô hình hóa mọi cách sắp xếp có thể hoặc mọi số lượng khối cố định có thể có.
