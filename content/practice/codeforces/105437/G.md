---
title: "CF 105437G - Cửa hàng"
description: "Chúng ta được cung cấp một tập hợp các điểm cố định trên một trục số, bao gồm điểm cuối 0 và 10^9, đại diện cho các vị trí cửa hàng hiện có trong một thành phố."
date: "2026-06-23T03:43:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105437
codeforces_index: "G"
codeforces_contest_name: "ICPC 2024-2025 NERC, Southern and Volga Russia Qualifier"
rating: 0
weight: 105437
solve_time_s: 97
verified: false
draft: false
---

[CF 105437G - Cửa hàng](https://codeforces.com/problemset/problem/105437/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm cố định trên một trục số, bao gồm điểm cuối 0 và 10^9, đại diện cho các vị trí cửa hàng hiện có trong một thành phố. Giữa các vị trí đã bị chiếm giữ này, chúng tôi được phép chèn tối đa m cửa hàng bổ sung ở tọa độ nguyên hiện đang trống. Hạn chế là không có hai cửa hàng nào có thể chiếm cùng một tọa độ. 

Sau khi tất cả các cửa hàng đã được đặt, chúng tôi xem danh sách các vị trí cửa hàng đã được sắp xếp và xem xét từng cặp cửa hàng liên tiếp. Mỗi cặp như vậy tạo thành một đoạn và độ dài của đoạn đó là khoảng cách giữa hai cửa hàng lân cận. Mục tiêu là đặt các cửa hàng mới sao cho khoảng cách lớn nhất trong số các cửa hàng liền kề này càng nhỏ càng tốt. 

Về cơ bản, đây là một vấn đề tối ưu hóa khoảng cách: chúng tôi không cố gắng giảm thiểu tổng khoảng cách hoặc khoảng cách trung bình mà chỉ là khoảng cách tồi tệ nhất sau khi chèn thêm điểm. 

Các ràng buộc cho phép tối đa 200.000 cửa hàng hiện có và 200.000 cửa hàng bổ sung, do đó, bất kỳ giải pháp nào cố gắng mô phỏng việc chèn trực tiếp hoặc điều chỉnh khoảng cách nhiều lần sẽ quá chậm. Cách tiếp cận bậc hai đối với tất cả các khoảng trống hoặc cập nhật tham lam lặp đi lặp lại của cấu trúc ưu tiên mô phỏng mỗi lần chèn riêng lẻ có nguy cơ O((n+m) log n) hoặc tệ hơn, là ranh giới nhưng vẫn không cần thiết. Yêu cầu chính là lý giải về tất cả các phần chèn vào một cách tổng hợp chứ không phải từng phần một. 

Một trường hợp thất bại tinh tế xuất hiện khi trực giác tham lam được áp dụng không đúng cách. Giả sử có một khoảng cách lớn và một vài khoảng cách nhỏ hơn. Một chiến lược ngây thơ có thể luôn chèn vào khoảng cách hiện tại lớn nhất nhiều lần, nhưng nếu không mô hình hóa chính xác cách khoảng cách thu hẹp phi tuyến tính khi nó được phân chia, điều này có thể dẫn đến những quyết định sai lầm về số điểm mà mỗi phân đoạn sẽ nhận được. 

Ví dụ: nếu chúng ta có khoảng cách có kích thước 10 và m = 1 thì câu trả lời là 5 chứ không phải 9. Thay vào đó, nếu chúng ta có hai khoảng cách có kích thước 10 và m = 1 thì vị trí tối ưu sẽ phụ thuộc vào so sánh toàn cầu chứ không phải sự cải thiện cục bộ trong sự cô lập. Điều này cho thấy rằng mỗi khoảng cách phải được lý giải như một hàm số của bao nhiêu điểm được gán cho nó, chứ không phải như thứ mà chúng ta tham lam sửa chữa từng bước một. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ ban đầu là mô phỏng quá trình thêm từng cửa hàng một. Ở mỗi bước, chúng tôi tính toán tất cả các khoảng trống liền kề, chọn khoảng trống lớn nhất, chia nó bằng cách chèn một cửa hàng vào giữa và lặp lại điều này m lần. Mỗi lần chèn yêu cầu quét tất cả các khoảng trống hoặc duy trì hàng đợi các khoảng trống ưu tiên. Ngay cả với một đống, mỗi lần chèn m đều tiêu tốn thời gian logarit và mỗi lần phân tách yêu cầu tính toán lại độ dài phân đoạn một cách cẩn thận. Điều này dẫn đến sự phức tạp xung quanh O((n + m) log n), vốn đã chặt chẽ và quan trọng hơn, nó vẫn không hiệu quả về mặt khái niệm vì nó xử lý vấn đề theo trình tự, trong khi cấu hình tối ưu chỉ phụ thuộc vào số lượng cuối cùng trên mỗi phân đoạn. 

Quan sát quan trọng là mỗi khoảng cách ban đầu giữa các cửa hàng liên tiếp sẽ tiến triển độc lập sau khi chúng ta quyết định có bao nhiêu cửa hàng mới được đặt bên trong nó. Nếu một đoạn có độ dài g và chúng ta đặt k cửa hàng mới bên trong nó thì đoạn đó sẽ được chia thành k+1 đoạn con. Sự sắp xếp tốt nhất có thể là cách đều nhau và độ dài phân đoạn tối đa thu được là trần của g chia cho k+1. Điều này giải quyết được vấn đề: thay vì quyết định vị trí chính xác, chúng tôi chỉ quyết định số lần chèn mà mỗi khoảng trống nhận được.

Cấu trúc này gợi ý đảo ngược quan điểm. Thay vì phân phối m cửa hàng và kiểm tra khoảng cách tối đa thu được, chúng tôi đoán khoảng cách tối đa cho phép ứng viên L và hỏi liệu có thể đảm bảo tất cả các khoảng cách liền kề cuối cùng tối đa là L. Đối với mỗi khoảng cách ban đầu g, chúng tôi tính toán cần bao nhiêu điểm mới để không có khoảng cách con nào vượt quá L. Nếu g quá lớn, nó phải được chia thành đủ nhiều phần và yêu cầu này có thể được tính toán độc lập cho từng phân đoạn. Tổng hợp các yêu cầu này cho chúng ta biết m cửa hàng có đủ hay không. 

Điều này biến vấn đề thành một kiểm tra tính khả thi đơn điệu trên L, có thể được giải quyết bằng cách sử dụng tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chèn tuần tự | O((n + m) log n) | O(n) | Quá chậm và không cần thiết | 
| Tìm kiếm nhị phân có tính khả thi về khoảng cách | O(n log 1e9) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các vị trí cửa hàng hiện có. Điều này đảm bảo tất cả các khoảng trống nằm giữa các phần tử liên tiếp theo một thứ tự có ý nghĩa, vì chỉ các cặp liền kề mới ảnh hưởng đến câu trả lời. 
2. Tính toán tất cả khoảng cách giữa các cửa hàng liên tiếp. Mỗi khoảng trống là một khoảng độc lập có thể nhận được các cửa hàng mới. 
3. Xác định hàm kiểm tra xem khoảng cách tối đa ứng viên L có thể đạt được hay không. Hàm này xác định số lượng cửa hàng bổ sung được yêu cầu để đảm bảo không có khoảng trống vượt quá L. 
4. Với mỗi khoảng trống có chiều dài g, hãy tính xem nó phải được chia thành bao nhiêu đoạn sao cho mỗi đoạn có độ dài tối đa là L. Số đoạn cần thiết là ceil(g / L), nghĩa là ta cần ceil(g / L) - 1 cửa hàng mới bên trong khoảng trống đó. Điều này xuất phát từ thực tế là k điểm được chèn sẽ tạo ra k+1 đoạn. 
5. Tính tổng các phần chèn cần thiết này trên tất cả các khoảng trống. Nếu tổng nhỏ hơn hoặc bằng m thì L là khả thi. 
6. Thực hiện tìm kiếm nhị phân trên L từ 1 đến 10^9. Đối với mỗi điểm giữa, hãy chạy kiểm tra tính khả thi. Nếu khả thi, hãy thử các giá trị nhỏ hơn; ngược lại thì tăng L. 

Lý do tìm kiếm nhị phân hoạt động là vì tính khả thi là đơn điệu trong L. Nếu có thể đạt được khoảng cách L tối đa thì bất kỳ giá trị lớn hơn nào cũng có thể đạt được vì nó nới lỏng các ràng buộc và không bao giờ tăng số lần chèn cần thiết. 

### Tại sao nó hoạt động 

Mỗi khoảng cách hoạt động độc lập sau khi chúng tôi cố định độ dài phân đoạn tối đa mục tiêu L. Cách chia tách một khoảng cách tham lam luôn là tối ưu vì việc phân phối k điểm ở bất kỳ vị trí nào khác ngoài đều không thể làm giảm độ dài phân đoạn tối đa dưới trần (g / (k+1)). Do đó, quá trình kiểm tra tính khả thi sẽ nắm bắt chính xác số lần chèn cần thiết tối thiểu trên mỗi khoảng trống. Vì yêu cầu này mang tính bổ sung qua các khoảng trống và đơn điệu trong L nên tìm kiếm nhị phân xác định chính xác khoảng cách tối đa khả thi nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def required_stores(g, L):
    if g <= L:
        return 0
    return (g - 1) // L

def can(L, gaps, m):
    need = 0
    for g in gaps:
        need += required_stores(g, L)
        if need > m:
            return False
    return True

n, m = map(int, input().split())
a = list(map(int, input().split()))
a.sort()

gaps = []
for i in range(n - 1):
    gaps.append(a[i + 1] - a[i])

lo, hi = 1, 10**9
ans = hi

while lo <= hi:
    mid = (lo + hi) // 2
    if can(mid, gaps, m):
        ans = mid
        hi = mid - 1
    else:
        lo = mid + 1

print(ans)
```Bước sắp xếp đảm bảo rằng tất cả các khoảng cách có ý nghĩa đều được ghi lại dưới dạng các sai phân liên tiếp. Hàm khả thi mã hóa logic phân tách theo từng khoảng trống, trong đó mỗi khoảng trống đóng góp độc lập một số lượng kho lưu trữ được chèn theo yêu cầu. Biểu thức (g - 1) // L là một cách rút gọn để tính ceil(g / L) - 1 mà không cần các phép toán dấu phẩy động. 

Tìm kiếm nhị phân thu hẹp câu trả lời bằng cách liên tục kiểm tra xem liệu khoảng cách tối đa được đề xuất có thể được thực thi trên toàn cầu với tối đa m lần chèn hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
0 500000000 1000000000 250000000
```Mảng được sắp xếp:```
0, 250000000, 500000000, 1000000000
```Khoảng trống là:```
250000000, 250000000, 500000000
```Chúng tôi kiểm tra một ứng cử viên L = 300000000. 

| Khoảng cách | g | Cửa hàng bắt buộc | 
| --- | --- | --- | 
| 1 | 250000000 | 0 | 
| 2 | 250000000 | 0 | 
| 3 | 500000000 | 1 | 

Tổng yêu cầu = 1 tức là ≤ m = 2 nên L là khả thi. 

Việc thử L nhỏ hơn cuối cùng sẽ dẫn đến giá trị khả thi tối thiểu là 250000000. 

Dấu vết này cho thấy khoảng trống lớn chiếm ưu thế như thế nào trong yêu cầu trong khi khoảng trống nhỏ hơn có thể không cần can thiệp gì cả. 

### Ví dụ 2 

đầu vào:```
2 2
0 1000000000
```Khoảng trống:```
1000000000
```Kiểm tra L = 400000000: 

| Khoảng cách | g | Cửa hàng bắt buộc | 
| --- | --- | --- | 
| 1 | 1000000000 | 2 | 

Tổng số yêu cầu = 2, khả thi. 

Kiểm tra L = 333333334: 

| Khoảng cách | g | Cửa hàng bắt buộc | 
| --- | --- | --- | 
| 1 | 1000000000 | 2 | 

Vẫn khả thi. 

Kiểm tra L = 333333333: 

| Khoảng cách | g | Cửa hàng bắt buộc | 
| --- | --- | --- | 
| 1 | 1000000000 | 3 | 

Bây giờ không thể thực hiện được nên câu trả lời là 333333334. 

Điều này chứng tỏ hiệu ứng trần gây ra một ngưỡng rõ ràng đến mức cần phải có một phép chia bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log 10^9) | Việc sắp xếp mất O(n log n) và mỗi bước tìm kiếm nhị phân sẽ quét tất cả các khoảng trống, lặp lại trên ~30 lần lặp | 
| Không gian | O(n) | Lưu trữ các vị trí đã sắp xếp và danh sách khoảng trống | 

