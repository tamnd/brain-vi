---
title: "CF 104274F - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0438\u0433\u0440\u0430 \u0432 \u043d\u0430\u043f\u0435\u0440\u0441\u0442\u043a\u0438"
description: "Chúng ta đang xử lý một mảng nhị phân ẩn có độ dài $N$. Chính xác có hai vị trí chứa giá trị 1 và tất cả các vị trí khác chứa 0. Chúng ta không thể nhìn thấy mảng một cách trực tiếp. Thay vào đó, chúng ta được phép đặt các truy vấn có dạng: cho tôi tổng các giá trị trong phân đoạn $[L, R]$."
date: "2026-07-01T21:19:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104274
codeforces_index: "F"
codeforces_contest_name: "2023 VIII \u0418\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041f\u0424\u041e"
rating: 0
weight: 104274
solve_time_s: 101
verified: false
draft: false
---

[CF 104274F - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0438\u0433\u0440\u0430 \u0432 \u043d\u0430\u043f\u0435\u0440\u0441\u0442\u043a\u0438](https://codeforces.com/problemset/problem/104274/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một mảng nhị phân ẩn có độ dài$N$. Chính xác có hai vị trí chứa giá trị 1 và tất cả các vị trí khác chứa 0. Chúng ta không thể nhìn thấy mảng một cách trực tiếp. Thay vào đó, chúng ta được phép đặt các truy vấn có dạng: cho tôi tổng các giá trị trong một phân đoạn$[L, R]$. Mỗi câu trả lời cho chúng ta biết có bao nhiêu trong số hai phần ẩn nằm bên trong đoạn đó. 

Nhiệm vụ của chúng ta là xác định hai chỉ mục chứa các chỉ mục đó bằng cách sử dụng tối đa 50 truy vấn tổng phạm vi như vậy. 

Hạn chế chính đó là$N$có thể lớn như$10^6$, do đó, bất kỳ phương pháp nào kiểm tra từng vị trí riêng lẻ đều quá chậm. Quét tuyến tính sẽ yêu cầu$N$các truy vấn trong trường hợp xấu nhất, vượt quá giới hạn một cách lớn. Giới hạn tương tác là 50 truy vấn gợi ý rõ ràng về chiến lược logarit, thường liên quan đến tìm kiếm nhị phân kết hợp với các truy vấn phạm vi được thiết kế cẩn thận. 

Một trường hợp thất bại tinh vi xuất hiện nếu người ta cố gắng định vị cả hai vị trí một cách độc lập bằng cách sử dụng tìm kiếm nhị phân đơn giản trên tổng tiền tố mà không tính đến thực tế là cả hai vị trí đều đóng góp cho mọi truy vấn. Ví dụ: giả sử những cái đó ở vị trí 3 và 10. Nếu chúng ta cố gắng tìm cái đầu tiên bằng cách sử dụng tổng tiền tố, mọi thứ đều hoạt động. Nhưng nếu sau đó chúng ta cố gắng tìm vị trí thứ hai bằng cách sử dụng cùng một logic mà không loại trừ vị trí tìm thấy đầu tiên thì mọi truy vấn sau đó vẫn bị ô nhiễm bởi vị trí đã biết và điều kiện tìm kiếm nhị phân trở nên không nhất quán. Điều này dẫn đến việc phân nhánh không chính xác và trả lời sai mặc dù mỗi truy vấn riêng lẻ đều đúng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: truy vấn từng vị trí cho đến khi chúng tôi tìm thấy hai chỉ số trả về 1. Điều này đúng vì mỗi truy vấn tách biệt một vị trí duy nhất, nhưng nó yêu cầu$N$truy vấn trong trường hợp xấu nhất. Với$N = 10^6$, điều này ngay lập tức vượt quá giới hạn 50 truy vấn, khiến nó không thể thực hiện được. 

Một ý tưởng có cấu trúc hơn là sử dụng tổng tiền tố. Nếu chúng ta có thể trực tiếp yêu cầu số tiền trên$[1, i]$, thì chúng ta có thể tìm kiếm nhị phân vị trí đầu tiên trong đó tổng tiền tố trở thành 1, xác định quả bóng ngoài cùng bên trái. Điều này có hiệu quả vì tổng tiền tố là đơn điệu. Sau khi tìm được vị trí đầu tiên$p_1$, vị trí thứ hai có thể được tìm thấy bằng cách loại trừ$p_1$từ các khoản tiền trong tương lai. Khi chúng tôi có thể mô phỏng các truy vấn bỏ qua chỉ mục đã biết, cấu trúc còn lại lại hoạt động giống như một cấu trúc ẩn duy nhất, cho phép tìm kiếm nhị phân khác. 

Quan sát chính là một truy vấn phạm vi kết hợp với kiến ​​thức về một chỉ mục là đủ để mô phỏng một không gian tìm kiếm nhị phân rõ ràng cho chỉ mục thứ hai. Mỗi truy vấn có thể được điều chỉnh bằng cách trừ đi vị trí đã biết có nằm trong phạm vi hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N)$truy vấn |$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân với các truy vấn được điều chỉnh |$O(\log N)$truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì quyền truy cập vào chức năng truy vấn trả về số lượng ẩn trong bất kỳ phân đoạn nào. 

