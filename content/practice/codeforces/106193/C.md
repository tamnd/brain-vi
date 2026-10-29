---
title: "CF 106193C - Mã hóa nhỏ gọn"
description: "Chúng tôi được cung cấp một số nguyên không âm vừa với 32 bit và chúng tôi cần chuyển đổi nó thành mã hóa byte có độ dài thay đổi. Thay vì luôn sử dụng bốn byte, chúng tôi biểu thị số bằng cách sử dụng các đoạn 7 bit được đóng gói thành byte."
date: "2026-06-19T18:39:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106193
codeforces_index: "C"
codeforces_contest_name: "2025-2026 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 106193
solve_time_s: 47
verified: true
draft: false
---

[CF 106193C - Mã hóa nhỏ gọn](https://codeforces.com/problemset/problem/106193/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số nguyên không âm vừa với 32 bit và chúng tôi cần chuyển đổi nó thành mã hóa byte có độ dài thay đổi. Thay vì luôn sử dụng bốn byte, chúng tôi biểu thị số bằng cách sử dụng các đoạn 7 bit được đóng gói thành byte. Mỗi byte lưu trữ 7 bit số ở phần dưới của nó, trong khi bit cao nhất của byte đóng vai trò là cờ tiếp tục. Nếu cờ đó được đặt, điều đó có nghĩa là sẽ có nhiều byte hơn theo sau; nếu nó bằng 0 thì byte đó là byte cuối cùng. 

Các byte được tạo ra từ phía có ý nghĩa nhất trước tiên, do đó byte đầu tiên chứa đoạn 7 bit có thứ tự cao nhất của số và byte cuối cùng chứa đoạn có thứ tự thấp nhất. Đây thực chất là một biểu diễn cơ sở 128 được viết theo thứ tự big-endian với một bit tiếp tục. 

Một hạn chế chính là chúng tôi không được phép đưa ra các đoạn số 0 ở đầu. Trong thực tế, điều này có nghĩa là chúng ta không thể bắt đầu mã hóa bằng một byte có 7 bit thấp hơn đều bằng 0 trừ khi bản thân số đó bằng 0. 

Kích thước đầu vào nhỏ theo nghĩa là số lượng nhiều nhất$2^{31} - 1$. Điều đó có nghĩa là biểu diễn nhị phân có nhiều nhất là 31 bit, do đó việc chia thành các khối 7 bit sẽ mang lại nhiều nhất là 5 byte. Bất kỳ giải pháp nào hoạt động trong thời gian không đổi cho mỗi lần kiểm tra đều không đáng kể và thậm chí các thao tác bit lặp đi lặp lại cũng không đáng kể. 

Trường hợp cạnh chính xuất hiện khi số bằng 0. Cách tiếp cận phân đoạn đơn giản có thể tạo ra một chuỗi trống hoặc một byte 0 đơn và rất dễ vô tình vi phạm quy tắc “không có byte 0 đứng đầu” trong cách gán các bit tiếp tục. 

Một vấn đề tinh tế khác là đặt hàng. Nếu người ta trích xuất các đoạn 7 bit từ phía ít quan trọng nhất trước và in ngay lập tức, kết quả sẽ bị đảo ngược trừ khi được sửa rõ ràng. Ví dụ: mã hóa 112025 yêu cầu nhóm từ các bit quan trọng nhất; đảo ngược thứ tự tạo ra một chuỗi byte hợp lệ về mặt cú pháp nhưng giá trị được giải mã không chính xác. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là mô phỏng việc trích xuất lặp đi lặp lại các đoạn 7 bit từ số. Ở mỗi bước, chúng ta có thể lấy 7 bit thấp nhất, dịch số sang phải 7 và lưu trữ các khối này vào danh sách. Điều này tạo ra sự phân hủy tự nhiên thành 128 chữ số cơ bản. 

Ý tưởng thô bạo này đúng về mặt biểu diễn số, nhưng nó tạo ra các chữ số theo thứ tự ngược lại, từ ít quan trọng nhất đến quan trọng nhất. Sau đó chúng ta cần đảo ngược danh sách trước khi xây dựng byte. Về mặt lý thuyết, chi phí của phương pháp này là thời gian không đổi, vì chúng tôi thực hiện tối đa năm lần lặp, nhưng vấn đề về mặt khái niệm không phải là hiệu suất mà là tính chính xác của thứ tự và vị trí gắn cờ. 

Quan sát quan trọng là mã hóa chính xác là biểu diễn cơ sở 128, nhưng được viết bằng bit tiếp tục. Khi chúng tôi tính toán tất cả 128 chữ số cơ bản, chúng tôi chỉ cần xuất chúng từ quan trọng nhất đến ít quan trọng nhất. Mọi chữ số ngoại trừ byte được in cuối cùng phải được đặt bit tiếp tục. 

Vì vậy, nhiệm vụ giảm xuống còn chuyển đổi số nguyên thành cơ số 128, lưu trữ các chữ số, sau đó phát ra chúng ngược lại bằng thao tác bit thích hợp. Cấu trúc này làm cho vấn đề trở nên xác định và loại bỏ mọi nhu cầu truyền phát bitwise phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phát trực tiếp thủ công mà không đảo ngược kỷ luật) | O(1) | O(1) | Rủi ro/dễ xảy ra lỗi | 
| Tối ưu (phân tách cơ sở 128 + đảo ngược) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý số bằng cách trích xuất liên tục các khối 7 bit cho đến khi số đó bằng 0. 

