---
title: "CF 104636A - Lời thề của người canh đêm"
description: "Chúng ta được cung cấp một danh sách những điểm mạnh của người quản lý và chúng ta cần quyết định xem Jon Snow sẽ hỗ trợ người quản lý nào. Người quản lý chỉ được hỗ trợ nếu tồn tại ít nhất một người quản lý có sức mạnh nhỏ hơn và ít nhất một người quản lý có sức mạnh lớn hơn."
date: "2026-06-29T17:04:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104636
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u043c\u0430\u0441\u0441\u0438\u0432\u044b, \u0441\u0442\u0440\u043e\u043a\u0438"
rating: 0
weight: 104636
solve_time_s: 71
verified: false
draft: false
---

[CF 104636A - Lời thề của người gác đêm](https://codeforces.com/problemset/problem/104636/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách những điểm mạnh của người quản lý và chúng ta cần quyết định xem Jon Snow sẽ hỗ trợ người quản lý nào. Người quản lý chỉ được hỗ trợ nếu tồn tại ít nhất một người quản lý có sức mạnh nhỏ hơn và ít nhất một người quản lý có sức mạnh lớn hơn. 

Điều kiện này được áp dụng độc lập cho từng vị trí trong mảng. Chúng tôi không chọn một tập hợp con hoặc sửa đổi các giá trị, chúng tôi chỉ đơn giản kiểm tra từng phần tử xem nó có bị “kẹp” giữa giá trị nhỏ hơn và giá trị lớn hơn ở đâu đó trong bộ sưu tập đầy đủ hay không. 

Kích thước đầu vào có thể đạt tới 100.000 phần tử và mỗi giá trị có thể lớn tới 10^9. Điều này ngay lập tức loại trừ mọi giải pháp so sánh từng phần tử với tất cả các phần tử khác. Cách tiếp cận bậc hai sẽ thực hiện theo thứ tự 10^10 phép so sánh trong trường hợp xấu nhất, vượt xa mức phù hợp với giới hạn hai giây. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị giống hệt nhau. Trong tình huống đó, không có phần tử nào có cả phần tử lân cận nhỏ hơn và lớn hơn hoàn toàn. Mặc dù có nhiều phần tử nhưng không có phần tử nào thỏa mãn điều kiện. Một trường hợp góc khác là khi chỉ tồn tại hai giá trị phân biệt. Khi đó, chỉ giá trị lớn hơn có lân cận nhỏ hơn và chỉ giá trị nhỏ hơn có lân cận lớn hơn, nhưng không có phần tử nào có thể thỏa mãn cả hai cùng một lúc. 

## Phương pháp tiếp cận 

Một cách trực tiếp để kiểm tra điều kiện cho mỗi người quản lý là quét toàn bộ mảng và tìm kiếm giá trị nhỏ hơn và giá trị lớn hơn. Đối với mỗi chỉ mục, chúng tôi sẽ chạy hai lần quét: một lần để phát hiện xem có giá trị nào nhỏ hơn không và một lần khác để phát hiện xem có giá trị nào lớn hơn hay không. Điều này hoạt động chính xác vì nó xác minh rõ ràng điều kiện trong định nghĩa. 

Tuy nhiên, điều này dẫn đến tổng số lần kiểm tra là O(n^2). Với n lên tới 100.000, điều này trở thành khoảng 10^10 so sánh, quá chậm. 

Quan sát quan trọng là điều kiện cho một giá trị chỉ phụ thuộc vào các điểm cực trị tổng thể trong mảng. Một giá trị chỉ có thể có phần tử nhỏ hơn nếu nó lớn hơn mức tối thiểu toàn cục. Tương tự, nó chỉ có thể có phần tử lớn hơn nếu nó nhỏ hơn mức tối đa toàn cục. Điều này làm giảm vấn đề xác định phần tử nào nằm hoàn toàn giữa giá trị tối thiểu và tối đa của mảng. 

Khi đã biết mức tối thiểu và tối đa, mỗi phần tử có thể được phân loại theo thời gian không đổi, giảm toàn bộ giải pháp thành một lần quét tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dịch điều kiện “có ít nhất một phần tử nhỏ hơn và ít nhất một phần tử lớn hơn” thành đặc tính chung bằng cách sử dụng các giá trị tối thiểu và tối đa. 

1. Tính giá trị nhỏ nhất trong mảng. Điều này thể hiện cường độ nhỏ nhất có thể, do đó, bất kỳ phần tử nào bằng nó đều không thể có phần tử lân cận nhỏ hơn. 
2. Tính giá trị lớn nhất trong mảng. Điều này thể hiện cường độ lớn nhất có thể, vì vậy bất kỳ phần tử nào bằng nó không thể có phần tử lân cận lớn hơn. 
3. Lặp lại tất cả các phần tử và đếm xem có bao nhiêu phần tử thỏa mãn giá trị điều kiện > giá trị nhỏ nhất và giá trị < giá trị lớn nhất. Điều này trực tiếp mã hóa yêu cầu rằng cả phần tử nhỏ hơn và phần tử lớn hơn đều tồn tại ở đâu đó trong mảng. 
4. Trả về số đếm làm câu trả lời. 

Ý tưởng chính là sự tồn tại của bất kỳ phần tử nhỏ hơn nào cũng tương đương với việc lớn hơn mức tối thiểu toàn cục và sự tồn tại của bất kỳ phần tử lớn hơn nào cũng tương đương với việc nhỏ hơn mức tối đa toàn cục. Điều này giúp loại bỏ mọi nhu cầu so sánh theo cặp. 

### Tại sao nó hoạt động

Tính chính xác xuất phát từ cách cực trị toàn cục mã hóa tất cả các so sánh có thể có. Nếu một phần tử bằng giá trị tối thiểu thì không có phần tử nào nhỏ hơn tồn tại ở bất kỳ đâu trong mảng. Nếu nó lớn hơn mức tối thiểu thì bản thân mức tối thiểu đảm bảo sự tồn tại của phần tử nhỏ hơn. Lý do tương tự áp dụng đối xứng cho mức tối đa. Do đó, việc thỏa mãn cả hai bất đẳng thức chính xác tương đương với việc thỏa mãn điều kiện ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    mn = min(a)
    mx = max(a)
    
    ans = 0
    for x in a:
        if mn < x < mx:
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc mảng và tính toán mức tối thiểu và tối đa của nó theo thời gian tuyến tính. Hai giá trị này là thông tin toàn cầu duy nhất cần thiết để đánh giá mọi phần tử. 

Vòng lặp sau đó sẽ kiểm tra từng phần tử theo các bất đẳng thức nghiêm ngặt. Điều kiện được viết cẩn thận là mn < x < mx để đảm bảo rằng các phần tử biên được loại trừ. Một lỗi phổ biến là sử dụng các phép so sánh không chặt chẽ, sẽ bao gồm không chính xác các phần tử bằng mức tối thiểu hoặc tối đa. 

Cấu trúc tổng thể rất đơn giản: tất cả các tính toán nặng được đẩy vào một lượt duy nhất và việc phân loại được thực hiện trong thời gian không đổi cho mỗi phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 5
```| tôi | giá trị | phút | tối đa | điều kiện (tối thiểu < x < tối đa) | tính | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 5 | sai | 0 | 
| 2 | 5 | 1 | 5 | sai | 0 | 

Giá trị tối thiểu là 1 và tối đa là 5. Cả hai điểm cuối đều không được hỗ trợ vì mỗi điểm cuối đều thiếu điểm lân cận nhỏ hơn hoặc lớn hơn. Kết quả là 0. 

### Ví dụ 2 

đầu vào:```
3
1 2 5
```| tôi | giá trị | phút | tối đa | điều kiện (tối thiểu < x < tối đa) | tính | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 5 | sai | 0 | 
| 2 | 2 | 1 | 5 | đúng | 1 | 
| 3 | 5 | 1 | 5 | sai | 1 | 

Ở đây giá trị 2 nằm hoàn toàn trong khoảng từ 1 đến 5, do đó, nó có cả phần tử nhỏ hơn và phần tử lớn hơn ở đâu đó trong mảng. Hai giá trị còn lại nằm ở mức cực đoan và không đáp ứng được một bên yêu cầu. 

Những dấu vết này cho thấy rằng chỉ có các phần tử bên trong đối với cực trị tổng thể là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tính tối thiểu và tối đa, một lượt để đếm các phần tử hợp lệ | 
| Không gian | O(1) | Chỉ một số biến được sử dụng ngoài mảng đầu vào | 

Quét tuyến tính nằm trong giới hạn n lên tới 100.000. Việc sử dụng bộ nhớ là tối thiểu do không yêu cầu cấu trúc dữ liệu phụ trợ ngoài danh sách đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample cases
assert run("2\n1 5\n") == "0", "sample 1"
assert run("3\n1 2 5\n") == "1", "sample 2"

# all equal
assert run("4\n7 7 7 7\n") == "0", "all equal"

# strictly increasing
assert run("5\n1 2 3 4 5\n") == "3", "middle elements only"

# two distinct values
assert run("6\n1 1 1 2 2 2\n") == "0", "no interior values"

# single element
assert run("1\n10\n") == "0", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 0 | không có yếu tố nào có thể thỏa mãn cả hai bên | 
| mảng tăng nghiêm ngặt | n-2 | chỉ các yếu tố nội thất mới đủ điều kiện | 
| mảng hai giá trị | 0 | chỉ cực đoan, không có nội thất hợp lệ | 
| phần tử đơn | 0 | trường hợp biên tối thiểu | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau thì cả giá trị tối thiểu và tối đa đều bằng nhau. Trong tình huống đó, điều kiện mn < x < mx không thành công đối với mọi phần tử vì không có số nào có thể nằm hoàn toàn giữa các giới hạn bằng nhau. Thuật toán trả về 0 một cách chính xác vì mọi phần tử đều bị loại trừ bởi cả hai phép so sánh. 

Khi chỉ có hai giá trị riêng biệt, ví dụ như một mảng như [1, 1, 1, 5, 5], giá trị tối thiểu là 1 và giá trị tối đa là 5. Cả 1 và 5 đều không nằm hoàn toàn giữa chúng, do đó, một lần nữa không có phần tử nào được tính. Điều này phù hợp với thực tế là bất kỳ phần tử nào được chọn đều không đạt ít nhất một mặt của yêu cầu. 

Khi mảng có đúng một phần tử thì cả giá trị nhỏ nhất và giá trị lớn nhất đều bằng phần tử đó và bất đẳng thức nghiêm ngặt sẽ loại bỏ phần tử đó ngay lập tức. Thuật toán tự nhiên trả về 0 mà không cần xử lý đặc biệt.
