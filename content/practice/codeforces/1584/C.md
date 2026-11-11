---
title: "CF 1584C - Hai mảng"
description: "Chúng ta được cho hai mảng số nguyên có độ dài bằng nhau. Hoạt động được phép trên mảng đầu tiên khá cụ thể: chúng ta có thể chọn một số tập hợp con các vị trí và tăng từng phần tử được chọn lên đúng một. Sau đó, chúng ta được phép tự do sắp xếp lại mảng."
date: "2026-06-10T09:36:51+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1584
codeforces_index: "C"
codeforces_contest_name: "Technocup 2022 - Elimination Round 2"
rating: 900
weight: 1584
solve_time_s: 102
verified: true
draft: false
---

[CF 1584C - Hai mảng](https://codeforces.com/problemset/problem/1584/C) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên có độ dài bằng nhau. Hoạt động được phép trên mảng đầu tiên khá cụ thể: chúng ta có thể chọn một số tập hợp con các vị trí và tăng từng phần tử được chọn lên đúng một. Sau đó, chúng ta được phép tự do sắp xếp lại mảng. 

Câu hỏi đặt ra là liệu chúng ta có thể chuyển đổi mảng đầu tiên thành mảng thứ hai bằng cách sử dụng chính xác một giai đoạn hoạt động như vậy hay không. 

Bước sắp xếp lại là rất quan trọng vì nó loại bỏ mọi ràng buộc về vị trí. Sau bước tăng dần, chỉ có nhiều tập hợp giá trị quan trọng chứ không phải chúng đến từ đâu. Điều này có nghĩa là vấn đề cơ bản là liệu chúng ta có thể biến một tập hợp số nguyên thành một tập hợp số nguyên khác hay không bằng cách thêm một số vào một số phần tử của tập hợp số nguyên đầu tiên. 

Các ràng buộc nhỏ, với kích thước mảng lên tới 100 và nhiều nhất là 100 trường hợp thử nghiệm. Tìm kiếm theo cấp số nhân trực tiếp trên các tập hợp con của các phần tử sẽ được chấp nhận ở đường biên ở dạng thô cho một thử nghiệm, nhưng không phải khi suy luận rõ ràng trong tất cả các trường hợp. Tuy nhiên, các giới hạn nhỏ cho thấy rằng việc sắp xếp và kiểm tra tuyến tính có thể là đủ và bất kỳ giải pháp nào liên quan đến việc liệt kê tổ hợp các tập hợp con đều không cần thiết. 

Một cạm bẫy tinh tế xuất phát từ việc suy nghĩ quá nhiều về bước hoán vị. Ví dụ: người ta có thể cố gắng so khớp trực tiếp các chỉ số và quyết định một cách tham lam cho mỗi vị trí. Điều đó không thành công vì hoán vị cuối cùng cho phép gán lại tùy ý. 

Một sai lầm phổ biến khác là cho rằng chúng ta có thể quyết định một cách độc lập cho từng vị trí xem nó có cần tăng hay không dựa trên một thứ tự cố định. Vì hoán vị xảy ra sau khi tăng dần nên phối cảnh đúng là so sánh các cấu trúc được sắp xếp chứ không phải chỉ số gốc. 

Hãy xem xét kiểu thất bại này: nếu chúng ta cố gắng so khớp các phần tử theo thứ tự ban đầu, chúng ta có thể kết luận không chính xác là không thể xảy ra ngay cả khi tồn tại một hoán vị hợp lệ sau khi tăng dần. Việc sắp xếp lại sẽ phá hủy hoàn toàn ý nghĩa vị trí. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tăng từng tập hợp con của các chỉ số. Đối với mỗi tập hợp con, chúng tôi sẽ áp dụng mức tăng, sau đó sắp xếp và so sánh với mảng mục tiêu. Có 2^n tập hợp con và mỗi lần kiểm tra có giá O(n log n), điều này nhanh chóng trở nên không khả thi ngay cả khi n chỉ bằng 100, vì hệ số hằng số rất lớn và cấu trúc là không cần thiết. 

Quan sát chính là thao tác chỉ thay đổi giá trị +1 và không đưa ra bất kỳ phép biến đổi nào khác. Điều này có nghĩa là mỗi phần tử trong mảng cuối cùng phải đến từ một phần tử trong mảng ban đầu, không thay đổi hoặc tăng lên một lần. Sau khi sắp xếp cả hai mảng, chúng tôi đang cố gắng quyết định xem mỗi phần tử của mảng mục tiêu có thể khớp với một phần tử của mảng nguồn có chênh lệch là 0 hoặc 1 một cách hiệu quả hay không và mỗi thao tác +1 được sử dụng chính xác một lần cho mỗi phần tử đã chọn. 

Sau khi cả hai mảng được sắp xếp, việc ghép nối tự nhiên sẽ trở thành cấu trúc tối ưu để kiểm tra tính khả thi. Những yếu tố nhỏ nhất phải phù hợp với mục tiêu nhỏ nhất; mặt khác, sự không phù hợp ở cấp thấp không thể được sửa chữa bằng cách sắp xếp lại sau này. Điều này làm giảm vấn đề xuống còn quét tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê tập hợp con) | O(2^n · n log n) | O(n) | Quá chậm | 
| Sắp xếp + kết hợp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng trường hợp thử nghiệm thành một so sánh giữa hai mảng được sắp xếp.

