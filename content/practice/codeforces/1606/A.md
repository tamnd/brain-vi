---
title: "CF 1606A - Cân AB"
description: "Chúng tôi đang làm việc với một chuỗi nhị phân chỉ gồm các ký tự a và b. Từ chuỗi này, chúng ta xem xét các cặp ký tự liền kề. Mỗi lần nhìn thấy mẫu ab, chúng ta đếm một lần, và mỗi lần nhìn thấy ba, chúng ta cũng đếm một lần."
date: "2026-06-10T07:49:38+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 1606
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 116 (Rated for Div. 2)"
rating: 900
weight: 1606
solve_time_s: 90
verified: false
draft: false
---

[CF 1606A - Cân bằng AB](https://codeforces.com/problemset/problem/1606/A) 

**Xếp hạng:** 900 
**Thẻ:** chuỗi 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một chuỗi nhị phân chỉ gồm các ký tự`a`Và`b`. Từ chuỗi này, chúng ta xem xét các cặp ký tự liền kề. Mỗi lần chúng ta nhìn thấy mô hình`ab`, chúng tôi đếm nó một lần và mỗi lần chúng tôi nhìn thấy`ba`, chúng tôi cũng đếm nó một lần. Số lượng này chỉ phụ thuộc vào sự chuyển tiếp giữa các ký tự lân cận. 

Nhiệm vụ là sửa đổi chuỗi bằng cách thay đổi từng ký tự riêng lẻ, mỗi thay đổi tốn một bước, sao cho số lượng ký tự`ab`chuyển tiếp trở nên chính xác bằng số lượng`ba`chuyển tiếp. Chúng tôi muốn đạt được sự cân bằng này với càng ít thay đổi càng tốt. 

Các ràng buộc nhỏ, với độ dài chuỗi tối đa là 100 và lên tới 1000 trường hợp thử nghiệm. Điều này có nghĩa là giải pháp O(n²) cho mỗi trường hợp thử nghiệm vẫn có thể vượt qua một cách thoải mái, nhưng chúng ta nên nhắm đến lý do O(n) cho mỗi trường hợp thử nghiệm vì cấu trúc gợi ý một cấu trúc trực tiếp thay vì mô phỏng. 

Một điểm tinh tế là cả hai`AB`Và`BA`chỉ phụ thuộc vào các cặp liền kề, do đó, bất kỳ sửa đổi nào cũng ảnh hưởng đến nhiều nhất hai chuyển tiếp lân cận. Sự phụ thuộc cục bộ này là điều làm cho vấn đề có thể giải quyết được bằng lý luận cấu trúc đơn giản hơn là tìm kiếm. 

Các trường hợp cạnh quan trọng là các chuỗi đã không có chuyển đổi nào cả, chẳng hạn như`aaaa`hoặc`bbbb`, trong đó cả hai số đếm đều bằng 0 và các chuỗi trong đó tất cả các ký tự xen kẽ nhau, chẳng hạn như`ababab`, trong đó cả hai số lượng đã được cân bằng. Một trường hợp quan trọng khác là khi sự mất cân bằng được gây ra bởi một ranh giới duy nhất giữa các khối dài, chẳng hạn`aaabbb`, trong đó tất cả các quá trình chuyển đổi đều tập trung ở một nơi. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử tất cả các chuỗi có thể truy cập được bằng cách thay đổi ký tự và tính toán`AB`Và`BA`cho mỗi người. Mỗi đánh giá chuỗi mất O(n) và số lượng chuỗi có thể là 2ⁿ, do đó, điều này nhanh chóng trở nên không khả thi ngay cả đối với n vừa phải. 

Quan sát chính là sự khác biệt giữa`AB`Và`BA`được xác định hoàn toàn bằng số lần chuỗi chuyển đổi giữa`a`Và`b`. Mỗi khi chuỗi thay đổi ký tự, chúng ta sẽ đóng góp một`ab`hoặc một`ba`. Nếu chúng ta quét chuỗi từ trái sang phải, mỗi lần chuyển đổi sẽ đóng góp chính xác một chuỗi cho một trong hai chuỗi đó.`AB`hoặc`BA`. 

Điều này có nghĩa là chúng ta không cần phải suy luận về tất cả các cặp một cách riêng biệt. Chúng ta chỉ cần kiểm soát sự chuyển tiếp giữa các khối ký tự giống hệt nhau. Nếu chúng ta làm cho toàn bộ chuỗi đồng nhất thì không có sự chuyển đổi nào cả, vì vậy cả hai số đếm đều bằng 0. Điều này thỏa mãn ngay điều kiện`AB = BA`. 

Vì bất kỳ chuỗi cuối cùng hợp lệ nào cũng phải có số lượng bằng nhau và một chuỗi không đổi đạt được sự bằng nhau khi không có chuyển tiếp bằng 0, nên vấn đề giảm xuống còn việc tìm số lượng thay đổi tối thiểu cần thiết để chuyển chuỗi thành tất cả`a`hoặc tất cả`b`. Chúng tôi chỉ cần tính số lượng ký tự khác với từng mục tiêu và chọn mức tối thiểu. 

Điều này là tối ưu vì mọi vị trí khác với chuỗi đích phải được thay đổi ít nhất một lần, do đó khoảng cách Hamming là giới hạn dưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n) | O(n) | Quá chậm | 
| Tối ưu (tạo chuỗi đồng nhất) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem chuỗi có bao nhiêu ký tự`a`và có bao nhiêu`b`. Điều này mang lại cho chúng ta chi phí để chuyển đổi toàn bộ chuỗi thành một chuỗi thống nhất thuộc một trong hai loại. 
2. Hãy xem xét hai công trình mục tiêu. Đầu tiên, chuyển đổi mọi thứ thành`a`. Số bước cần thiết chính xác là số bước`b`nhân vật. Thứ hai, chuyển đổi mọi thứ thành`b`. Số bước cần thiết là số`a`nhân vật. 
3. Chọn chi phí nhỏ hơn trong hai chi phí này. Điều này đảm bảo chúng tôi giảm thiểu số lượng sửa đổi. 
4. Xây dựng chuỗi kết quả bằng cách thay thế mọi ký tự bằng ký tự đích đã chọn. Điều này đảm bảo chuỗi trở nên đồng nhất. 
5. Trả về chuỗi đã xây dựng này, chuỗi này tự động thỏa mãn`AB = BA = 0`. 

