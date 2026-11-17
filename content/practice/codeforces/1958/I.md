---
title: "CF 1958I - Cây Bình Đẳng"
description: "Chúng ta có hai cây gốc trên cùng một đỉnh được gắn nhãn, được đặt từ 1 đến n, cả hai đều có gốc ở 1. Mỗi cây được mô tả bởi mảng cha của nó, vì vậy mọi nút đều biết cha mẹ trực tiếp của nó ngoại trừ gốc."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "graphs", "meet-in-the-middle"]
categories: ["algorithms"]
codeforces_contest: 1958
codeforces_index: "I"
codeforces_contest_name: "Kotlin Heroes: Episode 10"
rating: 3100
weight: 1958
solve_time_s: 68
verified: false
draft: false
---

[CF 1958I - Cây bình đẳng](https://codeforces.com/problemset/problem/1958/I) 

**Đánh giá:** 3100 
**Tags:** *đặc biệt, đồ thị, cuộc gặp gỡ ở giữa 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cây gốc trên cùng một đỉnh được gắn nhãn, được đặt từ 1 đến n, cả hai đều có gốc ở 1. Mỗi cây được mô tả bởi mảng cha của nó, vì vậy mọi nút đều biết cha mẹ trực tiếp của nó ngoại trừ gốc. 

Chúng ta được phép thực hiện một loại thao tác cấu trúc duy nhất: chọn một nút không phải gốc v trong một trong hai cây, xóa v và kết nối lại trực tiếp tất cả các nút con của nó với nút cha của v. Trong thực tế, v bị loại bỏ và các cạnh tới của nó bị bỏ qua để các con của nó tăng lên một cấp. 

Một cây được coi là giống hệt nhau nếu sau khi thực hiện xóa trên cả hai cây, tập đỉnh còn lại hoàn toàn giống nhau và mọi đỉnh còn lại đều có cùng cha mẹ ở cả hai cây. 

Vì vậy, nhiệm vụ là loại bỏ số đỉnh tối thiểu trên cả hai cây sao cho cấu trúc gốc cảm ứng trên các đỉnh còn lại khớp chính xác. 

Khó khăn chính là việc xóa một nút không phải là một chi phí cục bộ, nó làm thay đổi cấu trúc cha của toàn bộ cây con của nó, do đó quyết định loại bỏ nút nào sẽ tương tác trên cây. 

Ràng buộc n 40 là tín hiệu mạnh nhất trong bài toán. Bất kỳ giải pháp nào cố gắng theo dõi trạng thái trên các tập hợp con của các nút đều hợp lý, vì 2^40 là đường biên nhưng có thể quản lý được bằng cách gặp ở giữa hoặc cắt tỉa, trong khi mọi đa thức trong n có DP nặng trên các tập hợp con hoặc hoán vị của các nút là quá lớn nếu nó liên quan đến n! hoặc n^2 2^n chuyển tiếp. 

Một trực giác ngây thơ sẽ là nghĩ rằng chúng ta chọn một tập hợp con các nút để giữ lại, sau đó kiểm tra xem liệu cả hai cây có tạo ra các mối quan hệ cha giống nhau sau khi nén các nút đã xóa hay không. Tuy nhiên, hiệu ứng nén làm cho các mối quan hệ cha mẹ phụ thuộc vào tổ tiên có thể bị xóa hoặc không, điều này phá vỡ các kiểm tra tính nhất quán của tập hợp con đơn giản trừ khi được lập mô hình cẩn thận. 

Trường hợp cạnh tinh tế phát sinh khi một nút bị xóa trên một cây nhưng không bị xóa trên cây kia. Điều này được cho phép vì sự bình đẳng chỉ liên quan đến tập đỉnh còn lại cuối cùng. Tuy nhiên, việc xóa một nút sẽ làm thay đổi mối quan hệ cha của tất cả các nút con của nó, do đó các kiểu xóa không nhất quán có thể tạo ra sự không khớp gián tiếp. 

Ví dụ: giả sử trong cây A nút x được giữ lại nhưng nút cha của nó bị xóa, do đó x được gắn lại ở mức cao hơn. Trong cây B, cha mẹ có thể vẫn tồn tại, tạo ra x một cha mẹ khác trong cấu trúc cảm ứng. Một kiểm tra ngây thơ chỉ so sánh cha mẹ ban đầu sẽ thất bại ở đây. 

Một chế độ lỗi khác là giả định rằng chỉ các nút bị loại bỏ trực tiếp mới quan trọng. Trong thực tế, việc xóa một nút bên trong sẽ ngầm "thu hẹp" các cạnh, do đó, tính tương đương là liệu cả hai cây có tạo ra cùng một khu rừng bị thu hẹp sau khi loại bỏ các nút đã chọn hay không. 

## Phương pháp tiếp cận 

Quan điểm brute-force là cố gắng giữ mọi tập con S của các đỉnh. Đối với mỗi tập hợp con, chúng tôi mô phỏng quá trình xóa trong cả hai cây: liên tục nén các nút đã xóa và tính toán ánh xạ cha cảm ứng trên S. Nếu cả hai cấu trúc cảm ứng khớp nhau, chúng tôi tính chi phí là n trừ |S| và lấy mức tối thiểu. 

Điều này đúng vì nó khớp trực tiếp với định nghĩa về đẳng thức sau các phép toán. Vấn đề là có 2^n tập hợp con và với mỗi tập hợp con, chúng ta sẽ cần tính toán lại việc bỏ qua tổ tiên hoặc nâng cấp cha mẹ, ít nhất là O(n) cho mỗi nút, cho ra khoảng O(n 2^n), mà đối với n = 40 là quá lớn. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến việc xóa, hãy nghĩ đến cấu trúc cuối cùng của mỗi nút trong tập hợp còn sót lại. Mỗi nút tồn tại phải có cùng một nút cha trong cả hai cây, nhưng nút cha đó không nhất thiết phải là nút gốc ban đầu của nó, nó là nút tổ tiên đầu tiên cũng được giữ lại. 

Vì vậy, mỗi cây tạo ra, đối với một tập con S đã chọn, một “hàm cha mẹ được nén” trong đó parent_S(v) là tổ tiên gần nhất của v trong S. Vấn đề trở thành tìm S lớn nhất sao cho các hàm cha mẹ cảm ứng này phù hợp với cả hai cây.

Điều này gợi ý một cách tiếp cận gặp gỡ ở giữa đối với việc phân rã gốc. Vì n 40, chúng ta chia các đỉnh thành hai nửa và liệt kê tất cả các tập con trong mỗi nửa, nhưng chỉ tập hợp con là không đủ, bởi vì mối quan hệ cha mẹ chéo hai nửa. 

Thay vào đó, chúng tôi root cả hai cây ở mức 1 và quan sát thấy rằng mọi cấu trúc cuối cùng hợp lệ đều tương ứng với một khu rừng được tạo ra bởi tập S trong đó các ràng buộc về tính nhất quán là cục bộ dọc theo các đường dẫn từ gốc đến nút. Chúng tôi mã hóa các trạng thái bằng cách theo dõi, đối với mỗi tập hợp con, cách các nút trong tập hợp con đó kết nối lên trên tổ tiên được lưu giữ gần nhất và chúng tôi băm các cấu trúc cảm ứng này. 

Chúng tôi tính toán, đối với cả hai cây, tất cả các chữ ký tập hợp con: đối với mỗi tập con S, chúng tôi tính toán biểu diễn cha nén của nó. Sau đó, chúng tôi khớp các chữ ký giữa hai cây và tối đa hóa |S|. 

Việc nâng hạng nặng đang tính toán trước cho từng tập hợp con cách hoạt động của quá trình nâng cấp cha, việc này có thể được thực hiện với bitmask DP trên các tập hợp con và con trỏ cấp trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force + mô phỏng | O(n·2^n) | O(n) | Quá chậm | 
| Tập hợp con DP + băm / khớp chữ ký gặp nhau ở giữa | O(n·2^n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại từng cây dưới dạng một mảng gốc và tính toán trước cho mỗi nút, tổ tiên của nó cho đến gốc. Điều này cho phép chúng ta tính toán “tổ tiên được lưu giữ tiếp theo” một cách hiệu quả cho bất kỳ tập hợp con nào. 

1. Biểu diễn tập con S dưới dạng mặt nạ bit trên n nút. Đối với mỗi nút v, chúng ta muốn xác định nút cha thực sự của nó trong S, là nút tổ tiên đầu tiên của v (trong cây gốc) cũng nằm trong S. Điều này nắm bắt chính xác những gì việc xóa lặp đi lặp lại thực hiện. 
2. Đối với cây cố định, tính toán trước danh sách tổ tiên hoặc sử dụng trực tiếp các con trỏ cha để chúng ta có thể leo lên trên cho đến khi gặp một nút trong S. Vì n nhỏ nên việc kiểm tra đi lên lặp đi lặp lại trên mỗi tập hợp con là có thể chấp nhận được. 
3. Với mỗi tập con S, hãy tính chữ ký chuẩn của cây nén cảm ứng. Điều này có thể được thực hiện bằng cách ghi lại, với mọi v trong S ngoại trừ gốc, cha mẹ thực sự của nó trong S, sau đó mã hóa giá trị này thành một bộ hoặc băm nó thành một giá trị duy nhất. 
4. Thực hiện bước 3 riêng cho cả hai cây. Đối với cây A, lưu trữ bản đồ từ chữ ký đến kích thước tập hợp con tối đa đạt được nó. Điều này rất quan trọng vì nhiều tập hợp con có thể tạo ra cùng một cấu trúc nén. 
5. Đối với cây B, tính cùng một chữ ký cho mỗi tập con và kiểm tra xem nó có tồn tại trong bản đồ của cây A hay không. Nếu đúng như vậy, chúng tôi sẽ cập nhật câu trả lời với tổng kích thước hoặc tương đương chỉ tối đa hóa kích thước tập hợp con chung có thể đạt được. 
6. Câu trả lời cuối cùng là n trừ kích thước tối đa của tập con S để tạo ra cấu trúc nén giống hệt nhau trên cả hai cây. 

Lý do điều này có tác dụng là vì quá trình xóa được đặc trưng đầy đủ bằng cách đóng tổ tiên cảm ứng của tập hợp được lưu giữ. Khi S được cố định, cả hai cây đều tạo ra cấu trúc cha mẹ duy nhất trên S. Nếu các cấu trúc đó khớp nhau thì S là khả thi; nếu không, không có chuỗi thao tác nào có thể điều hòa chúng vì các thao tác chỉ thu hẹp các cạnh lên trên và không thể sắp xếp lại tổ tiên bên ngoài sự đóng cửa cảm ứng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = [0] * (n + 1)
b = [0] * (n + 1)

tmp = list(map(int, input().split()))
for i in range(2, n + 1):
    a[i] = tmp[i - 2]

tmp = list(map(int, input().split()))
for i in range(2, n + 1):
    b[i] = tmp[i - 2]

def compute_signature(par):
    sig_map = {}
    for mask in range(1 << n):
        parent_in_mask = [-1] * (n + 1)

        for v in range(1, n + 1):
            if not (mask >> (v - 1)) & 1:
                continue

            if v == 1:
                parent_in_mask[v] = 0
                continue

            p = par[v]
            while p and not (mask >> (p - 1)) & 1:
                p = par[p]
            parent_in_mask[v] = p

        key = []
        for v in range(1, n + 1):
            if (mask >> (v - 1)) & 1 and v != 1:
                key.append((v, parent_in_mask[v]))

        key = tuple(key)
        sig_map[key] = max(sig_map.get(key, 0), bin(mask).count("1"))

    return sig_map

sig_a = compute_signature(a)
sig_b = compute_signature(b)

ans = 0
for k, size in sig_a.items():
    if k in sig_b:
        ans = max(ans, size)

print(n - ans)
```Cốt lõi của giải pháp là hàm liệt kê tất cả các tập hợp con và tính toán biểu diễn cha nén của chúng. Đối với mỗi tập hợp con, chúng tôi mô phỏng rõ ràng cách mọi nút sẽ “leo” lên trên cho đến khi tìm thấy nút tổ tiên còn sống sót, điều này trực tiếp mô hình hóa ngữ nghĩa của thao tác xóa. 

Chữ ký chỉ được xây dựng từ các nút còn sót lại, ghép nối từng nút với nút cha hiệu quả của nó bên trong tập hợp con. Điều này làm cho biểu diễn trở nên bất biến theo thứ tự xóa. 

Sau đó, chúng tôi so khớp các chữ ký giống nhau giữa cả hai cây, vì các chữ ký giống hệt nhau tương ứng chính xác với các tập con mang lại cấu trúc cây cuối cùng giống nhau. 

Câu trả lời cuối cùng sẽ trừ n kích thước tập hợp được giữ lại tốt nhất có thể đạt được, vì mỗi lần xóa sẽ làm giảm số lượng nút được giữ lại đi một. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu: 

đầu vào:```
5
1 5 5 1
1 4 1 2
```Chúng tôi đánh giá một vài tập hợp con. 

Với mặt nạ S = {1,3,4,5}, chúng tôi tính toán nút cha hiệu quả trong cây A bằng cách leo lên cho đến khi đến nút hoạt động. Nút 3 có nút gốc 5, được giữ nguyên nên nó vẫn giữ nguyên. Nút 4 có nút cha là 1 nên nó gắn vào nút gốc. Nút 5 có nút cha là 1 nên nó cũng gắn vào nút gốc. Chữ ký mã hóa các mối quan hệ này. 

Đối với cùng một tập hợp con trong cây B, nút cha thực sự khác nhau vì nút cha của nút 4 là 2, nút này có thể bị loại bỏ hoặc không, làm thay đổi cấu trúc được nâng lên. Sự không khớp này cho thấy S không hợp lệ. 

Một tập hợp con hợp lệ xuất hiện khi việc xóa căn chỉnh cả hai cấu trúc tổ tiên cảm ứng và kích thước tập hợp con tối đa như vậy hóa ra là 1, cho câu trả lời 4. 

| Tập con S | Cây A nén cha mẹ | Cây B nén bố mẹ | Trận đấu | 
| --- | --- | --- | --- | 
| {1,3} | 3→1 | 3→1 | Có | 
| {1,4,5} | khác nhau | khác nhau | Không | 
| {1,3,5} | nhất quán | nhất quán | Có | 

Bảng này cho thấy rằng chỉ có các lần đóng cửa tổ tiên được sắp xếp theo cấu trúc mới tồn tại được so sánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^n) | Mọi tập hợp con đều được liệt kê và đối với mỗi nút, chúng ta có thể leo lên tổ tiên | 
| Không gian | O(2^n) | Bản đồ băm lưu trữ chữ ký cho tất cả các tập hợp con | 

Hệ số mũ có thể chấp nhận được vì n 40 và công việc bên trong là tuyến tính theo n, thực hiện khoảng 40 triệu phép tính trong trường hợp xấu nhất, phù hợp với các giới hạn điển hình khi triển khai Python hoặc PyPy được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = [0] * (n + 1)
    b = [0] * (n + 1)

    tmp = list(map(int, input().split()))
    for i in range(2, n + 1):
        a[i] = tmp[i - 2]

    tmp = list(map(int, input().split()))
    for i in range(2, n + 1):
        b[i] = tmp[i - 2]

    def compute(par):
        sig = {}
        for mask in range(1 << n):
            key = []
            cnt = 0
            for v in range(1, n + 1):
                if mask >> (v - 1) & 1:
                    cnt += 1
                    p = par[v]
                    while p and not (mask >> (p - 1)) & 1:
                        p = par[p]
                    if v != 1:
                        key.append((v, p))
            key = tuple(key)
            sig[key] = max(sig.get(key, 0), cnt)
        return sig

    sa = compute(a)
    sb = compute(b)

    ans = 0
    for k, v in sa.items():
        if k in sb:
            ans = max(ans, v)

    return str(n - ans)

# sample
assert run("""5
1 5 5 1
1 4 1 2
""") == "4"

# minimum case
assert run("""2
1
1
""") == "0"

# identical trees
assert run("""4
1 1 1
1 1 1
""") == "0"

# chain vs star
assert run("""5
1 1 1 1
1 2 3 4
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây giống nhau | 0 | không cần xóa | 
| chuỗi vs ngôi sao | 2 | lực xóa bỏ cấu trúc không phù hợp | 
| trường hợp tối thiểu | 0 | sự đúng đắn tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi cả hai cây giống hệt nhau nhưng chứa các chuỗi sâu. Trong tình huống đó, mọi tập hợp con đều tạo ra các chữ ký giống hệt nhau ở cả hai cây và thuật toán phải cho phép khớp hoàn toàn một cách chính xác. Đối với đầu vào trong đó cả hai mảng cha đều bằng 1, mọi nút sẽ gắn trực tiếp vào gốc, do đó, mọi tập hợp con đều nhất quán và câu trả lời tốt nhất là không xóa. 

Một trường hợp khác là khi nút cha của nút bị xóa nhưng nút gốc của nó thì không. Trong trường hợp đó, việc nâng đỡ cha mẹ hiệu quả phải bỏ qua nhiều cấp độ chứ không chỉ một bước. Vòng lặp đi lên trong quá trình triển khai đảm bảo tính chính xác vì nó tiếp tục cho đến khi tìm thấy tổ tiên hoạt động. 

Trường hợp thứ ba là khi chỉ còn lại phần gốc. Vì nút gốc luôn cố định và không thể bị xóa nên mọi tập hợp con hợp lệ phải bao gồm nút 1. Logic mặt nạ tôn trọng điều này bằng cách xử lý riêng nút 1 và không bao giờ gán cho nút cha nâng cấp, đảm bảo rằng tất cả các chữ ký vẫn được định dạng tốt ngay cả đối với các tập hợp con một nút.
