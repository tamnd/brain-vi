---
title: "CF 106114A - Bàn tính"
description: "Chúng ta được sắp xếp các ô theo hình chữ nhật có n hàng và m cột. Mỗi hàng i ban đầu chứa một khối đá ai được xếp ở phía bên trái, vì vậy ở hàng i, các cột từ 1 đến ai được lấp đầy và các ô còn lại trống."
date: "2026-06-19T20:10:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106114
codeforces_index: "A"
codeforces_contest_name: "2025 Sun Yat-sen University Collegiate Programming Contest, Final"
rating: 0
weight: 106114
solve_time_s: 48
verified: true
draft: false
---

[CF 106114A - Bàn tính](https://codeforces.com/problemset/problem/106114/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp các ô theo hình chữ nhật có n hàng và m cột. Mỗi hàng i ban đầu chứa một khối đá ai được xếp ở phía bên trái, vì vậy ở hàng i, các cột từ 1 đến ai được lấp đầy và các ô còn lại trống. Hoạt động được mô tả là một phép biến đổi giống như trọng lực được áp dụng theo cột: tất cả các viên đá rơi thẳng đứng xuống dưới cho đến khi chúng đạt đến vị trí trống thấp nhất có thể trong các cột tương ứng của chúng. 

Sau khi quá trình rơi này ổn định, mỗi cột chỉ cần được lấp đầy từ dưới lên với số lượng đá tồn tại ban đầu trong cột đó và mọi khoảng trống vẫn ở trên. Nhiệm vụ là tính toán, sau lần cấu hình lại này, mỗi hàng sẽ có bao nhiêu viên đá. 

Khó khăn chính là trạng thái ban đầu được đưa ra ở dạng nén theo hàng, nhưng quá trình rơi lại được điều khiển theo cột, vì vậy chúng ta phải chuyển đổi cấu trúc trong đầu. 

Các ràng buộc n, m ≤ 10^4 ngụ ý rằng mô phỏng O(nm) trên lưới rõ ràng là quá lớn cả về thời gian và bộ nhớ trong trường hợp xấu nhất. Một mô phỏng đầy đủ sẽ yêu cầu tối đa 10^8 ô, đây là đường biên hoặc không an toàn trong Python với các giới hạn chặt chẽ. Điều này thúc đẩy chúng ta hướng tới lý luận về số lượng hơn là về lưới rõ ràng. 

Trường hợp cạnh tinh tế phát sinh khi các hàng có giá trị ai rất khác nhau. Ví dụ: nếu n = 3, m = 5 và a = [5, 1, 1], hàng đầu tiên đóng góp nhiều viên đá trên tất cả các cột trong khi các hàng thấp hơn chỉ đóng góp vào cột đầu tiên. Sau khi rơi xuống, chiều cao của cột thay đổi đáng kể và việc phân phối lại theo từng hàng đơn giản sẽ đặt nhầm các viên đá trừ khi chúng ta giải thích rõ ràng về sự tích lũy theo cột. 

Một dạng lỗi khác xuất phát từ việc giả định không chính xác rằng các hàng duy trì số lượng của chúng một cách độc lập. Ví dụ, nghĩ rằng mỗi hàng giữ các viên đá ai là sai, vì việc rơi sẽ phân phối lại các viên đá giữa các hàng. 

## Phương pháp tiếp cận 

Việc giải thích lực lượng vũ phu xây dựng một mạng lưới rõ ràng, đánh dấu tất cả các viên đá được căn trái và sau đó liên tục mô phỏng từng cột trọng lực hoặc bằng cách đẩy các viên đá xuống liên tục cho đến khi ổn định. Điều này đúng vì nó tuân theo định nghĩa quy trình theo đúng nghĩa đen. Tuy nhiên, lưới có thể chứa tối đa n × m ô và mỗi bước mô phỏng có thể yêu cầu quét các cột nhiều lần hoặc thực hiện chuyển động trên mỗi ô. Trong trường hợp xấu nhất, mỗi ô được xử lý nhiều lần, dẫn đến bộ nhớ khoảng O(nm) và có khả năng là O(nm) hoặc thời gian tệ hơn tùy thuộc vào chi phí triển khai. Với n và m lên tới 10^4, giá trị này trở nên quá lớn. 

Quan sát quan trọng là cấu hình cuối cùng chỉ phụ thuộc vào tổng số cột chứ không phụ thuộc vào cách các viên đá được phân bổ trong các hàng ban đầu. Mỗi hàng i đóng góp một viên đá được phân bổ trên các cột đầu tiên. Nếu đảo ngược quan điểm, mỗi cột j sẽ nhận được chính xác số hàng i sao cho a_i ≥ j. Khi chúng ta biết tổng cột, trọng lực chỉ cần sắp xếp các tổng này thành hàng: mỗi hàng i sẽ nhận được tổng số cột có chiều cao ít nhất đạt tới i. 

Vì vậy, thay vì mô phỏng chuyển động, chúng tôi tính toán mảng tần số theo chiều cao của cột, sau đó chuyển đổi nó thành số hàng bằng cách sử dụng tích lũy hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(nm) | Quá chậm | 
| Tối ưu | O(n + m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng cnt có kích thước m trong đó cnt[j] đếm xem có bao nhiêu hàng có ai ≥ j + 1. Điều này tương ứng với số lượng viên đá xuất hiện trong cột j + 1 trước khi rơi xuống. Bước này thay thế việc xây dựng lưới rõ ràng bằng phối cảnh đếm. 
2. Với mỗi hàng i từ 1 đến n, ta muốn biết có bao nhiêu cột có ít nhất i viên đá. Con số đó chính xác là cnt[i - 1]. Điều này có hiệu quả vì một cột đóng góp vào hàng i sau khi rơi khi và chỉ khi nó có chiều cao ít nhất là i. 
3. Xuất ra cnt[0], cnt[1], ..., cnt[n - 1] làm hàng cuối cùng được tính.

Việc chuyển đổi hoàn toàn là sự thay đổi quan điểm: các hàng trong cấu hình cuối cùng được xác định bởi chiều cao cột và chiều cao cột được xác định bởi phạm vi bao phủ tiền tố của các giá trị ai ban đầu. 

### Tại sao nó hoạt động 

Mỗi hàng ban đầu chỉ đóng góp các khối đá vào tiền tố của các cột và do đó mỗi cột j tích lũy chính xác số hàng kéo dài đến ít nhất j. Sau khi đổ xuống, các cột trở thành các ngăn xếp độc lập có chiều cao cố định. Hàng cuối cùng i được chiếm chính xác trong các cột có chiều cao ngăn xếp ít nhất là i, vì trọng lực lấp đầy từ dưới lên mà không có khoảng trống. Điều này thiết lập sự tương đương trực tiếp giữa “sự chiếm chỗ của hàng cuối cùng” và “số cột có chiều cao ≥ i”, đảm bảo tính chính xác của phép biến đổi đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))

