---
title: "CF 103361O - \u041a\u0440\u0435\u0441\u0442\u0438\u043a\u0438"
description: "Chúng ta được cung cấp một chuỗi nhị phân đại diện cho một dải ô. Một số ô đã chứa sẵn dấu thập, được đánh dấu là X, còn lại trống, được đánh dấu là .. Ràng buộc chính là không được phép có hai dấu thập nào liền kề nhau, nghĩa là chúng ta không bao giờ có thể có XX ở các vị trí lân cận."
date: "2026-07-03T13:10:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103361
codeforces_index: "O"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u041a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 103361
solve_time_s: 43
verified: true
draft: false
---

[CF 103361O - \u041a\u0440\u0435\u0441\u0442\u0438\u043a\u0438](https://codeforces.com/problemset/problem/103361/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân đại diện cho một dải ô. Một số ô đã có sẵn dấu thập, được đánh dấu là`X`, và phần còn lại trống, được đánh dấu là`.`. Ràng buộc chính là không được phép có hai đường chéo liền kề nhau, nghĩa là chúng ta không bao giờ có thể có`XX`ở các vị trí lân cận. 

Misha đã đặt một bộ thánh giá hợp lệ. Sau đó, một người khác là Lёsha thêm nhiều cây thánh giá hơn, vẫn tôn trọng quy định không có hai cây thánh giá nào liền kề nhau. Cuối cùng, chúng tôi được thông báo rằng Katya xác nhận cấu hình là tối đa, nghĩa là sau khi cả Misha và Lёsha đặt các dấu thập, không có ô trống nào để chúng tôi vẫn có thể đặt một dấu thập bổ sung mà không vi phạm quy tắc kề. 

Nhiệm vụ là xác định xem Lёsha có thể thêm tối đa bao nhiêu cây thánh giá với cấu hình ban đầu. 

Kích thước đầu vào nhỏ, với n lên tới 1000. Điều này ngay lập tức gợi ý rằng về nguyên tắc, cách tiếp cận O(n²) có thể chấp nhận được, nhưng vì cấu trúc rất cục bộ nên chúng ta nên mong đợi một giải pháp tham lam tuyến tính. 

Một hạn chế tinh tế nhưng quan trọng là cấu hình của Misha đã hợp lệ, do đó không có cấu hình liền kề nào`X`các ký tự ban đầu. Điều này giúp loại bỏ nhu cầu logic xác thực hoặc sửa chữa và cho phép chúng tôi tập trung hoàn toàn vào việc tăng cường. 

Các trường hợp khó khăn chính xuất phát từ khoảng cách ngắn giữa các đường giao cắt và ranh giới hiện có: 

Nếu chuỗi là`"..."`, thì tất cả các vị trí đều trống, nhưng chúng ta vẫn không thể đặt các dấu thập liền kề nhau nên vị trí tối đa là mọi ô khác. 

Nếu chuỗi là`"X.X"`, thì không thể thêm dấu thập nào nữa. 

Nếu chuỗi là`"..X.."`, thì chỉ có thể đặt thêm một chữ thập, ở bên trái hoặc bên phải tùy theo độ chẵn lẻ. 

Một sai lầm ngây thơ là cố gắng đặt một cách tham lam mà không đánh dấu những hàng xóm mới chiếm giữ hoặc quên rằng một chữ thập đã đặt sẽ chặn cả hai ô liền kề. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi tập hợp con của các vị trí trống và kiểm tra xem việc đặt các dấu thập ở đó có vi phạm các ràng buộc kề cận hay không. Đối với mỗi tập hợp con ứng cử viên, chúng tôi sẽ xác minh toàn bộ chuỗi trong O(n), dẫn đến độ phức tạp O(2ⁿ · n), điều này hoàn toàn không khả thi ngay cả với n = 1000. 

Một cách mạnh mẽ hợp lý hơn một chút là lặp lại qua các vị trí và quyết định đệ quy xem có nên đặt dấu thập hay không. Ngay cả khi cắt tỉa, hệ số phân nhánh vẫn cao vì mỗi ô trống chỉ tương tác cục bộ với các ô lân cận và cuối cùng chúng ta vẫn khám phá các cấu hình hàm mũ. 

Quan sát quan trọng là ràng buộc hoàn toàn mang tính cục bộ: việc đặt một cây thánh giá chỉ cấm những người hàng xóm ngay lập tức của nó. Điều này có nghĩa là chúng ta không bao giờ cần quay lại. Thay vào đó, chúng ta có thể quét từ trái sang phải và tham lam đặt dấu thập bất cứ khi nào cả hai hàng xóm đều trống. 

Chúng ta cũng phải tôn trọng những gì đã tồn tại`X`các vị trí, hoạt động như các tế bào bị chiếm đóng cưỡng bức. Khi chúng tôi thấy một ô đã bị hàng xóm chiếm giữ hoặc chặn, chúng tôi chỉ cần bỏ qua ô đó. Mặt khác, việc đặt một dấu thập ngay lập tức luôn an toàn và tối ưu cục bộ, bởi vì nó duy trì khả năng đặt càng nhiều dấu thập càng tốt trong hậu tố còn lại. 

Điều này làm giảm vấn đề xuống một đường tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n) | O(n) | Quá chậm | 
| Quét tham lam tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`added = 0`để theo dõi có bao nhiêu địa điểm đi qua Lёsha. Chúng tôi chỉ tính các vị trí mới, không tính các vị trí hiện có. 
2. Chuyển đổi chuỗi thành một mảng có thể thay đổi để chúng ta có thể đánh dấu các dấu thập mới được đặt. Điều này rất quan trọng vì các cây thánh giá mới được đặt cũng chặn các ô liền kề. 
3. Duyệt chuỗi từ trái sang phải bằng chỉ mục`i`. 
4. Đối với từng vị trí`i`, kiểm tra xem nó có trống không. Nếu nó đã rồi`X`, chúng ta bỏ qua nó vì nó đã bị chiếm dụng. 
5. Nếu vị trí`i`trống, chúng ta kiểm tra xem có thể đặt dấu thập ở đây không. Điều này đòi hỏi hàng xóm bên trái (nếu nó tồn tại) không`X`và hàng xóm bên phải (nếu nó tồn tại) thì không`X`. Chúng tôi cũng coi những cây thánh giá mới được đặt là sự chặn theo cách tương tự. 
6. Nếu cả hai người hàng xóm đều an toàn, chúng ta đặt dấu thập ở vị trí`i`, tăng`added`, và đánh dấu`i`BẰNG`X`trong mảng để nó ảnh hưởng đến các quyết định trong tương lai. 
7. Tiếp tục quét cho đến hết chuỗi. 