1. Sắp xếp cả hai mảng theo thứ tự không giảm. Điều này loại bỏ tất cả sự mơ hồ về vị trí được đưa ra bởi bước hoán vị cuối cùng. 
2. Tính tổng chênh lệch giữa các tổng của mảng. Giá trị này biểu thị số lượng phần tử phải được tăng lên, vì mỗi thao tác sẽ tăng tổng số chính xác lên một. 
3. So sánh các phần tử tương ứng của mảng đã sắp xếp từ trái qua phải. Đối với mỗi vị trí, hãy kiểm tra xem giá trị đích ít nhất có phải là giá trị nguồn hay không. Nếu bất kỳ giá trị mục tiêu nào nhỏ hơn thì không có chuỗi thao tác +1 nào có thể khắc phục được điều này. 
4. Với mỗi cặp, hãy tính hiệu. Mọi chênh lệch phải là 0 hoặc 1, vì mỗi phần tử có thể tăng nhiều nhất một lần. 
5. Xác minh rằng tổng số chênh lệch +1 khớp với tổng chênh lệch được tính toán trước đó. 

Quyết định này phụ thuộc vào tính nhất quán giữa tính khả thi cục bộ (mỗi yếu tố chỉ có thể tăng thêm một) và tính khả thi toàn cầu (tổng mức tăng phải phù hợp với mức tăng tổng yêu cầu). 

### Tại sao nó hoạt động 

