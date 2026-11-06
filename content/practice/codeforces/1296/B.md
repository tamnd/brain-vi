---
title: "CF 1296B - Mua thực phẩm"
description: "Chúng tôi được cấp một số tiền ban đầu và chúng tôi liên tục thực hiện một loại hoạt động mua hàng rất cụ thể. Trong mỗi thao tác, chúng ta chọn một số tiền $x$ mà chúng ta có thể chi trả tại thời điểm đó, chi tiêu số tiền đó và ngay lập tức nhận lại $lfloor x/10 rfloor$."
date: "2026-06-16T04:48:11+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1296
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 617 (Div. 3)"
rating: 900
weight: 1296
solve_time_s: 133
verified: true
draft: false
---

[CF 1296B - Mua thực phẩm](https://codeforces.com/problemset/problem/1296/B) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số tiền ban đầu và chúng tôi liên tục thực hiện một loại hoạt động mua hàng rất cụ thể. Trong mỗi thao tác, chúng tôi chọn một số lượng$x$mà chúng ta có thể chi trả vào thời điểm đó, chi tiêu và ngay lập tức nhận lại$\lfloor x/10 \rfloor$. Số tiền hoàn lại đó có thể được sử dụng lại trong các lần mua hàng trong tương lai, vì vậy tiền có thể được lưu thông thông qua việc hoàn lại một phần nhiều lần. 

Nhiệm vụ không phải là mô phỏng việc mua hàng mà là xác định tổng số tiền tối đa có thể chi tiêu cho tất cả các hoạt động nếu chúng ta chọn trình tự mua hàng một cách tối ưu. 

Điều tinh tế quan trọng là số tiền hoàn lại tỷ lệ thuận với quy mô mua hàng đã chọn. Những khoản mua nhỏ sẽ mất gần như mọi thứ, trong khi những khoản mua lớn sẽ tái chế một phần giá trị đã chi. Vấn đề về cơ bản là hỏi có thể rút ra bao nhiêu "chi tiêu thêm" thông qua việc tái sử dụng nhiều lần số tiền thứ mười được hoàn lại. 

Kích thước đầu vào cho phép lên đến$10^4$trường hợp thử nghiệm có giá trị lên tới$10^9$. Điều này loại trừ bất kỳ mô phỏng nào phụ thuộc vào việc lặp qua từng đơn vị tiền hoặc thực hiện một số lượng lớn các lựa chọn tham lam từng bước cho mỗi trường hợp thử nghiệm. MỘT$O(s)$hoặc thậm chí$O(s/10)$mô phỏng là quá chậm trong trường hợp xấu nhất. 

Một cách tiếp cận tham lam ngây thơ cũng có thể thất bại theo những cách ít rõ ràng hơn. Ví dụ, luôn lấy số tiền lớn nhất có thể$x = s$có thể trông hợp lý, nhưng nó không tính đến thực tế là số tiền hoàn lại có thể được sử dụng lại nhiều lần và chiến lược tối ưu là buộc phải “tái chế 10 khối” lặp đi lặp lại. 

Mẫu trường hợp cạnh chính là khi số tiền còn lại nằm ngay dưới bội số của 10. Ví dụ: với$s = 19$, đang chọn$x = 19$đầu tiên hoàn lại 1, để lại 1 và mất cấu trúc. Thay vào đó, chiến lược tối ưu liên tục sử dụng các giao dịch mua cỡ 10 để tối đa hóa việc tái sử dụng chu kỳ hoàn tiền. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ mô phỏng tất cả các lựa chọn có thể có của$x$ở mỗi bước, phân nhánh trên tất cả$1 \le x \le s$. Sau mỗi thao tác, trạng thái sẽ thay đổi thành$s - x + \lfloor x/10 \rfloor$và chúng tôi sẽ thử đệ quy tất cả các khả năng để tối đa hóa tổng chi tiêu. Về nguyên tắc, điều này đúng vì nó khám phá mọi chuỗi hoạt động hợp lệ. 

Tuy nhiên, sự phân nhánh này sẽ bùng nổ ngay lập tức. Ngay cả khi chúng ta cắt bớt các trạng thái giống hệt nhau, số lượng trạng thái có thể tiếp cận vẫn tăng nhanh vì mỗi khoản chi tiêu sẽ tạo ra một lượng mới có thể phân nhánh thành nhiều khả năng. Với$s$lên đến$10^9$, ngay cả việc mô phỏng tuyến tính cũng không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ theo cách tùy tiện$x$, và thay vào đó hãy phân loại tiền thành hai thành phần: số chục đầy đủ và số dư. Việc chi 10 đơn vị là đặc biệt vì nó trả về chính xác 1, nghĩa là lỗ ròng là 9 nhưng nó vẫn bảo toàn khả năng tiếp tục hoạt động tương tự. Điều này tạo ra một chu kỳ ổn định: cứ 10 đơn vị chi tiêu sẽ tạo ra 1 đơn vị có thể tái sử dụng, từ đó cuối cùng có thể tạo thành 10 đơn vị khác. 

Vì vậy, vấn đề giảm xuống còn việc liên tục chuyển đổi các nhóm 10 người thành các đơn vị có thể sử dụng được nhiều hơn, đồng thời tích lũy tất cả chi tiêu có thể có từ các phần còn lại xếp tầng. 

Điều này dẫn đến một quy trình dựa trên chữ số: mỗi lần chúng tôi trích xuất hàng chục từ số tiền hiện tại, hàng chục đó sẽ tạo ra tiền bổ sung, số tiền này lại có thể tạo ra nhiều hàng chục hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O (các) | Quá chậm | 
| Tối ưu | O(log s) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng số một cách độc lập, liên tục trích xuất các đóng góp từ cấu trúc thập phân của nó. 

1. Khởi tạo một biến`ans`về 0. Điều này sẽ tích lũy tổng số tiền đã chi tiêu, bao gồm cả khoản chi tiêu ban đầu và khoản đóng góp hoàn lại tiền được tái sử dụng. 
2. Trong khi số tiền hiện tại`s`ít nhất là 10, hãy loại bỏ cấu trúc chữ số cuối cùng của nó bằng cách chia nó thành hàng chục và số dư. Cho phép`t = s // 10`. Chúng đại diện cho số lượng khối 10 đơn vị đầy đủ tồn tại. 
3. Thêm`t * 10`ĐẾN`ans`. Điều này tương ứng với việc chi tiêu trực tiếp toàn bộ hàng chục. 
4. Cập nhật`s`được`t + s % 10`. Bước này rất quan trọng: cứ 10 đơn vị chi tiêu sẽ tạo ra 1 đơn vị hoàn lại tiền, do đó số lượng đơn vị có thể sử dụng sẽ tăng thêm`t`, và chúng tôi cũng giữ lại các chữ số còn lại. 
5. Lặp lại cho đến khi`s < 10`. 
6. Cuối cùng, thêm phần còn lại`s`ĐẾN`ans`, vì mọi đơn vị còn sót lại có thể được sử dụng hết một lần mà không cần tái chế thêm. 

Lý do điều này có tác dụng là vì mỗi khối 10 được chi tiêu sẽ chuyển đổi một cách hiệu quả thành 1 đơn vị tiền tệ mới, bản thân đơn vị này có thể góp phần hình thành các khối 10 trong tương lai. Do đó, quy trình nén chi tiêu lặp đi lặp lại thành một quá trình tích lũy mang theo chữ số. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trạng thái của hệ thống có thể được biểu diễn dưới dạng số nguyên tiền. Chi tiêu 10 đơn vị là hoạt động duy nhất tạo ra lợi nhuận khác 0 và nó tạo ra chính xác 1 đơn vị. Điều này có nghĩa là hệ thống phát triển giống hệt như thực hiện số học cơ số 10, trong đó mỗi nhóm 10 số được sử dụng sẽ tạo ra một đơn vị mới trong lần lặp tiếp theo. 

Thuật toán bảo toàn bất biến`s`đại diện cho tất cả các đơn vị hiện chưa được chi tiêu, bao gồm cả số tiền hoàn lại chưa được chuyển đổi thành cơ hội chi tiêu đầy đủ cho 10 đơn vị. Mỗi lần lặp lại giải quyết tất cả các chu kỳ 10 đơn vị ngay lập tức có thể có trước khi chuyển lên trên, do đó không có cơ hội chi tiêu tiềm năng nào bị bỏ qua hoặc được tính gấp đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = int(input())
        ans = 0
        
        while s >= 10:
            t10 = s // 10
            ans += t10 * 10
            s = t10 + (s % 10)
        
        ans += s
        print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp tuân theo quá trình phân hủy lặp đi lặp lại của số tiền thành hàng chục và phần còn lại. Biến`t10`thể hiện số lượng mua hàng 10 chi phí đầy đủ có thể được thực hiện ở giai đoạn hiện tại. Những điều đó góp phần`t10 * 10`để trả lời. 

bản cập nhật`s = t10 + (s % 10)`mã hóa việc tái nạp tiền hoàn lại: mỗi nhóm 10 đơn vị chi tiêu sẽ tạo ra một đơn vị mới và phần còn lại không thay đổi. 

Vòng lặp tiếp tục cho đến khi không thể tạo thêm hàng chục nữa, lúc đó tất cả các đơn vị còn lại sẽ được thêm trực tiếp vào câu trả lời. 

Một điểm tinh tế là chúng tôi không bao giờ trừ đi một cách rõ ràng số tiền hoàn lại từ chi tiêu; thay vào đó, nó được ngầm thêm trở lại vào`s`, giúp giữ trạng thái nhỏ gọn và tránh theo dõi các giao dịch trong quá khứ. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào:`19`Và`9876`. 

### Ví dụ 1: s = 19 

| s | t10 = s//10 | trả lời thêm | mới | 
| --- | --- | --- | --- | 
| 19 | 1 | 10 | 1 + 9 = 10 | 
| 10 | 1 | 10 | 1 + 0 = 1 | 

Bước cuối cùng thêm 1 còn lại, tổng = 21. 

Điều này cho thấy khoản tiền hoàn lại từ 10 lần đầu tiên tạo ra một cơ hội mua hàng đầy đủ khác như thế nào, điều này rất cần thiết cho câu trả lời tối ưu. 

### Ví dụ 2: s = 9876 

| s | t10 | trả lời thêm | mới | 
| --- | --- | --- | --- | 
| 9876 | 987 | 9870 | 987 + 6 = 993 | 
| 993 | 99 | 990 | 99 + 3 = 102 | 
| 102 | 10 | 100 | 10 + 2 = 12 | 
| 12 | 1 | 10 | 1 + 2 = 3 | 

Câu trả lời cuối cùng cộng thêm 3, tổng cộng là 10973. 

Dấu vết này cho thấy việc nén lặp đi lặp lại hàng chục thành các đơn vị bậc cao hơn, thể hiện cách hoàn tiền lan truyền lên trên qua nhiều lần lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log_{10} s)$mỗi bài kiểm tra | Mỗi lần lặp lại làm giảm cường độ ít nhất 10 lần trong quy mô hiệu quả | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Hành vi logarit dễ dàng đủ nhanh để$10^4$trường hợp thử nghiệm với$s \le 10^9$, vì mỗi thử nghiệm chạy dưới 10 lần lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = int(input())
        ans = 0
        while s >= 10:
            t10 = s // 10
            ans += t10 * 10
            s = t10 + (s % 10)
        ans += s
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("6\n1\n10\n19\n9876\n12345\n1000000000\n") == \
"1\n11\n21\n10973\n13716\n1111111111"

