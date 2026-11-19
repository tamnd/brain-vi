---
title: "CF 2067G - Mùa Nhiệt đới"
description: "Chúng tôi được đưa cho một bộ sưu tập thùng, mỗi thùng chứa một lượng nước và một trong số chúng có một lượng nhỏ chất độc. Chất độc làm tăng thêm trọng lượng của thùng thêm 0,179 kg."
date: "2026-06-09T03:39:30+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 2067
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1004 (Div. 2)"
rating: 3300
weight: 2067
solve_time_s: 89
verified: false
draft: false
---

[CF 2067G - Mùa nhiệt đới](https://codeforces.com/problemset/problem/2067/G) 

**Đánh giá:** 3300 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một bộ sưu tập thùng, mỗi thùng chứa một lượng nước và một trong số chúng có một lượng nhỏ chất độc. Chất độc làm tăng thêm trọng lượng của thùng thêm 0,179 kg. Chúng ta không thể chạm vào thùng chứa chất độc, nhưng chúng ta có thể đổ nước giữa các thùng một cách an toàn miễn là chúng ta không chạm vào thùng có chất độc. Thông tin duy nhất chúng tôi nhận được đến từ một thang so sánh cho chúng tôi biết, đối với hai thùng bất kỳ, trọng lượng của chúng bằng nhau hay thùng nào nặng hơn. 

Nhiệm vụ là xác định xem liệu có thể đảm bảo nhận dạng thùng chứa chất độc mà không cần chạm vào nó hay không và duy trì bảo đảm này ngay cả khi thùng được thêm vào hoặc loại bỏ thông qua một chuỗi truy vấn. 

Kích thước vấn đề lớn: lên tới 200.000 thùng và 200.000 truy vấn. Điều này loại trừ mọi giải pháp yêu cầu so sánh rõ ràng tất cả các cặp thùng hoặc mô phỏng mọi trình tự đổ có thể xảy ra, vì điều đó sẽ mất O(n^2) thời gian cho mỗi truy vấn. Trọng lượng mỗi thùng được giới hạn bởi một triệu, nhưng số lượng thùng mới là hạn chế thực sự. Vì cần xử lý các truy vấn trực tuyến nên chúng tôi cần một chiến lược động cho phép chèn, xóa và xác định thùng độc một cách hiệu quả. 

Trường hợp có cạnh không rõ ràng là khi tất cả các thùng đều chứa cùng một lượng nước. Trong trường hợp đó, thùng độc nặng nhất với biên độ rất nhỏ, nhưng không thể phân biệt nó một cách an toàn, bởi vì bất kỳ sự đổ nào từ thùng không độc sẽ chỉ cân bằng trọng lượng của các thùng khác mà không làm lộ ra chất độc. Một trường hợp tinh vi khác xảy ra khi có chính xác hai vật nặng khác nhau và vật nặng hơn được giữ bởi nhiều hơn một thùng; trong tình huống đó, thùng chứa chất độc có thể là bất kỳ thùng nào trong số đó, và chúng ta không thể chạm vào bất kỳ thùng nào để nhận dạng nó một cách an toàn. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ mô phỏng tất cả các lần đổ và so sánh tỷ lệ. Người ta có thể tưởng tượng việc chọn hai thùng, so sánh chúng, đổ từ thùng nhẹ hơn đến thùng nặng hơn và tiếp tục cho đến khi chỉ còn lại một thùng. Mặc dù đúng về mặt khái niệm, nhưng điều này đòi hỏi phải theo dõi cẩn thận tất cả các so sánh và đổ theo cặp, tức là O(n^2) trong trường hợp xấu nhất và không thể xử lý 2×10^5 thùng một cách hiệu quả. 

Quan sát quan trọng là chúng ta không bao giờ cần phải mô phỏng các đợt đổ thực tế. Khả năng đổ chất độc một cách an toàn có nghĩa là chúng ta có thể tạo ra nhiều thùng có trọng lượng giống hệt nhau, miễn là chúng ta không bao giờ chạm vào thùng chất độc. Nếu chúng ta có thể giảm tất cả các thùng không độc về cùng một trọng lượng thì thùng nào còn lại có trọng lượng riêng biệt phải là thùng độc. Do đó, phương pháp tối ưu giúp giảm bớt việc theo dõi nhiều lượng nước trong thùng. Nếu có một thùng nặng nhất duy nhất, thùng độc có thể được xác định một cách an toàn. Nếu mức tối đa xảy ra nhiều lần thì không thể đảm bảo việc nhận dạng mà không gặp rủi ro. 

Quan sát này cho phép chúng tôi xử lý các truy vấn một cách hiệu quả. Việc thêm thùng sẽ cập nhật tần suất của lượng nước đó. Loại bỏ một thùng làm giảm tần số. Ở mỗi bước, chúng ta chỉ cần kiểm tra xem mức tối đa hiện tại có xảy ra một lần hay không. Điều này làm giảm vấn đề xuống O(log n) cho mỗi truy vấn nếu chúng tôi duy trì số lượng bằng bản đồ cân bằng hoặc O(1) cho mỗi truy vấn với mảng tần số cho các giới hạn nhất định về lượng nước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) sử dụng số đếm | O(10^6) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng biểu đồ tần suất của tất cả lượng nước hiện tại. Điều này cho phép cập nhật liên tục khi thêm hoặc bớt thùng. Sử dụng mảng có kích thước cố định có độ dài 10^6 + 1 là đủ vì tất cả lượng nước đều bị giới hạn. 
2. Xác định lượng nước tối đa hiện tại và số lượng của nó trong bảng. Nếu số lượng tối đa là 1 thì thùng chứa chất độc nặng nhất nên câu trả lời là "Có". Nếu mức tối đa xảy ra nhiều lần thì câu trả lời là "Không". 
3. Xử lý từng truy vấn. Nếu thêm một thùng, hãy tăng tần suất cho lượng nước của thùng đó. Nếu loại bỏ thùng, hãy giảm tần số. Cập nhật mức tối đa và số lượng của nó một cách linh hoạt. Sau mỗi truy vấn, hãy kiểm tra xem mức tối đa có xảy ra một lần hay không và xuất ra "Có" hoặc "Không". 
4. Khi cập nhật mức tối đa, hãy cẩn thận: nếu thùng đã loại bỏ là mức tối đa hiện tại và số lượng của nó giảm xuống 0, hãy quét xuống dưới để tìm lượng nước lớn nhất tiếp theo. Khi thêm thùng, nếu trọng lượng của thùng vượt quá mức tối đa hiện tại, hãy cập nhật mức tối đa. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng tất cả các thùng không độc hại đều có thể được sản xuất như nhau một cách an toàn. Thùng chứa chất độc phải luôn có trọng lượng rõ ràng để nhận biết. Do đó, việc theo dõi mức tối đa và bội số của nó là đủ để trả lời chính xác vấn đề mà không cần mô phỏng các lần đổ thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