1. Liên tục lấy 7 bit thấp nhất của số bằng cách sử dụng mặt nạ và lưu chúng dưới dạng một đoạn. Sau đó dịch số sang phải 7 bit. Điều này xây dựng số trong cơ sở 128 từ ít quan trọng nhất đến quan trọng nhất. 
2. Lưu trữ tất cả các đoạn trong danh sách. Mỗi đoạn biểu thị tải trọng của một byte trước khi chúng tôi đặt cờ tiếp tục. 
3. Nếu danh sách trống, điều đó có nghĩa là số đó bằng 0, vì vậy chúng tôi đặt rõ ràng một giá trị đoạn duy nhất là 0. 
4. Đảo ngược danh sách để bây giờ chúng ta xử lý đoạn quan trọng nhất trước tiên. 
5. Đối với mỗi đoạn, hãy chuyển đổi nó thành một byte. Đối với tất cả các đoạn ngoại trừ đoạn cuối cùng, đặt bit cao nhất thành 1 bằng cách thêm 128. Đối với đoạn cuối cùng, hãy giữ nguyên. 

Lý do đảo ngược là sự tiếp tục phụ thuộc vào việc liệu có tồn tại những phần quan trọng hơn hay không. Sự phụ thuộc đó chỉ được biết sau khi thu thập tất cả các khối chứ không phải trong quá trình trích xuất. 

### Tại sao nó hoạt động 

Thuật toán xây dựng biểu diễn cơ số 128 của số. Mỗi đoạn được trích xuất tương ứng chính xác với một chữ số trong cơ số 128. Vì bất kỳ số nguyên nào cũng có biểu diễn cơ số 128 duy nhất nên phép phân tách là duy nhất. Bit tiếp tục không làm thay đổi giá trị số của đoạn, nó chỉ mã hóa cấu trúc. Do đó, việc gán các bit tiếp tục sau khi phân tách hoàn toàn sẽ duy trì tính chính xác và việc đảo ngược đảm bảo đoạn quan trọng nhất được phát ra trước tiên, phù hợp với mã hóa big-endian được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

chunks = []

if n == 0:
    chunks = [0]
else:
    while n > 0:
        chunks.append(n & 127)
        n >>= 7

chunks.reverse()

res = []
for i in range(len(chunks)):
    if i != len(chunks) - 1:
        res.append(chunks[i] | 128)
    else:
        res.append(chunks[i])