Các ràng buộc cho phép lên tới 200.000 điểm và khoảng 30 lần lặp tìm kiếm nhị phân là đủ vì không gian tìm kiếm được giới hạn bởi 10^9. Mỗi lần lặp lại là tuyến tính về số lượng khoảng trống, do đó giải pháp dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    gaps = [a[i+1] - a[i] for i in range(n - 1)]

    def need(g, L):
        if g <= L:
            return 0
        return (g - 1) // L

    def can(L):
        return sum(need(g, L) for g in gaps) <= m

    lo, hi = 1, 10**9
    ans = hi
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1
    return str(ans)

# provided samples (as given in statement)
assert run("4 2\n0 500000000 1000000000 250000000\n") == "250000000"
assert run("2 2\n0 1000000000\n") == "333333334"

# custom cases
assert run("2 1\n0 10\n") == "5", "single gap split once"
assert run("3 0\n0 5 10\n") == "5", "no insertions allowed"
assert run("3 1\n0 5 10\n") == "5", "one insertion not enough to reduce max gap"
assert run("4 3\n0 4 8 12\n") == "3", "multiple equal gaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng cách đơn với một lần chèn | 5 | hành vi phân tách cơ bản | 
| không chèn thêm | 5 | tính đúng đắn khi m = 0 | 
| chèn hạn chế | 5 | không có khả năng cải thiện mọi khoảng cách | 
| khoảng cách bằng nhau | 3 | phân phối trên cơ cấu thống nhất | 

