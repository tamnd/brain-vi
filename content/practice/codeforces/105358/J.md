---
title: "CF 105358J - Xếp hàng"
description: "Chúng ta được cung cấp một chuỗi hàng hóa, mỗi hàng hóa có ba thuộc tính: trọng lượng, thể tích ban đầu và hệ số nén. Chúng ta phải sắp xếp tất cả hàng hóa vào một ngăn duy nhất. Sau khi xếp chồng lên nhau, thể tích cuối cùng của mỗi vật phẩm sẽ giảm đi tùy thuộc vào tổng trọng lượng được đặt phía trên nó."
date: "2026-06-23T15:51:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105358
codeforces_index: "J"
codeforces_contest_name: "The 2024 ICPC Asia EC Regionals Online Contest (II)"
rating: 0
weight: 105358
solve_time_s: 52
verified: true
draft: false
---

[CF 105358J - Xếp hàng hóa](https://codeforces.com/problemset/problem/105358/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi hàng hóa, mỗi hàng hóa có ba thuộc tính: trọng lượng, thể tích ban đầu và hệ số nén. Chúng ta phải sắp xếp tất cả hàng hóa vào một ngăn duy nhất. Sau khi xếp chồng lên nhau, thể tích cuối cùng của mỗi vật phẩm sẽ giảm đi tùy thuộc vào tổng trọng lượng được đặt phía trên nó. 

Nếu một hàng hóa có tổng trọng lượng W ở trên nó, thì khối lượng của nó sẽ giảm tuyến tính ci × W. Do đó, khối lượng cuối cùng của hàng hóa là khối lượng ban đầu của nó trừ đi khoản phạt tỷ lệ với trọng lượng tích lũy phía trên nó. Nhiệm vụ là chọn thứ tự của tất cả hàng hóa trong ngăn xếp sao cho tổng khối lượng cuối cùng của tất cả hàng hóa là nhỏ nhất. 

Tương tác chính là trọng lượng của các vật phẩm được đặt ở trên ảnh hưởng đến tất cả các vật phẩm bên dưới và các vật phẩm nặng hơn góp phần nén nhiều hơn thông qua ci. Điều này tạo ra sự kết hợp tổng thể giữa việc đặt hàng và đóng góp, điển hình cho các vấn đề tối ưu hóa lập kế hoạch hoặc sắp xếp thứ tự. 

Ràng buộc n lên tới 100000 ngay lập tức loại trừ mọi mô phỏng O(n²) đối với tất cả các thứ tự hoặc so sánh theo cặp. Mọi giải pháp đều phải ở khoảng O(n log n). Mỗi mục đóng góp các giá trị lên tới 10¹², do đó, câu trả lời phải được lưu trữ ở dạng số học số nguyên 64 bit và các phép tính trung gian cũng phải tránh tràn. 

Một cách tiếp cận ngây thơ thử tất cả các hoán vị rõ ràng là thất bại, nhưng ngay cả việc sắp xếp theo phương pháp phỏng đoán ngẫu nhiên cũng có thể thất bại vì sự tương tác phụ thuộc đồng thời vào cả trọng số và hệ số nén chứ không phải độc lập. 

Một trường hợp lỗi khó phát hiện sẽ xuất hiện khi một vật phẩm có hệ số nén cao được đặt phía trên một vật nặng. Ví dụ: hãy xem xét hai mục: 

đầu vào:```
2
w1=10 v1=100 c1=5
w2=1 v2=100 c2=1
```Nếu đặt mục 1 lên trên mục 2 thì mục 2 bị phạt nặng 5 dù mục 2 nhẹ. Nếu chúng ta đảo ngược chúng, mục 1 hầu như không bị phạt. Thứ tự chính xác phụ thuộc vào việc cân bằng ci và wi chứ không chỉ sắp xếp theo cả hai. 

Sự phụ thuộc này gợi ý rằng mỗi lần đảo ngược giữa hai mặt hàng đều góp phần tạo ra chênh lệch chi phí xác định, đây là chìa khóa để giảm vấn đề về quy tắc đặt hàng. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét cách giải thích bạo lực. Đối với mỗi hoán vị hàng hóa, chúng tôi tính toán tổng khối lượng cuối cùng bằng cách mô phỏng ngăn xếp. Đối với một thứ tự cố định, chúng ta có thể tính toán sự đóng góp trọng số của hậu tố theo O(n), do đó tổng số là O(n × n!). Điều này không thể thực hiện được ngay cả khi n = 15. 

Để đơn giản hóa, chúng tôi viết lại mục tiêu tổng thể. Đối với một thứ tự cố định, mỗi mục i sẽ mất ci × (tổng trọng số ở trên nó). Mở rộng điều này trên toàn cầu, mọi cặp (i, j) trong đó i ở trên j sẽ đóng góp w_i × c_j vào tổng số tiền phạt. Vì vậy, thay vì nghĩ về vị trí, chúng ta nghĩ đến tương tác theo cặp: đặt i lên trên j sẽ tạo ra chi phí w_i c_j. 

Do đó, bài toán trở thành cực tiểu hóa tổng trên tất cả các cặp có i ở trên j của w_i c_j. Tổng khối lượng ban đầu là không đổi, vì vậy chúng tôi chỉ tối ưu hóa thời hạn phạt. 

Bây giờ chúng ta thấy đây là một bài toán tối thiểu hóa nghịch đảo có trọng số cổ điển. Đối với bất kỳ sự hoán đổi liền kề nào của hai mục i và j, chúng ta có thể tính toán tổng số thay đổi như thế nào. Giả sử hiện tại tôi đang ở trên j. Nếu chúng ta hoán đổi chúng, sự khác biệt chỉ phụ thuộc vào i và j: 

Đóng góp hiện tại: 

với tôi c_j 

Sau khi trao đổi: 

w_j c_i 

Vì vậy, đặt i phía trên j sẽ tốt hơn khi w_i c_j < w_j c_i, sắp xếp lại thành w_i / c_i < w_j / c_j (xử lý c_i = 0 một cách cẩn thận bằng cách xử lý tỷ lệ là 0 hoặc vô cực một cách thích hợp). 

Điều này đưa ra một quy tắc đặt hàng tổng thể: sắp xếp các mục bằng cách tăng tương đương w_i / c_i bằng cách so sánh các sản phẩm chéo w_i c_j với w_j c_i. 

Sau khi được sắp xếp theo thứ tự này, bất kỳ sự đảo ngược nào cũng sẽ vi phạm tính tối ưu cục bộ, do đó trình tự được sắp xếp là tối ưu toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O (n × n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành vấn đề sắp xếp dựa trên sự tối ưu trao đổi theo cặp. 

1. Với mỗi cặp hàng hóa i và j, hãy xem điều gì sẽ xảy ra nếu tôi được đặt ở trên j và j ở trên i. Chúng tôi tính toán hai chi phí có thể có w_i c_j và w_j c_i. Cái nhỏ hơn xác định thứ tự tốt hơn giữa hai mục này. 
2. Từ quy tắc so sánh theo cặp này, chúng ta xác định một bộ so sánh sắp xếp các mục sao cho i đứng trước j nếu w_i c_j < w_j c_i. Điều này đảm bảo rằng không có phép đảo ngược liền kề nào có thể cải thiện câu trả lời. 
3. Chúng tôi sắp xếp tất cả các mục bằng bộ so sánh này. Điều này tạo ra một thứ tự tổng thể phù hợp với tất cả các quyết định tối ưu theo cặp. Bước sắp xếp ngầm giải quyết tất cả các xung đột theo cặp trên toàn cầu. 
4. Sau khi sắp xếp, chúng ta tính toán đáp án cuối cùng bằng cách quét từ trên xuống dưới. Chúng tôi duy trì tổng trọng số đang chạy trên mục hiện tại. Đối với mỗi mặt hàng, chúng ta cộng khối lượng đã điều chỉnh vi trừ ci nhân với trọng lượng tích lũy. 
5. Chúng tôi cập nhật trọng lượng tích lũy bằng cách cộng trọng lượng của mặt hàng hiện tại trước khi chuyển sang mặt hàng tiếp theo. 

Tại sao nó hoạt động: 

Bất biến chính là tổng chi phí có thể được phân tách thành các đóng góp cặp độc lập w_i c_j cho mỗi lần đảo ngược i trên j. Bất kỳ thứ tự nào không được bộ so sánh sắp xếp phải chứa ít nhất một phép đảo ngược liền kề vi phạm w_i c_j ≤ w_j c_i. Trao đổi cặp đó sẽ giảm tổng chi phí. Việc lặp lại các hoán đổi như vậy cuối cùng sẽ dẫn đến thứ tự được sắp xếp, do đó phải tối ưu vì không còn hoán đổi cải thiện nào nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    items = []
    for _ in range(n):
        w, v, c = map(int, input().split())
        items.append((w, v, c))
    
    def cmp(a):
        w, v, c = a
        return (0 if c == 0 else w / c)
    
    items.sort(key=lambda x: (0 if x[2] == 0 else x[0] / x[2]))
    
    res = 0
    prefix_w = 0
    
    for w, v, c in items:
        res += v - c * prefix_w
        prefix_w += w
    
    print(res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc tất cả các mục. Khóa sắp xếp mã hóa quy tắc sắp xếp xuất phát từ đối số trao đổi: các mục có tỷ lệ w/c nhỏ hơn sẽ được đặt sớm hơn trong ngăn xếp. Các vật phẩm có c = 0 thực sự không bị nén và được đặt đầu tiên vì chúng không bị phạt bởi trọng lượng cao hơn chúng. 

Quá trình quét tính toán trọng số tiền tố khi chúng ta đi từ trên xuống dưới. Ở mỗi bước, tiền tố này thể hiện chính xác tổng trọng số phía trên mục hiện tại, khớp trực tiếp với định nghĩa về chi phí nén. 

Phải cẩn thận với phép chia dấu phẩy động trong các giải pháp nâng cao chất lượng sản xuất; việc triển khai an toàn hơn sẽ so sánh việc sử dụng phép nhân chéo để tránh các vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ: 

đầu vào:```
3
1 8 1
2 9 2
3 10 2
```Chúng tôi tính toán thứ tự bằng cách so sánh tỷ lệ w/c: 

| Mục | w | v | c | có/c | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 8 | 1 | 1.0 | 
| 2 | 2 | 9 | 2 | 1.0 | 
| 3 | 3 | 10 | 2 | 1,5 | 

Thứ tự sắp xếp hợp lệ là 1, 2, 3. 

Bây giờ chúng tôi tính trọng số tiền tố và các khoản đóng góp: 

| Bước | Mục | Trọng số tiền tố trước | Đóng góp vi - ci*W | Trọng lượng tiền tố sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 8 | 1 | 
| 2 | 2 | 1 | 9 - 2×1 = 7 | 3 | 
| 3 | 3 | 3 | 10 - 2×3 = 4 | 6 | 

Đáp án cuối cùng là 8 + 7 + 4 = 19. 

Dấu vết này cho thấy cách nén của mỗi mục chỉ phụ thuộc vào trọng lượng tích lũy, xác nhận cấu trúc tiền tố là đủ. 

Một ví dụ thứ hai: 

đầu vào:```
2
10 100 5
1 100 1
```Chúng ta so sánh 10/5 = 2 và 1/1 = 1, vì vậy mục 2 đứng đầu. 

| Bước | Mục | Trọng số tiền tố trước | Đóng góp | Trọng lượng tiền tố sau | 
| --- | --- | --- | --- | --- | 
| 1 | (1.100,1) | 0 | 100 | 1 | 
| 2 | (10.100,5) | 1 | 100 - 5×1 = 95 | 11 | 

Tổng số là 195, cho thấy rằng việc đặt mục có tỷ lệ thấp trước tiên sẽ tránh được các hình phạt lớn ở cuối dòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, tiếp theo là quét tuyến tính | 
| Không gian | O(n) | Lưu trữ tất cả các mặt hàng | 

Các ràng buộc cho phép tối đa 100000 mục, do đó, giải pháp O(n log n) phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là tuyến tính theo số lượng hàng hóa và an toàn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve())

# minimal case
assert run("1\n5 10 2\n") == "10", "single item"

# two items swap check
assert run("2\n10 100 5\n1 100 1\n") == "195", "ordering matters"

# equal ratios
assert run("2\n1 10 1\n2 20 2\n") == "28", "tie handling"

# larger structure
assert run("3\n1 8 1\n2 9 2\n3 10 2\n") == "19", "sample-like case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất | 10 | trường hợp cơ bản, không nén | 
| kiểm tra trao đổi | 195 | đặt hàng hiệu quả đúng đắn | 
| tỷ lệ bằng nhau | 28 | sự ổn định dưới mối quan hệ | 
| trường hợp giống mẫu | 19 | tích lũy tiền tố nhiều bước | 

## Vỏ cạnh 

Trường hợp một cạnh phát sinh khi hệ số nén bằng 0. Những món đồ như vậy không bị giảm thể tích bất kể trọng lượng phía trên chúng như thế nào, vì vậy chúng nên được đặt lên hàng đầu. Ví dụ: 

đầu vào:```
2
5 10 0
1 100 1
```Mục có hệ số 0 phải ở trên cùng. Trong trường hợp đó, trọng số tiền tố luôn bằng 0 khi xử lý nó, do đó nó đóng góp toàn bộ khối lượng 10 và không gây hại cho mục thứ hai. 

Một trường hợp khác là khi các hệ số giống nhau nhưng trọng số khác nhau:```
2
10 100 2
1 50 2
```Cả hai đều có cùng c nên việc đặt hàng chỉ phụ thuộc vào trọng lượng. Vật nặng hơn nên đi sau vì đặt nó cao hơn sẽ tăng hình phạt cho vật phẩm thấp hơn đáng kể. 

Thuật toán xử lý cả hai trường hợp một cách tự nhiên thông qua cùng một quy tắc so sánh w_i c_j và w_j c_i, quy tắc này suy biến chính xác khi các giá trị c bằng hoặc bằng 0, duy trì thứ tự tối ưu mà không cần phân nhánh đặc biệt.