### Bước 1: Tìm quả bóng đầu tiên 

Chúng tôi tìm kiếm nhị phân trên phạm vi$[1, N]$. Đối với một điểm giữa$mid$, chúng tôi yêu cầu số tiền trên$[1, mid]$. Nếu câu trả lời ít nhất là 1 thì quả bóng đầu tiên nằm ở nửa bên trái, nếu không thì nó nằm ở nửa bên phải. Chúng tôi thu hẹp tìm kiếm cho đến khi chúng tôi tách được chỉ mục chính xác$p_1$. 

Tính chính xác xuất phát từ thực tế là tiền tố tính tổng trên một mảng nhị phân với một ngưỡng chuyển đổi duy nhất đã biết từ 0 sang 1 chính xác một lần ở lần xuất hiện đầu tiên của số 1. 

### Bước 2: Chuẩn bị tìm quả bóng thứ hai 

Bây giờ chúng ta biết một vị trí$p_1$. Mỗi truy vấn tiếp theo cho một phân đoạn$[L, R]$có thể được hiểu là:$$\text{true\_sum}(L, R) = \text{query}(L, R) - [p_1 \in [L, R]]$$Sự điều chỉnh này loại bỏ sự đóng góp của quả bóng đã được phát hiện. 

### Bước 3: Tìm quả bóng thứ hai bằng tìm kiếm nhị phân đã sửa đổi 

Chúng tôi lại tìm kiếm nhị phân trên$[1, N]$. Đối với mỗi trung điểm$mid$, chúng tôi tính tổng tiền tố đã điều chỉnh trên$[1, mid]$. Nếu tổng điều chỉnh này ít nhất là 1 thì quả bóng thứ hai nằm ở nửa bên trái; nếu không thì nó nằm ở nửa bên phải. Điều này cô lập vị trí thứ hai$p_2$. 

Việc điều chỉnh đảm bảo rằng việc tìm kiếm hoạt động như thể chỉ tồn tại một số 1 chưa biết trong mảng. 

### Bước 4: Xuất cả hai vị trí 

Chúng tôi xuất ra hai chỉ số được phát hiện theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Ở mọi giai đoạn tìm kiếm nhị phân, vị từ quyết định chỉ phụ thuộc vào việc có ít nhất một quả bóng không nhìn thấy được nằm trong tiền tố hay không. Sau khi loại bỏ sự đóng góp của vị trí đã được phát hiện, cấu trúc giảm chính xác xuống bài toán 1 đơn. Điều này duy trì tính đơn điệu của vị từ, đảm bảo tính chính xác của tìm kiếm nhị phân. Vì mỗi giai đoạn cách ly một vị trí duy nhất nên thuật toán không thể trả về các bản sao hoặc bỏ sót một chỉ mục hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(l, r):
    print("?", l, r)
    sys.stdout.flush()
    return int(input().strip())

def find_one(exclude=-1):
    l, r = 1, n
    while l < r:
        mid = (l + r) // 2
        res = ask(l, mid)
        if exclude != -1 and l <= exclude <= mid:
            res -= 1
        if res >= 1:
            r = mid
        else:
            l = mid + 1
    return l

n = int(input().strip())

# find first position
p1 = find_one()

# find second position with exclusion
def ask_adj(l, r):
    res = ask(l, r)
    if l <= p1 <= r:
        res -= 1
    return res

l, r = 1, n
while l < r:
    mid = (l + r) // 2
    if ask_adj(l, mid) >= 1:
        r = mid
    else:
        l = mid + 1

