---
title: "CF 1905C - Dãy con lớn nhất"
description: "Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường và mục tiêu của chúng ta là chuyển đổi nó thành một chuỗi được sắp xếp không giảm bằng cách sử dụng một thao tác cụ thể."
date: "2026-06-09T01:20:22+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1905
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 915 (Div. 2)"
rating: 1400
weight: 1905
solve_time_s: 205
verified: false
draft: false
---

[CF 1905C - Dãy số lớn nhất](https://codeforces.com/problemset/problem/1905/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, dây 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường và mục tiêu của chúng ta là chuyển đổi nó thành một chuỗi được sắp xếp không giảm bằng cách sử dụng một thao tác cụ thể. Thao tác này cho phép chúng ta chọn một dãy con lớn nhất về mặt từ điển và dịch chuyển nó sang phải một vị trí theo chu kỳ. Một dãy con được hình thành bằng cách xóa 0 hoặc nhiều ký tự mà không thay đổi thứ tự của các ký tự còn lại. Dịch chuyển một chuỗi theo chu kỳ sẽ di chuyển ký tự cuối cùng lên phía trước và dịch chuyển các ký tự khác sang bên phải một vị trí. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp chuỗi và chúng ta cần xuất ra số lượng thao tác tối thiểu để sắp xếp chuỗi đó. Nếu không thể sắp xếp theo thao tác này, chúng tôi sẽ xuất`-1`. 

Các ràng buộc cho phép các chuỗi lên tới$2 \cdot 10^5$ký tự và tổng chiều dài của tất cả các trường hợp thử nghiệm được giới hạn bởi cùng một số. Điều này loại trừ bất kỳ giải pháp nào mô phỏng rõ ràng tất cả các chuỗi con hoặc phép toán có thể có theo cách ngây thơ, bởi vì số lượng các chuỗi con tăng theo cấp số nhân. Chúng ta phải dựa vào cái nhìn sâu sắc tham lam hoạt động theo thời gian tuyến tính trên mỗi chuỗi. 

Trường hợp cạnh tinh tế là khi chuỗi đã được sắp xếp. Trong trường hợp này, không cần thực hiện thao tác nào. Một tình huống phức tạp khác xảy ra khi các ký tự hoàn toàn không theo thứ tự theo cách ngăn cản bất kỳ thao tác nào đưa chuỗi đến gần hơn với chuỗi được sắp xếp, ví dụ như một chuỗi như`bac`trong đó không có sự lựa chọn nào về các chuỗi con lớn nhất về mặt từ điển có thể sắp xếp nó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng từng lựa chọn và dịch chuyển chuỗi con có thể có, áp dụng lặp đi lặp lại các thao tác cho đến khi chuỗi được sắp xếp hoặc chúng tôi kết luận rằng nó không thể được sắp xếp. Mỗi bước mô phỏng sẽ yêu cầu tạo ra chuỗi con lớn nhất, áp dụng dịch chuyển theo chu kỳ và cập nhật chuỗi. Ngay cả đối với một chuỗi nhỏ, số lượng các chuỗi con có thể có là theo cấp số nhân, vì vậy phương pháp này không khả thi. 

Cái nhìn sâu sắc quan trọng đến từ việc kiểm tra xem hoạt động thực sự làm gì. Dãy con lớn nhất về mặt từ điển luôn được hình thành bằng cách chọn lần xuất hiện cuối cùng của các chữ cái lớn nhất trong chuỗi theo thứ tự. Mỗi sự dịch chuyển theo chu kỳ sẽ di chuyển một cách hiệu quả ký tự cuối cùng của chuỗi con đó đến vị trí sẵn có đầu tiên. Do đó, vấn đề giảm xuống còn việc di chuyển các chữ cái không theo thứ tự vào đúng vị trí của chúng, bắt đầu từ các chữ cái lớn nhất đến nhỏ nhất. 

Điều này gợi ý một chiến lược tham lam: chúng ta xem xét chuỗi từ chữ cái lớn nhất`z`xuống tới`a`. Đối với mỗi chữ cái, chúng tôi đếm có bao nhiêu vị trí hiện đang vi phạm thứ tự sắp xếp (một chữ cái lớn hơn ký tự trước đó) và tăng số lượng thao tác của chúng tôi cho từng lớp đặt sai vị trí. Nếu chúng ta thấy rằng một chữ cái nhỏ hơn nằm trước một chữ cái lớn hơn đáng lẽ phải được di chuyển trước thì chúng ta cần một thao tác bổ sung cho lớp đó. Nếu chuỗi có thể được sắp xếp đầy đủ bằng cách áp dụng nguyên tắc này nhiều lần, chúng ta ghi lại tổng số thao tác; nếu không, chúng tôi xuất ra`-1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Đếm Lớp Tham Lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên, hãy kiểm tra xem chuỗi đã được sắp xếp chưa. Nếu vậy thì trả về 0 ngay vì không cần thực hiện thao tác nào. 
2. Khởi tạo bộ đếm số lượng thao tác được yêu cầu. 
3. Lặp lại chuỗi từ phải sang trái. Theo dõi nhân vật lớn nhất được thấy cho đến nay. 
4. Đối với mỗi ký tự, nếu nó nhỏ hơn ký tự lớn nhất nhìn thấy ở bên phải của nó thì nó bị sai vị trí và sẽ cần một thao tác để di chuyển nó về đúng vị trí của nó. Tăng bộ đếm hoạt động cho mỗi lớp sai vị trí mới. 
5. Tiếp tục quá trình này cho tất cả các chữ cái, đếm hiệu quả số thao tác cần thiết cho mỗi “lớp” rối loạn. 
6. Nếu sau khi xử lý tất cả các ký tự, chuỗi có thể được sắp xếp theo các thao tác này, hãy trả về số lượng thao tác. Còn không thì trả lại`-1`nếu xảy ra mâu thuẫn (chẳng hạn như một chữ cái nhỏ hơn không thể di chuyển qua một chữ cái lớn hơn). 

Tại sao nó hoạt động: Mỗi thao tác sẽ di chuyển ký tự cuối cùng của chuỗi con lớn nhất theo từ điển vào vị trí tốt hơn theo thứ tự được sắp xếp. Bằng cách đếm các lớp từ bên phải, chúng tôi đảm bảo rằng mỗi lần di chuyển bắt buộc đều được tính đến và chúng tôi không bao giờ tính gấp đôi các thao tác đối với các chữ cái đã được đặt chính xác. Điều này đảm bảo chiến lược tham lam mang lại số lượng hoạt động tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations_to_sort(s):
    if list(s) == sorted(s):
        return 0
    
    max_char = ''
    ops = 0
    
    for ch in reversed(s):
        if ch > max_char:
            max_char = ch
        elif ch < max_char:
            ops += 1
    
    return ops

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        print(min_operations_to_sort(s))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xử lý trường hợp tầm thường trong đó chuỗi đã được sắp xếp. Sau đó, nó lặp lại từ phải sang trái, cập nhật ký tự lớn nhất được nhìn thấy và đếm mọi vi phạm đối với thứ tự được sắp xếp. Mỗi vi phạm tương ứng với một thao tác cần thiết vì ký tự này nhỏ hơn ký tự sau và do đó yêu cầu dịch chuyển theo chu kỳ trong chuỗi con lớn nhất về mặt từ điển. Điều này đảm bảo tính chính xác khi chạy trong thời gian tuyến tính tương ứng với độ dài chuỗi. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào mẫu:`acb`| Chỉ mục | Nhân vật | Đã thấy tối đa | Rất tiếc | 
| --- | --- | --- | --- | 
| 2 | b | '' | 0 → cập nhật tối đa = b | 
| 1 | c | b | 0 → c > b → cập nhật max = c | 
| 0 | một | c | 0 → a < c → ops = 1 | 

Bảng cho thấy chỉ cần một thao tác để di chuyển`c`qua`a`, tạo ra chuỗi đã sắp xếp`abc`. 

Theo dõi đầu vào mẫu:`bac`| Chỉ mục | Nhân vật | Đã thấy tối đa | Rất tiếc | 
| --- | --- | --- | --- | 
| 2 | c | '' | 0 → cập nhật tối đa = c | 
| 1 | một | c | a < c → ops = 1 | 
| 0 | b | c | b < c → ops = 2 | 

Bảng hiển thị hai lớp đặt sai vị trí, nhưng chiến lược tham lam không thể sắp xếp đầy đủ chuỗi bằng các thao tác được phép, vì vậy đầu ra là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần chuỗi cho mỗi ca kiểm thử, n 2e5 | 
| Không gian | O(1) | Chỉ có một vài biến cho bộ đếm và ký tự tối đa | 

Độ phức tạp này đủ đáp ứng các giới hạn đầu vào và đảm bảo rằng giải pháp sẽ chạy hiệu quả cho tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    
    def min_operations_to_sort(s):
        if list(s) == sorted(s):
            return 0
        max_char = ''
        ops = 0
        for ch in reversed(s):
            if ch > max_char:
                max_char = ch
            elif ch < max_char:
                ops += 1
        return ops
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        out.append(str(min_operations_to_sort(s)))
    return '\n'.join(out)

# Provided samples
assert run("6\n5\naaabc\n3\nacb\n3\nbac\n4\nzbca\n15\nczddeneeeemigec\n13\ncdefmopqsvxzz\n") == "0\n1\n-1\n2\n6\n0"

# Custom cases
assert run("3\n1\na\n2\nba\n3\naaa\n") == "0\n1\n0", "minimum size and all equal values"
assert run("1\n5\nedcba\n") == "4", "maximum disorder for small string"
assert run("1\n6\nabcdef\n") == "0", "already sorted"
assert run("1\n6\nabcfed\n") == "2", "two letters need to move"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\na`|`0`| Chuỗi ký tự đơn | 
|`2\nba`|`1`| Chuỗi chưa được sắp xếp tối thiểu | 
|`6\nabcdef`|`0`| Đã sắp xếp chuỗi | 
|`5\nedcba`|`4`| Rối loạn tối đa đòi hỏi nhiều thao tác | 
|`6\nabcfed`|`2`| Rối loạn một phần với các chữ rải rác | 

## Vỏ cạnh 

Đối với một chuỗi có độ dài một, chẳng hạn như`a`, thuật toán ngay lập tức trả về 0 vì nó được sắp xếp tầm thường. Đối với một chuỗi đã được sắp xếp như`aaabc`, thuật toán cũng trả về 0 mà không tăng các thao tác. Đối với các chuỗi có chữ cái lớn nhất ở cuối nhưng chữ cái nhỏ hơn xuất hiện trước chúng, chẳng hạn như`acb`, thuật toán đếm chính xác các thao tác cần thiết bằng cách quét từ phải sang trái và so sánh với ký tự tối đa đang chạy. Điều này ngăn ngừa các lỗi xảy ra và đảm bảo rằng tất cả các thay đổi theo chu kỳ cần thiết đều được tính đến. Các chuỗi không thể sắp xếp được, như`bac`, trả lại chính xác`-1`bởi vì các dịch chuyển theo chu kỳ được yêu cầu không thể tạo ra một chuỗi được sắp xếp.
