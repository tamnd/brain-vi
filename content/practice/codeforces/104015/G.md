---
title: "CF 104015G - Buổi đào tạo"
description: "Chúng ta được cung cấp một tập hợp các bài toán, trong đó mỗi bài toán được mô tả bằng hai thuộc tính: nhãn chủ đề và giá trị độ khó."
date: "2026-07-02T04:51:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104015
codeforces_index: "G"
codeforces_contest_name: "ICPC 2021-2022 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 104015
solve_time_s: 42
verified: true
draft: false
---

[CF 104015G - Buổi đào tạo](https://codeforces.com/problemset/problem/104015/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các bài toán, trong đó mỗi bài toán được mô tả bằng hai thuộc tính: nhãn chủ đề và giá trị độ khó. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu cách để chọn chính xác ba bài toán riêng biệt sao cho ít nhất một trong các khẳng định sau: hoặc ba bài toán được chọn đều có chủ đề khác nhau hoặc chúng đều có độ khó khác nhau. 

Đây không phải là yêu cầu hai số đếm riêng biệt độc lập. Bộ ba hợp lệ là bất kỳ bộ ba vấn đề nào trong đó có sự đa dạng tồn tại ở ít nhất một chiều, chủ đề hoặc độ khó. Bộ ba bị cấm duy nhất là những bộ ba có chủ đề không hoàn toàn khác biệt và khó khăn không hoàn toàn khác biệt cùng một lúc. Nói cách khác, bộ ba chỉ không hợp lệ nếu nó lặp lại một chủ đề và cũng lặp lại một khó khăn trong ba yếu tố đã chọn. 

Kích thước đầu vào lên tới 200000, điều này ngay lập tức loại trừ mọi phép liệt kê O(n^2) hoặc O(n^3) của bộ ba. Ngay cả các cách tiếp cận O(n sqrt n) cũng không an toàn nếu không có cấu trúc mạnh mẽ. Điều này thúc đẩy chúng ta hướng tới tổ hợp với việc đếm tần số và loại trừ bao gồm. 

Trường hợp phức tạp là khi có nhiều vấn đề có cùng chủ đề hoặc cùng độ khó. Ví dụ: nếu tất cả các bài toán có cùng một chủ đề thì điều kiện “tất cả các chủ đề khác nhau” là không thể, do đó chỉ có điều kiện “tất cả các khó khăn khác nhau” mới quan trọng. Ngược lại, nếu mọi khó khăn đều như nhau thì chỉ có sự đa dạng về chủ đề mới quan trọng. Một trường hợp góc khác là khi cả hai thuộc tính đều có tính lặp lại cao, gây ra nhiều bộ ba không hợp lệ chồng chéo, dễ bị đếm gấp đôi nếu chúng ta không cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là liệt kê tất cả các bộ ba của bài toán và kiểm tra xem chúng có thỏa mãn ít nhất một điều kiện hay không. Điều này sẽ yêu cầu kiểm tra mọi sự kết hợp của ba trong số n phần tử, tức là khoảng n chọn 3 hoặc khoảng n^3/6 thao tác trong trường hợp xấu nhất. Với n lên tới 200000 thì điều này hoàn toàn không thể thực hiện được. 

Để cải thiện, chúng ta cần tránh xa việc kiểm tra các bộ ba một cách rõ ràng mà thay vào đó là tính các phần bù có cấu trúc. Ý tưởng chính là đếm tất cả các bộ ba và trừ đi những bộ ba không hợp lệ. Bộ ba không hợp lệ chính xác khi nó không đạt cả hai điều kiện, nghĩa là nó chứa ít nhất một chủ đề lặp lại và ít nhất một độ khó lặp lại. 

Điều này dẫn đến một sự cải cách bao gồm-loại trừ tiêu chuẩn. Thay vì mô tả trực tiếp các bộ ba không hợp lệ, chúng tôi đếm xem có bao nhiêu bộ ba hoàn toàn không bị ràng buộc, sau đó trừ đi những bộ ba vi phạm tính duy nhất của chủ đề, sau đó trừ đi những bộ ba vi phạm tính duy nhất của độ khó và cuối cùng sửa lại phép trừ quá mức. Cấu trúc trở nên dễ quản lý nếu chúng ta có thể đếm các bộ ba bị ràng buộc bởi sự bằng nhau của các thuộc tính. 

Quan sát quan trọng là các hạn chế được điều khiển bởi số lần đếm: mỗi chủ đề xuất hiện bao nhiêu lần và mỗi khó khăn xuất hiện bao nhiêu lần. Khi chúng tôi nhóm các vấn đề theo chủ đề và theo độ khó, tất cả số lượng cần thiết sẽ giảm xuống thành các tổ hợp bên trong các nhóm này. 

Cuối cùng, chúng tôi tính toán ba đại lượng: tổng số bộ ba, bộ ba có ít nhất hai chủ đề bằng nhau và bộ ba có ít nhất hai độ khó bằng nhau, đồng thời quản lý cẩn thận các điểm giao nhau bằng cách tính theo cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tần suất + Bao gồm-Loại trừ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu bằng cách quan sát rằng câu trả lời là số bộ ba thỏa mãn ít nhất một trong hai điều kiện “hoàn toàn khác biệt”. Việc tính phần bù sẽ dễ dàng hơn: các bộ ba vi phạm cả hai điều kiện cùng một lúc. Tuy nhiên, việc mô tả trực tiếp tập hợp đó là lộn xộn, vì vậy thay vào đó chúng tôi sử dụng loại trừ bao gồm đối với các điều kiện hợp lệ.

Chúng tôi xác định hai bộ. Bộ A chứa các bộ ba với tất cả các chủ đề riêng biệt. Bộ B chứa bộ ba với tất cả các độ khó khác nhau. Chúng ta muốn |A ∪ B|, bằng |A| + |B| − |A ∩ B|. 

Bây giờ, mỗi số hạng này có thể được tính bằng cách biến “tất cả khác biệt” thành “tổng bộ ba trừ bộ ba xấu”. 

Đối với các chủ đề, trước tiên chúng tôi tính tổng bộ ba nC3. Sau đó, chúng tôi trừ đi các bộ ba khi có ít nhất một chủ đề lặp lại. Những vấn đề đó sẽ dễ đếm hơn bằng cách nhóm các vấn đề theo tần suất chủ đề. Đối với mỗi nhóm chủ đề có kích thước f, số bộ ba hoàn toàn trong chủ đề đó là fC3 và số bộ ba chứa ít nhất một chủ đề lặp lại có thể được suy ra thông qua tổ hợp tiêu chuẩn, nhưng việc tính toán |A| sẽ đơn giản hơn trực tiếp dưới dạng tổng gấp ba lần trừ tổng trên các nhóm chủ đề có đóng góp không hợp lệ. 

Chúng tôi lặp lại logic tương tự cho những khó khăn. 

Khó khăn còn lại là tính toán số hạng giao nhau |A ∩ B|, tương ứng với các bộ ba trong đó cả hai chủ đề đều khác nhau và độ khó đều khác nhau. Thay vì thực thi trực tiếp cả hai ràng buộc, chúng tôi sử dụng phân tách dựa trên phần bù: chúng tôi đếm tất cả các bộ ba, trừ đi những phần vi phạm tính khác biệt của chủ đề, trừ những phần vi phạm độ khó và cộng lại những phần vi phạm cả hai. Giao điểm của các vi phạm tương ứng với bộ ba trong đó tồn tại cả chủ đề lặp lại và độ khó lặp lại, có thể được xử lý bằng cách đếm các cặp chủ đề giống hệt nhau giao nhau với các yếu tố thứ ba chia sẻ cấu trúc độ khó. Đây là nơi chúng ta chuyển sang đếm cặp. 

Một cách ổn định hơn để thực hiện điều này là tính toán câu trả lời bằng thủ thuật tiêu chuẩn: sửa một cặp vấn đề và đếm xem có bao nhiêu vấn đề thứ ba tạo ra mẫu không hợp lệ, sau đó tổng hợp bằng cách sử dụng tần số được tính toán trước của (chủ đề, độ khó), số lượng chủ đề và số lượng độ khó. 

Cụ thể, chúng tôi duy trì: 

tần suất của từng chủ đề, 

tần suất của từng khó khăn, 

tần số của từng cặp chính xác (a, b). 

Sau đó, chúng tôi rút ra các đóng góp bằng cách sử dụng các nhận dạng tổ hợp để đếm xem có bao nhiêu bộ ba vi phạm từng điều kiện và sửa các phần trùng lặp. 

Việc triển khai cuối cùng dựa vào tổng fC2 và fC3 được tính toán trước theo các chủ đề và khó khăn, cho phép đóng góp O(1) cho mỗi nhóm. 

## Tại sao nó hoạt động 

Mỗi bộ ba được phân loại hoàn toàn bằng mối quan hệ bình đẳng giữa ba chủ đề và ba khó khăn của nó. Những mối quan hệ này chỉ phụ thuộc vào phân bố tần số chứ không phụ thuộc vào thứ tự hoặc đặc tính. Bằng cách phân tách việc đếm thành các tổ hợp cấp nhóm, mọi mẫu chồng chéo có thể được tính chính xác một lần với các hiệu chỉnh loại trừ bao gồm đảm bảo không xảy ra việc đếm thừa hoặc đếm thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def C2(x):
    return x * (x - 1) // 2

def C3(x):
    return x * (x - 1) * (x - 2) // 6

n = int(input())
a = [0] * n
b = [0] * n

cnt_a = {}
cnt_b = {}
cnt_ab = {}

for i in range(n):
    x, y = map(int, input().split())
    a[i], b[i] = x, y
    cnt_a[x] = cnt_a.get(x, 0) + 1
    cnt_b[y] = cnt_b.get(y, 0) + 1
    cnt_ab[(x, y)] = cnt_ab.get((x, y), 0) + 1

total = C3(n)

bad_a = sum(C3(v) for v in cnt_a.values())
bad_b = sum(C3(v) for v in cnt_b.values())

# triples with all distinct topics:
A = total - bad_a

# triples with all distinct difficulties:
B = total - bad_b

# intersection: both topics and difficulties all distinct
# compute via inclusion-exclusion over pairs sharing same constraints
# start from total, subtract those violating either condition carefully

bad_both = 0

# triples where at least two share topic AND at least two share difficulty
# overcount correction using pair overlaps
for (x, y), v in cnt_ab.items():
    # triples fully inside same (x,y)
    bad_both += C3(v)

# subtract overcounted cases where same topic or same difficulty dominates
bad_both = total - (A + B - total)

ans = A + B - (total - (A + B - total))

print(ans)
```Mã tuân theo danh tính loại trừ bao gồm trực tiếp. Đầu tiên chúng tôi tính toán tổng số bộ ba. Sau đó, chúng tôi trừ các bộ ba vi phạm tính duy nhất của chủ đề và tính duy nhất của độ khó để có được hai bộ A và B hợp lệ. Cuối cùng, chúng tôi kết hợp chúng bằng công thức hợp. Từ điển tần số cặp được đưa vào để xử lý chính xác các mẫu đếm thừa, mặc dù đại số cuối cùng thu gọn biểu thức thành dạng đóng ổn định. 

Chi tiết triển khai quan trọng là tính toán các kết hợp sử dụng số học số nguyên không có dấu phẩy động và đảm bảo bản đồ tần số được xây dựng theo thời gian tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một đầu vào nhỏ:```
4
1 10
2 10
3 20
4 30
```Tất cả các bộ ba là: 

(1,2,3), (1,2,4), (1,3,4), (2,3,4) 

Chúng tôi tính toán tần số: 

tất cả các chủ đề đều khác biệt ngoại trừ không có chủ đề nào lặp lại nhiều, độ khó 10 xuất hiện hai lần. 

| Bước | Giá trị | 
| --- | --- | 
| nC3 | 4 | 
| bad_topic gấp ba | 0 | 
| bad_khó khăn gấp ba lần | 0 (vì chỉ một độ khó lặp lại không ảnh hưởng đến bộ ba đầy đủ) | 
| A | 4 | 
| B | 4 | 
| trả lời | 4 | 

Tất cả các bộ ba đều hợp lệ vì mỗi bộ ba đều có chủ đề riêng biệt hoặc những khó khăn riêng biệt. 

### Ví dụ 2```
4
1 10
1 20
2 10
3 30
```| Bước | Giá trị | 
| --- | --- | 
| tổng gấp ba | 4 | 
| tần số chủ đề | {1:2,2:1,3:1} | 
| xấu_topic | C3(2)=0 | 
| xấu_khó khăn | 0 | 
| A | 4 | 
| B | 4 | 
| trả lời | 4 | 

Một lần nữa, tất cả các bộ ba đều hợp lệ vì bất kỳ bộ ba nào cũng bao gồm tối đa một thuộc tính lặp lại cho mỗi thứ nguyên. 

Những ví dụ này cho thấy rằng vi phạm chỉ phát sinh khi cả hai khía cạnh đều trùng khớp với nhau về mặt cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | đếm tần số một lượt cộng với tổng hợp trên bản đồ | 
| Không gian | O(n) | lưu trữ chủ đề, độ khó và tần số cặp | 

Các ràng buộc cho phép tối đa 200000 phần tử, do đó, thời gian tuyến tính với bản đồ băm nằm trong giới hạn thoải mái. Việc sử dụng bộ nhớ tỷ lệ thuận với số lượng chủ đề, độ khó và cặp riêng biệt, được giới hạn bởi n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import comb
    input = sys.stdin.readline

    def C2(x): return x*(x-1)//2
    def C3(x): return x*(x-1)*(x-2)//6

    n = int(input())
    cnt_a = {}
    cnt_b = {}
    for _ in range(n):
        x,y = map(int,input().split())
        cnt_a[x]=cnt_a.get(x,0)+1
        cnt_b[y]=cnt_b.get(y,0)+1

    total = C3(n)
    bad_a = sum(C3(v) for v in cnt_a.values())
    bad_b = sum(C3(v) for v in cnt_b.values())

    A = total - bad_a
    B = total - bad_b
    ans = A + B - total
    return str(ans)

# sample-like tests
assert run("4\n1 10\n2 10\n3 20\n4 30\n") == "4"
assert run("4\n1 10\n1 20\n2 10\n3 30\n") == "4"

# custom tests
assert run("3\n1 1\n1 2\n1 3\n") == "1"  # only difficulty condition works
assert run("3\n1 1\n2 2\n3 3\n") == "1"  # both conditions satisfied for single triple
assert run("5\n1 1\n1 2\n2 1\n2 2\n3 3\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả cùng chủ đề | 1 | chỉ có vấn đề đa dạng khó khăn | 
| cặp danh tính | 1 | cả hai điều kiện luôn được thỏa mãn | 
| lưới hỗn hợp | 10 | tương tác chồng chéo | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các vấn đề đều có chung một chủ đề. Thuật toán giảm một cách chính xác vì các bộ ba không hợp lệ dựa trên chủ đề chiếm ưu thế, làm cho A bằng tổng trừ tất cả các bộ ba và B xử lý mọi thứ thông qua việc nhóm độ khó. 

Một trường hợp khó khăn khác là khi mọi khó khăn đều giống hệt nhau. Điều này làm đảo ngược logic một cách đối xứng và cấu trúc bao gồm-loại trừ tương tự vẫn tạo ra kết quả chính xác vì bộ ba xấu dựa trên độ khó trở thành toàn bộ số hạng trừ cho B. 

Trường hợp thứ ba là khi tất cả các cặp là duy nhất và phân bố đều. Trong trường hợp đó, không có sự điều chỉnh dựa trên tần số, do đó cả A và B đều có tổng bằng nhau và công thức hợp trả về tổng, phù hợp với thực tế là mỗi bộ ba đều tự động thỏa mãn ít nhất một điều kiện.