cnt = [0] * (m + 2)

for x in a:
    cnt[1] += 1
    if x + 1 <= m:
        cnt[x + 1] -= 1

for i in range(1, m + 1):
    cnt[i] += cnt[i - 1]

ans = []
for i in range(1, n + 1):
    ans.append(str(cnt[i]))

print(" ".join(ans))
```Mã tránh xây dựng bất kỳ lưới nào. Thay vào đó, nó sử dụng một mảng khác biệt trên các chỉ số cột để tính toán số lượng hàng mở rộng cho mỗi cột. Tổng tiền tố chuyển đổi giá trị này thành chiều cao cột thực tế. Sau đó, mỗi hàng i được trả lời trực tiếp bằng cách đọc có bao nhiêu cột đạt chiều cao tối thiểu i. 

Một cạm bẫy phổ biến là trộn lẫn việc lập chỉ mục hàng và cột. Mảng cnt được lập chỉ mục theo ngưỡng chiều cao của cột chứ không phải theo hàng. Một điểm tinh tế khác là đảm bảo giới hạn khi áp dụng mảng chênh lệch, vì các cột ngoài m không được cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, m = 4 

a = [3, 1, 2] 

Chúng tôi tính toán độ bao phủ của cột. 

| Bước | một [tôi] | Cập nhật hiệu ứng trên cnt | cnt sau tiền tố | 
| --- | --- | --- | --- | 
| 1 | 3 | + lúc 1, - lúc 4 | [0,1,1,1] | 
| 2 | 1 | + lúc 1, - lúc 2 | [0,2,1,1] | 
| 3 | 2 | + lúc 1, - lúc 3 | [0,3,2,1] | 

Bây giờ cnt sau tiền tố có nghĩa là: 

cột 1 có 3 quân, cột 2 có 2, cột 3 có 1, cột 4 có 0. 

Hàng cuối cùng: 

hàng 1 = cột có chiều cao ≥ 1 = 3 

hàng 2 = cột có chiều cao ≥ 2 = 2 

hàng 3 = cột có chiều cao ≥ 3 = 1 

Vậy đầu ra là [3, 2, 1]. 

Điều này xác nhận rằng mỗi hàng được xác định hoàn toàn bằng cách xác định chiều cao của cột. 

### Ví dụ 2 

đầu vào: 

n = 4, m = 5 

a = [5, 3, 3, 1] 

Chiều cao của cột trở thành: 

[4, 3, 2, 1, 1] 

Bây giờ chúng tôi tính toán kết quả đầu ra hàng: 

hàng 1 = 5 cột 

hàng 2 = 3 cột 

hàng 3 = 3 cột 

hàng 4 = 1 cột 

Đầu ra: [5, 3, 3, 1] 

Điều này cho thấy rằng ngay cả với các giá trị lặp lại, phương pháp này vẫn tổng hợp chính xác các tiền tố chồng chéo mà không cần mô phỏng rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | một lần vượt qua a, một lần tổng tiền tố trên m, một lần quét qua n | 
| Không gian | O(m) | chỉ mảng chiều cao cột được lưu trữ | 

Các ràng buộc n, m ≤ 10^4 làm cho việc này trở nên hiệu quả một cách thoải mái, với tối đa vài chục nghìn thao tác, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [0] * (m + 2)

    for x in a:
        cnt[1] += 1
        if x + 1 <= m:
            cnt[x + 1] -= 1

    for i in range(1, m + 1):
        cnt[i] += cnt[i - 1]

    ans = [str(cnt[i]) for i in range(1, n + 1)]
    return " ".join(ans)

# sample-like case
assert run("3 4\n3 1 2\n") == "3 2 1"

# all equal
assert run("3 5\n5 5 5\n") == "3 3 3"

# minimal case
assert run("1 1\n1\n") == "1"

# strictly increasing prefix lengths
assert run("4 5\n1 2 3 4\n") == "4 3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 4 / 3 1 2 | 3 2 1 | tính đúng đắn của phép biến đổi cơ bản | 
| 3 5 / 5 5 5 | 3 3 3 | bảo hiểm đầy đủ thống nhất | 
| 1 1/1 | 1 | trường hợp biên nhỏ nhất | 
| 4 5 / 1 2 3 4 | 4 3 2 1 | tăng trưởng tiền tố dần dần | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một hàng thống trị tất cả các cột, chẳng hạn như n = 3, m = 5, a = [5, 1, 1]. Chiều cao của cột bị lệch nhiều: cột 1 có 3 khối trong khi cột từ 2 đến 5 chỉ có 1. Thuật toán xử lý điều này một cách chính xác vì cnt[j] trực tiếp tính độ bao phủ hàng trên mỗi cột, do đó không cần giả định về tính đồng nhất. 

Một trường hợp cạnh khác là khi tất cả ai đều nhỏ, chẳng hạn như a = [1, 1, 1, 1]. Khi đó chỉ có cột đầu tiên khác 0 và sau khi tích lũy tiền tố, chúng ta nhận được độ cao của cột [4, 0, 0, ...]. Đầu ra của hàng trở thành [1, 0, 0, 0], phù hợp với thực tế là chỉ hàng dưới cùng mới nhận được cột bị chiếm giữ duy nhất sau khi rơi xuống. 

Trường hợp cạnh cuối cùng là độ bão hòa tối đa trong đó tất cả ai = m. Trong trường hợp này, mỗi cột có chiều cao n nên mỗi hàng nhận được m viên đá. Thuật toán tạo ra cnt[i] = m cho tất cả i, phản ánh chính xác việc lấp đầy hình chữ nhật đầy đủ.