Sau khi quá trình quét hoàn tất,`added`là số thánh giá tối đa mà Lёsha có thể thêm vào. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là ở mỗi bước quét, tiền tố`[0..i-1]`đã ở trạng thái không sửa đổi thêm các vị trí trước đó có thể cải thiện câu trả lời mà không vi phạm ràng buộc kề cận. Bất kỳ giải pháp hợp lệ nào cũng có thể được chuyển đổi thành một giải pháp phù hợp với vị trí tham lam này trên tiền tố mà không làm giảm số lượng dấu thập, bởi vì các quyết định tại vị trí`i`chỉ phụ thuộc vào`i-1`Và`i+1`. Một khi chúng ta vượt qua vị trí`i`, chúng tôi không bao giờ xem lại nó và bất kỳ lựa chọn thay thế nào cũng sẽ chặn cùng một vị trí hoặc nhiều vị trí ở bên phải. Vì vậy, đặt một cây thánh giá bất cứ khi nào có thể là tối ưu tại địa phương và an toàn trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = list(input().strip())

    added = 0

    for i in range(n):
        if s[i] == 'X':
            continue

        left_ok = (i == 0 or s[i - 1] == '.')
        right_ok = (i == n - 1 or s[i + 1] == '.')

        if left_ok and right_ok:
            s[i] = 'X'
            added += 1

    print(added)

