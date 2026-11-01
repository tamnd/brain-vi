---
title: "CF 106443B - Balatro"
description: "Chúng ta được cho hai dãy số nguyên có độ dài bằng nhau. Một chuỗi thuộc về Drex, chuỗi còn lại thuộc về ông chủ. Trước khi bất kỳ tương tác nào xảy ra, Drex được phép tự do hoán đổi trình tự của mình."
date: "2026-06-20T03:59:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "B"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 54
verified: true
draft: false
---

[CF 106443B - Balatro](https://codeforces.com/problemset/problem/106443/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai dãy số nguyên có độ dài bằng nhau. Một chuỗi thuộc về Drex, chuỗi còn lại thuộc về ông chủ. Trước khi bất kỳ tương tác nào xảy ra, Drex được phép tự do hoán đổi trình tự của mình. Sau khi sự sắp xếp đó được cố định, hai chuỗi được căn chỉnh theo chỉ số và tổng thiệt hại được tính bằng tổng chênh lệch tuyệt đối giữa các phần tử được ghép nối. 

Nhiệm vụ là chọn thứ tự của dãy Drex sao cho tổng này càng nhỏ càng tốt. 

Chi tiết cấu trúc quan trọng là chỉ mảng của Drex mới có thể sắp xếp lại được. Mảng của ông chủ là cố định nên vấn đề cơ bản là ghép nối các phần tử của hai multiset một cách tối ưu. 

Ràng buộc n lên tới 2 × 10^5 ngay lập tức loại trừ mọi chiến lược ghép đôi bậc hai. Bất kỳ giải pháp nào thử tất cả các kết quả khớp hoặc thậm chí thử tham lam các hoán đổi cục bộ mà không có cấu trúc sẽ thất bại trong trường hợp xấu nhất vì số lượng hoán vị có thể tăng theo giai thừa. 

Chế độ lỗi phổ biến xuất hiện khi người ta cho rằng việc ghép các chỉ số bằng nhau hoặc thực hiện quét tham lam ngây thơ là đủ. Ví dụ: nếu Drex có`[10, 1, 100]`và ông chủ có`[2, 50, 60]`, việc ghép nối theo thứ tự ban đầu sẽ gây ra chi phí lớn và các giao dịch hoán đổi cục bộ không có thông tin chi tiết về sắp xếp vẫn có thể bỏ lỡ việc ghép nối tối ưu toàn cầu. 

Một trường hợp khó phát hiện khác là khi các giá trị bị sai lệch nhiều: một kẻ tham lam ngây thơ luôn khớp với các giá trị còn lại gần nhất mà không có thứ tự chung có thể bị mắc kẹt trong các lựa chọn ban đầu dưới mức tối ưu. Ví dụ: Drex`[1, 100, 101]`và ông chủ`[2, 3, 200]`yêu cầu một thứ tự nhất quán trên toàn cầu thay vì các quyết định ghép nối tối ưu cục bộ. 

Những vấn đề này cho thấy rằng vấn đề không nằm ở việc so khớp cục bộ mà là về sự liên kết toàn cục của cấu trúc được sắp xếp. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi hoán vị của mảng Drex, tính tổng kết quả của sự khác biệt tuyệt đối với mảng trùm và lấy mức tối thiểu. Điều này đúng vì nó đánh giá rõ ràng tất cả các cặp có thể xảy ra do hoán vị. Tuy nhiên, số lượng hoán vị là n!, và mỗi đánh giá tốn O(n), đưa ra các phép toán O(n · n!), điều này trở nên không khả thi ngay cả với n khoảng 10. 

Để cải thiện điều này, chúng tôi nhận thấy rằng mảng trùm đã được cố định và chúng tôi chỉ kiểm soát cách gán các giá trị của Drex cho các vị trí. Chi phí giá trị tuyệt đối gợi ý một cách giải thích hình học: chúng tôi đang cố gắng khớp hai tập hợp điểm trên một trục số với chi phí khớp L1 tối thiểu theo một chiều. 

Trong một chiều, chiến lược tối ưu để giảm thiểu tổng chênh lệch tuyệt đối là ghép các phần tử đã được sắp xếp với các phần tử đã được sắp xếp. Điều này có hiệu quả vì bất kỳ sự đảo ngược nào trong việc ghép cặp đều có thể được chứng minh là có thể cải thiện được: nếu hai cặp được ghép theo thứ tự đã sắp xếp, việc hoán đổi chúng không bao giờ làm tăng chi phí mà thường làm giảm chi phí. Đây là đối số trao đổi cổ điển để kết hợp tối ưu trên một dòng. 

Vì vậy, thay vì thử hoán vị, chúng ta sắp xếp cả hai mảng và khớp phần tử i với phần tử i. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + ghép nối) | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là việc ghép đôi sẽ trở nên tối ưu khi cả hai chuỗi được sắp xếp một cách nhất quán. 

### Các bước 

1. Đọc n, kích thước của cả hai mảng. 
2. Đọc mảng Drex và mảng trùm. 
3. Sắp xếp cả hai mảng theo thứ tự không giảm. 

Việc sắp xếp thực thi một cấu trúc toàn cục sao cho các giá trị nhỏ phù hợp với các giá trị nhỏ và giá trị lớn phù hợp với giá trị lớn. 
4. Khởi tạo bộ tích lũy cho tổng thiệt hại bằng 0. 
5. Lặp lại i từ 0 đến n − 1. 
6. Đối với mỗi chỉ số i, hãy cộng chênh lệch tuyệt đối giữa giá trị Drex nhỏ nhất thứ i và giá trị boss nhỏ nhất thứ i. 
7. Xuất số tiền tích lũy. 

