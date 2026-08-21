---
title: "CF 104066B - Hộp tò mò"
description: "Chúng ta được cho một thiết lập hình học trên một mặt phẳng. Có một đường tròn cố định đã biết tâm và bán kính, và trên đường tròn đó có một “thiết bị” cứng mang hai điểm được đánh dấu."
date: "2026-07-02T03:13:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104066
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u0431\u0430\u0437\u043e\u0432\u0430\u044f \u0432\u0435\u0440\u0441\u0438\u044f)"
rating: 0
weight: 104066
solve_time_s: 46
verified: true
draft: false
---

[CF 104066B - Hộp tò mò](https://codeforces.com/problemset/problem/104066/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một thiết lập hình học trên một mặt phẳng. Có một đường tròn cố định đã biết tâm và bán kính, và trên đường tròn đó có một “thiết bị” cứng mang hai điểm được đánh dấu. Điểm tự do chính là chúng ta được phép xoay thiết bị này quanh tâm vòng tròn, thiết bị này sẽ xoay hai điểm được đánh dấu xung quanh tâm đó một cách hiệu quả trong khi vẫn giữ khoảng cách của chúng với điểm đó. 

Bên cạnh đó, có một đường thẳng cố định trong mặt phẳng. Nhiệm vụ là xác định xem liệu chúng ta có thể xoay cấu hình đã đánh dấu của đường tròn sao cho đường thẳng tạo bởi hai điểm được đánh dấu trở nên song song với đường cố định đã cho hay không. 

Vì vậy, câu hỏi thực sự không phải là về bản thân đường tròn mà là về việc liệu đoạn được xác định bởi hai điểm có thể đạt được hướng trùng với hướng của đường thẳng đã cho sau khi quay cố định quanh tâm chung của chúng hay không. 

Đầu vào mô tả ba đối tượng hình học độc lập. Đầu tiên, tâm và bán kính hình tròn, xác định trục quay và cơ chế quay. Thứ hai, một đường thẳng có dạng tổng quát ax + by + c = 0, trong đó chỉ có vectơ chỉ phương (a, b) là quan trọng. Thứ ba, hai điểm xác định một đoạn được gắn chặt vào tâm vòng tròn, nghĩa là góc tương đối của chúng quanh tâm có thể thay đổi, nhưng khoảng cách của chúng với tâm là cố định. 

Đầu ra là một quyết định khả thi đơn giản: liệu có tồn tại một số góc quay làm cho hướng của đoạn thẳng song song với đường đã cho hay không. 

Các ràng buộc rất nhỏ, với tất cả tọa độ và hệ số được giới hạn bởi 10^4. Điều này ngay lập tức loại trừ bất kỳ nhu cầu nào về các thủ thuật tiền xử lý hình học nặng hoặc các thủ thuật chính xác về dấu phẩy động ngoài lượng giác cơ bản hoặc lý luận vectơ. Kiểm tra hình học liên tục theo thời gian cho mỗi trường hợp thử nghiệm là đủ. 

Trường hợp cạnh tinh tế xuất hiện khi hai điểm được đánh dấu trùng nhau. Trong trường hợp đó, “đường đi qua các điểm được đánh dấu” không được xác định. Việc triển khai ngây thơ tính toán một cách mù quáng một vectơ chỉ phương sẽ chia cho 0 hoặc coi nó là vectơ 0, điều này có thể dẫn đến kết luận sai. Ví dụ: nếu cả hai điểm giống hệt nhau, chẳng hạn như (0, 0) và (0, 0), đoạn đó không có hướng. Trong trường hợp đó, kết quả đầu ra đúng phụ thuộc vào cách diễn giải: vì không thể tạo được đường thẳng nên không thể làm cho nó song song với bất kỳ đường thẳng nào cho trước không suy biến, do đó câu trả lời phải là KHÔNG trừ khi bài toán cho phép suy biến song song, điều mà các bài toán hình học CF tiêu chuẩn không làm được. 

Một trường hợp góc khác là khi đường thẳng đã cho bị suy biến trong biểu diễn hướng, ví dụ khi cả a và b đều bằng 0. Điều đó sẽ làm cho dòng không hợp lệ, nhưng các ràng buộc thường đảm bảo điều này không xảy ra. Tuy nhiên, lý luận chắc chắn nên bỏ qua hoàn toàn c và chỉ tập trung vào (a, b) làm vectơ chỉ phương. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để nghĩ về điều này là mô phỏng chuyển động quay của đoạn xung quanh tâm và kiểm tra tất cả các hướng có thể có. Vì có sự quay liên tục nên người ta có thể rời rạc hóa góc và kiểm tra nhiều hướng ứng cử viên của đoạn thẳng. Về mặt khái niệm, điều này sẽ hoạt động bằng cách quét các góc từ 0 đến 2π và kiểm tra xem vectơ quay có thẳng hàng với hướng của đường thẳng đã cho hay không. 

Tuy nhiên, điều này là không cần thiết và không chính xác. Ngay cả khi được rời rạc hóa một cách tinh vi, nó vẫn có nguy cơ thiếu sự căn chỉnh chính xác do độ chi tiết của dấu phẩy động và số lượng mẫu cần thiết để đảm bảo tính chính xác không bị giới hạn một cách rõ ràng. 

Quan sát quan trọng là đoạn được xác định bởi hai điểm được đánh dấu có chiều dài cố định và quay cứng quanh tâm. Điều này có nghĩa là hướng của nó không được tự do lựa chọn cho mỗi cấu hình điểm cuối; thay vào đó, nó bao gồm tất cả các phép quay có thể có của một vectơ cơ sở. Do đó, câu hỏi duy nhất là liệu vectơ chỉ phương của đoạn thẳng có thể quay thành hướng song song với (a, b) hay không.

Hai vectơ có thể được tạo song song bằng cách quay khi và chỉ khi các góc của chúng khác nhau một góc quay nào đó, điều này luôn có thể xảy ra trừ khi đoạn thẳng suy biến thành một điểm. Phép quay trong mặt phẳng bảo toàn độ lớn và chỉ thay đổi hướng, do đó, bất kỳ vectơ nào khác 0 đều có thể được quay để khớp với bất kỳ hướng nào khác. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem đoạn đó có suy biến hay không. Nếu hai điểm khác nhau thì tồn tại một vectơ chỉ phương hợp lệ và chúng ta luôn có thể xoay nó để căn chỉnh với hướng của đường thẳng. Nếu chúng trùng nhau thì không tồn tại phương hướng và câu trả lời là KHÔNG. 

Vì vậy, toàn bộ hình học thu gọn lại thành một phép kiểm tra suy biến đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét góc vũ phu | O(T × K) | O(1) | Quá chậm/không đáng tin cậy | 
| Kiểm tra Vector hướng | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các thông số đường tròn, hệ số đường thẳng và hai điểm. Các tham số vòng tròn không thực sự được sử dụng trong tính toán, nhưng chúng thiết lập tâm quay về mặt khái niệm. 
2. Tính vectơ chỉ phương của đoạn thẳng tạo bởi hai điểm như (dx, dy) = (x2 − x1, y2 − y1). Vectơ này đại diện cho bậc tự do hình học có ý nghĩa duy nhất. 
3. Kiểm tra xem vectơ này có bằng 0 hay không. Nếu dx = 0 và dy = 0, hai điểm trùng nhau và không tồn tại hướng hợp lệ, do đó ngay lập tức xuất ra NO. 
4. Nếu vectơ khác 0, kết luận rằng nó có thể quay theo bất kỳ hướng nào trong mặt phẳng, kể cả một hướng song song với hướng đường thẳng (a, b). Đầu ra CÓ. 

Lý do đằng sau bước 4 là bất kỳ vectơ 2D nào khác 0 đều có thể quay liên tục qua mọi góc và độ song song chỉ phụ thuộc vào hướng chứ không phụ thuộc vào độ lớn. 

### Tại sao nó hoạt động 

Điều bất biến là thuộc tính duy nhất của đoạn được đánh dấu quan trọng là vectơ chỉ phương của nó và phép quay quanh tâm sẽ giữ nguyên chiều dài của đoạn đó trong khi cho phép thay đổi hướng liên tục trên toàn bộ vòng tròn. Tập hợp các hướng có thể tiếp cận là toàn bộ vòng tròn đơn vị cho bất kỳ vectơ nào khác 0. Do đó, trừ khi vectơ suy biến, luôn tồn tại hướng phù hợp với bất kỳ đường thẳng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x0, y0, r = map(int, input().split())
    a, b, c = map(int, input().split())
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    if dx == 0 and dy == 0:
        print("NO")
    else:
        print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai giữ mức logic tối thiểu vì tất cả độ phức tạp hình học đã được giải quyết bằng phương pháp phân tích. Các tham số vòng tròn được đọc để khớp với định dạng đầu vào nhưng không được sử dụng thêm. Tính toán quan trọng duy nhất là sự khác biệt giữa hai điểm được đánh dấu. 

Một lỗi phổ biến ở đây là cố gắng sử dụng độ dốc hoặc so sánh góc có dấu phẩy động. Điều đó là không cần thiết vì vấn đề được rút gọn thành một phép kiểm tra không suy biến đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

(0,0,6), đường (1,1,1), điểm (0,0) và (1,1) 

| Bước | dx | nhuộm | Quyết định | 
| --- | --- | --- | --- | 
| tính hướng | 1 | 1 | khác không | 

Vì đoạn có hướng hợp lệ nên nó có thể được xoay để căn chỉnh với bất kỳ hướng đường nào, vì vậy câu trả lời là CÓ. 

Điều này xác nhận rằng ngay cả khi đoạn thẳng ban đầu thẳng hàng với đường thẳng thì điều kiện vẫn được thỏa mãn khi xoay. 

### Ví dụ 2 

đầu vào: 

(0,0,6), đường (1,-1,1), điểm (0,0) và (1,1) 

| Bước | dx | nhuộm | Quyết định | 
| --- | --- | --- | --- | 
| tính hướng | 1 | 1 | khác không | 

Một lần nữa, phân đoạn này hợp lệ nên câu trả lời là CÓ. 

Điều này cho thấy hướng đường cụ thể không quan trọng; chỉ có sự tồn tại của một hướng cho phân khúc mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi lần kiểm tra | Chỉ một số phép tính số học và so sánh được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó giảm toàn bộ hình học xuống việc kiểm tra liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""0 0 6
1 1 1
0 0
1 1
""") == "YES"

assert run("""0 0 6
1 -1 1
0 0
1 1
""") == "YES"

# custom cases
assert run("""0 0 6
1 1 0
0 0
0 0
""") == "NO"  # degenerate segment

assert run("""5 5 2
2 3 4
1 2
3 6
""") == "YES"  # non-degenerate segment

assert run("""0 0 10
0 1 5
-2 -2
-2 -2
""") == "NO"  # identical points

assert run("""0 0 10
10 0 1
0 0
1 0
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm giống nhau | KHÔNG | xử lý phân đoạn thoái hóa | 
| điểm ngẫu nhiên bình thường | CÓ | tính đúng đắn của trường hợp tổng quát | 
| tất cả tọa độ bằng nhau | KHÔNG | trường hợp cạnh vector bằng không | 
| đoạn ngang | CÓ | trường hợp hướng đơn giản | 

## Vỏ cạnh 

Khi hai điểm được đánh dấu giống hệt nhau, thuật toán sẽ phát hiện dx = dy = 0 và ngay lập tức đưa ra NO. Điều này ngăn chặn bất kỳ khái niệm không xác định nào về hướng và tránh coi vectơ 0 là có hướng tùy ý. 

Ví dụ: nếu đầu vào là (0,0) và (0,0), vectơ được tính là (0,0). Thuật toán dừng ở việc kiểm tra suy biến và trả về NO mà không cần thực hiện bất kỳ suy luận hình học nào. 

Nếu các điểm cực kỳ gần nhau nhưng không giống nhau, chẳng hạn như (0,0) và (0,1), thì vectơ vẫn hợp lệ và thuật toán cho phép xoay chính xác. Điều này cho thấy rằng chỉ có sự bình đẳng chính xác mới quan trọng chứ không phải mức độ gần nhau về số lượng.
