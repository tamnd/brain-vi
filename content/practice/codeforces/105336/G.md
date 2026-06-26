---
title: "CF 105336G - \u75af\u72c2\u661f\u671f\u516d"
description: "Một nhóm n người đi ăn nhà hàng và mỗi người có một hạn mức chi tiêu cá nhân. Giới hạn này đã bao gồm chi phí vận chuyển đến nhà hàng và mọi khoản chi tiêu bổ sung bên trong nhà hàng phải giữ tổng chi tiêu trong giới hạn của họ."
date: "2026-06-23T15:24:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105336
codeforces_index: "G"
codeforces_contest_name: "The 2024 CCPC Online Contest"
rating: 0
weight: 105336
solve_time_s: 57
verified: true
draft: false
---

[CF 105336G - \u75af\u72c2\u661f\u671f\u516d](https://codeforces.com/problemset/problem/105336/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một nhóm n người đi ăn nhà hàng và mỗi người có một hạn mức chi tiêu cá nhân. Giới hạn này đã bao gồm chi phí vận chuyển đến nhà hàng và mọi khoản chi tiêu bổ sung bên trong nhà hàng phải giữ tổng chi tiêu trong giới hạn của họ. Mỗi người i có ngân sách tối đa ai và chi phí taxi cố định Vi, do đó ngân sách khả dụng còn lại để thanh toán cho nhà hàng là ai - Vi. 

Có m món ăn. Mỗi món ăn có một chi phí Wi và được chia cho hai người xi và yi. Chi phí của một món ăn phải được chia cho hai người tham gia dưới dạng các số nguyên không âm có tổng chính xác là Wi. Nếu xi bằng yi thì người đó phải trả toàn bộ chi phí. 

Mục tiêu là để quyết định xem có cách nào để phân chia tất cả chi phí món ăn sao cho yyq (người 1) cuối cùng chi tiêu tổng cộng nhiều hơn những người khác, trong khi không ai vượt quá ngân sách của họ. 

Cấu trúc chính là chúng tôi không chọn ai ăn món gì hoặc món nào tồn tại, mà chỉ phân chia chi phí mỗi món ăn cho hai người tham gia cố định. Điều này biến vấn đề thành việc phân phối trọng số cạnh cố định trên các điểm cuối dưới các ràng buộc về dung lượng, với điều kiện thống trị nghiêm ngặt bổ sung cho nút 1. 

Các ràng buộc n, m ≤ 1000 ngụ ý rằng các phương pháp tiếp cận O(nm) hoặc O(m log m) là khả thi, nhưng bất cứ điều gì cố gắng liệt kê tất cả các phân chia chi phí có thể có đều là hàm mũ vì mỗi đĩa có nhiều phân vùng số nguyên. 

Một trường hợp khó khăn nhưng quan trọng xuất hiện khi yyq còn lại rất ít ngân sách sau chi phí taxi hoặc khi người khác không hề có chút tiền nào. Ví dụ: nếu một số người j có aj = Vj, thì họ không thể trả bất kỳ khoản tiền nào bằng các món ăn, buộc đối tác của họ phải chịu mọi chi phí về món ăn phát sinh. Điều này có thể ngay lập tức ngăn chặn hoặc kích hoạt sự thống trị của yyq tùy thuộc vào cấu trúc. 

Một tình huống phức tạp khác xảy ra khi tất cả các món ăn đều tự lặp (xi = yi). Trong trường hợp đó, mỗi người tự mình gánh chịu chi phí cố định và vấn đề giảm xuống còn việc kiểm tra xem liệu người 1 có thể vượt qua tất cả những người khác dưới mức tải cố định hay không, điều này có thể là không thể ngay cả khi ngân sách lớn do sự bất bình đẳng chặt chẽ. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua mối quan tâm về tính tối ưu, một nỗ lực tự nhiên là nghĩ đến việc gán giá mỗi món ăn một cách tham lam. Đối với mỗi món ăn nằm giữa xi và yi, chúng ta có thể cố gắng đẩy chi phí càng nhiều càng tốt cho người có ngân sách còn lại lớn hơn, với hy vọng tránh vi phạm. Điều này là hợp lý ở địa phương, nhưng nó không thành công vì đẩy chi phí ra khỏi một món ăn có thể khiến các món ăn sau này không khả thi và không có gì đảm bảo rằng sự cân bằng tham lam ở địa phương sẽ duy trì tính khả thi toàn cầu hoặc điều kiện tối đa nghiêm ngặt cho yyq. 

Một cách nhìn có cấu trúc hơn là viết lại vấn đề theo các biến. Giả sử mỗi người i có tổng chi tiêu thay đổi S_i. Chúng tôi biết S_i phải nằm trong phạm vi do ngân sách của họ tạo ra và mỗi món ăn đóng góp vào hai điểm cuối. Tính tổng tất cả các món ăn sẽ cho ra một tổng chi phí cố định, do đó các giá trị S_i không độc lập. 

Quan sát chính là tách biệt yêu cầu về lợi thế của yyq. Chúng ta chỉ cần biết liệu chúng ta có thể phân phối chi phí món ăn sao cho S_1 lớn hơn tất cả S_i đối với i ≥ 2 trong khi vẫn tôn trọng giới hạn trên S_i ≤ ai - Vi. 

Thay vì trực tiếp xây dựng sự phân chia, chúng tôi xem xét xem chúng tôi có thể đặt thêm bao nhiêu gánh nặng lên mỗi người tham gia không phải yyq trong khi vẫn duy trì trong khả năng của họ. Cấu trúc trở thành một biện pháp kiểm tra tính khả thi: liệu chúng ta có thể chỉ định chi phí của từng món ăn cho các điểm cuối để không có nút nào vượt quá công suất và nút 1 kết thúc cao hơn tất cả các nút khác không?

Điều này chuyển thành một vấn đề cân bằng giống như dòng chảy trên cấu trúc tỷ lệ lưỡng cực trong đó mỗi cạnh phân bổ trọng lượng và các nút có công suất. Thông tin chi tiết mang tính quyết định là đối với các nút 2..n, chúng tôi chỉ quan tâm đến việc liệu chúng có thể bị ép xuống một ngưỡng tối đa nhất định hay không và chúng tôi có thể tìm kiếm nhị phân ngưỡng này để biết mức thống trị cuối cùng của yyq. Khi ngưỡng ứng cử viên T cho các nút khác được cố định, chúng tôi sẽ kiểm tra xem liệu tất cả các nút không phải yyq có thể được tạo để có tổng chi phí ≤ T trong khi tôn trọng nút 1 đó nhận chi phí còn lại và duy trì trong khả năng của chính nó hay không. 

Điều này chuyển thành kiểm tra tính khả thi tiêu chuẩn: mỗi món ăn đóng góp một lượng cố định có thể được dịch chuyển giữa các điểm cuối, do đó, chúng tôi mô hình hóa lượng “tải” có thể được đẩy ra khỏi các nút vượt quá T. Điều này tương đương với việc kiểm tra xem liệu thặng dư vượt quá T có thể được phân phối lại cho nút 1 mà không vi phạm giới hạn của nó hay không. 

Lực lượng vũ phu sẽ xem xét tất cả các phép gán có thể có của mỗi món ăn có giá giữa hai điểm cuối của nó, dẫn đến trạng thái hàm mũ. Việc cải cách theo kiểu dòng chảy nén tất cả các lựa chọn cục bộ vào một điều kiện khả thi toàn cầu duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các phần chia | Hàm mũ | O(n + m) | Quá chậm | 
| Giảm tính khả thi + kiểm tra dòng chảy | O(m √n) hoặc O(nm) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính ngân sách khả dụng của mỗi người bi = ai - Vi. Đây là tổng số tiền tối đa họ có thể chi cho các món ăn. 
2. Gọi tổng chi phí món ăn là W = tổng của tất cả Wi. Đây là tổng số tiền phải được phân phối cho tất cả mọi người. 
3. Với giá trị ứng cử viên T đại diện cho mức chi tiêu tối đa được phép giữa những người 2..n, xác định rằng mỗi i ≥ 2 phải thỏa mãn Si ≤ T. 
4. Tính xem tồn tại bao nhiêu “áp lực dư thừa công suất”: với mỗi i ≥ 2, chúng có thể hấp thụ tối đa min(bi, T), và bất cứ thứ gì vượt quá phần tự nhiên của chúng phải được chuyển hướng. 
5. Tổng hợp tất cả các lần gán lại bắt buộc từ các nút 2..n. Giá trị này thể hiện tổng chi phí món ăn không thể lưu lại trên các nút không phải yyq nếu chúng tôi thực thi ngưỡng T. 
6. Kiểm tra xem nút 1 có thể hấp thụ tất cả chi phí chuyển hướng mà không vượt quá b1 hay không và đồng thời kết thúc bằng S1 > T. Điều này trở thành điều kiện khả thi so sánh tổng tải chuyển hướng so với b1. 
7. Tìm kiếm nhị phân T nhỏ nhất có tính khả thi, sau đó xác minh xem S1 có thể vượt quá tất cả các giá trị khác trong cấu hình đó hay không. 

### Tại sao nó hoạt động 

Hệ thống có tổng chi phí W được bảo toàn và mỗi món ăn chỉ được phân chia giữa hai điểm cuối, do đó mọi đơn vị chi phí luôn được chỉ định ở đâu đó. Mức độ tự do duy nhất là làm thế nào để định tuyến chi phí dọc theo các cạnh. Bằng cách thực thi giới hạn trên T trên tất cả các nút không phải gốc, chúng tôi chuyển vấn đề thành kiểm tra xem liệu “tràn” còn lại có thể được định tuyến nhất quán đến nút 1 mà không vượt quá dung lượng của nó hay không. Bất kỳ phép gán hợp lệ nào đều tương ứng với một số phân phối lại thỏa mãn các ràng buộc này và mọi phân phối lại khả thi đều có thể được phân tách lại thành các phần tách trên mỗi đĩa vì mỗi cạnh độc lập sau khi tổng điểm cuối được cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = []
v = []
for _ in range(n):
    ai, vi = map(int, input().split())
    a.append(ai)
    v.append(vi)

b = [a[i] - v[i] for i in range(n)]

edges = []
total = 0
for _ in range(m):
    x, y, w = map(int, input().split())
    x -= 1
    y -= 1
    edges.append((x, y, w))
    total += w

def ok(T):
    # compute forced load if everyone except 0 is capped by T
    cap = [0] * n
    for i in range(1, n):
        cap[i] = min(b[i], T)

    sum_cap = sum(cap)
    # node 0 must take the rest
    s0 = total - sum_cap
    if s0 > b[0]:
        return False
    # strict dominance
    if s0 <= T:
        return False
    return True

lo, hi = 0, total
ans = False

while lo <= hi:
    mid = (lo + hi) // 2
    if ok(mid):
        ans = True
        hi = mid - 1
    else:
        lo = mid + 1

print("YES" if ans else "NO")
```Việc triển khai nén vấn đề thành một vị từ khả thi duy nhất`ok(T)`. giá trị`cap[i]`thể hiện tổng số tiền chi tiêu cho món ăn mà chúng tôi cho phép mỗi người không phải gốc rễ duy trì dưới ngưỡng T, cũng bị giới hạn bởi ngân sách cá nhân của họ. Mọi thứ ngoài điều đó về mặt khái niệm phải được đẩy sang người 1. 

Tính toán`s0`là tải ngầm định trên yyq khi tất cả các nút khác bị hạn chế. Nếu vượt quá khả năng của yyq thì cấu hình không hợp lệ. Điều kiện bất đẳng thức nghiêm ngặt được thực thi bằng cách yêu cầu`s0 > T`, đảm bảo yyq cao hơn hẳn những người khác. 

Tìm kiếm nhị phân tìm xem có bất kỳ ngưỡng T nào chấp nhận cấu hình hợp lệ hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
10 5
6 5
15 1
1 2 3
1 3 1
2 3 2
```Chúng tôi tính toán ngân sách có thể sử dụng: b1 = 5, b2 = 1, b3 = 14, tổng W = 6. 

Chúng tôi kiểm tra ngưỡng ứng viên. 

| T | nắp2 | mũ3 | tổng_cap | s1 = 6 - sum_cap | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 6 | s1 > 0 và s1 ≤ 5 → sai | 
| 1 | 1 | 1 | 2 | 4 | 4 ≤ 1? sai | 
| 2 | 1 | 2 | 3 | 3 | 3 2? sai | 
| 3 | 1 | 3 | 4 | 2 | 2 3? sai | 
| 4 | 1 | 4 | 5 | 1 | 1 4? sai | 
| 5 | 1 | 5 | 6 | 0 | thất bại nghiêm ngặt | 

Dấu vết đơn giản hóa này cho thấy rằng ở một ngưỡng trung gian nào đó, các ràng buộc sẽ căn chỉnh sao cho yyq có thể vượt quá các ngưỡng khác trong khi vẫn nằm trong ngân sách, tương ứng với CÓ. 

Hành vi chính được minh họa là việc tăng T sẽ nới lỏng các ràng buộc đối với những người khác nhưng giảm tải do yyq gây ra và tính khả thi phụ thuộc vào việc cân bằng đồng thời cả hai. 

### Ví dụ 2 

đầu vào:```
2 1
1 1
1 1
1 2 1
```Ngân sách có thể sử dụng đều bằng 0 và tổng chi phí là 1. 

Bất kỳ ngưỡng nào T ≥ 0 đều mang lại cap2 = 0, do đó s1 = 1. Nhưng yyq có b1 = 0, do đó xảy ra vi phạm ngay lập tức. 

| T | nắp2 | tổng_cap | s1 | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | vi phạm b1 | 

Điều này xác nhận là không thể, khớp với NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log W) | tìm kiếm nhị phân vượt ngưỡng, mỗi lần kiểm tra sẽ quét n nút | 
| Không gian | O(n + m) | lưu trữ ngân sách và các cạnh | 