Quyết định quan trọng là bước 3. Nếu không sắp xếp cả hai mảng, mọi cặp tham lam sẽ hoạt động trong một không gian mà các quyết định cục bộ không phản ánh tính tối ưu toàn cục. Việc sắp xếp biến bài toán thành một bài toán so khớp đơn điệu. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, cả hai chuỗi đều được sắp xếp trên cùng một trục. Nếu tồn tại một cặp chỉ số i < j sao cho Drex[i] được ghép với boss[j] và Drex[j] được ghép với boss[i], thì việc hoán đổi các cặp này không thể làm tăng tổng chi phí. Điều này là do khoảng cách có giá trị tuyệt đối trên một đường thỏa mãn tính chất là các cạnh giao nhau trong một kết quả khớp luôn có thể được bỏ qua mà không làm xấu đi tổng chiều dài. Việc áp dụng đối số này nhiều lần sẽ loại bỏ tất cả các nghịch đảo giữa các cặp, buộc việc ghép cặp nhận dạng trên các mảng được sắp xếp là một giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    d = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    d.sort()
    b.sort()
    
    ans = 0
    for i in range(n):
        ans += abs(d[i] - b[i])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện là trực tiếp khi nguyên tắc sắp xếp được hiểu. Sự tinh tế chính là đảm bảo cả hai mảng được sắp xếp độc lập trước khi ghép nối. Một lỗi phổ biến là chỉ sắp xếp mảng Drex hoặc cố gắng căn chỉnh một phần mà không sắp xếp đầy đủ, điều này phá vỡ cấu trúc đơn điệu cần thiết để đảm bảo tính chính xác. 

Việc tích lũy sử dụng một vòng lặp tiêu chuẩn vì n có thể lớn, nhưng số học số nguyên của Python xử lý thoải mái kích thước tổng tiềm năng vì các giá trị có thể đạt tới độ lớn 2 × 10^14. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
d = [3, 1, 2]
b = [3, 1, 2]
```Mảng được sắp xếp: 

| tôi | d[i] | b[i] | |d[i] - b[i]| | 

|---|------|------|--------------| 

| 0 | 1 | 1 | 0 | 

| 1 | 2 | 2 | 0 | 

| 2 | 3 | 3 | 0 | 

Tổng cộng = 0 

Điều này thể hiện trường hợp lý tưởng khi cả hai tập hợp đều giống hệt nhau. Việc sắp xếp sẽ sắp xếp các giá trị giống hệt nhau một cách hoàn hảo, tạo ra chi phí bằng 0. 

### Ví dụ 2 

đầu vào:```
n = 4
d = [10, 1, 100, 50]
b = [2, 3, 60, 80]
```Mảng được sắp xếp: 

d = [1, 10, 50, 100] 

b = [2, 3, 60, 80] 

| tôi | d[i] | b[i] | khác biệt | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 1 | 
| 1 | 10 | 3 | 7 | 
| 2 | 50 | 60 | 10 | 
| 3 | 100 | 80 | 20 | 

Tổng cộng = 38 

Điều này cho thấy cách sắp xếp toàn cầu thực thi việc ghép nối ổn định giữa các giá trị có tỷ lệ tương tự, tránh sự không khớp như ghép 100 với 2 hoặc 1 với 80. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | bị chi phối bởi việc sắp xếp cả hai mảng | 
| Không gian | O(1) hoặc O(n) | tùy thuộc vào việc sắp xếp việc thực hiện và lưu trữ mảng | 

Các ràng buộc cho phép tối đa 2 × 10^5 phần tử và việc sắp xếp theo tỷ lệ này là tiêu chuẩn. Việc quét tuyến tính sau đó là không đáng kể, đảm bảo giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("3\n3 1 2\n3 1 2\n") == "0"

# all equal values
assert run("4\n5 5 5 5\n5 5 5 5\n") == "0"

# reverse order stress
assert run("5\n1 2 3 4 5\n10 9 8 7 6\n") == "25"

# minimum size
assert run("1\n-5\n10\n") == "15"

# mixed values
assert run("3\n-1 4 2\n3 0 5\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | nhiều bộ giống hệt nhau không tốn chi phí | 
| thứ tự ngược lại | 25 | sắp xếp ngăn chặn sự không phù hợp tồi tệ nhất | 
| n = 1 | 15 | độ đúng ranh giới | 
| giá trị hỗn hợp | 5 | xử lý tiêu cực và đặt hàng | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi cả hai mảng đều chứa nhiều tập hợp giống hệt nhau nhưng theo thứ tự khác nhau. Ví dụ:```
n = 5
d = [4, 1, 3, 2, 5]
b = [1, 2, 3, 4, 5]
```Việc sắp xếp cả hai sẽ cho ra các mảng giống hệt nhau và thuật toán tạo ra chi phí bằng 0. Một kẻ tham lam ngây thơ không phân loại có thể ghép 4 với 1 sớm và không bao giờ khôi phục được sự liên kết tối ưu, dẫn đến tích lũy chi phí không cần thiết. 

Một trường hợp khác là lây lan cực độ:```
n = 3
d = [-100, 0, 100]
b = [-50, -49, 200]
```Sau khi sắp xếp: 

d = [-100, 0, 100] 

b = [-50, -49, 200] 

Ghép nối chỉ số khôn ngoan tạo ra: 

50 + 49 + 100 = 199 

Thay vào đó, nếu người ta cố gắng so khớp cục bộ các giá trị gần nhất mà không có thứ tự chung, người ta có thể ghép không chính xác 100 với -50 sớm, khiến -100 được ghép với 200, tạo ra tổng số lớn hơn nhiều. Việc ghép đôi được sắp xếp sẽ tránh được điều này bằng cách thực thi sự liên kết đơn điệu toàn cục, đảm bảo không có cặp giao nhau nào tồn tại trong lần so khớp cuối cùng.
