---
title: "CF 102503C - Sao chép một phần"
description: "Tên món ăn được xây dựng bằng cách ghép ba phần có thể có: TJ, si và log. Mỗi lần xuất hiện của một phần tượng trưng cho một khẩu phần thành phần tương ứng của nó."
date: "2026-08-06T19:01:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "C"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 274
verified: true
draft: false
---

[CF 102503C - Sao chép một phần](https://codeforces.com/problemset/problem/102503/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tên món ăn được xây dựng bằng cách ghép ba phần có thể có:`TJ`,`si`, Và`log`. Mỗi lần xuất hiện của một phần tượng trưng cho một khẩu phần thành phần tương ứng của nó. Các mảnh được trộn lẫn với nhau mà không có dải phân cách, và nhiệm vụ là tìm xem mỗi mảnh trong số ba mảnh xuất hiện bao nhiêu lần. 

Đầu vào đưa ra một số tên món ăn. Mỗi tên đều có độ dài riêng nhưng độ dài chỉ ở đó dưới dạng siêu dữ liệu đầu vào vì bản thân chuỗi chứa tất cả thông tin cần thiết. Đối với mỗi tên, đầu ra phải chứa ba số đếm theo thứ tự`TJ`,`si`, Và`log`. 

Tổng độ dài của tất cả các tên món ăn tối đa là 100000. Điều đó có nghĩa là thuật toán chỉ xử lý mỗi ký tự với số lần không đổi. Cách tiếp cận bậc hai sẽ thực hiện khoảng 1000002 phép toán trong trường hợp xấu nhất, vượt xa mức cần thiết và sẽ không phù hợp thoải mái trong thời gian giới hạn. Quét tuyến tính là đủ vì mỗi ký tự thuộc về chính xác một trong ba phần hợp lệ. 

Các trường hợp cạnh chính xuất phát từ thực tế là các mảnh có độ dài khác nhau. Một giải pháp bất cẩn có thể đếm các ký tự thay vì mã thông báo hoặc cho rằng mọi phân đoạn chữ thường đều có cùng kích thước. 

Ví dụ:```
1
TJTJ
```Đầu ra đúng là:```
2 0 0
```Đếm số lượng`T`các ký tự hoặc coi hai ký tự như một mã thông báo hoạt động ở đây một cách tình cờ, nhưng nó thất bại ngay khi`si`Và`log`xuất hiện. 

Một trường hợp khác là:```
1
silog
```Đầu ra đúng là:```
0 1 1
```Một giải pháp tìm kiếm`log`đầu tiên và bỏ qua ba ký tự có thể bỏ sót không chính xác`si`một phần nếu nó không duy trì vị trí quét chính xác. 

Trường hợp ranh giới cuối cùng là tên chỉ chứa một mã thông báo:```
1
log
```Đầu ra đúng là:```
0 0 1
```Việc triển khai phải xử lý các chuỗi ngắn mà không cố đọc các ký tự ở cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là liên tục tìm kiếm các phần đã biết trong chuỗi và loại bỏ chúng. Vì chuỗi được đảm bảo là hợp lệ nên chúng ta luôn có thể tìm thấy sự phân tách thành`TJ`,`si`, Và`log`. Phương pháp này đúng vì mỗi phần bị loại bỏ tương ứng với một phần ăn. 

Vấn đề với cách tiếp cận này là việc tìm kiếm và xây dựng lại chuỗi còn lại lặp đi lặp lại. Nếu một chuỗi có độ dài 100000, việc quét liên tục các phần lớn của chuỗi đó có thể dẫn đến khoảng 100000 × 100000 thao tác trong trường hợp xấu nhất, tức là khoảng 10 tỷ lần kiểm tra ký tự. Điều đó là không cần thiết. 

Quan sát quan trọng là chuỗi đã được sắp xếp thành một chuỗi các phần hợp lệ. Chúng tôi không cần phải tìm các mảnh ở đâu trên toàn cầu. Chúng ta chỉ cần quyết định quân cờ hiện tại khi di chuyển từ trái sang phải. Ký tự đầu tiên xác định hoàn toàn loại mã thông báo. MỘT`T`phải bắt đầu một`TJ`, trong khi ký tự chữ thường bắt đầu`si`hoặc`log`. Trong số các mã thông báo viết thường, ký tự thứ hai phân biệt chúng vì`si`bắt đầu bằng`s`Và`log`bắt đầu bằng`l`. 

Brute-force hoạt động vì mọi phân tách hợp lệ đều cho cùng số lượng, nhưng nó thất bại vì nó tiếp tục khám phá lại các vị trí mà một lần quét có thể xác định ngay lập tức. Nhận thấy rằng mỗi mã thông báo có một mẫu bắt đầu duy nhất cho phép chúng tôi giảm toàn bộ tác vụ xuống một trình phân tích cú pháp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ ký tự đầu tiên của tên món ăn và di chuyển từ trái sang phải. Duy trì ba quầy cho`TJ`,`si`, Và`log`. 
2. Nếu ký tự hiện tại là`T`, đếm một`TJ`và di chuyển chỉ số về phía trước hai vị trí. Đảm bảo tính hợp lệ có nghĩa là ký tự tiếp theo phải là`J`. 
3. Nếu ký tự hiện tại là`s`, đếm một`si`và di chuyển chỉ số về phía trước hai vị trí. Ký tự tiếp theo phải là`i`, vì vậy mã thông báo đã được sử dụng hết. 
4. Nếu ký tự hiện tại là`l`, đếm một`log`và di chuyển chỉ số về phía trước ba vị trí. Hai ký tự tiếp theo được đảm bảo hoàn thành mã thông báo. 
5. Sau khi quét đến cuối chuỗi, hãy in ba bộ đếm. 

Lý do phân tích cú pháp này hoạt động là vì không có hai mã thông báo hợp lệ nào có cùng ký tự bắt đầu. Khi quá trình quét bắt đầu với mã thông báo còn lại, sẽ có chính xác một cách diễn giải khả dĩ, do đó không cần phải quay lui hoặc lập trình động. 

Tại sao nó hoạt động: 

Điều bất biến trong quá trình quét là mọi ký tự trước chỉ mục hiện tại đã được chia thành các mã thông báo hoàn chỉnh và được tính chính xác. Ở mỗi bước, mã thông báo tiếp theo được xác định duy nhất bởi ký tự đầu tiên của nó, do đó thuật toán sử dụng chính xác một mã thông báo hợp lệ và tăng chính xác bộ đếm tương ứng của nó. Vì đầu vào được đảm bảo là tên món ăn hợp lệ nên quá trình quét sẽ sử dụng toàn bộ chuỗi, để lại bộ đếm bằng số lần xuất hiện của mỗi mã thông báo thành phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, s = input().split()

        tj = 0
        si = 0
        log = 0

        i = 0
        while i < len(s):
            if s[i] == 'T':
                tj += 1
                i += 2
            elif s[i] == 's':
                si += 1
                i += 2
            else:
                log += 1
                i += 3

        ans.append(f"{tj} {si} {log}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc bằng cách sử dụng`sys.stdin.readline`vì có thể có nhiều trường hợp thử nghiệm. Hàm xử lý từng chuỗi một cách độc lập và lưu trữ các câu trả lời trước khi in chúng cùng nhau. 

Ba bộ đếm biểu thị trực tiếp các giá trị đầu ra được yêu cầu. Biến chỉ mục được nâng cao theo độ dài của mã thông báo đã được nhận dạng. Đây là chi tiết triển khai quan trọng vì việc tăng thêm một chi tiết sẽ liên tục kiểm tra các ký tự đã là một phần của mã thông báo được tính. 

Không cần kiểm tra giới hạn trước khi đọc các ký tự tiếp theo của mã thông báo vì đầu vào đảm bảo rằng mọi tên món ăn đều hợp lệ. Tràn số nguyên không phải là vấn đề đáng lo ngại trong Python và giá trị bộ đếm tối đa có thể chỉ là độ dài chuỗi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
TJTJTJTJTJ
```Quá trình quét liên tục tìm thấy mã thông báo gồm hai ký tự`TJ`. 

| Chỉ mục | Mã thông báo hiện tại | số lượng TJ | đếm nhé | số lượng nhật ký | 
| --- | --- | --- | --- | --- | 
| 0 | TJ | 1 | 0 | 0 | 
| 2 | TJ | 2 | 0 | 0 | 
| 4 | TJ | 3 | 0 | 0 | 
| 6 | TJ | 4 | 0 | 0 | 
| 8 | TJ | 5 | 0 | 0 | 

Bất biến hiển thị ở đây: sau mỗi lần nhảy, tiền tố được xử lý chỉ bao gồm các mã thông báo hoàn chỉnh. Câu trả lời cuối cùng là`5 0 0`. 

### Mẫu 2, trường hợp đầu tiên 

đầu vào:```
TJsilogsilogloglog
```| Chỉ mục | Mã thông báo hiện tại | số lượng TJ | đếm nhé | số lượng nhật ký | 
| --- | --- | --- | --- | --- | 
| 0 | TJ | 1 | 0 | 0 | 
| 2 | si | 1 | 1 | 0 | 
| 4 | nhật ký | 1 | 1 | 1 | 
| 7 | si | 1 | 2 | 1 | 
| 9 | nhật ký | 1 | 2 | 2 | 
| 12 | nhật ký | 1 | 2 | 3 | 
| 15 | nhật ký | 1 | 2 | 4 | 

Dấu vết cho thấy tại sao chỉ cần nhìn vào ký tự đầu tiên là đủ. Mỗi chuyển đổi có thể có một mã thông báo, do đó trình phân tích cú pháp không bao giờ cần phải xem xét lại các lựa chọn trước đó. Câu trả lời cuối cùng là`1 2 4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập một lần như một phần của chính xác một mã thông báo. | 
| Không gian | O(1) | Chỉ có ba bộ đếm và chỉ mục hiện tại được lưu trữ. | 

Tổng kích thước đầu vào tối đa là 100000 ký tự, do đó quét tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ các chuỗi đầu vào theo yêu cầu của Python. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, s = input().split()
        tj = si = log = 0
        i = 0

        while i < len(s):
            if s[i] == 'T':
                tj += 1
                i += 2
            elif s[i] == 's':
                si += 1
                i += 2
            else:
                log += 1
                i += 3

        out.append(f"{tj} {si} {log}")

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    result = solve()
    sys.stdin = old_stdin
    return result

assert run("""1
10 TJTJTJTJTJ
""") == "5 0 0", "sample 1"

assert run("""2
18 TJsilogsilogloglog
10 silogsilog
""") == "1 2 4\n0 2 2", "sample 2"

assert run("""1
2 TJ
""") == "1 0 0", "single TJ token"

assert run("""1
3 log
""") == "0 0 1", "single log token"

assert run("""1
14 silogloglog
""") == "0 1 3", "multiple adjacent log tokens"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`TJ`|`1 0 0`| Xử lý mã thông báo hotdog ngắn nhất có thể. | 
|`log`|`0 0 1`| Xử lý mã thông báo trứng ngắn nhất có thể. | 
|`silogloglog`|`0 1 3`| Kiểm tra xem trình phân tích cú pháp có phân tách chính xác các mã thông báo liền kề hay không. | 

## Vỏ cạnh 

Đối với đầu vào:```
1
TJTJ
```thuật toán bắt đầu tại chỉ mục`0`, thấy`T`, thêm một vào`TJ`bộ đếm và nhảy tới chỉ mục`2`. Nó lặp lại hành động tương tự và tạo ra:```
2 0 0
```Điều này xử lý các chuỗi chỉ được tạo từ một loại mã thông báo. 

Đối với đầu vào:```
1
silog
```thuật toán nhìn thấy đầu tiên`s`, đếm`si`, và di chuyển đến`l`tính cách. Sau đó nó đếm`log`. Kết quả là:```
0 1 1
```Điều này xác nhận rằng các mã thông báo ngắn hơn có thể xuất hiện ngay trước các mã thông báo dài hơn mà không có sự mơ hồ. 

Đối với đầu vào:```
1
log
```thuật toán đi vào`else`nhánh một lần, tăng`log`bộ đếm và di chuyển qua cả ba ký tự. Đầu ra là:```
0 0 1
```Quá trình quét không bao giờ cố gắng truy cập các ký tự bên ngoài chuỗi vì chỉ mục di chuyển chính xác theo kích thước của mã thông báo được nhận dạng.