# custom cases
assert run("1\n9\n") == "9", "single digit no operations"
assert run("1\n10\n") == "11", "single exact block"
assert run("1\n20\n") == "22", "two tens cascade"
assert run("1\n99\n") == "109", "repeated cascading carry behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9 | 9 | không có chu kỳ hoàn tiền | 
| 10 | 11 | vòng chuyển đổi đơn | 
| 20 | 22 | gia cố nhiều chu kỳ | 
| 99 | 109 | hành vi mang chữ số xếp tầng | 

## Vỏ cạnh 

Đối với đầu vào nhỏ như$s = 1$bởi vì$s = 9$, vòng lặp không bao giờ chạy. Thuật toán trực tiếp trả về cùng một giá trị vì không tồn tại chu trình 10 đơn vị. Ví dụ, với$s = 7$,`s >= 10`là sai, vì vậy`ans = 0`và đầu ra cuối cùng là 7. 

Đối với bội số chính xác của 10, chẳng hạn như$s = 10$, lần lặp đầu tiên sẽ chuyển đổi nó thành đơn vị hoàn lại tiền:`t10 = 1`, Vì thế`ans = 10`, Và`s`trở thành 1. Kết quả cuối cùng trở thành 11, phù hợp với thực tế là số tiền hoàn lại cho phép thêm một đơn vị chi tiêu. 

Đối với các trường hợp xếp tầng lớn hơn như$s = 19$, 10 đơn vị đầu tiên tạo ra một đơn vị mới ngay lập tức kết hợp lại thành 10 đơn vị khác, cho thấy rằng quy trình nắm bắt chính xác việc tái sử dụng tiền hoàn lại ở nhiều cấp độ mà không cần đệ quy rõ ràng.
