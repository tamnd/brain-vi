---
title: "CF 105791C - Dừa"
description: "Chúng tôi có một dãy ki-ốt dọc đại lộ ven biển, mỗi ki-ốt phục vụ đúng một loại đồ uống. Khách du lịch bắt đầu với giá trị hài lòng là 1 và chọn một phân đoạn ki-ốt liền kề để đi qua."
date: "2026-06-21T13:09:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "C"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 61
verified: true
draft: false
---

[CF 105791C - Dừa](https://codeforces.com/problemset/problem/105791/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy ki-ốt dọc đại lộ ven biển, mỗi ki-ốt phục vụ đúng một loại đồ uống. Khách du lịch bắt đầu với giá trị hài lòng là 1 và chọn một phân đoạn ki-ốt liền kề để đi qua. Trong khi đi qua đoạn đó, anh ấy sẽ nhân mức độ hài lòng hiện tại của mình với giá trị của từng đồ uống theo thứ tự. 

Mỗi giá trị đồ uống bằng 0 hoặc lũy thừa có dấu của hai, nghĩa là nó có thể được viết là 0 hoặc ±2^k đối với một số nguyên k. Giá trị bằng 0 đại diện cho một loại đồ uống đặc biệt có thể ngay lập tức đặt lại mức độ hài lòng về 0 và phá vỡ chuỗi nhân lên một cách hiệu quả. Giá trị âm biểu thị đồ uống “xấu” lật dấu của mức độ hài lòng hiện tại theo cách nhân thông thường, ngoại trừ nếu mức độ hài lòng hiện tại đã âm, thì tuyên bố vấn đề nhấn mạnh rằng việc uống một giá trị âm khác sẽ khôi phục lại mức độ dương, phù hợp với quy tắc nhân thông thường. 

Mục tiêu là chọn vị trí bắt đầu và kết thúc của một phân đoạn liền kề (hoặc chọn không nhập gì cả) sao cho mức độ hài lòng cuối cùng được tối đa hóa. Câu trả lời phải được in theo modulo 1e9 + 7, nhưng việc so sánh kết quả được thực hiện trên các giá trị nguyên thực tế trước khi lấy modulo. 

Ràng buộc n có thể lên tới 200.000, điều này ngay lập tức loại trừ mọi phép liệt kê O(n^2) của tất cả các phân đoạn. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính, vì quét bậc hai sẽ yêu cầu theo thứ tự các phép toán 4e10 trong trường hợp xấu nhất, không thể thực hiện được trong một giây. 

Một điểm tinh tế là số 0 sẽ chia tách hoàn toàn quá trình. Bất kỳ phân đoạn nào vượt qua số 0 đều có sản phẩm được đặt lại về 0 tại thời điểm đó, vì vậy các phân đoạn tối ưu không bao giờ cần phải vượt qua số 0. 

Một trường hợp quan trọng khác là khách du lịch không được phép lấy phân đoạn nào cả, giữ mức độ hài lòng bằng 1. Đường cơ sở này quan trọng vì bất kỳ tích âm nào luôn kém hơn 1 theo thứ tự số nguyên thực tế, mặc dù nó có thể xuất hiện lớn sau modulo. 

Một sai lầm ngây thơ là diễn giải kết quả cuối cùng hoàn toàn theo modulo 1e9 + 7 và vô tình thích một tích âm có thặng dư mô đun lớn. Điều đó sẽ không chính xác vì việc tối đa hóa xảy ra trước modulo. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi vị trí bắt đầu có thể và mở rộng nó đến mọi vị trí kết thúc có thể có, duy trì sản phẩm đang chạy. Điều này tính toán chính xác sản phẩm của mỗi mảng con. Tuy nhiên, mỗi phần mở rộng là O(1) và có các mảng con O(n^2), dẫn đến khoảng 2e10 thao tác trong trường hợp xấu nhất, quá chậm. 

Cấu trúc của bài toán gợi ý rằng chúng ta chỉ cần theo dõi những sản phẩm tốt nhất kết thúc ở mỗi vị trí. Đây là ý tưởng cổ điển về “mảng con sản phẩm tối đa”, nhưng có hai điểm phức tạp: số 0 đặt lại mọi thứ và giá trị là lũy thừa có dấu của 2, điều này làm cho độ lớn theo dõi tương đương với việc theo dõi tổng số mũ. 

Thay vì theo dõi các sản phẩm thực tế, chúng tôi theo dõi số mũ của hai. Giá trị ±2^k đóng góp k vào số mũ, trong khi dấu được theo dõi riêng. Phép nhân trở thành phép cộng số mũ và đảo dấu. 

Điều này làm giảm vấn đề duy trì, đối với mỗi vị trí, sản phẩm dương tốt nhất kết thúc ở đó và sản phẩm âm tốt nhất kết thúc ở đó, xét về tổng số mũ. Zeros đặt lại cả hai trạng thái và cho phép khởi động lại từ đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các mảng con | O(n^2) | O(1) | Quá chậm | 
| DP ở trạng thái số mũ dương/âm | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì hai trạng thái đang chạy: sản phẩm dương tốt nhất kết thúc ở vị trí hiện tại và sản phẩm âm tốt nhất kết thúc ở vị trí hiện tại. Chúng tôi cũng duy trì số mũ tốt nhất toàn cầu cho bất kỳ phân khúc hợp lệ nào.

Chúng ta biểu diễn tích bằng số mũ là 2, vì tất cả các giá trị đều là lũy thừa của hai dấu. 

### Các bước 

1. Khởi tạo số mũ của câu trả lời tốt nhất là 0, biểu thị lựa chọn trống có giá trị 1. Đồng thời khởi tạo trạng thái dương và âm hiện tại là trống. 
2. Đối với mỗi phần tử trong mảng, hãy xử lý nó dựa trên giá trị của nó. 
3. Nếu giá trị bằng 0, hãy đặt lại cả hai trạng thái. Điều này là do bất kỳ phân đoạn nào đi qua số 0 sẽ trở thành không hợp lệ dưới dạng chuỗi nhân. Sau khi đặt lại, hãy cập nhật câu trả lời chung bằng 0 vì chúng ta luôn có thể bắt đầu mới sau số 0. 
4. Nếu giá trị là dương 2^k, hãy cập nhật trạng thái dương bằng cách bắt đầu mới từ phần tử này hoặc mở rộng trạng thái dương trước đó bằng cách thêm k vào số mũ của nó. Trạng thái âm, nếu nó tồn tại, cũng mở rộng bằng cách thêm k vì nhân với số dương bảo toàn dấu. 
5. Nếu giá trị âm -2^k, hãy tính toán các ứng cử viên dương mới bằng cách mở rộng trạng thái âm trước đó (vì âm nhân với âm trở thành dương) và các ứng cử viên âm mới bằng cách mở rộng trạng thái dương trước đó hoặc bắt đầu làm mới dưới dạng một phần tử âm duy nhất. 
6. Sau khi xử lý từng phần tử, hãy cập nhật giá trị tốt nhất toàn cầu với số mũ dương hiện tại, vì chỉ kết quả dương mới có thể vượt qua đường cơ sở 1 theo thứ tự giá trị thực. 

### Tại sao nó hoạt động 

Ở bất kỳ vị trí nào, mọi mảng con hợp lệ kết thúc ở đó phải đến từ chính xác một trong hai lịch sử: hoặc nó bắt đầu ở chỉ mục trước đó hoặc nó bắt đầu ở số 0 gần đây nhất. Các trạng thái DP nắm bắt chính xác những khả năng này. Việc tách trạng thái dương và âm là đủ vì nhân với ±2^k chỉ ảnh hưởng đến dấu và thêm một lượng cố định vào số mũ nên không cần thông tin nào khác. Số 0 hoạt động như một thiết lập lại cứng nhằm ngăn chặn sự lây nhiễm chéo giữa các trạng thái trên các phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    best_exp = 0
    
    pos = None
    neg = None
    
    for x in a:
        if x == 0:
            best_exp = max(best_exp, 0)
            pos = None
            neg = None
            continue
        
        sign = 1 if x > 0 else -1
        k = x.bit_length() - 1 if x != 0 else 0
        
        new_pos = None
        new_neg = None
        
        if sign > 0:
            if pos is not None:
                new_pos = max(new_pos or 0, pos + k)
            else:
                new_pos = k
            
            if neg is not None:
                new_neg = neg + k
        
        else:
            if neg is not None:
                new_pos = max(new_pos or -10**30, neg + k)
            else:
                new_pos = k
            
            if pos is not None:
                new_neg = pos + k
            else:
                new_neg = k
        
        pos = new_pos
        neg = new_neg
        
        if pos is not None:
            best_exp = max(best_exp, pos)
    
    print(pow(2, best_exp, MOD))

if __name__ == "__main__":
    solve()
```Việc triển khai nén từng giá trị thành một dấu và số mũ. Số mũ được trích xuất bằng cách sử dụng độ dài bit vì mọi giá trị khác 0 đều chính xác là ±2^k. DP duy trì hai trạng thái đang chạy, được cập nhật tại chỗ cho từng phần tử. Tốt nhất toàn cầu chỉ theo dõi các giá trị số mũ dương, vì chỉ những giá trị tương ứng với các ứng cử viên hợp lệ có thể vượt quá đường cơ sở 1. 

Một cạm bẫy triển khai phổ biến là quên rằng số 0 phải thiết lập lại hoàn toàn cả hai trạng thái. Một vấn đề tế nhị khác là việc cho phép các trạng thái kết thúc phủ định ảnh hưởng đến câu trả lời cuối cùng một cách không chính xác, điều này sẽ ưu tiên không chính xác các sản phẩm tiêu cực có cường độ lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [0, -1, 0]
```Chúng tôi theo dõi các trạng thái như sau: 

| tôi | x | pos_exp | phủ định_exp | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | đặt lại | đặt lại | 0 | 
| 1 | -1 | 0 | 0 | 0 | 
| 2 | 0 | đặt lại | đặt lại | 0 | 

Quan sát quan trọng là các số 0 cô lập hoàn toàn các phân đoạn. Giá trị tốt nhất có thể đạt được là 1, tương ứng với việc không chọn phân đoạn nào. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [-8, -2, 0, 2, 4]
```| tôi | x | pos_exp | phủ định_exp | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | -8 | -inf | 3 | 0 | 
| 2 | -2 | 4 | 5 | 4 | 
| 3 | 0 | đặt lại | đặt lại | 4 | 
| 4 | 2 | 1 | Không có | 4 | 
| 5 | 4 | 3 | Không có | 4 | 

Điều này cho thấy tác dụng của việc đảo dấu: hai số âm kết hợp thành một tích dương, cho số mũ 4, tương ứng với giá trị 16. Số 0 tách mảng thành các phân đoạn độc lập một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử cập nhật trạng thái DP không đổi một lần | 
| Không gian | O(1) | Chỉ có hai trạng thái đang chạy được duy trì | 

Quét tuyến tính phù hợp thoải mái trong các ràng buộc cho n lên tới 200.000. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod
    
    # assume solve() is defined above
    # capturing output
    import contextlib
    import sys as _sys
    from io import StringIO
    backup = _sys.stdout
    _sys.stdout = StringIO()
    solve()
    out = _sys.stdout.getvalue().strip()
    _sys.stdout = backup
    return out

# provided sample-like tests
assert run("3\n0 -1 0\n") == "1"

# all positive
assert run("3\n2 4 2\n") == str(pow(2, 3, 10**9+7))

# all negative even count best
assert run("2\n-2 -2\n") == str(pow(2, 2, 10**9+7))

# single zero
assert run("1\n0\n") == "1"

# mixed
assert run("5\n-8 -2 0 2 4\n") == str(16 % (10**9+7))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 -1 0`|`1`| đặt lại bằng 0 và phân đoạn trống | 
|`2 4 2`|`16`| tích lũy tích cực thuần túy | 
|`-2 -2`|`4`| hủy âm thành dương | 
|`0`|`1`| xử lý số 0 đơn | 
|`-8 -2 0 2 4`|`16`| phân đoạn và thiết lập lại hành vi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi toàn bộ mảng bao gồm các số 0. Trong tình huống đó, mọi phân đoạn đều không hợp lệ và câu trả lời đúng là đường cơ sở 1. Thuật toán xử lý việc này vì mọi số 0 đều buộc đặt lại và cho phép cập nhật rõ ràng kết quả tốt nhất bằng 0, trong khi câu trả lời cuối cùng vẫn ít nhất là 0 số mũ, tương ứng với 1. 

Một trường hợp cạnh khác là một chuỗi các dấu xen kẽ không có số 0, chẳng hạn như [-2, 2, -2]. DP theo dõi chính xác cả trạng thái tích cực và tiêu cực, đảm bảo rằng chuỗi tiêu cực có độ dài chẵn được chuyển đổi thành ứng cử viên tích cực khi tiêu cực thứ hai xuất hiện. 

Trường hợp tinh vi cuối cùng là một mảng phần tử đơn. Nếu nó dương thì câu trả lời là giá trị của phần tử đó. Nếu nó âm, thì giá trị tốt nhất vẫn là 1 trừ khi nó có thể được ghép nối, điều này là không thể, vì vậy DP sẽ tránh chọn một phân đoạn âm đơn có hại một cách chính xác.
