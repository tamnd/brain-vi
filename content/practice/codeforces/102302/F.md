---
title: "CF 102302F - Thẻ vẽ"
description: "Ban đầu có N thẻ trong hộp, một thẻ cho mỗi nhãn từ 1 đến N. Bất cứ khi nào nhãn được rút ra lần đầu tiên, thẻ đó sẽ được đặt trên bàn và một thẻ mới sẽ được thêm vào hộp. Nhãn của thẻ mới được chọn thống nhất từ ​​tất cả N nhãn có thể."
date: "2026-08-13T07:40:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "F"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 132
verified: true
draft: false
---

[CF 102302F - Thẻ vẽ](https://codeforces.com/problemset/problem/102302/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ban đầu có N thẻ trong hộp, một thẻ cho mỗi nhãn từ 1 đến N. Bất cứ khi nào nhãn được rút ra lần đầu tiên, thẻ đó sẽ được đặt trên bàn và một thẻ mới sẽ được thêm vào hộp. Nhãn của thẻ mới được chọn thống nhất từ ​​tất cả N nhãn có thể. Việc rút thẻ lặp đi lặp lại của nhãn đã xuất hiện không làm thay đổi bộ sưu tập thẻ trong hộp. 

Chúng ta cần số lượng nhãn riêng biệt dự kiến ​​xuất hiện vào thời điểm nhãn 1 xuất hiện lần đầu tiên. Vì thẻ có nhãn 1 được đặt trên bàn vào thời điểm đó nên câu trả lời ít nhất là 1. 

Ràng buộc N <= 10^6 đủ lớn để mô phỏng quá trình ngẫu nhiên không phải là giải pháp phù hợp. Ngay cả một giải pháp thực hiện các thao tác O(N^2) cũng sẽ vượt xa giới hạn một giây. Chúng ta cần khai thác một tính chất của quá trình ngẫu nhiên cho phép chúng ta thu được kỳ vọng một cách trực tiếp, lý tưởng nhất là trong thời gian không đổi. 

Những trường hợp tế nhị lại đơn giản đến mức đáng ngạc nhiên. Với N = 1, thẻ duy nhất đã được gắn nhãn 1, vì vậy câu trả lời chính xác là 1. Một công thức bất cẩn liên quan đến cách chia N - 1 hoặc giả định rằng có một nhãn khác tồn tại có thể xử lý sai trường hợp này. Với N = 2, hai nhãn hoàn toàn đối xứng, do đó một trong hai nhãn sẽ xuất hiện đầu tiên. Do đó, số nhãn dự kiến ​​trên bàn là 1 khi 1 đứng đầu và 2 khi 2 đứng đầu, cho kết quả là 1,5. Bất kỳ trực giác dựa trên mô phỏng nào giả định nhãn 1 được ưu tiên bằng cách nào đó vì đó là nhãn dừng sẽ không chính xác. 

Ví dụ, đầu vào`1`có câu trả lời`1.0000000000`. đầu vào`2`có câu trả lời`1.5000000000`. Câu trả lời cho`3`là`2.0000000000`, vì nhãn 2 và 3 đều có xác suất một nửa xuất hiện trước nhãn 1. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng toàn bộ thí nghiệm ngẫu nhiên. Chúng ta có thể lưu trữ bội số của mỗi nhãn trong hộp, liên tục chọn một thẻ ngẫu nhiên, đánh dấu một nhãn như đã thấy khi nó xuất hiện lần đầu tiên và dừng lại khi nhãn 1 được rút ra. Điều này sẽ mô hình hóa chính xác quy trình, nhưng nó không tính toán kỳ vọng chính xác. Mô phỏng Monte Carlo chỉ ước tính câu trả lời và số lần rút trước khi nhãn 1 xuất hiện là ngẫu nhiên và không có giới hạn trường hợp xấu nhất cố định. Đặc biệt, luôn có xác suất dương để tránh nhãn 1 cho nhiều lần rút thăm tùy ý, do đó không có số phép toán hữu hạn trong trường hợp xấu nhất làm cho mô phỏng trở thành một thuật toán chính xác hợp lệ. 

Quan sát quan trọng là chúng ta không cần phải hiểu sự tiến hóa phức tạp của chiếc hộp. Hãy xem xét bất kỳ nhãn nào i khác với 1. Chúng tôi chỉ quan tâm liệu tôi có xuất hiện trước 1 hay không. Toàn bộ quá trình ngẫu nhiên xử lý các nhãn một cách đối xứng. Hoán đổi tên 1 và i không thay đổi bất kỳ quy tắc hoặc xác suất nào: mỗi nhãn ban đầu xuất hiện một lần và mọi thẻ mới được tạo đều chọn nhãn của nó một cách thống nhất từ 1 đến N. 

Do đó, nhãn 1 và i có sự phân bố giống hệt nhau đối với nhãn nào xuất hiện đầu tiên. Một trong số chúng phải xuất hiện đầu tiên, vì vậy mỗi cái có xác suất chính xác bằng 1/2 lần xuất hiện đầu tiên. Như vậy xác suất để tôi xuất hiện trước số 1 cũng đúng bằng 1/2. 

Bây giờ hãy giới thiệu chỉ báo X_i cho mọi i từ 2 đến N. Đặt X_i là 1 nếu nhãn i xuất hiện trước lần xuất hiện đầu tiên của nhãn 1 và 0 nếu ngược lại. Khi nhãn 1 cuối cùng xuất hiện, số thẻ trên bàn là 

1 + X_2 + X_3 + ... + X_N. 

Lấy kỳ vọng và sử dụng tính tuyến tính của kỳ vọng mang lại 

E = 1 + tổng E[X_i]. 

Mỗi X_i có kỳ vọng là 1/2, vì vậy 

E = 1 + (N - 1) / 2 = (N + 1) / 2. 

Phần quan trọng là X_i không cần phải độc lập. Tính tuyến tính của kỳ vọng hoạt động bất kể sự phụ thuộc của chúng. Điều này hoàn toàn loại bỏ nhu cầu lập mô hình số lượng thẻ trùng lặp được tạo ra hoặc quá trình chạy trong bao lâu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Thời gian chạy ngẫu nhiên không giới hạn | O(N) | Quá chậm và chỉ ước tính | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc N, số lượng nhãn có thể. 
2. Xét một nhãn tùy ý i với 2 <= i <= N. Quá trình này đối xứng khi trao đổi nhãn 1 và i, vì ban đầu cả hai đều có một thẻ và mọi nhãn mới tạo đều được chọn giống nhau. 
3. Vì 1 hoặc i phải là nhãn đầu tiên xuất hiện trong hai nhãn này nên tính đối xứng cho P(i xuất hiện trước 1) = 1/2. 
4. Xác định một chỉ báo cho mỗi nhãn i từ 2 đến N. Giá trị của nó là 1 chính xác khi nhãn đó đã có trên bàn khi nhãn 1 được rút ra lần đầu tiên. Do đó giá trị kỳ vọng của nó là 1/2. 
5. Số lá bài cuối cùng trên bàn bằng 1 cộng với tổng các chỉ số này. Số 1 bổ sung tương ứng với chính nhãn 1. 
6. Áp dụng tính tuyến tính của kỳ vọng. Kích thước bàn dự kiến là 1 + (N - 1) / 2, đơn giản hóa thành (N + 1) / 2. 
7. In kết quả dưới dạng số dấu phẩy động. Không cần mô phỏng, mảng, số ngẫu nhiên hoặc quá trình lặp lại. 

### Tại sao nó hoạt động 

Bất biến đằng sau đối số là tính đối xứng nhãn. Ở mọi giai đoạn, việc thay thế mọi lần xuất hiện nhãn 1 bằng nhãn i và mọi lần xuất hiện nhãn i bằng nhãn 1 sẽ tạo ra một quy trình có phân bố xác suất giống hệt nhau. Vì vậy, không nhãn nào có thể có lợi thế khi là nhãn hiệu đầu tiên gặp phải. Với mọi i != 1, sự kiện i xuất hiện trước 1 có xác suất là 1/2. 

Số lượng bàn cuối cùng có thể được biểu thị dưới dạng tổng của các đóng góp chỉ số có vẻ độc lập, nhưng không cần phải có tính độc lập. Tính tuyến tính của kỳ vọng cho biết tổng kỳ vọng là tổng kỳ vọng của từng cá nhân. Mỗi nhãn trong số N - 1 không phải 1 đóng góp 1/2 kỳ vọng, trong khi nhãn 1 đóng góp chính xác là 1. Do đó, công thức`(N + 1) / 2`là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
answer = (n + 1) / 2.0
print(f"{answer:.10f}")
```Đầu vào chỉ chứa một số nguyên, do đó một lệnh gọi tới`input()`là đủ. biểu thức`(n + 1) / 2.0`thực hiện phép chia dấu phẩy động và đánh giá trực tiếp kỳ vọng dẫn xuất. 

Không cần phải xây dựng hộp hoặc theo dõi những nhãn nào đã xuất hiện. Làm như vậy sẽ giải quyết được một vấn đề phức tạp hơn nhiều so với vấn đề thực sự được hỏi. Đối số đối xứng đã tính đến mọi chuỗi tạo thẻ ngẫu nhiên có thể xảy ra. 

sử dụng`.10f`đưa ra mười chữ số sau dấu thập phân, độ chính xác cao hơn mức dung sai yêu cầu. Số nguyên Python có thể biểu thị chính xác N và giá trị trung gian N + 1 rất nhỏ so với phạm vi số nguyên của Python, do đó, tràn không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1: N = 2 

Đối với hai nhãn, nhãn 2 có xác suất một nửa xuất hiện trước nhãn 1. Bảng cuối cùng chứa nhãn 1 trong mọi kết quả và chứa nhãn 2 chính xác khi nhãn 2 xuất hiện đầu tiên. 

| N | Nhãn được xem xét | P(nhãn trước 1) | Đóng góp dự kiến ​​| Tổng dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 1/2 | 1/2 | 1 + 1/2 = 1,5 | 

Câu trả lời là`1.5000000000`, phù hợp với mẫu Dấu vết này thể hiện đối số đối xứng trong trường hợp không tầm thường nhỏ nhất của nó. 

### Ví dụ 2: N = 3 

Nhãn 2 và 3 đều đối xứng với nhãn 1. Mỗi nhãn có một nửa xác suất xuất hiện trước 1, bất kể hành vi trùng lặp thẻ phức tạp có thể xảy ra trước đó. 

| N | Nhãn được xem xét | P(nhãn trước 1) | Đóng góp dự kiến ​​| Tổng số dự kiến ​​đang chạy | 
| --- | --- | --- | --- | --- | 
| 3 | 2 | 1/2 | 1/2 | 1,5 | 
| 3 | 3 | 1/2 | 1/2 | 2.0 | 

Câu trả lời là`2.0000000000`. Điều này chứng tỏ rằng các sự kiện riêng lẻ cho nhãn 2 và 3 không cần phải độc lập. Chúng tôi chỉ thêm mong đợi của họ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một biểu thức số học được đánh giá sau khi đọc N. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu nào phát triển với N. | 

N tối đa là 10^6, nhưng giải pháp thực hiện cùng một lượng công việc không đổi cho mọi kích thước đầu vào. Nó thoải mái trong cả giới hạn thời gian một giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Kết quả dấu phẩy động chính xác phải được so sánh với dung sai trong bộ dây thử nghiệm chắc chắn. Các thử nghiệm sau đây sử dụng logic giải pháp tương tự và xác thực các ranh giới quan trọng cũng như một số giá trị lớn hơn.```python
# helper: run solution on input string, return output string
import sys
import io

def solve(data: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)
    try:
        input = sys.stdin.readline
        n = int(input())
        answer = (n + 1) / 2.0
        return f"{answer:.10f}"
    finally:
        sys.stdin = old_stdin

def run(inp: str) -> str:
    return solve(inp)

# provided sample
assert run("2\n") == "1.5000000000", "sample 1"

# minimum-size input
assert run("1\n") == "1.0000000000", "N = 1"

# first odd value where the answer is an integer
assert run("3\n") == "2.0000000000", "N = 3"

# larger even boundary case
assert run("1000000\n") == "500000.5000000000", "maximum N"

# another odd value, checking the half-integer result
assert run("999999\n") == "500000.0000000000", "large odd N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1.0000000000`| N tối thiểu và trường hợp không có nhãn khác | 
|`2`|`1.5000000000`| Cung cấp mẫu và trường hợp đối xứng không tầm thường đầu tiên | 
|`3`|`2.0000000000`| Kỳ vọng N lẻ và số nguyên nhỏ | 
|`999999`|`500000.0000000000`| Số học N lẻ lớn và nửa số nguyên | 
|`1000000`|`500000.5000000000`| N tối đa và N lớn chẵn | 

## Vỏ cạnh 

Với N = 1, đầu vào là`1`. Không có nhãn nào khác ngoài 1 nên quân bài đầu tiên được rút ra phải có nhãn 1. Lúc đó bàn có đúng một quân bài. Công thức cho`(1 + 1) / 2 = 1`, do đó thuật toán in`1.0000000000`. 

Với N = 2, đầu vào là`2`. Nhãn 1 và 2 bắt đầu bằng mỗi thẻ một thẻ và mỗi thẻ được tạo sẽ chọn giữa chúng với xác suất bằng nhau. Tổng quát hơn, toàn bộ quá trình vẫn không thay đổi nếu tên của họ được trao đổi. Do đó, cả hai nhãn đều có khả năng được gặp đầu tiên như nhau. Nếu 1 đến trước, một lá bài sẽ ở trên bàn. Nếu 2 đến trước, cả hai nhãn cuối cùng đều được biểu thị khi 1 được rút ra, vậy là có hai lá bài trên bàn. Kỳ vọng là`(1 + 2) / 2 = 1.5`. 

Với N = 3, đầu vào là`3`. Nhãn 2 có 1/2 cơ hội xảy ra trước nhãn 1 và nhãn 3 độc lập có xác suất cận biên bằng 1/2 của nhãn 1 trước đó. Chúng ta không cần xác định liệu cả hai có thể xảy ra cùng với một xác suất cụ thể hay không. Đóng góp dự kiến ​​của họ là mỗi người 1/2, mang lại`1 + 1/2 + 1/2 = 2`. 

Để có đầu vào tối đa`1000000`, công thức cho`1000001 / 2 = 500000.5`. Thuật toán đạt được kết quả này với khối lượng công việc tương đương với N = 1. Đây cũng là lý do tại sao việc theo dõi hộp tăng trưởng linh hoạt sẽ tốn chi phí không cần thiết: số lượng thẻ có thể có có thể trở nên lớn, trong khi giá trị mong đợi được xác định hoàn toàn bằng tính đối xứng.