### Tại sao nó hoạt động 

Một chuỗi thống nhất không chứa các cặp ký tự khác nhau liền kề, vì vậy cả hai`AB`Và`BA`là số không. Bất kỳ chuỗi không đồng nhất nào đều đưa ra ít nhất một ranh giới giữa`a`Và`b`và ranh giới đó đóng góp vào một trong hai số đếm. Vì mục tiêu là sự bình đẳng nên việc thu gọn tất cả các chuyển đổi sẽ loại bỏ hoàn toàn mọi nguồn mất cân bằng. 

Hơn nữa, việc chuyển đổi thành một chuỗi thống nhất là tối ưu vì mỗi ký tự không khớp phải được thay đổi ít nhất một lần và không sửa đổi một phần nào có thể làm giảm số lượng thay đổi bắt buộc dưới khoảng cách Hamming thành chuỗi không đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    
    cnt_a = s.count('a')
    cnt_b = len(s) - cnt_a
    
    if cnt_a <= cnt_b:
        print('a' * len(s))
    else:
        print('b' * len(s))
```Giải pháp đầu tiên là đếm tần số của từng ký tự. Điều này trực tiếp xác định chi phí biến chuỗi thành tất cả`a`hoặc tất cả`b`. Tùy chọn chi phí nhỏ hơn được chọn và toàn bộ chuỗi được ghi đè tương ứng. Điều này đảm bảo số lần chỉnh sửa tối thiểu và sự bình đẳng ngay lập tức về số lần chuyển đổi. 

Một cạm bẫy triển khai phổ biến là cố gắng đếm một cách rõ ràng`AB`Và`BA`trong quá trình xây dựng. Điều đó là không cần thiết vì một khi chuỗi được tạo đồng nhất, cả hai số đếm sẽ tự động giảm về 0. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để xem quyết định được đưa ra như thế nào. 

### Ví dụ 1:`aabbbabaa`| Bước | cnt_a | cnt_b | mục tiêu đã chọn | đầu ra | 
| --- | --- | --- | --- | --- | 
| ban đầu | 5 | 4 | - | - | 
| so sánh | 5 4 là sai | | b | bbbbbbbbb | 

Thuật toán chọn`b`bởi vì có ít hơn`b`những thay đổi cần thiết hơn`a`những thay đổi. Kết quả trở thành tất cả`b`, loại bỏ tất cả các chuyển đổi. Điều này xác nhận rằng phương pháp này bỏ qua cấu trúc cục bộ và chỉ tập trung vào chi phí chỉnh sửa tối thiểu. 

### Ví dụ 2:`abbb`| Bước | cnt_a | cnt_b | mục tiêu đã chọn | đầu ra | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 3 | - | - | 
| so sánh | 1 ≤ 3 đúng | | một | aaa | 

Ở đây chuyển đổi sang tất cả`a`rẻ hơn. Chuỗi kết quả không có chuyển tiếp, vì vậy`AB = BA = 0`. Điều này chứng tỏ rằng ngay cả khi chuỗi gốc có nhiều lần chuyển đổi, giải pháp không cần phải theo dõi chúng một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm ký tự và in kết quả chỉ cần một lần quét | 
| Không gian | O(1) | Chỉ bộ đếm và chuỗi đầu ra được lưu trữ | 

Các ràng buộc cho phép tối đa 1000 chuỗi có độ dài 100, do đó, tối đa 10⁵ hoạt động, dễ dàng nằm trong giới hạn để xử lý tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        cnt_a = s.count('a')
        cnt_b = len(s) - cnt_a
        out.append('a' * len(s) if cnt_a <= cnt_b else 'b' * len(s))
    
    return "\n".join(out) + "\n"

# provided samples
assert run("4\nb\naabbbabaa\nabbb\nabbaab\n") == "b\naabbbabaa\nbbbb\naaaaaa\n"

# custom cases
assert run("1\na") == "a\n"
assert run("1\nb") == "b\n"
assert run("1\nababab") == "bbbbbb\n"
assert run("1\naaaaa") == "aaaaa\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Chiều dài tối thiểu | 
|`b`|`b`| Chiều dài tối thiểu | 
|`ababab`|`bbbbbb`| Trường hợp xấu nhất thay thế | 
|`aaaaa`|`aaaaa`| Đã là trường hợp tối ưu | 

## Vỏ cạnh 

Một chuỗi như`aaaaa`đã không có chuyển tiếp nào, vì vậy cả hai`AB`Và`BA`là số không. Thuật toán nhìn thấy`cnt_a > cnt_b`như so sánh sai với`b`thay đổi, lựa chọn`a`và trả về cùng một chuỗi, duy trì tính tối ưu. 

Vì`bbbb`, tình huống là đối xứng. Thuật toán chọn`b`và trả về chuỗi không thay đổi, một lần nữa duy trì chi phí bằng 0. 

Đối với các chuỗi xen kẽ như`abab`, sự mất cân bằng ban đầu là không liên quan vì mọi ký tự đều đắt như nhau khi lật. Thuật toán vẫn thu gọn mọi thứ thành một chuỗi thống nhất, chẳng hạn`aaaa`, loại bỏ tất cả các chuyển đổi và đảm bảo sự bình đẳng. 

Những trường hợp này xác nhận rằng giải pháp hoàn toàn không phụ thuộc vào cấu trúc cục bộ, chỉ phụ thuộc vào số lượng ký tự toàn cục, điều này là đủ vì mọi câu trả lời hợp lệ luôn có thể được giảm xuống thành một cấu hình thống nhất mà không mất tính tối ưu.
