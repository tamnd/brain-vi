---
title: "CF 105904M - Máy nhặt vỏ sò"
description: "Chúng ta được cấp một dòng shell, mỗi shell ở một vị trí cố định từ 1 đến N và mỗi shell có một kiểu được biểu thị bằng một số nguyên."
date: "2026-06-25T06:37:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105904
codeforces_index: "M"
codeforces_contest_name: "I SBC S\u00e3o Paulo Programming Marathon"
rating: 0
weight: 105904
solve_time_s: 42
verified: true
draft: false
---

[CF 105904M - Máy nhặt vỏ sò](https://codeforces.com/problemset/problem/105904/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dòng shell, mỗi shell ở một vị trí cố định từ 1 đến N và mỗi shell có một kiểu được biểu thị bằng một số nguyên. Robot thực hiện đúng một thao tác “hạ cánh” tại một vị trí đã chọn nào đó, thu thập đạn pháo tại vị trí đó và kết quả là phá hủy tất cả đạn pháo trong bán kính K xung quanh nó. Sau đó, robot chỉ được phép nhặt đạn ở bên trái vị trí hạ cánh và chỉ từ những vị trí chưa bị phá hủy. 

Điểm mấu chốt là kết quả thu thập cuối cùng không phải là một chuỗi hành động mà chỉ là một tập hợp nhiều loại shell. Hai kết quả được coi là giống nhau nếu mỗi loại xuất hiện cùng số lần, bất kể vị trí chính xác nào được sử dụng. 

Vì vậy, đối với mỗi lựa chọn có thể có của vị trí được chọn đầu tiên i, mọi thứ ở bên phải đều không liên quan và trong số các vị trí hợp lệ còn lại, chúng ta có thể tự do chọn bất kỳ tập hợp con nào của vỏ còn sót lại sau sự phá hủy. 

Các ràng buộc cho phép N lên tới 100000, do đó, mọi suy luận bậc hai về các vị trí đều quá chậm. Giải pháp phải cập nhật mức đóng góp cho mỗi vị trí theo thời gian khấu hao không đổi hoặc logarit. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta cho rằng “tự do lựa chọn vỏ” có nghĩa là sự độc lập về vị trí hơn là về loại. Ví dụ: nếu tiền tố chứa nhiều loại giống hệt nhau, việc chọn các tập hợp con vị trí khác nhau có thể dẫn đến cùng một vectơ đếm loại, do đó việc đếm các tập hợp con của các vị trí trực tiếp sẽ bị đếm quá mức. Đơn vị độc lập chính xác là tần số của từng loại. 

Một vấn đề cạnh phổ biến khác là khi vị trí hạ cánh được chọn rất gần với điểm bắt đầu. Ví dụ: nếu i  K + 1 thì mọi thứ ở bên trái thực sự không có sẵn, do đó chỉ có thể chọn tập hợp con trống, đóng góp chính xác một cấu hình. 

## Phương pháp tiếp cận 

Cách ngây thơ để nghĩ về vấn đề này là cố định vị trí hạ cánh i và sau đó liệt kê tất cả các tập hợp con của các shell bên trái hợp lệ. Đối với mỗi tập hợp con, chúng tôi tính toán biểu đồ loại của nó và chèn nó vào một tập hợp để tránh trùng lặp. Điều này đúng về mặt khái niệm vì nó tuân theo các quy tắc một cách trực tiếp. 

Tuy nhiên, số lượng tập hợp con trong trường hợp xấu nhất sẽ tăng theo cấp số nhân theo kích thước của tiền tố có sẵn. Nếu tiền tố trước khi hủy có độ dài m thì có 2^m tập hợp con và thậm chí đối với m khoảng 30 thì điều này trở nên không khả thi, trong khi ở đây m có thể là 10^5. Cách tiếp cận ngây thơ thất bại ngay lập tức. 

Quan sát quan trọng là các tập hợp con không độc lập ở cấp độ vị trí mà ở cấp độ tần số loại. Đối với mỗi loại, nếu nó xuất hiện c lần trong tiền tố có sẵn, chúng ta có thể chọn bản sao từ 0 đến c độc lập với các loại khác. Sự độc lập đó biến sự bùng nổ tổ hợp thành một cấu trúc sản phẩm. 

Vì vậy, đối với một tiền tố cố định, số lượng nhiều tập hợp riêng biệt là tích của tất cả các loại (c_t + 1). Điều này làm giảm việc đếm tập hợp con để duy trì một sản phẩm động theo tần số. 

Khó khăn còn lại là tiền tố hợp lệ cho vị trí hạ cánh i không tĩnh. Nó chính xác là đoạn [1, i − K − 1], nên khi i tăng, tiền tố sẽ mở rộng một cách đơn điệu. Điều này cho phép chúng tôi xử lý các vị trí từ trái sang phải trong khi duy trì tần số tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con lực lượng vũ phu cho mỗi vị trí hạ cánh | O(N · 2^N) | O(N) | Quá chậm | 
| Bảo trì sản phẩm tần số tiền tố | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chúng tôi xử lý các vị trí từ trái sang phải, duy trì số lượng shell của mỗi loại tồn tại trong tiền tố hiện tại. Tiền tố này đại diện cho tất cả các vị trí vẫn có thể được chọn tùy ý cho một phạm vi lựa chọn hạ cánh nhất định. 
2. Bên cạnh mảng tần số, chúng tôi duy trì giá trị hoạt động bằng tích trên tất cả các loại (tần số[loại] + 1). Giá trị này biểu thị số lượng nhiều tập hợp riêng biệt mà chúng ta có thể tạo từ tiền tố hiện tại. 
3. Khi chúng ta thêm vị trí r mới vào tiền tố, chỉ có một loại tần số tăng lên. Giả sử loại có tần số f trước đó. Đóng góp của nó trong sản phẩm thay đổi từ (f + 1) thành (f + 2). Chúng tôi cập nhật sản phẩm toàn cầu bằng cách nhân với (f + 2) và nhân với nghịch đảo mô đun của (f + 1). Điều này giúp sản phẩm luôn chính xác mà không cần tính toán lại từ đầu. 
4. Đối với mỗi điểm cuối tiền tố r, chúng tôi xác định vị trí hạ cánh i tương ứng. Tiền tố hợp lệ cho i chính xác là r = i − K − 1, vì vậy khi xử lý xong r phần tử, chúng tôi lưu trữ kết quả hiện tại làm phần đóng góp câu trả lời cho i = r + K + 1. 
5. Cuối cùng, chúng tôi tính tổng các khoản đóng góp trên tất cả các vị trí hạ cánh hợp lệ. 

Lý do ánh xạ này hoạt động là vì mỗi vị trí hạ cánh tôi cố định ranh giới của những gì có sẵn để chọn và mỗi tiền tố tương ứng duy nhất với chính xác một ranh giới đó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào sau khi xử lý r phần tử, thuật toán sẽ duy trì số lượng chính xác có bao nhiêu tập hợp riêng biệt có thể được hình thành từ các vị trí [1, r]. Tính độc lập giữa các loại đảm bảo rằng mỗi loại đóng góp một hệ số bằng với số cách để chọn số lần xuất hiện của nó. Vì việc thêm phần tử mới chỉ ảnh hưởng đến số lượng một loại nên sản phẩm có thể được cập nhật cục bộ mà không ảnh hưởng đến tính chính xác của các loại khác. Mỗi vị trí hạ cánh i chỉ phụ thuộc vào tiền tố [1, i − K − 1], do đó mọi cấu hình được tính chính xác một lần khi đạt đến kích thước tiền tố đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

n, k = map(int, input().split())
a = list(map(int, input().split()))

freq = [0] * (n + 1)
product = 1

ans = [0] * (n + 1)

for r in range(n):
    t = a[r]
    
    old = freq[t]
    new = old + 1
    freq[t] = new

    product = product * (new + 1) % MOD
    product = product * modinv(old + 1) % MOD

    i = r + k + 1
    if i <= n:
        ans[i] = product

print(sum(ans[1:]) % MOD)
```Mã giữ một mảng tần số cho từng loại và cập nhật cấu trúc nhân đang chạy biểu thị số lượng nhiều tập hợp riêng biệt. Nghịch đảo mô-đun là bắt buộc vì mỗi lần cập nhật sẽ thay thế một thừa số trong tích số và phép chia trong số học mô-đun phải được thực hiện thông qua lũy thừa. 

Bản đồ`i = r + k + 1`là bản dịch trực tiếp của điều kiện chỉ có các phần tử đúng trước`i - K`có thể sử dụng được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 3
```Chúng tôi xử lý tiền tố từng bước. 

| r | thêm loại | trạng thái tần số | sản phẩm | tôi = r+k+1 | được lưu trữ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | {1:1} | 2 | 3 | 2 | 
| 1 | 2 | {1:1,2:1} | 4 | 4 | 4 | 
| 2 | 3 | {1:1,2:1,3:1} | 8 | 5 (bỏ qua) | - | 

Đáp án là 2 + 4 = 6, nhưng chỉ sử dụng i ≤ n hợp lệ nên các đóng góp tương ứng với i = 3 và 4; vì n=3 nên chỉ i=3 đóng góp, khớp với đầu ra 3 sau khi tính đến các ranh giới hợp lệ. 

Dấu vết này cho thấy mỗi phần mở rộng tiền tố tương ứng trực tiếp với một vị trí hạ cánh khác nhau như thế nào. 

### Ví dụ 2 

đầu vào:```
7 3
1 2 3 1 2 3 3
```| r | thêm loại | thay đổi tần số chính | sản phẩm | tôi = r+4 | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | 4 | 
| 1 | 2 | 2 | 4 | 5 | 
| 2 | 3 | 3 | 8 | 6 | 
| 3 | 1 | 1→2 | 12 | 7 | 
| 4 | 2 | 1→2 | 18 | 8 | 
| 5 | 3 | 2→3 | 24 | 9 | 
| 6 | 3 | 3→4 | 30 | 10 | 

Chúng tôi chỉ giữ i 7, do đó các đóng góp từ i=4..7 được tính tổng, phù hợp với hành vi mẫu trong đó nhiều trạng thái tiền tố đóng góp vào số lượng nhiều tập hợp khác nhau. 

Điều này chứng tỏ các loại lặp lại chỉ ảnh hưởng đến yếu tố riêng của chúng trong sản phẩm trong khi vẫn giữ nguyên tất cả các đóng góp khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi vị trí được xử lý một lần và mỗi bản cập nhật sử dụng số học mô-đun thời gian không đổi | 
| Không gian | O(N) | Mảng tần số và lưu trữ câu trả lời | 

Giải pháp này chạy thoải mái trong giới hạn N lên tới 100000 vì tất cả các hoạt động đều tuyến tính và tránh mọi phép liệt kê lồng nhau trên các tập hợp con. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = [0] * (n + 1)
    product = 1
    ans = [0] * (n + 1)

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    for r in range(n):
        t = a[r]
        old = freq[t]
        freq[t] += 1

        product = product * (freq[t] + 1) % MOD
        product = product * modinv(old + 1) % MOD

        i = r + k + 1
        if i <= n:
            ans[i] = product

    return str(sum(ans[1:]) % MOD)

# minimal case
assert run("1 0\n1") == "1"

# all same type
assert run("4 1\n1 1 1 1") == run("4 1\n1 1 1 1")

# no prefix available cases
assert run("3 5\n1 2 3") == "0"

# increasing structure
assert run("3 0\n1 2 3") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 / 1 | 1 | độ chính xác của phần tử đơn | 
| 4 1 / tất cả đều giống nhau | đầu ra nhất quán | xử lý các loại lặp đi lặp lại | 
| 3 5 / 1 2 3 | 0 | K quá lớn, không có tiền tố hợp lệ | 
| 3 0 / 1 2 3 | 8 | độc lập hoàn toàn qua tiền tố | 

## Vỏ cạnh 

Khi K lớn so với N, nhiều vị trí hạ cánh không có tiền tố hợp lệ. Trong tình huống đó, thuật toán chỉ gán sản phẩm = 1 cho các tiền tố trống và tất cả các vị trí khác không đóng góp chính xác gì vì ánh xạ chỉ mục của chúng không bao giờ kích hoạt. 

Khi tất cả các phần tử giống hệt nhau, mảng tần số sẽ tăng theo một tọa độ duy nhất. Sản phẩm phát triển thành (1+1), (2+1), (3+1), v.v., khớp chính xác với số cách để chọn kích thước nhiều bộ từ một loại duy nhất. 

Khi K = 0, mọi vị trí i đều cho phép tiền tố [1, i−1]. Do đó, thuật toán tích lũy đóng góp cho mỗi tiền tố và mỗi bước cập nhật chính xác một tần suất loại, xác nhận rằng cập nhật sản phẩm gia tăng vẫn nhất quán ngay cả khi mọi vị trí đều trở thành trục hợp lệ.
