---
title: "CF 104636E - CÓ hay CÓ?"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm một chuỗi rất ngắn có đúng ba ký tự. Nhiệm vụ là quyết định xem chuỗi này có đại diện cho từ “CÓ” hay không khi chúng ta bỏ qua trường hợp chữ cái."
date: "2026-06-29T17:06:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104636
codeforces_index: "E"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u043c\u0430\u0441\u0441\u0438\u0432\u044b, \u0441\u0442\u0440\u043e\u043a\u0438"
rating: 0
weight: 104636
solve_time_s: 73
verified: true
draft: false
---

[CF 104636E - CÓ hay CÓ?](https://codeforces.com/problemset/problem/104636/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm một chuỗi rất ngắn có đúng ba ký tự. Nhiệm vụ là quyết định xem chuỗi này có đại diện cho từ “CÓ” hay không khi chúng ta bỏ qua trường hợp chữ cái. Cho phép mọi sự kết hợp giữa chữ hoa và chữ thường, vì vậy tất cả các biến thể như “có”, “Có” hoặc “YEs” đều phải được chấp nhận. 

Đầu ra cũng tùy theo từng trường hợp thử nghiệm. Đối với mỗi chuỗi đầu vào, chúng tôi in xác nhận nếu nó khớp với “CÓ” trong phép so sánh không phân biệt chữ hoa chữ thường, nếu không, chúng tôi sẽ từ chối chuỗi đó. 

Các ràng buộc cực kỳ nhỏ: mỗi chuỗi có độ dài ba và có tối đa 1000 trường hợp kiểm thử. Điều này có nghĩa là ngay cả việc xử lý từng ký tự trực tiếp nhất cũng không đáng kể về mặt hiệu suất. Bất kỳ giải pháp nào thực hiện công việc liên tục cho mỗi trường hợp thử nghiệm đều đủ và ngay cả những cách tiếp cận hơi lãng phí như chuyển đổi chuỗi nhiều lần hoặc so sánh với nhiều biến thể vẫn sẽ vượt qua một cách thoải mái. 

Không có trường hợp cạnh ẩn phức tạp nào về độ dài hoặc cấu trúc, vì mọi đầu vào đều được đảm bảo có chính xác ba ký tự chữ cái. Sự tinh tế duy nhất là bình thường hóa trường hợp. Một phép so sánh đơn giản như kiểm tra sự bằng nhau với chuỗi ký tự "CÓ" mà không điều chỉnh chữ hoa chữ thường sẽ không thành công đối với các đầu vào hợp lệ như "có" hoặc "yEs". Một lỗi tiềm ẩn khác là kiểm tra một phần các chữ cái, chẳng hạn như chỉ so sánh ký tự đầu tiên hoặc sử dụng các thủ thuật từ điển giả sử cách viết hoa giống nhau. 

Một trường hợp thất bại đại diện là: 

đầu vào:```
yEs
```Đầu ra đúng:```
YES
```Một triển khai ngây thơ để kiểm tra`s == "YES"`sẽ in không chính xác NO, vì các trường hợp khác nhau mặc dù các chữ cái khớp với nhau về mặt khái niệm. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là liệt kê tất cả các cách thể hiện hợp lệ của từ “CÓ” bằng cách viết hoa chữ thường. Vì mỗi ký tự trong số ba ký tự này có thể độc lập là chữ hoa hoặc chữ thường nên có$2^3 = 8$các biến thể có thể: 

CÓ, CÓ, CÓ, CÓ, CÓ, CÓ, CÓ, CÓ. 

Chúng tôi có thể lưu trữ bộ này và kiểm tra tư cách thành viên cho mỗi truy vấn. Điều này đúng và chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm, nhưng nó gián tiếp không cần thiết đối với một vấn đề biến đổi nhỏ như vậy. Nó cũng đưa ra những chi phí có thể tránh được trong việc xây dựng hoặc lưu trữ bộ này. 

Một quan sát rõ ràng hơn là tất cả các chuỗi hợp lệ đều trở nên giống hệt nhau sau khi áp dụng một bước chuẩn hóa duy nhất: chuyển đổi mọi ký tự sang cùng một kiểu chữ. Nếu chúng ta chuyển đổi chuỗi đầu vào thành chữ thường hoặc chữ hoa thì mọi biến thể hợp lệ sẽ chính xác trở thành "có" hoặc "CÓ". Điều này làm giảm vấn đề thành một so sánh chuỗi đơn. 

Vì vậy, thay vì xử lý nhiều mẫu, chúng tôi giảm từng trường hợp thử nghiệm thành: 

chuyển đổi s thành chữ thường, sau đó so sánh với "có". 

Đây là sự đơn giản hóa chính: cấu trúc của từ mục tiêu là cố định và trường hợp là nguồn biến thể duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các biến thể | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 
| Bình thường hóa trường hợp và so sánh | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Điều này xác định số lượng kiểm tra độc lập mà chúng tôi sẽ thực hiện. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc chuỗi 3 ký tự. Không cần xử lý trước vì độ dài được cố định. 
3. Chuyển đổi chuỗi thành dạng chuẩn hóa, thường là chữ thường. Điều này loại bỏ tất cả các phân biệt kiểu chữ để các chuỗi giống nhau về mặt logic ánh xạ tới cùng một biểu diễn. 
4. So sánh chuỗi đã chuẩn hóa với chuỗi đích “có”. Nếu chúng khớp chính xác thì đầu vào phải là một số biến thể kiểu chữ “CÓ”, vì vậy chúng ta xuất ra “CÓ”. 
5. Ngược lại, xuất ra “NO”. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là chuyển đổi kiểu chữ là ánh xạ nhiều-một để duy trì nhận dạng chữ cái trong khi loại bỏ các khác biệt về định dạng. Mọi chuỗi đầu vào hợp lệ đều bao gồm các chữ cái giống như “CÓ” theo thứ tự cách viết hoa nào đó, do đó, sau khi chuẩn hóa, tất cả chúng đều thu gọn về một dạng chính tắc duy nhất. Bất kỳ chuỗi nào khác nhau về các chữ cái đều không thể trở thành “có” sau khi chuẩn hóa, vì vậy nó sẽ không bao giờ tạo ra kết quả dương tính giả. 

Điều này tạo ra một bất biến rõ ràng: sau bước 3, chuỗi bằng “có” khi và chỉ khi chuỗi ban đầu là hoán vị chữ hoa chữ thường của “CÓ”. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        if s.lower() == "yes":
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập. các`.lower()`call là phép biến đổi duy nhất được áp dụng, đảm bảo so sánh thống nhất. Việc loại bỏ là cần thiết để loại bỏ ký tự dòng mới khỏi đầu vào; quên đây là nguồn phổ biến của những so sánh không chính xác. 

Các câu lệnh in sử dụng đầu ra chữ hoa, nhưng mọi trường hợp đều được chấp nhận theo câu lệnh vấn đề. Bản thân logic hoàn toàn dựa trên sự bình đẳng được chuẩn hóa. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một vài đầu vào đại diện từ mẫu. 

### Ví dụ 1 

đầu vào:```
YES
yES
Noo
```| s | s.low() | so sánh với "có" | đầu ra | 
| --- | --- | --- | --- | 
| CÓ | vâng | bằng | CÓ | 
| CÓ | vâng | bằng | CÓ | 
| Không | không | không bằng | KHÔNG | 

Điều này cho thấy các biến thể kiểu chữ được thu gọn một cách chính xác, trong khi các chữ cái không khớp vẫn có thể phân biệt được. 

### Ví dụ 2 

đầu vào:```
Yes
YeS
XES
```| s | s.low() | so sánh với "có" | đầu ra | 
| --- | --- | --- | --- | 
| Có | vâng | bằng | CÓ | 
| Có | vâng | bằng | CÓ | 
| XES | xe | không bằng | KHÔNG | 

Điều này chứng tỏ rằng ngay cả một ký tự sai cũng sẽ phá vỡ đẳng thức sau khi chuẩn hóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện chuyển đổi và so sánh chữ thường trong thời gian không đổi | 
| Không gian | O(1) | Không cần thêm dung lượng lưu trữ tương ứng với kích thước đầu vào | 

Giải pháp dễ dàng phù hợp trong giới hạn vì ngay cả trong trường hợp xấu nhất là 1000 trường hợp thử nghiệm, tổng số thao tác ký tự chỉ là vài nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        s = sys.stdin.readline().strip()
        if s.lower() == "yes":
            output.append("YES")
        else:
            output.append("NO")
    return "\n".join(output) + ("\n" if output else "")

# provided samples
assert run("10\nYES\nyES\nyes\nYes\nYeS\nNoo\norZ\nyEz\nYas\nXES\n") == \
"YES\nYES\nYES\nYES\nYES\nNO\nNO\nNO\nNO\nNO\n"

# custom cases
assert run("3\nyes\nYES\nYeS\n") == "YES\nYES\nYES\n", "all valid forms"
assert run("2\nabc\nyes\n") == "NO\nYES\n", "mixed validity"
assert run("2\nYeS\nYES\n") == "YES\nYES\n", "uppercase variants"
assert run("1\nXyZ\n") == "NO\n", "completely different string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các biến thể trường hợp | CÓ CÓ CÓ | độ chính xác chuẩn hóa | 
| dây hỗn hợp | KHÔNG CÓ | từ chối vs chấp nhận | 
| các biến thể chữ hoa | CÓ CÓ | xử lý toàn vụ | 
| chuỗi không liên quan | KHÔNG | phòng ngừa dương tính giả | 

## Vỏ cạnh 

Trường hợp một cạnh là khi dữ liệu đầu vào đã được định dạng chính xác nhưng lại là trường hợp hỗn hợp, chẳng hạn như “YeS”. Thuật toán chuyển đổi nó thành “có” và khớp thành công. Dấu vết là: 

Đầu vào: “Có” → chữ thường trở thành “có” → bằng mục tiêu → đầu ra CÓ. 

Một trường hợp khác là một chuỗi hoàn toàn không liên quan như “XyZ”. Sau khi chuẩn hóa, nó trở thành “xyz”, không khớp với “có”, do đó nó tạo ra NO một cách chính xác. 

Không có trường hợp cạnh cấu trúc nào liên quan đến độ dài hoặc đầu vào trống vì vấn đề đảm bảo các chuỗi có độ dài cố định, do đó tính chính xác phụ thuộc hoàn toàn vào việc khớp ký tự sau khi chuẩn hóa.
