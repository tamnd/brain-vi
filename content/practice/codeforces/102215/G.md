---
title: "CF 102215G - Akinator"
description: "Hãy coi chiến lược đặt câu hỏi như một cây quyết định nhị phân. Mỗi nút bên trong là một câu hỏi, hai cạnh đi ra của nó tương ứng với các câu trả lời \"Có\" và \"Không\", và mỗi lá xác định chính xác một người."
date: "2026-08-18T22:02:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "G"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 451
verified: true
draft: false
---

[CF 102215G - Akinator](https://codeforces.com/problemset/problem/102215/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 31 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hãy coi chiến lược đặt câu hỏi như một cây quyết định nhị phân. Mỗi nút bên trong là một câu hỏi, hai cạnh đi ra của nó tương ứng với các câu trả lời "Có" và "Không", và mỗi lá xác định chính xác một người. Bởi vì một câu hỏi có thể chứa bất kỳ tập hợp con nào của những người hiện có thể có, nên bất kỳ phân vùng nhị phân nào của các ứng cử viên đều có thể được sử dụng tại một nút. Vì vậy, trò chơi gốc chính xác là vấn đề xây dựng cây tiền tố nhị phân. 

Nếu người (i) đi sâu vào (l_i), Akinator sẽ hỏi chính xác (l_i) câu hỏi khi người đó được chọn. Vì (p_i=a_i/\sum a_j), số câu hỏi dự kiến là 

[ 
\frac{\sum_i a_i l_i}{\sum_i a_i}. 
] 

Mẫu số là cố định nên mục tiêu tối ưu hóa thực sự là số nguyên 

[ 
\sum_i a_i l_i. 
] 

Cây phải có độ sâu tối đa nhiều nhất (k). Cây nhị phân có (n) lá và chiều cao tối đa (k) tồn tại chính xác khi (n\le 2^k). Nếu điều này không thành công thì không có chiến lược đặt câu hỏi nào có thể phân biệt được tất cả mọi người trong số lượng câu hỏi cho phép. Điều kiện tương tự là giới hạn Kraft cho mã tiền tố nhị phân. 

Các ràng buộc nhỏ về (n), nhưng chúng không cho phép liệt kê các cây hoặc tập hợp con. Với (n\le100), thậm chí (O(n^3)) là vô hại, trong khi bất kỳ số mũ nào trong (n) là hoàn toàn không khả thi. Xác suất được biểu thị bằng các số nguyên lớn tới (10^{12}), do đó việc triển khai sẽ hoạt động hoàn toàn với các số nguyên chính xác thay vì dấu phẩy động. 

Có một số trường hợp ranh giới có thể đánh lừa việc triển khai ngây thơ. Với một người có thể, không cần câu hỏi nào cả. Ví dụ,```
1 1
1000000000000
```có câu trả lời`0/1`. Một giải pháp giả định rằng mỗi người cần ít nhất một câu hỏi sẽ đưa ra một giá trị dương không chính xác. 

Giới hạn độ sâu có thể khiến việc xây dựng Huffman bình thường trở nên bất khả thi. Ví dụ,```
3 1
1 2 3
```có đầu ra`No solution`, bởi vì một câu hỏi nhị phân chỉ có hai chuỗi câu trả lời có thể có. Chạy mã hóa Huffman thông thường mà không kiểm tra giới hạn chiều cao có thể âm thầm tạo ra cây có độ sâu hai. 

Ranh giới ngược lại là khi (n=2^k). Khi đó mỗi chiếc lá phải có độ sâu chính xác (k), bất kể trọng lượng. Ví dụ,```
4 2
1 2 3 4
```có chi phí gia quyền (2(1+2+3+4)=20), vì vậy câu trả lời là`2/1`. Một chiến lược cố gắng cung cấp cho một người một mã ngắn hơn không thể làm như vậy mà không khiến người khác hiểu sâu hơn hai câu hỏi. 

Cuối cùng, trọng số bằng nhau rất hữu ích cho việc phát hiện lỗi đặt hàng. Với```
3 2
1 1 1
```độ sâu tối ưu là (1,2,2), đưa ra tổng chi phí (5) và câu trả lời`5/3`. Người thường xuyên nhất ở đây không phải là người đặc biệt, vì vậy mọi cách xử lý cà vạt đúng cách vẫn phải tạo ra tổng số như nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi cây quyết định nhị phân có thể. Tại một tập hợp (S) gồm những người hiện có thể có, một câu hỏi sẽ chọn một tập hợp con đúng khác trống (A\tập hợp con S), với (A) trở thành nhánh Có và (S\setminus A) trở thành nhánh Không. Một chương trình động đệ quy trên các tập hợp con có thể ghi nhớ chi phí tốt nhất cho mỗi tập hợp con và độ sâu còn lại. 

Điều này đúng vì mọi chiến lược đặt câu hỏi khả thi đều có chính xác phân vùng đầu tiên như vậy và hai bài toán con thu được sẽ độc lập sau câu hỏi đầu tiên. Thật không may, có (2^n) tập hợp con và việc xem xét tất cả các phần tách có thể mang lại kết quả theo cấp số nhân. Trên tất cả các tập hợp con, số lượng phân chia không tầm thường có thứ tự là 

[ 
\sum_{S\ne\varnothing} (2^{|S|}-2) 
=3^n-2^{n+1}+1. 
] 

Với (n=100), (3^{100}) là khoảng (5,15\cdot10^{47}), vì vậy cách tiếp cận này không khả thi chút nào. 

Quan sát hữu ích là chúng ta không thực sự quan tâm đến việc người nào truy cập chính xác chuỗi nhị phân nào. Chúng tôi chỉ quan tâm đến độ sâu (l_i). Một tập hợp độ sâu có thể được thực hiện bằng cây tiền tố nhị phân một cách chính xác khi nó thỏa mãn bất đẳng thức Kraft 

[ 
\sum_i2^{-l_i}\le1. 
] 

Đối với cây tối ưu, đẳng thức được giữ nguyên vì nếu tổng nhỏ hơn chúng ta có thể rút ngắn một số mã mà không vi phạm ràng buộc. Do đó, bài toán trở thành bài toán mã hóa Huffman có giới hạn độ dài: cực tiểu hóa (\sum a_i l_i) theo (1\le l_i\le k) và (\sum 2^{-l_i}=1). 

Có một cách đặc biệt rõ ràng để giải quyết vấn đề Huffman bị ràng buộc này. Hãy tưởng tượng rằng ban đầu mọi người đều có độ dài bằng không. Tổng Kraft khi đó là (n). Việc tăng chiều dài của người (i) từ (l-1) lên (l) sẽ giảm tổng Kraft xuống (2^{-l}) và chi phí chính xác là (a_i). Chúng ta cần thực hiện tăng chiều dài có tổng mức giảm Kraft là (n-1). 

Điều này biến bài toán thành bài toán thu thập tiền nhị phân. Với mỗi người (i), tạo ra (k) đồng xu có mệnh giá 

[ 
2^{-1},2^{-2},\ldots,2^{-k}, 
] 

và đưa ra giá trị cho mỗi đồng tiền đó (a_i). Chúng ta cần một bộ sưu tập có giá trị tối thiểu có tổng mệnh giá là (n-1). Nếu đồng xu tương ứng với cấp độ (l) được chọn, điều đó thể hiện việc tăng độ dài mã của người đó thông qua cấp độ (l). Đây là mức giảm tiêu chuẩn đằng sau việc hợp nhất gói để mã hóa Huffman có giới hạn độ dài. 

Bởi vì tất cả các mệnh giá đều là lũy thừa của hai nên bài toán đồng xu có một giải pháp tham lam. Ở mệnh giá nhỏ nhất, bất kỳ đồng xu nào được chọn đều phải xuất hiện theo cặp, vì mỗi mệnh giá lớn hơn sẽ lớn gấp đôi. Cặp rẻ nhất có thể được hình thành từ hai đồng tiền rẻ nhất hiện có. Cặp đó sau đó có thể được coi là một đồng tiền có mệnh giá tiếp theo. Nếu còn lại một đồng xu, nó không bao giờ có thể tham gia vào giải pháp chính xác, vì vậy đồng xu còn lại đắt nhất có thể bị loại bỏ. Việc lặp lại quá trình này sẽ tạo ra thuật toán hợp nhất gói. 

Lực lượng vũ phu hoạt động vì nó khám phá rõ ràng tất cả các phân vùng nhị phân. Quan sát hợp nhất gói cho phép chúng tôi thay thế tất cả các cây đó bằng (k) danh sách được sắp xếp chứa các gói có liên quan rẻ nhất. Vì (n,k\le100), ngay cả việc triển khai (O(nk)) đơn giản cũng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^n)) | (O(2^n)) | Quá chậm | 
| Hợp nhất gói tối ưu | (O(nk)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các trọng số (a_i) theo thứ tự không giảm. Trọng lượng bằng nhau có thể được đặt theo thứ tự bất kỳ. Thuật toán hợp nhất gói luôn cần các mục theo thứ tự giá trị tăng dần và một gói được hình thành từ hai mục được sắp xếp liền kề có giá trị không nhỏ hơn gói trước đó. 
2. Kiểm tra xem (n\le2^k). Nếu không, không thể có (n) lá khác nhau trong phạm vi độ sâu (k), vì vậy hãy in`No solution`. 
3. Coi trọng số đã sắp xếp là danh sách ban đầu của các đồng tiền có mệnh giá (2^{-k}). Đây là những đồng tiền nhỏ nhất nên chúng là những đồng tiền đầu tiên có thể được ghép đôi. 
4. Lặp lại quy trình sau (k-1) lần. Ghép nối các phần tử liên tiếp của danh sách được sắp xếp hiện tại, bắt đầu với hai phần tử rẻ nhất. Mỗi cặp trở thành một gói có giá trị là tổng của hai phần tử của nó. Sau đó hợp nhất danh sách gói hàng với một bản sao khác của trọng lượng ban đầu và sắp xếp kết quả. 

Trọng lượng ban đầu đại diện cho những đồng xu thông thường có mệnh giá mới, lớn gấp đôi. Các gói này đại diện cho hai đồng xu nhỏ hơn đã được kết hợp thành cùng một mệnh giá. Giữ cả hai lựa chọn trong một danh sách được sắp xếp là điều cho phép các quyết định sau này lựa chọn giữa một đồng xu đắt tiền và một gói rẻ hơn. 
5. Sau các vòng (k-1) đó, hãy ghép các phần tử liên tiếp lần cuối cùng nhưng không hợp nhất các gói kết quả với trọng số ban đầu. Các gói này hiện có mệnh giá (1). 
6. Chọn (n-1) gói cuối cùng rẻ nhất và cộng giá trị của chúng. Tổng này là số lượng câu hỏi có trọng số tối thiểu có thể có. 
7. Chia số nguyên thu được cho (S=\sum a_i). Tính ước số chung lớn nhất của tử số và (S), chia cả hai cho nó và in phân số rút gọn. 

### Tại sao nó hoạt động 

Gọi (l_i) là độ sâu cuối cùng được gán cho người (i). Bắt đầu với mọi (l_i=0), tổng Kraft là (n). Việc tăng (l_i) từ (l_i-1) lên (l_i) sẽ giảm tổng đó đi (2^{-l_i}), trong khi tăng mục tiêu lên (a_i). Để đạt được cây nhị phân hoàn chỉnh hợp lệ, tổng Kraft phải chính xác bằng (1), do đó các mức giảm được chọn phải có tổng giá trị (n-1). 

Cấu trúc hợp nhất gói giải quyết chính xác vấn đề lựa chọn giá trị tối thiểu này. Ở mỗi mệnh giá, một nghiệm hợp lệ chỉ có thể sử dụng mệnh giá nhỏ nhất một số lần chẵn. Nếu nó sử dụng (2r) những đồng tiền như vậy, thì việc sử dụng (2r) những đồng tiền rẻ nhất luôn là tối ưu và những đồng tiền đó có thể được nhóm thành (r) cặp liên tiếp. Việc thay thế mỗi cặp bằng một gói sẽ giữ nguyên cả mệnh giá và tổng giá trị của nó. Do đó, việc giải quyết lớp có mệnh giá nhỏ hơn một cách tham lam sẽ tạo ra chính xác tập hợp các lựa chọn cần thiết cho lớp tiếp theo. 

Ở lớp cuối cùng, mọi mục được chọn đều có mệnh giá (1), do đó việc chọn (n-1) trong số chúng sẽ có tổng mệnh giá (n-1). Việc mở rộng các gói sẽ mang lại một đồng xu được chọn cho mỗi lần tăng đơn vị của mỗi độ dài mã, do đó tổng giá trị gói chính xác là (\sum_i a_i l_i). Vì việc hợp nhất gói giảm thiểu giá trị đó nên mã kết quả có số lượng câu hỏi dự kiến ​​​​tối thiểu. Độ dài kết quả thỏa mãn đẳng thức Kraft và tối đa là (k), do đó, cây tiền tố nhị phân nhận ra chúng tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def merge_sorted(a, b):
    """Merge two already sorted lists."""
    res = []
    i = 0
    j = 0

    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            res.append(a[i])
            i += 1
        else:
            res.append(b[j])
            j += 1

    if i < len(a):
        res.extend(a[i:])
    if j < len(b):
        res.extend(b[j:])

    return res

def solve():
    n, k = map(int, input().split())
    weights = list(map(int, input().split()))
    weights.sort()

    if n > (1 << k):
        print("No solution")
        return

    current = weights[:]

    # Move from denomination 2^(-k) up to denomination 2^(-1).
    # At every intermediate level, packages are merged with
    # the original coins of the new denomination.
    for _ in range(k - 1):
        packages = []
        for i in range(0, len(current) - 1, 2):
            packages.append(current[i] + current[i + 1])

        current = merge_sorted(weights, packages)

    # One final packaging step creates denomination-1 items.
    final_packages = []
    for i in range(0, len(current) - 1, 2):
        final_packages.append(current[i] + current[i + 1])

    # final_packages is already sorted.
    numerator = sum(final_packages[:n - 1])
    denominator = sum(weights)

    g = __import__("math").gcd(numerator, denominator)
    numerator //= g
    denominator //= g

    print(f"{numerator}/{denominator}")

if __name__ == "__main__":
    solve()
```Các trọng số đầu vào được sắp xếp một lần ngay từ đầu. Việc sắp xếp là đủ vì các giá trị gói được hình thành bằng cách thêm các phần tử liền kề của danh sách đã sắp xếp, do đó danh sách gói kết quả cũng được sắp xếp. 

các`merge_sorted`Hàm khai thác thuộc tính này. Nó hợp nhất (n) trọng số ban đầu với danh sách gói hàng hiện tại theo thời gian tuyến tính. Việc sắp xếp lại toàn bộ danh sách ở mọi cấp độ vẫn sẽ diễn ra thoải mái đối với (n,k\le100), nhưng việc hợp nhất làm cho độ phức tạp dự định (O(nk)) trở nên rõ ràng. 

Vòng lặp chỉ chạy qua các cấp độ (k-1) đầu tiên. Cấp độ cuối cùng được xử lý riêng biệt vì các gói cuối cùng có mệnh giá (1) và không tồn tại đồng tiền gốc có mệnh giá (1). Việc trộn các trọng số ban đầu vào danh sách cuối cùng đó sẽ tạo ra từng lỗi nhỏ trong cấu trúc mệnh giá. 

Phạm vi được sử dụng khi tạo cặp là`range(0, len(current) - 1, 2)`. Nếu danh sách hiện tại có số phần tử lẻ thì phần tử cuối cùng của nó sẽ không được sử dụng. Vì danh sách đã được sắp xếp nên phần tử đó là phần tử đắt nhất, chính xác là phần tử mà việc hợp nhất gói sẽ loại bỏ. 

Việc kiểm tra tính khả thi sử dụng`1 << k`, do đó không có logarit dấu phẩy động và không có vấn đề làm tròn. Số nguyên Python cũng xử lý các giá trị trung gian lớn nhất có thể một cách an toàn. Chi phí có trọng số phù hợp lớn nhất nằm ở mức (n^2\cdot10^{12}), thấp hơn nhiều so với mức mà các số nguyên có độ chính xác tùy ý của Python có thể xử lý một cách thoải mái. 

Danh sách gói cuối cùng đã được sắp xếp vì`current`được sắp xếp và tổng của mỗi cặp liên tiếp không giảm. Do đó (n-1) gói rẻ nhất chỉ đơn giản là phần tử (n-1) đầu tiên của nó. 

Đối với (n=1), lát cắt đó trống, tử số bằng 0. Mẫu số là dương nên đáp án rút gọn là đúng`0/1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4 1
8 1 9 2
```Có bốn người nhưng chỉ có một câu hỏi. Một câu hỏi nhị phân chỉ có hai chuỗi câu trả lời có thể có, vì vậy không thể phân biệt được bốn người khác nhau. 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 4 | 
| (k) | 1 | 
| (2^k) | 2 | 
| Khả thi? | Không | 

Thuật toán dừng trước khi xây dựng bất kỳ gói và bản in nào`No solution`. Điều này khẳng định rằng việc kiểm tra dung lượng là cần thiết và cũng tránh việc xử lý gói vô nghĩa khi cây không thể tồn tại. 

### Mẫu 2 

Đầu vào là```
4 2
1 2 3 4
```Các trọng số đã được sắp xếp đã có`[1, 2, 3, 4]`. 

| Sân khấu | Danh sách hiện tại | Bao bì sản xuất | 
| --- | --- | --- | 
| Ban đầu |`[1, 2, 3, 4]`|`[3, 7]`| 
| Sau khi hợp nhất |`[1, 2, 3, 3, 4, 7]`|`[3, 6, 11]`| 
| Lựa chọn cuối cùng |`[3, 6, 11]`|`3 + 6 + 11 = 20`| 

Có (n-1=3) gói cuối cùng nên cả ba gói đều được chọn. Chi phí có trọng số là (20), trong khi tổng trọng lượng là (1+2+3+4=10). Phân số rút gọn là (20/10=2/1). 

Độ dài tối ưu tương ứng là (2,2,2,2). Tổng Kraft là (4\cdot2^{-2}=1) và mỗi người đều được tìm thấy trong đúng hai câu hỏi. 

### Mẫu 3 

cho```
4 3
1 2 3 4
```danh sách trung gian đầu tiên là```
[1, 2, 3, 3, 4, 7]
```Bước đóng gói tiếp theo đưa ra`[3, 6, 11]`, được hợp nhất với các trọng số ban đầu:```
[1, 2, 3, 3, 4, 6, 7, 11]
```Các gói cuối cùng sau đó`[3, 6, 10, 18]`. 

| Sân khấu | Danh sách hiện tại | Gói | 
| --- | --- | --- | 
| Ban đầu |`[1, 2, 3, 4]`|`[3, 7]`| 
| Cấp 2 |`[1, 2, 3, 3, 4, 7]`|`[3, 6, 11]`| 
| Cấp 3 |`[1, 2, 3, 3, 4, 6, 7, 11]`|`[3, 6, 10, 18]`| 
| Chọn 3 giá rẻ nhất |`[3, 6, 10]`|`19`| 

Chi phí có trọng số là (19) và tổng trọng lượng là (10), vì vậy câu trả lời là`19/10`. 

Các gói đã chọn tương ứng với độ dài (3,3,3,3) cho trọng lượng (1,2,3) và độ dài ngắn hơn cho trọng lượng (4). Chiến lược thu được có thể hỏi về người 4 trước, sau đó đến người 3 nếu cần, và cuối cùng phân biệt người 1 và 2, khớp chính xác với cấu trúc tối ưu được mô tả bởi mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nk)) | Mỗi cấp độ (k) tạo ra các gói (O(n)) và hợp nhất hai danh sách các phần tử (O(n)) đã được sắp xếp. | 
| Không gian | (O(n)) | Chỉ trọng lượng ban đầu, danh sách hiện tại và một danh sách gói hàng được lưu trữ. | 

Với (n,k\le100), thuật toán chỉ thực hiện khoảng (10^4) hoạt động ở cấp danh sách cho đến các hệ số không đổi. Các giá trị số nguyên cũng đủ nhỏ để số học có độ chính xác tùy ý không tốn kém, do đó, giải pháp vừa vặn thoải mái trong giới hạn 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def merge_sorted(a, b):
    res = []
    i = 0
    j = 0

    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            res.append(a[i])
            i += 1
        else:
            res.append(b[j])
            j += 1

    res.extend(a[i:])
    res.extend(b[j:])
    return res

def solve_text(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        n, k = map(int, input().split())
        weights = list(map(int, input().split()))
        weights.sort()

        if n > (1 << k):
            return "No solution"

        current = weights[:]

        for _ in range(k - 1):
            packages = [
                current[i] + current[i + 1]
                for i in range(0, len(current) - 1, 2)
            ]
            current = merge_sorted(weights, packages)

        final_packages = [
            current[i] + current[i + 1]
            for i in range(0, len(current) - 1, 2)
        ]

        numerator = sum(final_packages[:n - 1])
        denominator = sum(weights)

        g = math.gcd(numerator, denominator)
        return f"{numerator // g}/{denominator // g}"
    finally:
        sys.stdin = old_stdin

def run(inp: str) -> str:
    return solve_text(inp)

# Provided samples
assert run("4 1\n8 1 9 2\n") == "No solution", "sample 1"
assert run("4 2\n1 2 3 4\n") == "2/1", "sample 2"
assert run("4 3\n1 2 3 4\n") == "19/10", "sample 3"

# Minimum-size input
assert run("1 1\n1000000000000\n") == "0/1", "single person needs no questions"

# Boundary where exactly 2^k people fit
assert run("4 2\n1 1 1 1\n") == "2/1", "all leaves must have depth 2"

# Smallest impossible case
assert run("3 1\n1 2 3\n") == "No solution", "three people cannot fit at depth 1"

# All equal weights with a non-complete power of two
assert run("3 2\n1 1 1\n") == "5/3", "optimal lengths are 1, 2, 2"

# Maximum-size case
assert run("100 100\n" + " ".join(["1"] * 100) + "\n") == "168/25", \
    "100 equal weights require total length 672"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1000000000000`|`0/1`| Trường hợp kích thước tối thiểu và câu trả lời không có câu hỏi | 
|`4 2 / 1 1 1 1`|`2/1`| Ranh giới chính xác (n=2^k) | 
|`3 1 / 1 2 3`|`No solution`| Kiểm tra công suất và ranh giới độ sâu | 
|`3 2 / 1 1 1`|`5/3`| Độ sâu tối ưu không đồng đều và trọng lượng bằng nhau | 
|`100 100 / 100 ones`|`168/25`| Số học số nguyên tối đa (n), lớn (k) và số nguyên chính xác | 

## Vỏ cạnh 

Đối với một người, đầu vào```
1 1
1000000000000
```vượt qua bài kiểm tra năng lực. Danh sách ban đầu chứa một giá trị và không có cặp nào để tạo thành. Danh sách gói cuối cùng trống, vì vậy việc chọn (n-1=0) gói rẻ nhất sẽ không tốn phí. Mẫu số là dương, cho`0/1`. Điều này phù hợp với thực tế là Akinator đã biết người đó là ai. 

Đối với quá nhiều người, hãy xem xét```
3 1
1 2 3
```Thuật toán kiểm tra (3>2^1) ngay lập tức. Không có cây nhị phân có chiều cao nào có thể có ba lá, vì vậy nó in ra`No solution`. Điều này ngăn danh sách gói cuối cùng không đầy đủ bị nhầm lẫn với mã hợp lệ. 

Đối với ranh giới công suất chính xác,```
4 2
1 1 1 1
```bốn lá phải chiếm cả bốn vị trí ở độ sâu hai. Bao bì đầu tiên tạo ra hai gói giá trị (2), bao bì cuối cùng chỉ tạo một gói giá trị (4) sau khi danh sách trung gian được hình thành và kết quả là chi phí được chọn là (8). Chia cho tổng trọng lượng (4) được`2/1`. Không có chỗ cho từ mã ngắn hơn vì việc rút ngắn một lá sẽ buộc một lá khác vượt quá độ sâu hai. 

Để có trọng lượng bằng nhau với ba người,```
3 2
1 1 1
```danh sách gói đầu tiên là`[2]`, được hợp nhất với các trọng số ban đầu để thu được`[1,1,1,2]`. Các gói cuối cùng được`[2,3]`, cả hai đều phải được chọn. Tổng giá trị của chúng là (5), trong khi tổng trọng số xác suất là (3), cho`5/3`. Các độ dài tương ứng là (1,2,2), có tổng Kraft là (1/2+1/4+1/4=1). 

Đối với kích thước đầu vào lớn nhất,```
100 100
1 1 1 ... 1
```với một trăm trọng lượng bằng một, giới hạn độ sâu hào phóng không bị ràng buộc. Cây nhị phân có trọng số bằng nhau tối ưu có 28 lá ở độ sâu 6 và 72 lá ở độ sâu 7, cho 

[ 
28\cdot6+72\cdot7=672. 
] 

Số câu hỏi dự kiến là (672/100=168/25). Thử nghiệm xác nhận rằng việc xây dựng gói tiếp tục chính xác ở nhiều cấp độ và phần cuối cùng được giảm chính xác.
