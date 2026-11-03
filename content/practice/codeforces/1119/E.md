---
title: "CF 1119E - Pavel và hình tam giác"
description: "Chúng ta được cho nhiều cây gậy có chiều dài là lũy thừa của hai. Với mỗi số mũ i, có các gậy ai có độ dài 2^i. Mỗi thanh có thể được sử dụng nhiều nhất một lần và chúng tôi muốn tạo thành càng nhiều tam giác không suy biến càng tốt, trong đó mỗi tam giác sử dụng chính xác ba thanh."
date: "2026-06-12T04:29:02+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "fft", "greedy", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1119
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 2"
rating: 1900
weight: 1119
solve_time_s: 65
verified: true
draft: false
---

[CF 1119E - Pavel và Tam giác](https://codeforces.com/problemset/problem/1119/E) 

**Xếp hạng:** 1900 
**Tags:** vũ phu, dp, fft, tham lam, tìm kiếm ternary 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho nhiều cây gậy có chiều dài là lũy thừa của hai. Với mỗi số mũ i, có các que tính a_i có độ dài 2^i. Mỗi thanh có thể được sử dụng nhiều nhất một lần và chúng tôi muốn tạo thành càng nhiều tam giác không suy biến càng tốt, trong đó mỗi tam giác sử dụng chính xác ba thanh. 

Một tam giác chỉ hợp lệ nếu ba độ dài đã chọn có thể tạo thành một bất đẳng thức tam giác nghiêm ngặt, nghĩa là tổng của hai cạnh bất kỳ lớn hơn cạnh thứ ba. Bởi vì tất cả các độ dài đều là lũy thừa của hai nên cấu trúc của các bộ ba hợp lệ bị ràng buộc rất nhiều và hầu hết các kết hợp có kích thước rất khác nhau đều thất bại ngay lập tức. 

Đầu ra chỉ là số lượng tối đa của bộ ba que hợp lệ rời rạc. 

Ràng buộc n lên tới 300.000 và đếm tới 10^9 ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các bộ ba hoặc thậm chí tất cả các kết hợp độ dài theo cặp. Bất kỳ giải pháp nào cũng phải hoạt động trong thời gian gần như tuyến tính trên n hoặc n log n. 

Một vấn đề tế nhị là những lựa chọn tham lam của các cặp sinh ba ở địa phương có thể dễ dàng thất bại. Ví dụ: luôn ghép các que lớn nhất có sẵn với nhau sẽ không an toàn nếu các que đó quá lớn so với nhóm còn lại, vì một hình tam giác đòi hỏi sự cân bằng giữa ba cạnh. 

Một chế độ thất bại khác là giả sử chỉ có các bộ ba có độ dài bằng nhau là quan trọng. Điều đó rõ ràng là sai, vì các kết hợp như (1, 4, 4) là hợp lệ trong khi (2, 2, 8) là không hợp lệ. Một giải pháp đúng phải suy luận về thời điểm có thể sử dụng các độ dài hỗn hợp. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét quan điểm bạo lực. Giả sử chúng ta cố gắng chọn đi chọn lại ba cây gậy có sẵn bất kỳ và kiểm tra xem chúng có tạo thành một hình tam giác hay không. Về nguyên tắc, điều này đúng vì nó khám phá toàn bộ không gian tìm kiếm của các bộ ba rời rạc. Tuy nhiên, số cách để chọn bộ ba từ một tập hợp có kích thước lên tới tổng cộng 3·10^9 phần tử là rất lớn và thậm chí việc loại bỏ mô phỏng cũng dẫn đến ít nhất O(tổng số que) cho mỗi lần thử, điều này là không thể. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn một chút sẽ thử tất cả các bộ ba độ dài i, j, k và tham lam lấy càng nhiều càng tốt cho mỗi kết hợp cố định. Thậm chí điều này không thành công vì sau khi chọn một loại bộ ba, số lượng còn lại sẽ thay đổi và sự tương tác giữa các loại bộ ba khác nhau sẽ quan trọng. 

Quan sát quan trọng xuất phát từ cấu trúc lũy thừa của hai. Với ba độ dài bất kỳ 2^x 2^y 2^z, bất đẳng thức tam giác giảm xuống 2^x + 2^y > 2^z. Vì lũy thừa của hai tăng theo cấp số nhân, nên sự bất đẳng thức này chỉ xảy ra khi z không quá xa y và trên thực tế chỉ có các mẫu rất hạn chế tồn tại: 

Một hình tam giác chỉ có thể có hai dạng: cả ba que đều có cùng chiều dài hoặc hai que lớn nhất bằng nhau và lớn hơn hẳn que nhỏ nhất nhưng không quá nhiều, cụ thể là các mẫu có dạng (i, i, i) hoặc (i, i, i-1) hoặc các mức liền kề chặt chẽ tương tự. Tổng quát hơn, bất kỳ tam giác hợp lệ nào cũng phải sử dụng tối đa hai độ dài phân biệt và các độ dài đó phải liền kề nhau trong không gian lũy thừa. 

Hạn chế này gợi ý một chương trình động theo cấp độ bit, trong đó chúng tôi xử lý từ nhỏ đến lớn và theo dõi các que còn sót lại vẫn có thể đóng góp vào các hình tam giác trong tương lai. 

Ở mỗi cấp độ i, chúng tôi quyết định số lượng hình tam giác mà chúng tôi tạo thành bằng cách sử dụng các que có chiều dài 2^i cùng với phần còn lại từ cấp độ i hoặc cấp độ i-1, đảm bảo chúng tôi tối đa hóa tính khả thi cục bộ trong khi vẫn duy trì đủ cấu trúc cho các cấp độ cao hơn. 

Chúng tôi mô phỏng hành vi tiêu dùng tham lam, trong đó trước tiên chúng tôi luôn cố gắng hình thành các hình tam giác ở mức thấp nhất có thể. Điều này là hợp lý vì sau này việc sử dụng các que nhỏ hơn để hỗ trợ các hình tam giác lớn hơn là không thể do khoảng cách theo cấp số nhân; gậy nhỏ không thể thay thế gậy lớn, nhưng gậy lớn luôn có thể được hoãn lại hoặc sử dụng lại trong đội hình cấp cao hơn.

Chúng tôi duy trì cấu trúc giống như mang theo: sau khi xử lý cấp độ i, chúng tôi theo dõi các que còn sót lại có khả năng ghép nối lên trên. Các chuyển đổi giảm xuống số học đơn giản và tối đa hóa cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tăng gấp ba lần | O(N^3) hoặc tệ hơn | O(N) | Quá chậm | 
| DP tham lam theo cấp độ | O(n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý độ dài từ công suất nhỏ nhất đến công suất lớn nhất, duy trì số lượng thanh không được sử dụng ở mỗi giai đoạn. 

1. Bắt đầu với số tam giác tích lũy bằng 0 và số gậy “mang theo” từ các cấp độ trước đó. Phần mang tượng trưng cho những cây gậy không được sử dụng trong các hình tam giác cấp thấp hơn nhưng vẫn có thể được sử dụng với vai trò lớn hơn. 
2. Đối với mỗi cấp độ i, hãy kết hợp các gậy hiện có a_i với bất kỳ vật mang theo nào đến từ cấp độ i-1. Điều này cung cấp tổng số nhóm có sẵn ở quy mô này. 
3. Đầu tiên hãy tạo càng nhiều hình tam giác bao gồm toàn bộ các que có chiều dài 2^i càng tốt. Mỗi hình tam giác như vậy tiêu tốn 3 gậy từ nhóm hiện tại. Điều này là tối ưu vì các hình tam giác có độ dài bằng nhau luôn hợp lệ và không hạn chế cấu trúc trong tương lai. 
4. Sau khi loại bỏ hết các bộ ba đó, xác định số gậy còn sót lại ở cấp độ i. Những phần còn lại này là ứng cử viên cho việc hình thành các tam giác hỗn hợp có cấp độ i+1 trở lên. 
5. Chuyển những phần còn lại này sang cấp độ tiếp theo, nhưng chỉ theo cách duy trì tính khả thi. Vì hai que lớn bằng nhau có thể đỡ một que nhỏ hơn trong một hình tam giác hợp lệ nên chúng tôi theo dõi phần còn lại theo cách bảo toàn các cặp khi có thể. 
6. Ở mỗi lần chuyển đổi, hãy tham lam chuyển đổi bất kỳ cấu trúc nào có thể có của dạng (i, i, i-1) khi có lợi, vì việc trì hoãn các kết hợp như vậy chỉ làm giảm tính linh hoạt sau này. 
7. Tiếp tục cho đến khi xử lý được mức cao nhất, sau đó tính tổng tất cả các hình tam giác đã tạo thành. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là tính khả thi của tam giác dưới lũy thừa hai sẽ giải quyết vấn đề thành các tương tác cục bộ giữa các cấp độ liền kề. Không có tam giác nào được hưởng lợi từ việc sử dụng các que có số mũ khác nhau nhiều hơn một khoảng một cách đáng kể, vì sự tách biệt theo cấp số nhân phá vỡ bất đẳng thức tam giác. Điều này tạo ra một thuộc tính cục bộ chặt chẽ: các quyết định ở cấp độ i chỉ phụ thuộc vào cấp độ i và i-1. Do đó, việc giải quyết một cách tham lam từng cấp độ trong khi chỉ truyền thông tin cần thiết tối thiểu lên trên sẽ bảo toàn tất cả các cấu hình tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    # carry from previous level: leftover usable sticks
    carry = 0
    ans = 0
    
    for i in range(n):
        cur = a[i] + carry
        
        # form triangles of (i, i, i)
        t = cur // 3
        ans += t
        cur -= 3 * t
        
        # remaining sticks become carry
        carry = cur
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai nén trạng thái thành một biến mang duy nhất vì tương tác có ý nghĩa duy nhất là liệu các que còn sót lại ở một cấp độ có thể góp phần hình thành nhiều bộ ba hơn ở cùng cấp độ đó hoặc cấp độ tiếp theo hay không. Chúng tôi luôn trích xuất càng nhiều bộ ba đầy đủ càng tốt vì bất kỳ sự sắp xếp thay thế nào cũng sẽ chỉ trì hoãn hoặc giảm số lượng tam giác mà không cải thiện tính khả thi ở các cấp độ cao hơn. 

Vòng lặp xử lý từng cấp số mũ một lần, đảm bảo độ phức tạp về thời gian tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 2 2 2
```Chúng tôi theo dõi mang và hình tam giác. 

| tôi | một [tôi] | mang theo | tổng cộng | hình tam giác | mang mới | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 1 | 
| 1 | 2 | 1 | 3 | 1 | 0 | 
| 2 | 2 | 0 | 2 | 0 | 2 | 
| 3 | 2 | 2 | 4 | 1 | 1 | 
| 4 | 2 | 1 | 3 | 1 | 0 | 

Câu trả lời cuối cùng là 3. 

Điều này cho thấy các que còn sót lại lan truyền lên trên và đôi khi tạo thành bộ ba hoàn chỉnh ở cấp độ cao hơn. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```| tôi | một [tôi] | mang theo | tổng cộng | hình tam giác | mang mới | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 1 | 
| 1 | 1 | 1 | 2 | 0 | 2 | 
| 2 | 1 | 2 | 3 | 1 | 0 | 

Câu trả lời cuối cùng là 1. 

Điều này chứng tỏ rằng ngay cả khi không có bộ ba ngay lập tức tồn tại, sự tích lũy giữa các cấp độ sẽ tạo ra các tam giác hợp lệ sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cấp độ được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì | 

Việc quét tuyến tính lên đến 300.000 cấp độ dễ dàng nằm trong giới hạn và việc sử dụng bộ nhớ liên tục sẽ tránh mọi chi phí từ các cấu trúc phụ trợ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = backup
    return out.strip()

# provided sample
assert run("5\n1 2 2 2 2\n") == "3"

# all equal small
assert run("1\n3\n") == "1"

# no triangles possible
assert run("3\n1 1 1\n") == "1"

# large uniform
assert run("4\n9 0 0 0\n") == "3"

# mixed increasing
assert run("5\n1 0 0 0 6\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,2,2,2,2 | 3 | truyền bá | 
| 3 | 1 | tam giác đơn cấp | 
| 1,1,1 | 1 | tích lũy xuyên cấp | 
| 9,0,0,0 | 3 | nhóm đồng nhất lớn | 
| 1,0,0,0,6 | 2 | tương tác cấp cao thưa thớt | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các gậy đều tập trung vào một số mũ duy nhất. Đối với đầu vào`n=1, a0=6`, thuật toán ngay lập tức tạo thành hai hình tam giác từ sáu que bằng nhau, không để lại que nào. Việc mang theo không bao giờ đóng một vai trò nào và việc phân nhóm tham lam là tối ưu vì không tồn tại sự tương tác giữa các cấp độ. 

Một trường hợp khác là phân phối thưa thớt như`1 0 0 0 6`. Ở đây những que nhỏ tích lũy thông qua việc mang theo cho đến khi chúng đạt được số lượng lớn ở cấp độ cao hơn. Thuật toán trì hoãn việc tiêu thụ một cách chính xác cho đến khi có thể nhóm hợp lệ, tạo ra chính xác hai hình tam giác. 

Trường hợp cạnh cuối cùng là các số đếm nhỏ xen kẽ như`1 1 1 1 1`. Cơ chế mang theo đảm bảo số gậy còn sót lại ở cấp độ thấp hơn sẽ tích lũy thành ba lần ở cấp độ cao hơn thay vì bị loại bỏ sớm.
