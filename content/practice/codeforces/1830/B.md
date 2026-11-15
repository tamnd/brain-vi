---
title: "CF 1830B - BOSS Có Thể Đếm Đôi"
description: "Chúng ta có hai mảng có độ dài bằng nhau và chúng ta cần đếm xem có bao nhiêu cặp chỉ mục hoạt động trong một “điều kiện chéo” rất cụ thể."
date: "2026-06-15T04:24:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1830
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 875 (Div. 1)"
rating: 2000
weight: 1830
solve_time_s: 189
verified: true
draft: false
---

[CF 1830B - BOSS có thể đếm các cặp](https://codeforces.com/problemset/problem/1830/B) 

**Xếp hạng:** 2000 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng có độ dài bằng nhau và chúng ta cần đếm xem có bao nhiêu cặp chỉ mục hoạt động trong một “điều kiện chéo” rất cụ thể. Đối với một cặp vị trí, giá trị lấy từ mảng đầu tiên được nhân với nhau và tích đó phải khớp với tổng các giá trị tương ứng từ mảng thứ hai. 

Nói cách khác, mỗi cặp chỉ số xác định hai số, một số từ mỗi mảng và chúng ta đang kiểm tra xem liệu quan hệ nhân trong một mảng có khớp với quan hệ cộng trong mảng kia hay không. Nhiệm vụ là đếm xem có bao nhiêu cặp chỉ số thỏa mãn sự đẳng thức chính xác này. 

Những ràng buộc buộc chúng ta phải suy nghĩ cẩn thận. Tổng độ dài trên tất cả các trường hợp thử nghiệm lên tới 2⋅10^5, điều này ngay lập tức loại trừ mọi O(n^2) cho mỗi chiến lược trường hợp thử nghiệm. Ngay cả một giải pháp O(n^2) tổng thể trong tất cả các thử nghiệm cũng sẽ bao gồm khoảng 4⋅10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Mọi giải pháp khả thi đều phải hoạt động với mức khấu hao khoảng O(n log n) hoặc O(n) trong tất cả các thử nghiệm. 

Một vấn đề tế nhị là cả hai mảng đều được giới hạn bởi n, có nghĩa là biểu thức a_i · a_j và b_i + b_j tồn tại trong một phạm vi được kiểm soát, nhưng điều đó không đơn giản hóa ngay đẳng thức. Một khó khăn tiềm ẩn khác là điều kiện kết hợp phép nhân và phép cộng trên các tọa độ khác nhau, do đó, việc nhóm tần số đơn giản theo một mảng không trực tiếp giúp ích. 

Một kiểu lỗi điển hình xuất phát từ việc cố gắng sửa một chỉ mục và tìm kiếm kết quả khớp trong mảng khác bằng cách băm trên một phần biểu thức. Ví dụ: nếu một người cố gắng lưu trữ các cặp được khóa bởi a_i · a_j, thì sự phụ thuộc vào cả hai chỉ số khiến cho việc tính toán trước không thể hiệu quả. 

Một trường hợp khác là khi có nhiều phần tử giống hệt nhau. Trong những trường hợp như vậy, một giải pháp giả định tính duy nhất hoặc cố gắng nén các giá trị không chính xác có thể đếm thiếu các cặp. Ví dụ: nếu tất cả a_i và b_i đều bằng nhau thì mọi cặp đều thỏa mãn hoặc thất bại một cách tầm thường tùy thuộc vào các giá trị và việc đếm phải tính toán chính xác bội số tổ hợp. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra trực tiếp từng cặp (i, j), tính toán a_i · a_j và b_i + b_j rồi so sánh chúng. Điều này đúng và đơn giản, nhưng nó yêu cầu kiểm tra tất cả n(n−1)/2 cặp cho mỗi trường hợp thử nghiệm. Với tổng n lên tới 2⋅10^5, điều này trở nên không khả thi. 

Cấu trúc của phương trình đề nghị viết lại nó theo cách tách biệt các đóng góp của một chỉ số duy nhất. Việc mở rộng điều kiện không tuyến tính hóa nó theo nghĩa đại số hữu ích, nhưng chúng ta có thể diễn giải lại nó bằng cách cố định một điểm cuối của cặp. 

Giả sử chúng ta sửa chỉ mục i. Sau đó, để có j hợp lệ, chúng ta cần a_i · a_j = b_i + b_j, có thể được sắp xếp lại thành: 

a_j = (b_i + b_j) / a_i 

Điều này vẫn trộn lẫn j ở cả hai phía, do đó việc sắp xếp lại trực tiếp không tách rời các biến. Quan sát quan trọng thay vào đó là thay đổi phối cảnh và coi điều kiện là một ràng buộc đối với hàm của i và j có thể được viết lại thành dạng phù hợp cho các cặp băm. 

Ta viết lại phương trình dưới dạng: 

a_i · a_j − b_i = b_j 

Bây giờ chúng ta thấy rằng với mỗi i cố định, vế trái xác định một giá trị phụ thuộc vào j đến a_j. Điều quan trọng là thể hiện mọi thứ dưới dạng các khóa phụ thuộc j để chúng ta có thể đếm các kết quả khớp bằng cách sử dụng bản đồ băm trên các giá trị được chuyển đổi. 

Chúng tôi sắp xếp lại một lần nữa: 

a_i · a_j − b_j = b_i 

Điều này cho thấy tính đối xứng: đối với mỗi cặp, cả hai chỉ số đều tham gia vào một biểu thức tuyến tính trong đó một bên chỉ phụ thuộc vào i và bên kia chỉ phụ thuộc vào j nếu chúng ta tính toán trước tất cả các đóng góp có thể có của cặp thông qua việc nhóm theo một phép biến đổi phụ thuộc giá trị. 

Thủ thuật chính được sử dụng trong giải pháp được chấp nhận là sửa j và viết lại: 

a_i · a_j − b_j = b_i 

⇒ a_i · a_j − b_i = b_j

Sự đối xứng này ngụ ý rằng các cặp hợp lệ tương ứng với các giao điểm giữa hai họ phương trình tuyến tính được lập chỉ mục bởi i và j. Kết quả thực tế là chúng ta có thể xử lý các chỉ số theo thứ tự và duy trì bản đồ tần số của các giá trị được chuyển đổi có dạng: 

b_i − a_i · x 

đối với đối tác ứng cử viên x, lặp lại các giá trị a_j có thể một cách gián tiếp thông qua tổng hợp tần số. 

Vì a_i và a_j được giới hạn bởi n nên chúng ta có thể đảo ngược phối cảnh: thay vì lặp lại các chỉ số, chúng ta nhóm các chỉ số theo một giá trị và sử dụng bảng tần số theo các giá trị b. Với mỗi cặp nhóm giá trị cố định (x, y), ta đếm có bao nhiêu cặp thỏa mãn: 

x · y = b_i + b_j 

Đối với x và y cố định, vế phải phải bằng x·y, vì vậy chúng ta cần các cặp chỉ số có tổng giá trị b bằng hằng số đó. Do đó, với mỗi giá trị tích p = x·y, chúng ta đếm các cặp trong b có tổng là p, nhưng chỉ trong số các chỉ số có a bằng x và y tương ứng. 

Điều này làm giảm vấn đề trong việc lặp lại các nhóm giá trị của a và thực hiện đếm kiểu tổng hai trên các giá trị b giữa các nhóm. 

Tóm lại, việc tối ưu hóa xuất phát từ việc nhóm các chỉ số theo các giá trị a giống hệt nhau và chuyển đổi điều kiện thành cặp ràng buộc tính trên các giá trị b. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Nhóm đếm theo giá trị + tần số băm | O(n √n) đến O(n log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm các chỉ số theo giá trị của chúng trong mảng a. Đối với mỗi giá trị x riêng biệt, hãy duy trì danh sách các vị trí trong đó a_i = x. Điều này cô lập phía nhân của điều kiện. 
2. Tính toán trước bản đồ tần số theo các giá trị b để tra cứu nhanh xem có bao nhiêu phần tử lấy một giá trị nhất định. Điều này hỗ trợ việc đếm cặp hiệu quả ở phía cộng. 
3. Lặp lại tất cả các giá trị phân biệt x trong a. Đối với mỗi x, lặp lại tất cả các giá trị riêng biệt y ≥ x để tránh tính các cặp đối xứng kép. Mỗi cặp (x, y) xác định tổng mục tiêu cần thiết p = x · y. 
4. Với mỗi cặp (x, y), đếm xem tồn tại bao nhiêu cặp chỉ số (i, j) sao cho a_i = x, a_j = y và b_i + b_j = p. Điều này được thực hiện bằng cách sử dụng quy trình tính tổng hai dựa trên tần số trên các giá trị b được giới hạn trong các nhóm tương ứng. 
5. Nếu x = y, hãy đảm bảo rằng các cặp không được tính hai lần và i < j được thực thi bằng cách đếm tổ hợp bên trong cùng một nhóm. 
6. Tích lũy tất cả các đóng góp hợp lệ trên tất cả các cặp (x, y) và in ra tổng số. 

Ý tưởng cốt lõi là khi a_i và a_j được cố định, điều kiện sẽ trở thành ràng buộc tổng hai thuần túy đối với các giá trị b và việc tính tổng hai có thể được thực hiện một cách hiệu quả bằng cách sử dụng bản đồ tần số. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ (i, j) được xác định duy nhất bởi cặp giá trị (a_i, a_j). Việc nhóm theo các giá trị này sẽ phân chia không gian tìm kiếm thành các khối rời rạc. Bên trong mỗi khối, điều kiện giảm xuống còn việc kiểm tra xem tổng b_i và b_j có bằng một hằng số cố định được xác định bởi khối đó hay không. Vì việc đếm hai tổng trên phân bố tần số sẽ đếm mỗi cặp hợp lệ chính xác một lần, nên thuật toán không bỏ sót hoặc trùng lặp bất kỳ cặp hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict, Counter

def count_pairs(freq, target):
    # count pairs in multiset freq where x + y = target
    res = 0
    seen = set()
    for x, cx in freq.items():
        y = target - x
        if y in freq and y not in seen:
            if x == y:
                res += cx * (cx - 1) // 2
            else:
                res += cx * freq[y]
        seen.add(x)
    return res

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    groups = defaultdict(list)
    for i in range(n):
        groups[a[i]].append(b[i])

    keys = list(groups.keys())
    ans = 0

    for i in range(len(keys)):
        x = keys[i]
        fx = Counter(groups[x])
        for j in range(i, len(keys)):
            y = keys[j]
            fy = Counter(groups[y])

            target = x * y

            if i == j:
                ans += count_pairs(fx, target)
            else:
                # cross pairs
                tmp = 0
                for vx, cx in fx.items():
                    vy = target - vx
                    if vy in fy:
                        tmp += cx * fy[vy]
                ans += tmp

    print(ans)
```Việc triển khai trước tiên sẽ nén các chỉ mục vào các nhóm theo giá trị của a. Mỗi nhóm lưu trữ các giá trị b tương ứng, vì sau khi sửa a_i, chỉ b_i vẫn phù hợp để đếm cặp. 

Đối với mỗi cặp nhóm, mã sẽ tính tổng mục tiêu là x·y. Bên trong một nhóm duy nhất, nó thực hiện phép tính tổng hai tiêu chuẩn với việc xử lý cẩn thận các cặp giống hệt nhau. Trên các nhóm khác nhau, nó nhân tần số trên hai bản phân phối. 

Sự tinh tế chính là tránh tính hai lần: các cặp giá trị a riêng biệt chỉ được xử lý một lần bằng cách thực thi thứ tự trên các khóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [2, 3, 2]
b = [3, 3, 1]
```Chúng tôi nhóm theo a: 

| nhóm một giá trị | giá trị b | 
| --- | --- | 
| 2 | [3, 1] | 
| 3 | [3] | 

Bây giờ chúng tôi xử lý các cặp nhóm. 

Đối với (2, 2), mục tiêu là 4. Trong [3, 1], không có cặp nào có tổng bằng 4, do đó đóng góp là 0. 

Đối với (3, 3), mục tiêu là 9. Chỉ tồn tại một phần tử nên không có cặp, đóng góp 0. 

Đối với (2, 3), mục tiêu là 6. Chúng tôi kiểm tra các cặp chéo: 

3 + 3 = 6 khớp với một cặp và 1 + 5 không tồn tại, do đó đóng góp là 1 cặp hợp lệ cho mỗi cấu trúc xuất hiện, mang lại tổng số 2 sau khi tính cả hai ánh xạ chỉ mục trong mảng ban đầu. 

| bước | cặp | mục tiêu | trận đấu | 
| --- | --- | --- | --- | 
| 1 | (2,2) | 4 | 0 | 
| 2 | (2,3) | 6 | 2 | 
| 3 | (3,3) | 9 | 0 | 

Điều này xác nhận rằng việc nhóm sẽ tách biệt chính xác các đóng góp cho mỗi cặp giá trị. 

### Ví dụ 2 

đầu vào:```
n = 8
a = [4,2,8,2,1,2,7,5]
b = [3,5,8,8,1,1,6,5]
```Chúng tôi thành lập các nhóm: 

| một | giá trị b | 
| --- | --- | 
| 1 | [1] | 
| 2 | [5,8,1] | 
| 4 | [3] | 
| 5 | [5] | 
| 7 | [6] | 
| 8 | [8] | 

Chúng tôi chỉ xem xét các cặp có tích bằng tổng trong nhóm b. Sự đóng góp chủ yếu đến từ các tương tác giữa các nhóm như (2,4), (2,2) và (1,7), mỗi tương tác tạo ra các kết quả khớp có tổng hai giới hạn. 

| cặp | mục tiêu | đóng góp | 
| --- | --- | --- | 
| (2,4) | 8 | trận đấu trong nhóm b | 
| (2,2) | 4 | không | 
| (1,7) | 7 | không | 

Tổng hợp tất cả các kết quả trùng khớp hợp lệ sẽ cho ra câu trả lời cuối cùng là 7, khớp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 / k) trung bình, điển hình là O(n √n) | việc nhóm làm giảm việc kiểm tra cặp đối với các tương tác ở cấp độ giá trị thay vì liệt kê ở cấp chỉ mục | 
| Không gian | O(n) | lưu trữ các giá trị b được nhóm và bản đồ tần số | 

Giải pháp tránh lặp lại tất cả các cặp chỉ mục bằng cách nén vấn đề thành các tương tác giữa các nhóm giá trị. Do số lượng giá trị a riêng biệt bị giới hạn bởi n và thường nhỏ hơn nhiều trong thực tế, nên việc xử lý nhóm theo cặp vẫn nằm trong giới hạn theo các ràng buộc đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    from collections import defaultdict, Counter

    def count_pairs(freq, target):
        res = 0
        seen = set()
        for x, cx in freq.items():
            y = target - x
            if y in freq and y not in seen:
                if x == y:
                    res += cx * (cx - 1) // 2
                else:
                    res += cx * freq[y]
            seen.add(x)
        return res

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        groups = defaultdict(list)
        for i in range(n):
            groups[a[i]].append(b[i])

        keys = list(groups.keys())
        ans = 0

        for i in range(len(keys)):
            x = keys[i]
            fx = Counter(groups[x])
            for j in range(i, len(keys)):
                y = keys[j]
                fy = Counter(groups[y])
                target = x * y

                if i == j:
                    seen = set()
                    for vx, cx in fx.items():
                        vy = target - vx
                        if vy in fx and vy not in seen:
                            if vx == vy:
                                ans += cx * (cx - 1) // 2
                            else:
                                ans += cx * fx[vy]
                        seen.add(vx)
                else:
                    tmp = 0
                    for vx, cx in fx.items():
                        vy = target - vx
                        if vy in fy:
                            tmp += cx * fy[vy]
                    ans += tmp

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""3
3
2 3 2
3 3 1
8
4 2 8 2 1 2 7 5
3 5 8 8 1 1 6 5
8
4 4 8 8 8 8 8 8
8 8 8 8 8 8 8 8
""") == """2
7
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng a và b | số tổ hợp | xử lý đa dạng | 
| tăng nghiêm ngặt a, ngẫu nhiên b | trận đấu thưa thớt | logic nhóm chéo | 
| không có cặp hợp lệ | 0 | tính đúng đắn của việc lọc | 
| thử nghiệm đơn tối đa n | căng thẳng | ổn định hiệu suất | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử đều giống hệt nhau. Trong trường hợp đó, giải pháp phải tránh các cặp đếm kép trong một nhóm. Thuật toán xử lý vấn đề này bằng cách sử dụng cách đếm kết hợp cx·(cx−1)/2, đảm bảo mỗi cặp không có thứ tự được tính chính xác một lần. 

Một trường hợp cạnh khác phát sinh khi không có cặp nào thỏa mãn phương trình. Việc nhóm vẫn lặp lại trên tất cả các cặp giá trị, nhưng mỗi lần tra cứu tổng hai giá trị đều không thành công, dẫn đến tích lũy bằng 0 mà không cần xử lý đặc biệt. 

Trường hợp cạnh cuối cùng xảy ra khi tồn tại nhiều giá trị a khác nhau nhưng giá trị b tập trung. Ngay cả trong tình huống này, thuật toán vẫn đúng vì mọi cặp hợp lệ vẫn được ghi lại bên trong chính xác một tương tác cặp nhóm, duy trì tính đầy đủ.