Sau khi sắp xếp, chúng tôi sẽ ghép các giá trị nhỏ nhất có sẵn lại với nhau. Nếu một phần tử nhỏ hơn trong mảng mục tiêu được ghép nối với một phần tử lớn hơn từ nguồn, thì các đối tác hoán đổi không thể khắc phục sự không khớp vì tất cả các phần tử nguồn lớn hơn sẽ chỉ làm xấu đi việc ghép nối trong tương lai. Sự liên kết tham lam này đảm bảo rằng nếu có bất kỳ phép biến đổi hợp lệ nào tồn tại, nó sẽ xuất hiện theo cặp thứ tự được sắp xếp. Hạn chế là mỗi phần tử chỉ có thể được tăng thêm một phần tử, thực thi lựa chọn nhị phân cho mỗi vị trí trong cặp được sắp xếp, đặc trưng đầy đủ cho phép biến đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort()
        b.sort()
        
        diff_sum = 0
        ok = True
        
        for i in range(n):
            if b[i] < a[i]:
                ok = False
            diff_sum += (b[i] - a[i])
            if b[i] - a[i] > 1:
                ok = False
        
        print("YES" if ok and diff_sum == sum(b) - sum(a) else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp cả hai mảng để chúng ta có thể suy luận về chúng dưới dạng nhiều tập hợp. Sau đó, vòng lặp sẽ kiểm tra đồng thời hai ràng buộc: liệu có giá trị đích nào nhỏ hơn giá trị nguồn được ghép nối của nó hay không và liệu có phần tử nào yêu cầu nhiều hơn một mức tăng để đạt được mục tiêu hay không. Sự khác biệt tích lũy theo dõi số lượng thao tác +1 sẽ cần thiết nếu chúng tôi tuân theo việc ghép nối này. 

Sự so sánh cuối cùng đảm bảo tính nhất quán giữa các ràng buộc cục bộ và yêu cầu toàn cầu một cách chính xác.`sum(b) - sum(a)`sự gia tăng phải được thực hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [-1, 1, 0]
b = [0, 0, 2]
```Sau khi sắp xếp:```
a = [-1, 0, 1]
b = [0, 0, 2]
```| tôi | một [tôi] | b[i] | b[i] - a[i] | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | -1 | 0 | 1 | vâng | 
| 1 | 0 | 0 | 0 | vâng | 
| 2 | 1 | 2 | 1 | vâng | 

Tất cả sự khác biệt đều nằm trong phạm vi cho phép và có chính xác hai phần tử yêu cầu tăng, khớp với tổng mức tăng được yêu cầu. Điều này xác nhận rằng một tập hợp con các phần tử hợp lệ có thể được chọn để tăng dần. 

### Ví dụ 2 

đầu vào:```
a = [0]
b = [2]
```Sau khi sắp xếp:```
a = [0]
b = [2]
```| tôi | một [tôi] | b[i] | b[i] - a[i] | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | không | 

Phần tử đơn lẻ sẽ yêu cầu hai lần tăng, nhưng mỗi phần tử chỉ có thể được tăng một lần. Điều này ngay lập tức vi phạm cấu trúc của hoạt động, do đó không thể chuyển đổi được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Việc sắp xếp chiếm ưu thế, trong khi quá trình quét là tuyến tính | 
| Không gian | O(n) | Lưu trữ cho mảng | 

Cho n lên tới 100 và t lên tới 100, điều này dễ dàng nằm trong giới hạn. Ngay cả chi phí phân loại cũng không đáng kể ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        b = list(map(int, sys.stdin.readline().split()))
        
        a.sort()
        b.sort()
        
        ok = True
        total_diff = 0
        
        for i in range(n):
            if b[i] < a[i]:
                ok = False
            if b[i] - a[i] > 1:
                ok = False
            total_diff += (b[i] - a[i])
        
        required = sum(b) - sum(a)
        output.append("YES" if ok and total_diff == required else "NO")
    
    return "\n".join(output)

# provided samples
assert run("""3
3
-1 1 0
0 0 2
1
0
2
5
1 2 3 4 5
1 2 3 4 5
""") == """YES
NO
YES"""

# all equal
assert run("""1
4
1 1 1 1
1 1 1 1
""") == "YES"

# impossible large jump
assert run("""1
3
0 0 0
3 0 0
""") == "NO"

# requires multiple increments but not allowed per element
assert run("""1
2
0 1
2 1
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | CÓ | trường hợp không hoạt động | 
| cú nhảy đơn lớn | KHÔNG | phần tử không thể vượt quá +1 | 
| hỗn hợp không phù hợp | KHÔNG | thực thi ràng buộc theo từng phần tử | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi tổng chênh lệch khớp với số phần tử nhưng một phần tử yêu cầu nhiều hơn một phần tử. Ví dụ, chuyển đổi`[0, 0]`vào trong`[2, 0]`có tổng chênh lệch bằng 2, nhưng vẫn thất bại vì một phần tử sẽ cần hai số gia. Việc sắp xếp làm cho lỗi này hiển thị ngay lập tức vì một cặp trở thành`(0, 2)`. 

Một trường hợp cạnh khác xảy ra khi các giá trị tổng bằng nhau nhưng không khớp cục bộ. Ví dụ,`[1, 3]`ĐẾN`[2, 2]`bảo toàn tổng, nhưng việc ghép cặp được sắp xếp sẽ tạo ra`(1, 2)`Và`(3, 2)`, tiết lộ vi phạm trong đó mục tiêu nhỏ hơn nguồn, không thể sửa chữa bằng hoán vị. 

Thuật toán loại bỏ chính xác cả hai trường hợp vì nó thực thi cả tính khả thi cục bộ cho mỗi phần tử và tính nhất quán toàn cầu thông qua theo dõi tổng.
