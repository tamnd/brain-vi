---
title: "CF 105828A - \u0411\u0435\u0441\u043a\u043e\u043d\u0435\u0447\u043d\u0430\u044f \u0438\u0433\u0440\u0430"
description: "Chúng ta được cung cấp một hàng ô, mỗi ô chứa một chữ cái viết thường. Mã thông báo bắt đầu ở ô đầu tiên và di chuyển liên tục theo quy tắc xác định."
date: "2026-06-21T13:03:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105828
codeforces_index: "A"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0412\u041a\u041e\u0428\u041f.Junior 2025"
rating: 0
weight: 105828
solve_time_s: 46
verified: true
draft: false
---

[CF 105828A - \u0411\u0435\u0441\u043a\u043e\u043d\u0435\u0447\u043d\u0430\u044f \u0438\u0433\u0440\u0430](https://codeforces.com/problemset/problem/105828/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàng ô, mỗi ô chứa một chữ cái viết thường. Mã thông báo bắt đầu ở ô đầu tiên và di chuyển liên tục theo quy tắc xác định. Từ vị trí hiện tại, nếu tồn tại ít nhất một ô khác trong toàn bộ hàng chứa cùng một chữ cái, mã thông báo sẽ chuyển đến một trong các ô đó; nếu không nó sẽ di chuyển một bước sang phải. Quá trình chỉ dừng khi mã thông báo di chuyển qua ô cuối cùng. 

Câu hỏi không phải là mô phỏng tính ngẫu nhiên mà là để xác định xem có cách nào để quá trình tiếp tục mãi mãi hay không, nghĩa là mã thông báo không bao giờ vượt quá vị trí cuối cùng vì nó liên tục bị mắc kẹt trong các chu kỳ được tạo bởi các chữ cái lặp đi lặp lại. 

Kích thước đầu vào lên tới 100000 ký tự, loại trừ mọi mô phỏng khám phá chuyển đổi từng bước trong nhiều bước. Bất kỳ cách tiếp cận nào có thể truy cập lại các trạng thái nhiều lần mà không bị phát hiện đều có rủi ro về thời gian chạy theo cấp số nhân hoặc vô hạn. Một giải pháp đúng phải giảm quy trình thành thuộc tính cấu trúc của chuỗi. 

Một trường hợp lỗi tinh vi xuất hiện khi các chữ cái lặp đi lặp lại tạo thành chu kỳ thông qua các chỉ số chuyển tiếp. Ví dụ, trong`aaa`, từ vị trí 1, bạn có thể đi đến vị trí 2, từ 2 đến 3 và từ 3 quay lại 1, tạo ra một vòng lặp không bao giờ đến được điểm cuối. Một trực giác ngây thơ chỉ hướng về phía trước sẽ bỏ lỡ rằng việc nhảy lùi có thể xảy ra do “bất kỳ sự cố nào khác”. 

Một trường hợp cạnh khác là một chuỗi như`abca`. Mặc dù các chữ cái lặp lại, cấu trúc không nhất thiết tạo ra một chu trình chặn lối ra, bởi vì chuyển động cuối cùng vẫn có thể đạt đến vị trí mà không tồn tại chu trình bẫy chuyển tiếp. 

## Phương pháp tiếp cận 

Cách diễn giải brute-force coi mỗi vị trí là một trạng thái và áp dụng nhiều lần quy tắc chuyển tiếp. Từ một chỉ mục i cho trước, chúng ta xem xét tất cả các lần xuất hiện của s[i], chọn một trong số chúng và di chuyển đến đó; nếu không thì chúng ta tiến tới i + 1. Điều này xác định một cách tự nhiên một đồ thị có hướng trên các vị trí. Chúng tôi có thể cố gắng mô phỏng tất cả các đường dẫn có thể hoặc chạy phân tích khả năng tiếp cận từ trạng thái 1 đến trạng thái n + 1. 

Vấn đề là mỗi nút có thể có các cạnh đi tới nhiều nút khác và các cạnh đó có thể tạo ra các chu kỳ. Việc khám phá đầy đủ các trạng thái có thể tiếp cận được tính theo cấp số nhân trong trường hợp xấu nhất vì mỗi nhóm chữ cái tạo ra sự kết nối dày đặc giữa các lần xuất hiện của nó. 

Quan sát quan trọng là tính ngẫu nhiên là không liên quan. Điều duy nhất quan trọng là liệu có tồn tại một chu trình trong đồ thị cảm ứng có hướng của các chuyển tiếp cưỡng bức hay không. Nếu từ một chỉ mục nào đó, chúng ta có thể tiếp cận một chỉ mục khác có cùng chữ cái nằm ở bên trái của nó, thì chúng ta có thể xoay vòng giữa các lần xuất hiện của lớp chữ cái đó và không bao giờ tiến tới ranh giới bên phải. 

Điều này làm giảm vấn đề phát hiện xem có tồn tại bất kỳ chữ cái nào xuất hiện ít nhất hai lần ở các vị trí i < j < k sao cho chuyển động có thể quay ngược lại bên trong cùng một lớp chữ cái hay không. Trên thực tế, cấu trúc còn đơn giản hóa hơn nữa: nếu bất kỳ chữ cái nào xuất hiện ít nhất hai lần, thì từ lần xuất hiện đầu tiên của chữ cái đó, quy trình có thể chuyển sang lần xuất hiện sau đó và từ đó, các bước nhảy lặp lại duy nhất có thể giữ chúng ta trong cùng một bộ chỉ mục, ngăn chặn việc thoát được đảm bảo. 

Do đó, vấn đề được giải quyết thành việc kiểm tra xem chuỗi có ký tự lặp lại nào không. Nếu mỗi ký tự xuất hiện đúng một lần thì mã thông báo không bao giờ có thể nhảy lùi hoặc lặp lại, do đó, mã thông báo sẽ di chuyển hoàn toàn sang phải và cuối cùng thoát ra. Nếu bất kỳ ký tự nào lặp lại, sẽ có một chu kỳ có thể truy cập được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(số mũ lớn) | O(n) | Quá chậm | 
| Kiểm tra tần số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và tính tần số của từng ký tự trong khi quét từ trái sang phải. Bước này ghi lại xem có bất kỳ chữ cái nào tạo ra nhiều đích đến có thể hay không. 
2. Nếu bất kỳ ký tự nào xuất hiện nhiều lần, ngay lập tức kết luận rằng quy trình có thể bị mắc kẹt trong một chu trình và xuất ra NO. Lý do là các chữ cái lặp đi lặp lại giới thiệu các bước di chuyển thay thế có thể chuyển hướng mã thông báo khỏi các chỉ số tăng dần. 
3. Nếu tất cả các ký tự đều khác biệt, hãy kết luận rằng không thể lặp lại vòng lặp ngược hoặc vòng lặp bên, do đó, mỗi bước sẽ di chuyển hoàn toàn đến một vị trí duy nhất chưa được truy cập hoặc thoát khỏi mảng, dẫn đến kết thúc được đảm bảo. 

### Tại sao nó hoạt động 

Quá trình này chỉ tạo ra hành vi không chuyển tiếp khi một vị trí có sự xuất hiện khác của chữ cái của nó. Điều kiện đó tương đương với sự tồn tại của các ký tự trùng lặp. Khi tồn tại các bản sao, hệ thống sẽ chứa ít nhất một thành phần được kết nối mạnh mẽ trên các chỉ số được tạo ra bởi các chữ cái giống nhau, cho phép xem lại và ngăn chặn việc thoát khỏi được đảm bảo. Nếu không tồn tại các bản sao, mỗi vị trí chỉ có một hướng chuyển tiếp chính xác và không có chuyển đổi thay thế nào, buộc phải có một đường dẫn tăng dần cho đến khi kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
cnt = [0] * 26

for ch in s:
    cnt[ord(ch) - 97] += 1

for x in cnt:
    if x > 1:
        print("NO")
        break
else:
    print("YES")
```Giải pháp duy trì một dải tần số có kích thước cố định trên 26 chữ cái viết thường, đủ vì bảng chữ cái đã cố định. Mỗi ký tự tăng bộ đếm của nó theo thời gian không đổi. 

Vòng lặp cuối cùng kiểm tra xem có ký tự nào lặp lại hay không. các`else`mệnh đề trên vòng lặp đảm bảo rằng chúng tôi chỉ in`YES`nếu không tìm thấy sự lặp lại. Điều này tránh việc theo dõi trạng thái bổ sung và giữ cho logic tuyến tính và rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`abc`| Bước | Nhân vật | Cập nhật tần suất | Tìm thấy trùng lặp | 
| --- | --- | --- | --- | 
| 1 | một | một:1 | Không | 
| 2 | b | b:1 | Không | 
| 3 | c | c:1 | Không | 

Vì không có chữ cái nào lặp lại nên thuật toán đưa ra CÓ. Điều này phản ánh rằng mọi bước di chuyển đều bị buộc phải tiến về phía trước và mã thông báo cuối cùng phải rời khỏi mảng. 

### Ví dụ 2:`aaa`| Bước | Nhân vật | Cập nhật tần suất | Tìm thấy trùng lặp | 
| --- | --- | --- | --- | 
| 1 | một | một:1 | Không | 
| 2 | một | một:2 | Có | 
| 3 | một | một:3 | Có | 

Một bản sao được phát hiện ngay lập tức, vì vậy đầu ra là KHÔNG. Điều này tương ứng với khả năng nhảy giữa các lần xuất hiện của`a`và duy trì trong một vòng khép kín. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và tần số được kiểm tra theo thời gian không đổi | 
| Không gian | O(1) | Đã sửa mảng kích thước 26 cho chữ thường | 

Quét tuyến tính là tối ưu cho n lên tới 100000 và bộ nhớ không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    cnt = [0] * 26
    for ch in s:
        cnt[ord(ch) - 97] += 1

    for x in cnt:
        if x > 1:
            return "NO"
    return "YES"

# provided samples
assert run("abc\n") == "YES"
assert run("aaa\n") == "NO"

# custom cases
assert run("a\n") == "YES"
assert run("abac\n") == "NO"
assert run("abcdefghijklmnopqrstuvwxyz\n") == "YES"
assert run("zzzzzz\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một | CÓ | kích thước tối thiểu, ký tự đơn | 
| bàn tính | KHÔNG | phát hiện trùng lặp không liền kề | 
| abcdefghijklmnopqrstuvwxyz | CÓ | trường hợp khác biệt tối đa | 
| zzzzzz | KHÔNG | cạnh lặp lại hoàn toàn bằng nhau | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như`a`, thuật toán sẽ đếm nó một lần và không tìm thấy bản sao nào, vì vậy nó xuất ra CÓ. Quá trình di chuyển sang phải một cách tầm thường và thoát ra ngay lập tức. 

Đối với một chuỗi như`abac`, nhân vật`a`xuất hiện nhiều lần. Mảng tần số đánh dấu một bản sao, kích hoạt NO. Điều này phù hợp với sự tồn tại của nhiều vị trí có cùng một chữ cái, cho phép xem lại các chỉ mục trước đó và tạo thành một chu trình trong biểu đồ chuyển tiếp.