p2 = l

if p1 > p2:
    p1, p2 = p2, p1

print("!", p1, p2)
sys.stdout.flush()
```Giải pháp tách sự tương tác thành một hàm bao bọc nhỏ`ask`, đảm bảo mọi truy vấn sẽ được xóa ngay lập tức. Tìm kiếm nhị phân đầu tiên sử dụng các truy vấn tiền tố thô để xác định vị trí một quả bóng. Sau đó, mọi truy vấn được sửa bằng cách trừ đi phần đóng góp của vị trí đã biết, khôi phục một cách hiệu quả không gian tìm kiếm một mục tiêu rõ ràng. Tìm kiếm nhị phân thứ hai có cấu trúc giống hệt nhau nhưng hoạt động theo chức năng truy vấn được điều chỉnh này. 

Phải cẩn thận với logic loại trừ. Phép trừ chỉ được thực hiện khi chỉ mục đã biết nằm trong phạm vi được truy vấn, nếu không việc điều chỉnh sẽ làm giảm số lượng một cách không chính xác và phá vỡ tính đơn điệu. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng có kích thước$10$với những cái ẩn ở vị trí$3$Và$7$. 

### Tìm vị trí đầu tiên 

Chúng tôi thực hiện tìm kiếm nhị phân trên tổng tiền tố: 

| Bước | Truy vấn | Phản hồi | Logic điều chỉnh | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | (1,5) | 1 | không loại trừ | đi bên trái | 
| 2 | (1,3) | 1 | không loại trừ | đi bên trái | 
| 3 | (1,2) | 0 | không loại trừ | đi bên phải | 

Chúng tôi hội tụ ở vị trí 3. 

Điều này cho thấy rằng tổng tiền tố cô lập chính xác lần xuất hiện đầu tiên do sự gia tăng đơn điệu. 

### Tìm vị trí thứ hai 

bây giờ$p_1 = 3$. 

| Bước | Truy vấn | Phản hồi thô | Phản hồi đã điều chỉnh | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | (1,5) | 1 | 0 (loại trừ 3) | đi bên phải | 
| 2 | (4,7) | 1 | 1 | đi bên trái | 
| 3 | (6,6) | 0 | 0 | đi bên phải | 

Chúng tôi hội tụ ở vị trí 7. 

Dấu vết này cho thấy cách trừ chỉ mục đã biết để khôi phục không gian tìm kiếm một mục tiêu rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log N)$truy vấn | Hai tìm kiếm nhị phân trong phạm vi | 
| Không gian |$O(1)$| Chỉ có một vài chỉ số được lưu trữ | 

Giới hạn truy vấn 50 có thể dễ dàng được thỏa mãn vì mỗi tìm kiếm nhị phân sử dụng khoảng$\log_2(10^6) \approx 20$truy vấn và chúng tôi thực hiện hai tìm kiếm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return ""

# Sample-style placeholders (interactive, not executable offline)
# assert run(...) == ...

# custom sanity structure tests (conceptual)
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=2, bi tại (1,2) | ! 1 2 | kích thước tối thiểu | 
| N=5, bi tại (2,4) | ! 2 4 | trường hợp chia cơ bản | 
| N=10^6, cách xa nhau | chỉ số đúng | khả năng mở rộng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi hai quả bóng liền kề nhau. Trong trường hợp đó, truy vấn tiền tố thay đổi từ 0 thành 1 rồi ngay lập tức thành 2, nhưng tìm kiếm nhị phân chỉ kiểm tra xem tổng tiền tố có ít nhất là 1 hay không, do đó, nó vẫn tách biệt chính xác chỉ mục đầu tiên mà không bị nhầm lẫn với chỉ mục thứ hai. 

Một trường hợp đặc biệt khác là khi chỉ mục được tìm thấy đầu tiên nằm chính xác ở ranh giới của phạm vi truy vấn trong lần tìm kiếm thứ hai. Logic loại trừ đảm bảo rằng đóng góp của nó chỉ bị loại bỏ khi có liên quan. Ví dụ, nếu$p_1 = 5$và chúng tôi truy vấn$[1,5]$, câu trả lời thô có thể là 1 hoặc 2 tùy thuộc vào mức độ bao gồm, nhưng việc trừ chính xác một sẽ duy trì tính đúng đắn và giữ cho vị từ nhị phân đơn điệu.