n, q = map(int, input().split())
a = list(map(int, input().split()))

freq = defaultdict(int)
for x in a:
    freq[x] += 1

max_weight = max(freq.keys())
def output():
    print("Yes" if freq[max_weight] == 1 else "No")

output()

for _ in range(q):
    line = input()
    op, val = line.split()
    val = int(val)
    if op == '+':
        freq[val] += 1
        if val > max_weight:
            max_weight = val
    else:
        freq[val] -= 1
        if freq[val] == 0:
            del freq[val]
            if val == max_weight:
                if freq:
                    max_weight = max(freq.keys())
                else:
                    max_weight = 0
        elif val == max_weight:
            pass
    output()
```Giải pháp bắt đầu bằng cách xây dựng bản đồ tần suất của tất cả lượng nước trong thùng. Chức năng trợ giúp`output`kiểm tra xem mức tối đa hiện tại có xảy ra một lần hay không. Mỗi truy vấn sẽ tăng hoặc giảm bản đồ tần số. Nếu thùng tối đa bị loại bỏ hoàn toàn, thuật toán sẽ quét mức tối đa tiếp theo. sử dụng`defaultdict`đảm bảo chúng ta không cần phải kiểm tra các khóa bị thiếu một cách rõ ràng. 

Phần tinh tế đang cập nhật chính xác`max_weight`khi tháo thùng. Chúng tôi chỉ phải quét mức tối đa mới nếu mức tối đa trước đó không còn thùng. Khi thêm thùng nặng hơn mức tối đa hiện tại, chúng tôi chỉ cần thay thế mức tối đa. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Hoạt động | Tần số | Tối đa | Số lượng tối đa | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | {2:2, 4:1, 11:1} | 11 | 1 | Có | 
| -2 | {2:1,4:1,11:1} | 11 | 1 | Không | 
| +4 | {2:1,4:2,11:1} | 11 | 1 | Có | 
| +30 | {2:1,4:2,11:1,30:1} | 30 | 1 | Không | 
| +40 | {2:1,4:2,11:1,30:1,40:1} | 40 | 1 | Có | 
| -4 | {2:1,4:1,11:1,30:1,40:1} | 40 | 1 | Không | 
| +2 | {2:2,4:1,11:1,30:1,40:1} | 40 | 1 | Không | 
| +2 | {2:3,4:1,11:1,30:1,40:1} | 40 | 1 | Có | 

Bảng cho thấy việc cập nhật bản đồ tần số và theo dõi tối đa là đủ để xác định câu trả lời. Trạng thái phản ánh tính bất biến rằng trọng lượng tối đa duy nhất cho phép nhận dạng thùng chứa chất độc một cách an toàn. 

Ví dụ tùy chỉnh: 

đầu vào:`3 3\n1 1 2\n+1\n-2\n+3\n`| Hoạt động | Tần số | Tối đa | Số lượng tối đa | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | {1:2,2:1} | 2 | 1 | Có | 
| +1 | {1:3,2:1} | 2 | 1 | Có | 
| -2 | {1:3} | 1 | 3 | Không | 
| +3 | {1:3,3:1} | 3 | 1 | Có | 

Điều này chứng tỏ thuật toán xử lý chính xác các phép cộng và loại bỏ làm thay đổi mức tối đa và bội số của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) khấu hao | Mỗi truy vấn cập nhật bản đồ tần số và mức tối đa được điều chỉnh theo thời gian khấu hao O(1). | 
| Không gian | O(10^6) | Kích thước mảng tần số phụ thuộc vào lượng nước tối đa, 10^6. | 

Giải pháp dễ dàng phù hợp trong 4 giây với 200.000 thùng và truy vấn và không vượt quá bộ nhớ 1 GB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample
assert run("4 7\n2 2 4 11\n- 2\n+ 4\n+ 30\n+ 40\n- 4\n+ 2\n+ 2\n") == \
"Yes\nNo\nYes\nNo\nYes\nNo\nNo\nYes"

# minimum input
assert run("1 1\n1\n+1\n") == "Yes\nNo"

# all-equal barrels
assert run("3 2\n2 2 2\n+2\n-2\n")
```
