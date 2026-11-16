---
title: "CF 1890A - Sơn Doremy 3"
description: "Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng các số nguyên dương. Nhiệm vụ không phải là xây dựng một mảng mới từ đầu mà là quyết định xem liệu chúng ta có thể sắp xếp lại các phần tử đã cho sao cho mọi cặp phần tử liền kề trong chuỗi kết quả có cùng tổng hay không."
date: "2026-06-08T22:03:52+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1890
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 906 (Div. 2)"
rating: 800
weight: 1890
solve_time_s: 80
verified: true
draft: false
---

[CF 1890A - Doremy's Paint 3](https://codeforces.com/problemset/problem/1890/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng các số nguyên dương. Nhiệm vụ không phải là xây dựng một mảng mới từ đầu mà là quyết định xem liệu chúng ta có thể sắp xếp lại các phần tử đã cho sao cho mọi cặp phần tử liền kề trong chuỗi kết quả có cùng tổng hay không. Nói cách khác, sau khi sắp xếp lại, tổng của phần tử thứ nhất và thứ hai phải bằng tổng của phần tử thứ hai và thứ ba, và sự đẳng thức này phải tiếp tục xuyên suốt mảng. 

Ràng buộc chính là chúng ta được phép hoán vị mảng một cách tùy ý nên cấu trúc không cố định. Chúng ta chỉ quan tâm liệu có tồn tại thứ tự nào đó thỏa mãn điều kiện tổng kề hay không. 

Kích thước đầu vào nhỏ, tối đa 100 phần tử cho mỗi trường hợp thử nghiệm và nhiều nhất là 100 trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ giải pháp nào gần như bậc hai cho mỗi trường hợp thử nghiệm đều đã đủ nhanh, nhưng chúng ta vẫn nên hướng tới việc kiểm tra hằng số hoặc tuyến tính cho mỗi trường hợp vì cấu trúc của bài toán cho phép mô tả đặc tính trực tiếp. 

Một cách tiếp cận đơn giản sẽ thử tất cả các hoán vị của mảng và kiểm tra điều kiện. Đó là giai thừa phức tạp và trở nên không thể ngay cả khi n bằng 10. 

Một cách tiếp cận không chính xác tinh tế hơn sẽ cho rằng việc sắp xếp giúp ích trực tiếp, ví dụ như kiểm tra xem một mảng được sắp xếp có thỏa mãn điều kiện hay không. Điều này không thành công vì điều kiện phụ thuộc vào cấu trúc kề chứ không phải độ lớn thứ tự. 

Một kịch bản sai lệch điển hình xuất hiện khi các giá trị lặp lại. Ví dụ: một mảng như 1, 1, 4, 5 không thể được coi là hợp lệ ngay cả khi nó chứa các bản sao có thể gợi ý tính linh hoạt. Nỗ lực sắp xếp lại tham lam thường thất bại vì các bản sửa lỗi cục bộ phá vỡ sự bình đẳng toàn cục của các tổng liền kề. 

Khó khăn cốt lõi là sự ràng buộc kết hợp mọi cặp liền kề, do đó tính nhất quán cục bộ tạo ra một cấu trúc toàn cầu rất cứng nhắc. 

## Phương pháp tiếp cận 

Nếu chúng tôi cố gắng sử dụng vũ lực, chúng tôi sẽ tạo ra mọi hoán vị của mảng và kiểm tra xem tất cả các tổng liền kề có bằng nhau hay không. Đối với mỗi hoán vị, chúng ta tính n trừ một tổng, do đó độ phức tạp tổng cộng sẽ trở thành O(n! · n). Ngay cả khi n bằng 10, điều này đã vượt quá giới hạn khả thi. 

Quan sát quan trọng xuất phát từ việc viết ra điều kiện ngụ ý gì về mặt đại số. Giả sử tồn tại một sự sắp xếp hợp lệ. Khi đó với mọi chỉ số i, chúng ta phải có b[i] + b[i+1] bằng cùng một hằng số k. So sánh các đẳng thức liên tiếp ta có b[i] + b[i+1] = b[i+1] + b[i+2], đơn giản hóa ngay thành b[i] = b[i+2]. Điều này có nghĩa là chuỗi phải xen kẽ chặt chẽ giữa hai giá trị. 

Khi chúng ta đạt đến điểm này, toàn bộ cấu trúc sẽ sụp đổ thành một mô hình rất đơn giản. Bất kỳ mảng hợp lệ nào cũng phải có dạng x, y, x, y, x, y, v.v. hoặc điểm bắt đầu ngược lại. Do đó, tối đa hai giá trị riêng biệt có thể xuất hiện trong mảng. Nếu có nhiều hơn hai giá trị phân biệt tồn tại thì không có hoán vị nào có thể thỏa mãn điều kiện. 

Khi tồn tại chính xác hai giá trị, chẳng hạn x và y với tần số c1 và c2, câu hỏi duy nhất còn lại là liệu chúng ta có thể đặt chúng ở các vị trí xen kẽ nhau hay không. Các vị trí được chia thành hai nhóm: chỉ số 1, 3, 5, v.v. và chỉ số 2, 4, 6, v.v. Một trong những nhóm này phải hoàn toàn là x và nhóm còn lại hoàn toàn là y. Điều này buộc nhiều tập hợp số đếm phải khớp với sự phân chia chẵn lẻ của độ dài mảng. 

Với chiều dài n, một nhóm có kích thước trần (n/2) và nhóm còn lại có kích thước sàn (n/2). Vì vậy, tần số phải khớp chính xác với các kích thước này theo một thứ tự nào đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Phân tích tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc mảng cho ca kiểm thử và tính tần số của từng giá trị riêng biệt. Bước này làm giảm vấn đề từ việc sắp xếp các phần tử sang suy luận về số lượng, vì thứ tự sẽ được xây dựng một cách ngầm định. 
2. Nếu số lượng các giá trị riêng biệt lớn hơn hai, hãy kết luận ngay rằng không có sự sắp xếp hợp lệ nào tồn tại. Điều này tuân theo yêu cầu thay thế, chỉ cho phép hai giá trị trong bất kỳ chuỗi hợp lệ nào. 
3. Nếu chỉ có một giá trị riêng biệt thì mảng đó đã hợp lệ ở mức độ nhỏ vì mọi tổng liền kề đều giống hệt nhau. 
4. Nếu có chính xác hai giá trị phân biệt, hãy trích xuất tần số c1 và c2 của chúng. Tính toán hai kích thước nhóm được yêu cầu, đó là sàn (n/2) và trần (n/2). 
5. Kiểm tra xem nhiều tần số có khớp với hai kích thước nhóm này hay không. Nếu c1 và c2 chính xác là hai giá trị này theo thứ tự bất kỳ thì tồn tại một sự sắp xếp xen kẽ hợp lệ. 
6. Nếu không, hãy trả lại rằng không thể có sự sắp xếp hợp lệ. 

Lý do đằng sau các bước này xuất phát từ đẳng thức bắt buộc b[i] = b[i+2]. Điều này sẽ thu gọn bất kỳ cấu hình hợp lệ nào thành một vị trí có hai màu nghiêm ngặt và quyền tự do duy nhất còn lại là gán giá trị nào cho các chỉ số lẻ và giá trị nào cho các chỉ số chẵn. 

### Tại sao nó hoạt động 

Điều kiện tổng liền kề buộc mọi phần tử thứ hai phải bằng nhau vì các tổng bằng nhau sẽ loại bỏ số hạng ở giữa khi trừ các ràng buộc liên tiếp. Điều này tạo ra một cấu trúc tuần hoàn cứng nhắc của giai đoạn hai. Khi đã biết cấu trúc này, vấn đề sẽ hoàn toàn giảm xuống việc kiểm tra xem liệu nhiều tập hợp đã cho có thể được chia thành hai nhóm khớp với số lượng vị trí cố định của mẫu xen kẽ đó hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1
    
    if len(freq) == 1:
        print("Yes")
        return
    
    if len(freq) > 2:
        print("No")
        return
    
    vals = list(freq.values())
    vals.sort()
    
    n1 = n // 2
    n2 = n - n1
    
    if (vals[0] == n1 and vals[1] == n2) or (vals[0] == n2 and vals[1] == n1):
        print("Yes")
    else:
        print("No")

t = int(input())
for _ in range(t):
    solve()
```Giải pháp nén mảng thành bản đồ tần số, đây là phép biến đổi quan trọng thay thế lý luận hoán vị bằng lý luận đếm. Các trường hợp đặc biệt cho một và nhiều hơn hai giá trị riêng biệt mã hóa trực tiếp ràng buộc cấu trúc xuất phát trước đó. Kiểm tra cuối cùng sẽ căn chỉnh tần số với sự phân chia chẵn lẻ bắt buộc của các chỉ số. 

Một cạm bẫy triển khai phổ biến là quên rằng hai giá trị có thể được gán cho một trong hai lớp chẵn lẻ. Đây là lý do tại sao cả hai bài tập đều được kiểm tra đối xứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 2
```| Bước | Giá trị riêng biệt | Tần số | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | {1, 2} | 1:2, 2:1 | Hai giá trị riêng biệt | 
| Chia kích thước | - | n1 = 1, n2 = 2 | Cần chia 1 và 2 | 
| Kiểm tra | - | {1,2} trận đấu | Có | 

Điều này chứng tỏ rằng mặc dù mảng chưa xen kẽ nhưng nó có thể được sắp xếp lại thành 1, 2, 1, thỏa mãn các tổng liền kề không đổi. 

### Ví dụ 2 

đầu vào:```
4
1 1 4 5
```| Bước | Giá trị riêng biệt | Tần số | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | {1,4,5} | 1:2, 4:1, 5:1 | Nhiều hơn hai giá trị | 
| Quy tắc | - | - | Không thể | 

Điều này cho thấy tại sao việc có ba giá trị riêng biệt sẽ phá vỡ cấu trúc xen kẽ, vì chuỗi giai đoạn hai không thể chứa ký hiệu thứ ba. 

Ví dụ thứ hai xác nhận rằng thuật toán loại bỏ các trường hợp trong đó các ràng buộc về cấu trúc bị vi phạm ngay cả khi việc sắp xếp lại cục bộ có vẻ hợp lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần để tạo tần số | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa n khóa riêng biệt | 

Cho rằng n tối đa là 100 cho mỗi trường hợp thử nghiệm, giải pháp này nằm trong giới hạn thoải mái ngay cả với số lượng trường hợp thử nghiệm tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    
    t = int(input())
    
    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        
        if len(freq) == 1:
            out.append("Yes")
            return
        
        if len(freq) > 2:
            out.append("No")
            return
        
        vals = list(freq.values())
        vals.sort()
        
        n1 = n // 2
        n2 = n - n1
        
        if (vals[0] == n1 and vals[1] == n2) or (vals[0] == n2 and vals[1] == n1):
            out.append("Yes")
        else:
            out.append("No")
    
    for _ in range(t):
        solve()
    
    return "\n".join(out)

# provided samples
assert run("""5
2
8 9
3
1 1 2
4
1 1 4 5
5
2 3 3 3 3
4
100000 100000 100000 100000
""") == """Yes
Yes
No
No
Yes"""

# custom cases
assert run("""3
2
1 2
3
7 7 7
4
1 2 3 4
""") == """Yes
Yes
No"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 1 2 | Có | Tối thiểu n=2 luôn hoạt động | 
| 3, 7 7 7 | Có | Tất cả các giá trị bằng nhau | 
| 4, 1 2 3 4 | Không | Nhiều hơn hai giá trị riêng biệt | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng có đúng hai phần tử. Bất kỳ thứ tự nào của hai số đều thỏa mãn điều kiện vì chỉ có một tổng liền kề. Thuật toán xử lý việc này một cách ngầm định vì kích thước tần số khớp với mức phân chia yêu cầu là 1 và 1. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều giống hệt nhau. Trong tình huống đó, bản đồ tần số có kích thước bằng một và thuật toán ngay lập tức trả về thành công. Điều này phù hợp với thực tế là bất kỳ hoán vị nào cũng bảo toàn tổng kề không đổi. 

Một trường hợp tinh tế hơn nữa xảy ra khi có chính xác hai giá trị riêng biệt nhưng số lượng bị mất cân bằng theo cách không khớp với sự phân chia chẵn lẻ. Ví dụ: trong một mảng có độ dài 5, nếu số đếm là 4 và 1 thì không có cách sắp xếp xen kẽ nào có thể phân phối chính xác các giá trị vào các vị trí 1,3,5 và 2,4. Thuật toán từ chối điều này vì cặp tần số được sắp xếp không khớp với 3 và 2. 

Những trường hợp này xác nhận rằng điều kiện dựa trên tần số nắm bắt đầy đủ tất cả các ràng buộc về cấu trúc được áp đặt bởi yêu cầu tổng kề.