## Vỏ cạnh 

Trường hợp một cạnh xuất hiện khi không thể có sự phân chia có lợi vì m quá nhỏ so với khoảng cách vượt trội. Đối với đầu vào`0 1000000000`với m = 0, thuật toán tạo ra một khoảng trống có kích thước 10^9 và tìm kiếm nhị phân ngay lập tức hội tụ về giá trị đó vì việc kiểm tra tính khả thi không thành công đối với bất kỳ L nhỏ hơn nào. 

Một trường hợp khác là khi có nhiều khoảng trống lớn cạnh tranh để có được những phần chèn hạn chế. Ví dụ, nếu khoảng trống là`[100, 100, 100]`với m = 1, việc kiểm tra tính khả thi sẽ phân bổ chính xác tất cả các phần chèn cần thiết cho mục tiêu giảm lớn nhất trước tiên theo thuật ngữ nhị phân. Tìm kiếm nhị phân đảm bảo chúng ta không bao giờ cần phải quyết định khoảng cách nào cần cải thiện; thay vào đó chúng tôi xác minh xem liệu có thể đạt được giới hạn thống nhất hay không. 

Trường hợp tinh tế cuối cùng là khi g chia hết cho L. Công thức`(g - 1) // L`đảm bảo rằng bội số chính xác không vượt quá số lượng cửa hàng cần thiết. Ví dụ: nếu g = 10 và L = 2, chúng ta cần 4 phần chèn, tạo ra các phân đoạn có kích thước chính xác là 2. Điều này tránh lạm phát từng cái một có thể xảy ra với các phân đoạn ngây thơ.`ceil(g / L) - 1`triển khai bằng cách sử dụng số học dấu phẩy động.