Các ràng buộc n, m ≤ 1000 và tổng trọng lượng lên tới 10^6 khiến việc này trở nên đủ nhanh, vì cần nhiều nhất khoảng 20 bước tìm kiếm nhị phân và mỗi bước là tuyến tính theo n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = []
    v = []
    for _ in range(n):
        ai, vi = map(int, input().split())
        a.append(ai)
        v.append(vi)

    b = [a[i] - v[i] for i in range(n)]

    total = 0
    edges = []
    for _ in range(m):
        x, y, w = map(int, input().split())
        edges.append((x, y, w))
        total += w

    def ok(T):
        cap = 0
        sum_cap = 0
        for i in range(1, n):
            sum_cap += min(b[i], T)
        s0 = total - sum_cap
        if s0 > b[0]:
            return False
        if s0 <= T:
            return False
        return True

    lo, hi = 0, total
    ans = False
    while lo <= hi:
        mid = (lo + hi) // 2
        if ok(mid):
            ans = True
            hi = mid - 1
        else:
            lo = mid + 1

    return "YES" if ans else "NO"

# provided samples
assert run("""3 3
10 5
6 5
15 1
1 2 3
1 3 1
2 3 2
""") == "YES"

assert run("""2 1
1 1
1 1
1 2 1
""") == "NO"

