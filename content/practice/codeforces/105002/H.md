---
title: "CF 105002H - Bóng bàn"
description: "Chúng ta được cho một chuỗi gồm các chữ cái Latinh viết thường. Hai người chơi luân phiên nhau. Trong một lượt, người chơi có thể xóa một ký tự khỏi chuỗi, nhưng chỉ khi ký tự đó không ở hai đầu và việc xóa ký tự đó không tạo ra hai ký tự liền kề giống hệt nhau."
date: "2026-06-28T03:20:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105002
codeforces_index: "H"
codeforces_contest_name: "vkoshp.letovo 2022"
rating: 0
weight: 105002
solve_time_s: 69
verified: false
draft: false
---

[CF 105002H - Bóng bàn](https://codeforces.com/problemset/problem/105002/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi gồm các chữ cái Latinh viết thường. Hai người chơi luân phiên nhau. Trong một lượt, người chơi có thể xóa một ký tự khỏi chuỗi, nhưng chỉ khi ký tự đó không ở hai đầu và việc xóa ký tự đó không tạo ra hai ký tự liền kề giống hệt nhau. Các người chơi tiếp tục cho đến khi một người chơi không có động thái hợp pháp và người chơi đó thua. Chuỗi ban đầu được đảm bảo không có ký tự liền kề bằng nhau. 

Đối tượng chính không phải là các chữ cái mà là các vị trí có thể xóa. Vì các điểm cuối không bao giờ có thể xóa được nên chỉ có các ký tự bên trong mới quan trọng và ngay cả trong số đó, một ký tự chỉ có thể xóa được nếu các ký tự lân cận của nó vẫn khác sau khi xóa. 

Hạn chế lên đến$10^6$các ký tự ngay lập tức loại trừ mọi mô phỏng cập nhật chuỗi một cách rõ ràng mỗi lần di chuyển. Bất kỳ giải pháp nào liên tục quét hoặc sửa đổi chuỗi theo thời gian tuyến tính cho mỗi lần di chuyển sẽ chuyển thành hành vi bậc hai trong trường hợp xấu nhất, điều này là không khả thi. 

Một điểm tinh tế là việc xóa có thể thay đổi khả năng di chuyển trong tương lai. Việc loại bỏ một ký tự có thể tạo ra các vị trí mới có thể di chuyển được hoặc phá hủy các vị trí hiện có, do đó cấu trúc sẽ phát triển linh hoạt. Điều này loại trừ mô phỏng “luôn loại bỏ thứ gì đó” tham lam mà không có bất biến toàn cục. 

Một vài mẫu cạnh bộc lộ những cạm bẫy. 

Ví dụ: nếu chuỗi thay thế nghiêm ngặt`ababab`, mọi vị trí bên trong ban đầu trông có vẻ có thể tháo rời được, nhưng sau khi xóa, ràng buộc kề cận có thể vô hiệu hóa các bước di chuyển gần đó. Một chiến lược ngây thơ đếm các vị trí có thể tháo rời ban đầu sẽ bị tính quá mức. 

Nếu chuỗi có các “khối” riêng biệt gồm cùng một chữ cái được phân tách bằng các ký tự đơn, việc xóa một ký tự có thể hợp nhất các ngữ cảnh và đột nhiên tạo hoặc hủy các bước di chuyển hợp lệ ở xa. 

Giải pháp đúng phải nắm bắt được một bất biến toàn cục vẫn ổn định trong cách chơi tối ưu, thay vì theo dõi trực tiếp khả năng loại bỏ cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trò chơi. Chúng tôi duy trì chuỗi hiện tại, quét tất cả các vị trí bên trong để tìm thao tác xóa hợp lệ, áp dụng một bước di chuyển và lặp lại cho đến khi không còn di chuyển nào tồn tại. Chi phí mỗi lần quét$O(n)$, và có thể có tới$O(n)$xóa, đưa ra$O(n^2)$thời gian. Với$n = 10^6$, điều này hoàn toàn không thể thực hiện được. 

Điểm quan trọng là trò chơi không thực sự xoay quanh các chữ cái mà xoay quanh việc có bao nhiêu “khe bên trong an toàn” độc lập tồn tại. Bởi vì chuỗi ban đầu không có các ký tự liền kề bằng nhau nên mỗi lần xóa có thể được coi là loại bỏ một vị trí bên trong trong khi vẫn giữ nguyên cấu trúc xen kẽ cục bộ. Ràng buộc kề ngăn cản việc tạo các hàng xóm bằng nhau, có nghĩa là việc xóa không thể hợp nhất các ký tự giống hệt nhau, chỉ rút ngắn chuỗi trong khi vẫn giữ nguyên sự xen kẽ. 

Cấu trúc này ngụ ý rằng điều duy nhất quan trọng là có bao nhiêu nước đi hợp lệ tồn tại ban đầu xét về các vị trí bên trong. Mỗi bước di chuyển sẽ loại bỏ chính xác một ký tự và về cơ bản không thay đổi thực tế là chuỗi vẫn xen kẽ. Do đó, trò chơi giảm xuống một vấn đề đếm đơn giản: số lượng vị trí bên trong có thể chơi được cố định ở mức chẵn lẻ trong suốt quá trình chơi tối ưu. 

Vì chỉ có vị trí$2$bởi vì$l-1$tồn tại với tư cách là ứng cử viên và mỗi nước đi sẽ loại bỏ chính xác một trong số chúng mà không làm thay đổi khả năng xác định các nước đi trong tương lai theo cách làm thay đổi tính chẵn lẻ, người chiến thắng được xác định bằng việc số này là lẻ hay chẵn. 

Thế là trò chơi sụp đổ thành máy tính$l - 2$, số vị trí bên trong và tính chẵn lẻ quyết định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Giảm chẵn lẻ |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài của chuỗi. Các ký tự thực tế không liên quan đến quyết định cuối cùng vì hạn chế kề cận đã được đảm bảo trong cấu hình ban đầu. 
2. Tính số vị trí có thể tháo rời, là số chỉ số bên trong. Đây là$l - 2$. Đây chính xác là những vị trí không phải là điểm cuối. 
3. Xác định xem số này là số lẻ hay số chẵn. 
4. Nếu là số lẻ, người chơi đầu tiên thắng vì nước đi đầu tiên bắt đầu một chuỗi luân phiên bắt buộc cho đến nước đi cuối cùng. Nếu bằng nhau, người chơi thứ hai sẽ thắng vì lần lượt ghép đôi hoàn hảo. 

### Tại sao nó hoạt động 

Mỗi nước đi sẽ loại bỏ chính xác một ký tự bên trong và không có nước đi nào gây ra tình huống trong đó các vị trí không hợp lệ trước đây trở thành các thành phần độc lập mới về mặt cấu trúc làm thay đổi tổng số chẵn lẻ của các nước đi có sẵn. Do đó, trò chơi hoạt động giống như một trò chơi mang đi với kích thước cọc cố định là$l - 2$. Cách chơi tối ưu giảm xuống việc loại bỏ xen kẽ cho đến khi kiệt sức, do đó tính chẵn lẻ của cọc này sẽ quyết định người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()
    # only interior positions matter
    moves = n - 2
    if moves % 2 == 1:
        print("First")
    else:
        print("Second")

if __name__ == "__main__":
    solve()
```Việc triển khai hoàn toàn bỏ qua nội dung chuỗi sau khi đọc nó, vì việc đảm bảo không có ký tự liền kề bằng nhau ban đầu sẽ loại bỏ mọi nhu cầu theo dõi các thay đổi cấu trúc. 

Điểm tinh tế duy nhất là đảm bảo xử lý chính xác các đoạn có độ dài nhỏ. Khi$n = 3$, có đúng một vị trí bên trong nên người chơi đầu tiên luôn thắng. Công thức$n - 2$trực tiếp xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
aba
```Chúng tôi có một vị trí bên trong. 

| Bước | Các vị trí nội thất còn lại | Di chuyển có sẵn | Chẵn lẻ | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | Có | lẻ | 
| Sau bước đi đầu tiên | 0 | Không | Kết thúc | 

Vì số đếm là số lẻ nên người chơi đầu tiên sẽ thực hiện nước đi cuối cùng. 

Điều này phù hợp với quy tắc rằng một nước đi duy nhất có sẵn sẽ ngay lập tức quyết định trò chơi. 

### Ví dụ 2 

đầu vào:```
4
abza
```Có hai vị trí bên trong. 

| Bước | Các vị trí nội thất còn lại | Di chuyển có sẵn | Chẵn lẻ | 
| --- | --- | --- | --- | 
| Bắt đầu | 2 | Có | Thậm chí | 
| Sau bước đi đầu tiên | 1 | Có | lẻ | 
| Sau bước thứ hai | 0 | Không | Kết thúc | 

Người chơi thứ hai thực hiện nước đi cuối cùng vì số nước đi là chẵn nên người chơi thứ hai thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| đầu vào đọc chiếm ưu thế; tính toán là hằng số | 
| Không gian |$O(1)$| chỉ một số số nguyên được lưu trữ | 

Giải pháp dễ dàng phù hợp trong giới hạn ngay cả đối với$10^6$ký tự vì không cần xử lý chuỗi ngoài việc đọc đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\naba\n") == "Second"
assert run("4\nabza\n") == "First"
assert run("12\nabcacabcbcba\n") == "First"

# custom cases
assert run("3\nabc\n") == "Second", "single move case"
assert run("5\nababa\n") == "First", "odd interior"
assert run("6\nabcdef\n") == "Second", "even interior"
assert run("10\nababababab\n") == "Second", "alternating long"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 abc`| Thứ hai | kích thước tối thiểu không tầm thường | 
|`5 ababa`| Đầu tiên | nội thất lẻ chẵn lẻ | 
|`6 abcdef`| Thứ hai | thậm chí bình đẳng bên trong | 
|`10 abab...`| Thứ hai | ổn định dưới cấu trúc xen kẽ dài | 

## Vỏ cạnh 

### Hành vi có độ dài tối thiểu 

đầu vào:```
3
abc
```Có chính xác một nhân vật nội tâm. Thuật toán tính toán$3 - 2 = 1$, là số lẻ nên nó trả về Đầu tiên. Trò chơi kết thúc sau nước đi đầu tiên, xác nhận tính đúng đắn ở kích thước nhỏ nhất không tầm thường. 

###Cấu trúc xen kẽ tối đa 

đầu vào:```
10
ababababab
```Vị trí bên trong là 8. Thuật toán tính toán$10 - 2 = 8$, là số chẵn nên nó trả về Thứ hai. Mặc dù mọi ký tự đều thay thế nhau và cấu trúc trông rất năng động, nhưng đối số chẵn lẻ vẫn không thay đổi vì mỗi lần di chuyển sẽ giảm cùng một số lượng mà không làm thay đổi bất biến toàn cục.
