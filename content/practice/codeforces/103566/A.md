---
title: "CF 103566A - \u0411\u0443\u043a\u0432\u044b \u043d\u0430 \u0437\u0430\u043a\u0430\u0437"
description: "Vấn đề biến ngôn ngữ thành thuộc tính cấu trúc của các chữ cái. Mỗi chữ cái tiếng Anh viết thường chỉ được phân loại theo số lượng “lỗ” mà nó chứa khi được vẽ bằng phông chữ cụ thể mà người đặt vấn đề sử dụng."
date: "2026-07-03T04:57:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103566
codeforces_index: "A"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 103566
solve_time_s: 48
verified: true
draft: false
---

[CF 103566A - \u0411\u0443\u043a\u0432\u044b \u043d\u0430 \u0437\u0430\u043a\u0430\u0437](https://codeforces.com/problemset/problem/103566/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề biến ngôn ngữ thành thuộc tính cấu trúc của các chữ cái. Mỗi chữ cái tiếng Anh viết thường chỉ được phân loại theo số lượng “lỗ” mà nó chứa khi được vẽ bằng phông chữ cụ thể mà người đặt vấn đề sử dụng. Chính xác là một chữ cái có hai lỗ và sáu chữ cái có một lỗ, trong khi tất cả các chữ cái còn lại đều không có lỗ. 

Cho một chuỗi bao gồm các chữ cái viết hoa, nhiệm vụ là xác định tổng cộng có bao nhiêu lỗ xuất hiện khi chuỗi được viết bằng cách sử dụng ánh xạ phông chữ này. Mỗi lần xuất hiện của một chữ cái đều đóng góp độc lập theo số lỗ trống của nó, vì vậy các chữ cái lặp lại sẽ tích lũy phần đóng góp của chúng. 

Đầu vào là một chuỗi đơn. Đầu ra là một số nguyên biểu thị tổng số lỗ trên tất cả các ký tự. 

Mặc dù câu lệnh ngắn nhưng ý tưởng chính là ánh xạ cố định và nhỏ, do đó việc tính toán hoàn toàn là vấn đề phân loại theo từng ký tự. 

Từ góc độ phức tạp, độ dài đầu vào có thể được coi là lớn theo kiểu Codeforces điển hình, tối đa khoảng 10^5 ký tự trở lên. Điều đó ngay lập tức hàm ý rằng giải pháp phải tuyến tính theo độ dài chuỗi. Bất kỳ cách tiếp cận nào quét hoặc xử lý từng ký tự nhiều lần trong các vòng lặp lồng nhau sẽ trở nên quá chậm. Kích thước bảng chữ cái là không đổi, do đó cần phải tra cứu theo thời gian liên tục cho mỗi ký tự. 

Một số trường hợp đặc biệt rất dễ bị bỏ sót nếu người ta sử dụng kiểu chữ tiếng Anh thông thường thay vì định nghĩa tùy chỉnh của vấn đề. Ví dụ: việc xử lý các chữ cái như “B” hoặc “D” một cách không nhất quán tùy thuộc vào giả định về phông chữ sẽ phá vỡ tính chính xác. Một trường hợp tinh vi khác là khi chuỗi chỉ chứa các chữ cái không có lỗ, các chữ cái này sẽ xuất ra số 0 một cách chính xác thay vì kết quả trống hoặc giá trị chưa được khởi tạo. Một ví dụ tối thiểu là đầu vào`C`, phải trả lại`0`, vì chữ cái đó không có lỗ hổng trong định nghĩa này. 

## Phương pháp tiếp cận 

Cách trực tiếp để giải quyết vấn đề là đối với mỗi ký tự, hãy kiểm tra thủ công xem nó thuộc tập hợp các chữ cái một lỗ hay chữ cái hai lỗ đặc biệt. Đối với mỗi ký tự, chúng ta có thể so sánh nó với tất cả bảy chữ cái có liên quan và tích lũy câu trả lời tương ứng. Điều này hoạt động vì việc phân loại là tĩnh và độc lập cho mỗi ký tự. 

Trong cách diễn giải brute-force, đối với mỗi ký tự, chúng ta có thể lặp qua danh sách các chữ cái có lỗ đã biết và kiểm tra tư cách thành viên. Với tối đa 7 phép so sánh cho mỗi ký tự, tổng chi phí tỷ lệ thuận với 7n. Mặc dù điều này đã tuyến tính trong thực tế, nhưng cấu trúc sẽ trở nên rõ ràng hơn một chút nếu chúng ta chuyển đổi việc kiểm tra tư cách thành viên thành cấu trúc tra cứu theo thời gian không đổi, chẳng hạn như mảng boolean hoặc tập hợp băm. Điều đó loại bỏ sự so sánh lặp đi lặp lại và đơn giản hóa logic. 

Điều quan trọng nhất là bảng chữ cái cố định và rất nhỏ. Chúng ta có thể tính toán trước ánh xạ từ ký tự đến số lỗ và sau đó tính tổng các giá trị trên chuỗi trong một lần chuyển. Điều này làm giảm vấn đề lập chỉ mục mảng cho mỗi ký tự. 

Cách tiếp cận bạo lực có hiệu quả vì chúng tôi chỉ kiểm tra tư cách thành viên nhưng nó trở nên lặp đi lặp lại một cách không cần thiết. Cách tiếp cận được tối ưu hóa sẽ nén tất cả logic phân loại vào một bảng tra cứu theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| So sánh từng ký tự với danh sách | O(n) | O(1) | Đã chấp nhận | 
| Bảng tra cứu trực tiếp từng ký tự | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng ánh xạ trực tiếp từ các ký tự đến số lỗ trống của chúng, sau đó tích lũy kết quả trong khi quét chuỗi một lần. 

1. Xác định tập hợp chữ có đúng một lỗ là`A, D, O, P, Q, R`. Gán mỗi giá trị là 1. Xác định chữ cái có hai lỗ như`B`, được gán giá trị 2. Tất cả các chữ cái khác ánh xạ ngầm tới 0. Mã hóa này biến thuộc tính trực quan thành số học. 
2. Tạo một cấu trúc tra cứu, thường là một mảng có kích thước 26 hoặc một từ điển, ánh xạ từng chữ cái viết hoa vào số lỗ của nó. Điều này đảm bảo truy cập liên tục theo thời gian cho mỗi ký tự mà không cần so sánh lặp lại. 
3. Khởi tạo biến tích lũy`ans = 0`nó sẽ lưu trữ tổng số lỗ trên chuỗi. 
4. Lặp lại từng ký tự trong chuỗi đầu vào. Đối với mỗi ký tự, hãy thêm giá trị được ánh xạ của nó vào`ans`. Tính chính xác đến từ việc xử lý từng chữ cái xuất hiện một cách độc lập. 
5. Đầu ra`ans`sau khi xử lý tất cả các ký tự. 

### Tại sao nó hoạt động 

Mỗi ký tự đóng góp độc lập vào tổng số và đóng góp chỉ phụ thuộc vào phân loại cố định không thay đổi trong quá trình xử lý. Thuật toán duy trì bất biến sau khi xử lý k ký tự đầu tiên,`ans`bằng tổng số lỗ cho k ký tự đó. Vì mỗi bước chỉ thêm phần đóng góp chính xác cho ký tự hiện tại nên bất biến giữ nguyên cho đến cuối chuỗi, khi đó nó bằng tổng số được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

hole = [0] * 26
hole[ord('A') - ord('A')] = 1
hole[ord('D') - ord('A')] = 1
hole[ord('O') - ord('A')] = 1
hole[ord('P') - ord('A')] = 1
hole[ord('Q') - ord('A')] = 1
hole[ord('R') - ord('A')] = 1
hole[ord('B') - ord('A')] = 2

ans = 0
for ch in s:
    ans += hole[ord(ch) - ord('A')]

print(ans)
```Giải pháp tính toán trước một mảng cố định`hole`trong đó mỗi chỉ số tương ứng với một chữ cái. Ánh xạ sử dụng độ lệch ASCII để mỗi ký tự được chuyển đổi thành chỉ mục trong thời gian không đổi. Điều này tránh mọi chuỗi có điều kiện bên trong vòng lặp. 

Vòng tích lũy là cốt lõi của giải pháp. Mỗi lần lặp chỉ thực hiện một phép truy cập mảng và một phép cộng, cả hai đều là các phép toán có thời gian không đổi. Thứ tự thực hiện rất đơn giản vì không có trạng thái nào phụ thuộc vào các ký tự trong tương lai. 

Một lỗi phổ biến là tính toán lại tư cách thành viên bằng cách sử dụng tìm kiếm chuỗi hoặc so sánh lặp lại, điều này gây tốn kém không cần thiết. Một vấn đề nhỏ khác là quên loại bỏ đầu vào, điều này có thể tạo ra các ký tự dòng mới và phá vỡ việc lập chỉ mục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ABCD
```Chúng tôi theo dõi đóng góp từng bước. 

| Nhân vật | Giá trị | Tổng số chạy | 
| --- | --- | --- | 
| A | 1 | 1 | 
| B | 2 | 3 | 
| C | 0 | 3 | 
| D | 1 | 4 | 

Kết quả cuối cùng là 4, khớp với tổng số lỗ do mỗi chữ cái đóng góp. Điều này xác nhận rằng ánh xạ xử lý chính xác các phân loại hỗn hợp, bao gồm cả các chữ cái không có lỗ. 

### Ví dụ 2 

đầu vào:```
QQQ
```| Nhân vật | Giá trị | Tổng số chạy | 
| --- | --- | --- | 
| Q | 1 | 1 | 
| Q | 1 | 2 | 
| Q | 1 | 3 | 

Kết quả là 3, chứng tỏ rằng các chữ cái lặp lại sẽ tích lũy chính xác và không được coi là các thực thể duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý chính xác một lần với tra cứu theo thời gian liên tục | 
| Không gian | O(1) | Mảng ánh xạ có kích thước cố định (26) bất kể độ dài đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc thông thường vì ngay cả đối với 10^5 ký tự, chỉ có 10^5 truy cập mảng đơn giản được thực hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    hole = [0] * 26
    hole[0] = 1
    hole[3] = 1
    hole[14] = 1
    hole[15] = 1
    hole[16] = 1
    hole[17] = 1
    hole[1] = 2
    ans = 0
    for ch in s:
        ans += hole[ord(ch) - 65]
    return str(ans)

assert run("ABCD\n") == "4"
assert run("QQQ\n") == "3"
assert run("C\n") == "0"
assert run("BBBB\n") == "8"
assert run("ADOPQR\n") == "6"
assert run("Z\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ABCD | 4 | Chữ hỗn hợp, độ chính xác cơ bản | 
| QQQ | 3 | Tích lũy lặp lại | 
| C | 0 | Xử lý thư không lỗ | 
| BBBB | 8 | Chia tỷ lệ chữ hai lỗ | 
| QUẢNG CÁO | 6 | Bảo hiểm toàn bộ một lỗ | 
| Z | 0 | Tính chính xác của chữ cái không được liệt kê | 

## Vỏ cạnh 

Trường hợp một cạnh là đầu vào bao gồm toàn bộ các chữ cái không có lỗ. Ví dụ, đầu vào`XYZ`nên sản xuất`0`. Thuật toán xử lý từng ký tự, tìm thấy số 0 trong bảng tra cứu và không tích lũy gì. Tổng cuối cùng vẫn bằng 0, điều này đúng. 

Một trường hợp cạnh khác là sự lặp lại tối đa của chữ cái có hai lỗ. Đối với đầu vào`BBBBB`, mỗi bước cộng thêm 2, do đó tổng số lần chạy sẽ tăng lên thành 2, 4, 6, 8, 10. Vì việc tra cứu độc lập với mỗi ký tự nên không cần xử lý đặc biệt và kết quả vẫn ổn định ngay cả đối với các lần lặp lại lớn. 

Trường hợp tinh tế cuối cùng là đảm bảo lập chỉ mục ký tự chính xác. Nếu mảng ánh xạ được xây dựng bằng cách sử dụng offset ASCII thì mọi chữ cái viết hoa phải căn chỉnh chính xác với chỉ mục của nó. Bất kỳ lỗi nào trong việc lập chỉ mục sẽ phân loại sai các chữ cái, nhưng vì mỗi ký tự được ánh xạ trực tiếp qua`ord(ch) - 65`, ánh xạ vẫn nhất quán trên tất cả các đầu vào.