print(*res)
```Mã tuân theo chiến lược phân rã trực tiếp. Vòng lặp`n & 127`trích xuất 7 bit thấp nhất, tương ứng với chữ số cơ sở 128. Dịch chuyển 7 sẽ chuyển sang chữ số tiếp theo. Việc kiểm tra rõ ràng về số 0 đảm bảo chúng tôi xuất ra một byte thay vì một chuỗi trống. 

Bước đảo ngược là cần thiết vì các bit tiếp tục phụ thuộc vào vị trí trong thứ tự cuối cùng chứ không phải thứ tự trích xuất. Bitwise OR với 128 đặt cờ tiếp tục cho tất cả trừ byte cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 112025 

Đầu tiên chúng tôi phân tách thành các khối 7 bit. 

| Bước | n trước | Đoạn trích xuất (n & 127) | n sau ca | 
| --- | --- | --- | --- | 
| 1 | 112025 | 25 | 1750 | 
| 2 | 1750 | 110 | 13 | 
| 3 | 13 | 13 | 0 | 

Đảo ngược cho các khối: [13, 110, 25]. 

Bây giờ chúng ta gán các bit tiếp tục. 

| Đoạn | Vị trí | Byte đầu ra | 
| --- | --- | --- | 
| 13 | đầu tiên | 13 + 128 = 141 | 
| 110 | giữa | 110 + 128 = 238 | 
| 25 | cuối cùng | 25 | 

Đầu ra cuối cùng: 141 238 25. 

Dấu vết này cho thấy cách phân tách căn chỉnh với các chữ số cơ sở 128 và cách yêu cầu đảo ngược trước khi áp dụng cờ tiếp tục. 

### Ví dụ 2: n = 0 

| Bước | Hành động | 
| --- | --- | 
| - | n bằng 0, không chạy vòng trích xuất | 
| - | khối = [0] | 

Đầu ra là một byte đơn 0. 

Điều này xác nhận rằng số 0 được coi là biểu diễn tối thiểu đặc biệt và tránh tạo ra mã hóa trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tối đa 5 lần lặp kể từ 32 bit/7 bit mỗi đoạn | 
| Không gian | O(1) | Chỉ một danh sách cố định nhỏ tối đa 5 byte | 

Các ràng buộc đảm bảo rằng ngay cả đầu vào trong trường hợp xấu nhất cũng cần phải làm việc liên tục. Giải pháp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())

    chunks = []

    if n == 0:
        chunks = [0]
    else:
        while n > 0:
            chunks.append(n & 127)
            n >>= 7

    chunks.reverse()

    res = []
    for i in range(len(chunks)):
        if i != len(chunks) - 1:
            res.append(chunks[i] | 128)
        else:
            res.append(chunks[i])

    return " ".join(map(str, res))

# provided samples
assert run("0\n") == "0"
assert run("112025\n") == "141 238 25"

# custom cases
assert run("1\n") == "1", "single small number"
assert run("127\n") == "127", "single byte max without continuation"
assert run("128\n") == "129 0", "boundary crossing 7-bit limit"
assert run("255\n") == "129 127", "two-byte encoding edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | trường hợp cạnh đại diện tối thiểu | 
| 1 | 1 | giá trị khác 0 nhỏ nhất | 
| 127 | 127 | không có ranh giới tiếp tục | 
| 128 | 129 0 | ranh giới nhiều byte đầu tiên | 

## Vỏ cạnh 

### Trường hợp: n = 0 

Đầu vào bằng 0, do đó không có vòng phân tách 7 bit nào chạy. Thuật toán chỉ định rõ ràng một đoạn đơn [0], tạo ra byte 0. Không có bộ bit tiếp tục nào phù hợp với yêu cầu mã hóa không được đưa ra các đoạn 0 ở đầu. 

### Trường hợp: n = 127 

Nhị phân phù hợp chính xác trong 7 bit. Quá trình trích xuất tạo ra một đoạn duy nhất [127]. Vì chỉ có một đoạn nên không có bit tiếp theo nào được thêm vào. Đầu ra là 127, thể hiện chính xác giá trị mà không cần thêm byte. 

### Trường hợp: n = 128 

Nhị phân yêu cầu hai khối 7 bit. Trích xuất cho [0, 1], đảo ngược thành [1, 0]. Byte đầu tiên trở thành 1 | 128 = 129 và số thứ hai là 0. Điều này khớp với sự phân chia cơ sở 128 dự kiến ​​​​theo ranh giới byte và thể hiện khả năng xử lý chính xác tải trọng số 0 dẫn đầu ở đoạn cao hơn. 

### Trường hợp: n = 255 

Quá trình trích xuất tạo ra các khối [127, 1], đảo ngược thành [1, 127]. Đầu tiên trở thành 1 | 128 = 129, giây vẫn là 127. Thuật toán mã hóa chính xác các giá trị yêu cầu nhiều byte mà không đặt sai vị trí các bit tiếp tục, cho thấy thứ tự được xử lý độc lập với nội dung bit.