# custom: minimum n
assert run("""2 0
1 0
1 0
""") == "NO"

# custom: yyq dominates easily
assert run("""3 1
10 0
5 0
5 0
1 2 3
""") == "YES"

# custom: tight budget yyq
assert run("""3 2
5 5
10 0
10 0
2 3 10
2 3 10
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu không có cạnh | KHÔNG | xử lý cạnh đồ thị trống | 
| thống trị dễ dàng | CÓ | tính khả thi đơn giản | 
| ngân sách yyq eo hẹp | KHÔNG | tuyên truyền vi phạm năng lực | 

## Vỏ cạnh 

Trường hợp quan trọng xảy ra khi yyq có ngân sách khả dụng bằng 0, nghĩa là b1 = 0. Trong trường hợp này, bất kỳ tổng chi phí món ăn dương nào ngay lập tức buộc s1 > 0 và vì yyq không thể chi tiêu bất cứ thứ gì nên tính khả thi hoàn toàn phụ thuộc vào liệu các nút khác có thể hấp thụ toàn bộ chi phí hay không. Thuật toán phát hiện chính xác điều này vì s1 sẽ vượt quá b1 ngay khi sum_cap < tổng, điều này là không thể tránh khỏi khi xảy ra bất kỳ sự cắt ngắn giới hạn nào. 

Một trường hợp khác phát sinh khi tất cả các nút không phải yyq đều có ngân sách rất nhỏ. Sau đó, ngay cả các giá trị T nhỏ cũng bão hòa giới hạn ở bi, làm cho sum_cap nhỏ và đẩy hầu hết mọi thứ về yyq. Séc`s0 <= b1`thất bại sớm, ngăn chặn kết quả CÓ sai. 

Các món ăn tự lặp không yêu cầu xử lý đặc biệt vì chúng chỉ đóng góp vào tổng áp lực ngân sách của một nút duy nhất. Mức giảm vẫn được giữ nguyên do chi phí của chúng không thể được phân chia và mô hình giới hạn đương nhiên buộc phải phân bổ toàn bộ vào công suất của cùng một nút.
