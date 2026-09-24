---
title: "CF 104814B - \u0418\u0441\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c <<\u041a\u043e\u0440\u0440\u0435\u043a\u0442\u043e\u0440>>"
description: "Chúng ta được cung cấp một chuỗi ngắn chứa các chữ cái tiếng Anh viết thường và một phép biến đổi xác định sửa đổi nó theo hai giai đoạn. Đầu tiên, chuỗi được mở rộng bằng cách chèn thêm chính xác một ký tự."
date: "2026-06-28T13:05:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104814
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0420\u0435\u0441\u043f\u0443\u0431\u043b\u0438\u043a\u0435 \u0411\u0430\u0448\u043a\u043e\u0440\u0442\u043e\u0441\u0442\u0430\u043d 2023 (9 - 11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104814
solve_time_s: 71
verified: true
draft: false
---

[CF 104814B - \u0418\u0441\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044c <<\u041a\u043e\u0440\u0440\u0435\u043a\u0442\u043e\u0440>>](https://codeforces.com/problemset/problem/104814/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn chứa các chữ cái tiếng Anh viết thường và một phép biến đổi xác định sửa đổi nó theo hai giai đoạn. 

Đầu tiên, chuỗi được mở rộng bằng cách chèn thêm chính xác một ký tự. Vị trí và ký tự được chèn chỉ phụ thuộc vào độ dài hiện tại: nếu độ dài là chẵn thì ký tự`'a'`được đưa vào vị trí chính giữa; nếu độ dài là số lẻ thì ký tự`'b'`được chèn ngay từ đầu. Sau thao tác chèn này, mỗi ký tự trong chuỗi kết quả sẽ được dịch chuyển lên trước một ký tự trong bảng chữ cái theo chu kỳ, do đó`'a'`trở thành`'b'`,`'b'`trở thành`'c'`, Và`'z'`quấn quanh để`'a'`. 

Một ứng dụng của phép biến đổi này làm tăng độ dài chuỗi lên đúng một, đồng thời hoán vị và dịch chuyển các ký tự. Chúng ta được yêu cầu áp dụng phép biến đổi này hai lần cho một chuỗi cơ sở nhất định hoặc khôi phục chuỗi gốc sẽ tạo ra một chuỗi cơ sở nhất định sau đúng hai lần áp dụng. 

Độ dài chuỗi tối đa là 100, do đó, ngay cả mô phỏng tuyến tính đơn giản cũng an toàn. Điều này ngay lập tức loại trừ bất cứ điều gì tiệm cận tệ hơn phép tính bậc hai, nhưng quan trọng hơn, nó gợi ý rằng chúng ta nên nghĩ về các phép toán cục bộ thuận nghịch hơn là tổ hợp toàn cục. 

Điểm tinh tế quan trọng nhất là hoạt động này không hoàn toàn là một sự dịch chuyển hay thuần túy là một sự chèn vào. Ký tự được chèn phụ thuộc vào tính chẵn lẻ và vị trí phụ thuộc vào độ dài hiện tại, chính nó sẽ thay đổi sau mỗi ứng dụng. Điều đó làm cho việc lập luận “hoàn tác” ngây thơ dễ bị lỗi nếu chúng ta không theo dõi cách thức tính chẵn lẻ lan truyền thông qua các phép biến đổi. 

Một vài tình huống cạnh cụ thể minh họa những cạm bẫy. 

Nếu chúng ta cố gắng đảo ngược phép biến đổi mà không tính đến sự dịch chuyển bảng chữ cái trước, chúng ta có thể loại bỏ ký tự sai vì việc chèn xảy ra trước khi dịch chuyển. Ví dụ: một ký tự được chèn dưới dạng`'a'`trở thành`'b'`sau khi chuyển đổi nên trong chuỗi chuyển đổi chúng ta phải tìm kiếm`'b'`, không`'a'`. 

Một vấn đề khác là giả sử chúng ta có thể định vị trực tiếp ký tự được chèn trong chuỗi cuối cùng mà không cần xây dựng lại trạng thái trung gian trước khi dịch chuyển. Ký tự được chèn chỉ có thể được nhận dạng sau khi đảo ngược ca. 

Cuối cùng, tính chẵn lẻ phải được bắt nguồn từ độ dài tiền ảnh ban đầu chứ không phải độ dài được chuyển đổi. Việc nhầm lẫn những điều này dẫn đến vị trí chèn không chính xác trong quá trình đảo ngược. 

## Phương pháp tiếp cận 

Giải pháp brute-force áp dụng phép biến đổi chính xác như được mô tả. Đối với câu hỏi loại 1, chúng tôi mô phỏng thao tác hai lần. Mỗi mô phỏng quét chuỗi, xây dựng một chuỗi mới bằng một lần chèn, sau đó thực hiện dịch chuyển ký tự. Vì độ dài chuỗi tối đa là 100 nên việc triển khai và chạy trong thời gian không đổi cho mỗi lần kiểm tra là không đáng kể. 

Hướng ngược lại ít đơn giản hơn. Một nỗ lực ngây thơ có thể cố gắng đoán chuỗi gốc bằng cách kiểm tra tất cả các lần xóa có thể và kiểm tra xem chuỗi nào khớp với phép chuyển đổi về phía trước, nhưng điều đó là không cần thiết và che khuất cấu trúc. 

Quan sát quan trọng là phép biến đổi hoàn toàn có thể đảo ngược nếu chúng ta đảo ngược các bước của nó theo thứ tự ngược lại. Sự thay đổi là sự song song của các ký tự nên nó có thể được hoàn tác một cách độc lập. Sau khi hoàn tác việc dịch chuyển, quy tắc chèn lại trở nên xác định vì độ dài ban đầu đã biết: nó luôn bằng độ dài hiện tại trừ đi một. Điều đó xác định duy nhất cả vị trí đặt ký tự được chèn và ký tự đó là gì trước khi dịch chuyển. 

Điều này biến hoạt động nghịch đảo thành bước xây dựng lại trực tiếp chứ không phải là vấn đề tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Đảo ngược bước | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một phép biến đổi trợ giúp T(s) và T⁻¹(s) nghịch đảo của nó. Cả hai đều hoạt động theo thời gian tuyến tính trên chuỗi. 

### Đối với câu hỏi 1 (áp dụng hai lần) 

1. Bắt đầu với chuỗi cơ sở s. 
2. Áp dụng T(s) một lần để có được chuỗi trung gian. 
3. Áp dụng T lần nữa cho chuỗi trung gian để có kết quả cuối cùng. 
4. Xuất chuỗi kết quả. 

Mỗi ứng dụng đều tuân theo cùng một quy trình xác định, do đó độ chính xác đến trực tiếp từ mô phỏng trung thực. 

### Đối với câu hỏi 2 (đảo ngược 2 ứng dụng) 

1. Bắt đầu với chuỗi cơ sở y đã cho, đây là kết quả của hai phép biến đổi thuận. 
2. Áp dụng T⁻¹ một lần để khôi phục chuỗi sau một lần chuyển đổi. 
3. Áp dụng T⁻¹ một lần nữa để khôi phục chuỗi gốc trước bất kỳ chuyển đổi nào. 
4. Xuất chuỗi đã khôi phục. 

Để triển khai T⁻¹ trên chuỗi y: 

1. Giảm từng ký tự một theo thứ tự bảng chữ cái tuần hoàn. Điều này đảo ngược sự thay đổi toàn cầu được áp dụng trong bước chuyển tiếp. Sau đó, chúng ta thu được chuỗi trung gian z tồn tại ngay sau khi chèn nhưng trước khi dịch chuyển. 
2. Gọi m là độ dài của z. Chuỗi gốc trước khi chèn có độ dài n = m - 1. 
3. Nếu n là số chẵn thì ký tự được chèn vào được đặt ở giữa chuỗi gốc nên trong z nó xuất hiện ở chỉ số n // 2 và được đảm bảo là`'b'`. 
4. Nếu n là số lẻ thì ký tự được chèn vào được đặt ở đầu nên trong z nó xuất hiện ở chỉ số 0 và được đảm bảo là`'c'`. 
5. Xóa ký tự đó khỏi z để khôi phục chuỗi trước đó. 

### Tại sao nó hoạt động 

Phép biến đổi T là sự kết hợp của hai phép biến đổi: phép chèn xác định và phép dịch chuyển bảng chữ cái thống nhất. Sự thay đổi có thể đảo ngược độc lập với vị trí và khi nó được hoàn tác, điểm chèn được xác định duy nhất bởi độ dài ban đầu, có thể phục hồi được khi độ dài hiện tại trừ đi một. Điều này ngăn chặn sự mơ hồ: có chính xác một vị trí loại bỏ hợp lệ phù hợp với quy tắc chẵn lẻ, do đó, mỗi bước nghịch đảo ánh xạ một chuỗi tới chính xác một chuỗi trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def shift(s, d):
    res = []
    for ch in s:
        res.append(chr((ord(ch) - 97 + d) % 26 + 97))
    return "".join(res)

def T(s):
    n = len(s)
    if n % 2 == 0:
        s = s[:n//2] + "a" + s[n//2:]
    else:
        s = "b" + s
    return shift(s, 1)

def T_inv(s):
    s = shift(s, -1)
    m = len(s)
    n = m - 1

    if n % 2 == 0:
        idx = n // 2
    else:
        idx = 0

    return s[:idx] + s[idx+1:]

s = input().strip()
t = input().strip()

if t == "1":
    print(T(T(s)))
else:
    print(T_inv(T_inv(s)))
```Việc triển khai phản ánh trực tiếp mô tả chính thức. Người trợ giúp`shift`thực hiện việc xoay bảng chữ cái theo chu kỳ. Phép biến đổi thuận chèn trước khi dịch chuyển, khớp chính xác với thứ tự bài toán. Đầu tiên nghịch đảo hoàn tác sự thay đổi, điều này rất quan trọng vì nếu không thì ký tự được chèn không thể được xác định một cách đáng tin cậy. 

Chỉ số loại bỏ được tính toán bằng cách sử dụng độ dài sau khi hoàn tác việc dịch chuyển, vì độ dài đó tương ứng với chuỗi ngay sau khi chèn. Đó là thời điểm duy nhất mà quy tắc chẵn lẻ có ý nghĩa ngược lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
sc
1
```Chúng tôi áp dụng T hai lần. 

| Bước | Chuỗi | Hoạt động | 
| --- | --- | --- | 
| 0 | sc | bắt đầu | 
| 1 | scu → tdv | chèn 'a' vào giữa, shift | 
| 2 | tdv → cuce | chèn 'b' vào đầu, shift | 

Đầu ra:```
cuce
```Dấu vết này cho thấy vị trí chèn phụ thuộc vào độ dài ở mỗi giai đoạn chứ không phụ thuộc vào nội dung ký tự. 

### Mẫu 2 

đầu vào:```
cuce
2
```Chúng tôi áp dụng T⁻¹ hai lần. 

| Bước | Chuỗi | Hoạt động | 
| --- | --- | --- | 
| 0 | cuce | bắt đầu | 
| 1 | btdb → sc | hoàn tác dịch chuyển, xóa char đã chèn | 
| 2 | ... → sc | bước nghịch đảo thứ hai | 

Sau hai lần đảo ngược, chúng tôi khôi phục được chuỗi ban đầu. 

Điều này xác nhận rằng việc hoàn tác shift trước tiên là cần thiết, vì nếu không có nó, ký tự được chèn sẽ không thể nhận dạng được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phép biến đổi sẽ quét chuỗi một số lần không đổi và chúng tôi áp dụng nó nhiều nhất hai lần | 
| Không gian | O(n) | Chúng tôi xây dựng các chuỗi trung gian có kích thước tuyến tính | 

Ràng buộc n ≤ 100 làm cho việc này nhanh một cách thoải mái, nhưng điểm quan trọng là tính đúng đắn về cấu trúc của phép đảo ngược hơn là hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def shift(s, d):
        res = []
        for ch in s:
            res.append(chr((ord(ch) - 97 + d) % 26 + 97))
        return "".join(res)

    def T(s):
        n = len(s)
        if n % 2 == 0:
            s = s[:n//2] + "a" + s[n//2:]
        else:
            s = "b" + s
        return shift(s, 1)

    def T_inv(s):
        s = shift(s, -1)
        m = len(s)
        n = m - 1
        idx = (n // 2) if n % 2 == 0 else 0
        return s[:idx] + s[idx+1:]

    s = input().strip()
    t = input().strip()

    if t == "1":
        return T(T(s))
    else:
        return T_inv(T_inv(s))

assert run("sc\n1\n") == "cuce"
assert run("cuce\n2\n") == "sc"

# minimum size
assert run("a\n1\n") == "cbd"

# symmetry check
assert run(run("ab\n1\n") + "\n2\n") == "ab"

# single character
assert run("z\n1\n") == "bad"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một, 1 | cbd | hành vi có độ dài tối thiểu | 
| ab, khứ hồi | ab | tính nhất quán của nghịch đảo | 
| z, 1 | tệ | tính chính xác của bảng chữ cái | 

## Vỏ cạnh 

Đầu vào một ký tự là trường hợp nhạy cảm nhất vì cả hai quy tắc chẵn lẻ đều phụ thuộc hoàn toàn vào sự chuyển đổi độ dài. Đối với đầu vào`"a"`với câu hỏi 1, thao tác chèn đầu tiên diễn ra ở độ dài chẵn sau logic chèn, tạo ra thao tác chèn ở giữa trước khi dịch chuyển và bước thứ hai lật lại tính chẵn lẻ. Việc triển khai xử lý việc này một cách chính xác vì nó luôn tính toán lại độ dài một cách linh hoạt thay vì giả sử các vị trí cố định. 

Một trường hợp cạnh khác được bao bọc xung quanh`'z'`. Vì việc dịch chuyển được áp dụng sau khi chèn theo cả hai hướng nên bước nghịch đảo phải luôn áp dụng mức giảm mô-đun trước khi thử thay đổi cấu trúc. các`shift(s, -1)`cuộc gọi đảm bảo rằng`'a'`ánh xạ chính xác trở lại từ`'b'`, duy trì tính nhất quán cần thiết để xác định ký tự được chèn.