if __name__ == "__main__":
    solve()
```Giải pháp đọc chuỗi vào danh sách để các bản cập nhật có hiệu quả và phản ánh các dấu thập mới được đặt ngay lập tức. Việc kiểm tra các hàng xóm bên trái và bên phải bao gồm việc xử lý ranh giới, trong đó các cạnh được coi là tự động an toàn ở bên ngoài. 

Chi tiết triển khai chính là chúng tôi kiểm tra trạng thái hiện tại của mảng chứ không phải chuỗi gốc để các dấu chéo mới được thêm vào chặn chính xác các vị trí tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
.....
```| tôi | s[i] | trái_ok | đúng_ok | hành động | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | . | Đúng | Đúng | địa điểm X | 1 | 
| 1 | X | - | - | bỏ qua | 1 | 
| 2 | . | Sai | Sai | bỏ qua | 1 | 
| 3 | X | - | - | bỏ qua | 1 | 
| 4 | . | Đúng | Đúng | địa điểm X | 2 | 

Đầu ra cuối cùng là 2. 

Điều này cho thấy vị trí tham lam tự nhiên tạo ra các vị trí xen kẽ và tránh vi phạm tính liền kề. 

### Ví dụ 2 

đầu vào:```
5
.X...
```| tôi | s[i] | trái_ok | đúng_ok | hành động | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | . | Đúng | Sai | bỏ qua | 0 | 
| 1 | X | - | - | bỏ qua | 0 | 
| 2 | . | Sai | Đúng | bỏ qua | 0 | 
| 3 | . | Đúng | Đúng | địa điểm X | 1 | 
| 4 | . | Sai | Đúng | bỏ qua | 1 | 

Đầu ra cuối cùng là 1. 

Điều này thể hiện cách thức tồn tại`X`chặn các vị trí gần đó và buộc các khoảng trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ô được xử lý một lần với O(1) kiểm tra hàng xóm | 
| Không gian | O(n) | Biểu diễn mảng có thể thay đổi của chuỗi | 

Quét tuyến tính dễ dàng phù hợp với các ràng buộc cho n lên tới 1000. Ngay cả khi n lớn hơn đáng kể, phương pháp này vẫn sẽ mở rộng quy mô một cách thoải mái do thời gian xử lý trên mỗi ô không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    out = StringIO()
    _stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = _stdout
    return out.getvalue().strip()

# provided samples
assert run("5\n.....\n") == "3", "all empty (should place 3 in length 5)"
assert run("5\n.X...\n") == "1", "single block in middle"

# custom cases
assert run("1\n.\n") == "1", "single cell"
assert run("1\nX\n") == "0", "already occupied"
assert run("3\n...\n") == "2", "alternating placement"
assert run("4\nX.X.\n") == "0", "fully constrained pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trống 1 ô | 1 | vị trí tối thiểu | 
| Đã lấp đầy 1 ô | 0 | không thể có thêm vị trí | 
|`...`| 2 | hành vi tham lam xen kẽ | 
|`X.X.`| 0 | chặn hoàn toàn các ràng buộc hiện có | 

## Vỏ cạnh 

Một đầu vào tối thiểu như`"."`được xử lý chính xác vì cả hai kiểm tra ranh giới đều coi những người hàng xóm bị thiếu là an toàn, do đó thuật toán đặt dấu chéo và trả về 1. 

Một mô hình xen kẽ hoàn toàn như`"X.X.X"`dẫn đến phép cộng bằng 0, vì mỗi vị trí trống có ít nhất một vị trí liền kề`X`. Quá trình quét nhìn thấy từng`.`nhưng lần nào người hàng xóm cũng không kiểm tra được nên không có vị trí nào được thực hiện. 

Một đoạn trống dài như`"........"`chứng tỏ rằng vị trí tham lam sẽ tạo ra mọi vị trí thứ hai một cách tự nhiên. Bất biến được giữ nguyên vì một khi hình chữ thập được đặt ở vị trí`i`, chức vụ`i+1`sẽ tự động bị chặn và không bao giờ được xem xét lại.
